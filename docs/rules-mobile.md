# Project Rules – Pet Groomer App (Mobile)

## Domain

Petzibu is a mobile-first, multi-tenant SaaS for pet grooming salons in Turkey. It replaces the phone-contacts-and-Google-Calendar workflow groomers use today. The MVP user is the **salon owner**, usually working solo, on a phone, with an animal in hand: speed and large touch targets beat polish. The mobile app covers every daily operation; a web app comes later as a back office for desk work (reports, import/export), not as a copy of mobile.

Core entities:

- **Business** is the tenant. Every record belongs to one business. The owner is a **User** of that business; staff users arrive in Phase 2, so never assume one user per business.
- **Customer** is a pet owner, identified by phone number (unique per business). Phone is what users think in. Tracks tags, notes, no-show count and outstanding balance.
- **Pet** belongs to one customer, who may have several. Holds species, breed, weight, **size tier** (small / medium / large, suggested from weight, editable), alert tags (e.g. bites, needs muzzle), allergies, cut preferences, vaccinations (rabies, combination; date-based: valid / expiring within 30 days / expired / unknown) and photo history.
- **Service** has a name and a duration + price **per size tier**. Extra charges (dematting, difficult handling) are separate catalogue items.
- **Appointment** is one customer at one time slot with **one or more pet lines**. Each line is a pet plus one or more services; price comes from the pet's size tier and is editable per line. Pets are groomed sequentially, so duration is the sum of lines. Statuses: pending, confirmed, arrived, completed, no_show, cancelled (by customer or salon). Overlaps warn but never block.
- **Grooming report** is per pet: before/after photos and a note, shared to WhatsApp through the OS share sheet. Photos are appended to the pet's history.
- **Statement & Payment**: completing an appointment produces a statement (services, extra charges, discount). Income is recorded only when a **payment** is taken (cash, card, bank transfer; partial allowed). The unpaid remainder becomes customer balance. A statement is not a legal invoice.
- **Expense** is entered manually against a category (consumables, rent, bills). No stock module. The only question answered is "how much did I spend this month".
- **Reminder** (appointment, rebook, vaccination) is semi-automatic: the app notifies the owner, who sends a prefilled WhatsApp message with one tap. Message templates are editable and will be reused when sending becomes automatic.
- **Intake form** is a fixed-template public link the owner shares; customers fill in their own and their pets' details without an account, including KVKK consent. Submissions wait in a review queue and are matched to existing customers by phone. This is the only customer-facing surface in the MVP.

Out of MVP scope, do not build: staff, roles, shifts, commission/payroll; online booking, waitlist, slot management; online payments, card on file, deposits; e-invoice (e-arşiv); automatic WhatsApp/SMS sending; marketing campaigns; recurring appointments; form builder; boarding/daycare. The pet-owner marketplace is a later phase and must not shape MVP decisions.

## Stack

| Concern            | Choice                                                                    |
| ------------------ | ------------------------------------------------------------------------- |
| Framework          | Expo (managed workflow), SDK 57 — bump this line on upgrade               |
| Language           | TypeScript, `strict: true`                                                |
| Routing            | Expo Router                                                               |
| Styling            | NativeWind                                                                |
| Server state       | TanStack Query                                                            |
| Client state       | Zustand (only for true app-wide UI state)                                 |
| Forms + validation | React Hook Form + Zod                                                     |
| API contract       | `@repo/contracts` — the schemas the API and this app share                |
| Backend            | `apps/api` in this monorepo (NestJS)                                      |
| HTTP client        | `fetch` wrapped in `lib/api.ts`                                           |
| Token storage      | expo-secure-store                                                         |
| Dates              | date-fns, `tr` locale                                                     |
| Images             | expo-image, expo-image-picker                                             |
| Dev device         | Expo Go on a physical phone; EAS dev build when a native module is needed |

No other libraries without a clear reason. One library per concern.

## Folder structure

```
src/
  app/                  # Expo Router routes only. Thin: compose feature components.
    (auth)/
    (tabs)/             # index (calendar), customers, cashbox, settings
  features/
    <feature>/          # appointments, customers, pets, services, finance, reports
      api.ts            # Endpoint calls for this feature. Only place that uses lib/api.ts.
      queries.ts        # TanStack Query hooks + query keys
      schema.ts         # Screen-only derivations (form values). Never entity schemas.
      labels.ts         # Turkish copy for the contract's enums
      components/
  components/ui/        # Shared primitives: Button, Input, Card, ListItem, Screen
  lib/                  # api.ts, auth-token.ts, session.ts, query-client.ts, format.ts, theme.ts, utils.ts (cn)
```

- One feature never imports another feature's `api.ts`. Share via `lib/` or compose in `app/`.
- No `utils/` dumping ground. A helper lives next to its only user until a second user exists.

## Backend client

- `lib/api.ts` is the single HTTP client. No direct `fetch` anywhere else.
  - Base URL from `EXPO_PUBLIC_API_URL`.
  - Attaches the access token from `lib/auth-token.ts`.
  - Throws a typed `ApiError` (status, code, message, errors) on any non-2xx response.
  - Handles `401` in one place: refresh once, replay the request, and sign out only if the refresh fails.
- Flow: component → `queries.ts` hook → `api.ts` function → `lib/api.ts`.
- Every response is parsed with its contract schema in `api.ts`, wrapped in `unwrap()` or `unwrapPaginated()` from `@repo/contracts`. The parsed type is the only type used downstream. No hand-written response interfaces.
- Tenant is derived by the API from the auth token. The client never sends `businessId` as authority.
- Both tokens live only in expo-secure-store. Never in Zustand, AsyncStorage or the query cache.
- The session shell reads `GET /auth/me` through `useMe()`: business status, onboarding state and pending consents all come from that one call.
- Mutations invalidate the exact affected query keys. No manual cache patching unless required.

## State

- Server data lives only in TanStack Query. Never copy it into Zustand or `useState`.
- Form state lives only in React Hook Form.
- Zustand only for cross-screen UI state that isn't server data. Default is none. The one store today is `lib/session.ts`: signed-in status only, never the token. The root layout's `Stack.Protected` guards read it.

## Styling

- `className` only. No `StyleSheet`, no inline `style` except for truly dynamic values (e.g. computed widths).
- All colors, spacing, radii, font sizes come from tokens: CSS variables in `global.css`, mapped in `tailwind.config.js`, mirrored for React Navigation in `lib/theme.ts`. Change a color in both files. No arbitrary values (`bg-[#123456]`, `p-[13px]`).
- Primitives come from React Native Reusables, copied into `components/ui/` with `npx @react-native-reusables/cli@latest add <name>` and then owned by us. Variants use `cva`, class merging uses `cn` from `lib/utils.ts`. Text goes through `components/ui/text`, never bare `Text` from react-native outside `components/ui/`.
- Variants live inside the component in `components/ui/`, not repeated at call sites.
- Fonts: Poppins everywhere, loaded in the root layout from `lib/fonts.ts`. `font-display` is the heading alias (Poppins SemiBold today). Weight is a family in React Native, so use `font-sans`, `font-sans-medium`, `font-sans-semibold`, `font-sans-bold`, `font-display`. Never `font-medium` / `font-bold`: they fake-bold on Android.
- Never add `shadow-*`, `animate-*` or `transition-*` classes conditionally (e.g. `selected && 'shadow-sm'`). They set CSS variables or animation state; NativeWind remounts a component that gains them after the first render, and in dev its upgrade warning crashes with a navigation-context error. Give them from the first render or not at all; toggle with `border-*` / `bg-*` instead.
- Touch targets ≥ 48px. Primary actions reachable one-handed.

## API contract

`@repo/contracts` is the single source of the API contract: entity schemas, request bodies,
enums, error codes and the envelope. The API builds its DTOs from the same file, so a schema
copied into this app would be a second source and is not allowed.

- **Import schemas from `@repo/contracts`.** A `features/*/schema.ts` file holds screen-only
  derivations — form values typed as text, a `sameForAll` toggle — and nothing that crosses
  the wire. A new field goes into contracts first, then API and mobile in the same PR.
- Turkish labels for a contract enum live in the feature's `labels.ts`. The contract carries
  no display copy.
- Envelope, parsed by `lib/api.ts`; features never see the wrapper.
  - Success: `{ success: true, status, data, message? }` → `unwrap(schema)` returns `data`.
  - List: `{ success: true, status, data: T[], meta }` → `unwrapPaginated(schema)` returns `{ items, meta }`. `meta` is required: `{ page, limit, total, totalPages, hasNextPage, hasPreviousPage }`. No `count`.
  - Error (any non-2xx): `{ success: false, status, code, message, errors?: [{ field?, message, code? }] }` → thrown as `ApiError(status, code, message, errors)`. `code` is machine-readable and listed in `contracts/errors.ts` (`NOT_FOUND`, `VALIDATION_FAILED`, `TENANT_SUSPENDED`…); `message` is human text and is never branched on. `errors[].field` maps to form fields via `error.fieldErrors`.
- List queries use `ListQuery` from `@repo/contracts` (`page, limit, orderBy, sortDirection, search, dateFrom, dateTo`) plus the endpoint's own filters, and `toQuery()` to build the string. Page-based; `useInfiniteQuery` reads `meta.hasNextPage` / `meta.page`.
- Date-window lists (the calendar) are not paginated: `dateFrom` + `dateTo`, plain `unwrap(z.array(x))`.
- Money: integer kuruş in API and code. Format only at display.
- Time: absolute moments are UTC ISO 8601 with a `Z`; a salon calendar day is `YYYY-MM-DD` and a salon time is `HH:mm`, both plain strings that are never turned into a `Date`. Display in `Europe/Istanbul`.
- Phone: E.164 (`+905xxxxxxxxx`). Normalized in the form schema before sending. Phone is the customer lookup key.
- IDs are opaque strings on the client.

## Code rules

- Fail fast: throw on unmet preconditions. No fallbacks, no "just in case" branches.
- Let TypeScript catch errors; no runtime checks for things the type system guarantees. Validate only at boundaries (API responses, forms) with Zod.
- No `any`, no non-null `!` assertions without a comment explaining why.
- One component per file. Named exports only, except files in `src/app/`: Expo Router requires route and layout files to `export default` their screen.
- UI copy is Turkish, inline. No i18n library in MVP.
- Surgical changes: touch only what the task requires.
- Tests: jest-expo + React Native Testing Library. Test files live in `src/__tests__/` or next to the feature they cover, never inside `src/app/` (Expo Router treats every file there as a route). `render` is async: `await render(...)`.

## Not now

Client codegen, i18n, offline sync, payments, e-invoice, the web back office (K47).

The monorepo and the shared contract package are no longer "not now": the app lives in
`apps/mobile` next to `apps/api`, and schemas come from `@repo/contracts`.
