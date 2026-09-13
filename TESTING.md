# Validation record — development preview

The verification command is `npm run verify`: TypeScript strict checking, ESLint, Vitest, both production builds and a manifest/build-isolation audit. The automated tests use synthetic HTTP replies and a simulated DOM. They do not contact Microsoft 365.

## Executed coverage

| Area | Evidence |
| --- | --- |
| Permission normalization | Missing inheritedFrom, direct/unknown origins, custom role names, unresolved principal types, SharePoint permission masks |
| Sharing | Four scopes, returned vs absent recipients, future/past/null/missing/MinValue/invalid expiration, no invented external identities |
| Network | Same-origin/object pagination, errors 401/403/404, 429/503 and Retry-After, bounded concurrency, request/page/item budgets, cancellation |
| Integration | Real adapter methods called with fixture HTTP transport, Graph/SharePoint audience split, metadata revalidation, missing SharePoint consent |
| Identity | RFC7636 PKCE vector, exact callback/state, duplicate authorization code, scoped consent lists, expired and malformed session handling |
| History | Different account/site/object/source/coverage rejection, lost-visibility hiding, retention, redaction, no false revocations |
| UI | Shared React view, tabs, filter, error state dropping old names, untrusted names escaped |
| Packaging | MV3 schema basics, allowed permissions, background/panel assets present, no content scripts/remote code, fixtures absent in extension, auth absent in demo |

The machine-readable test report and full verification log in the delivery artifacts are the authoritative result/count for the delivered build. A successful build does not establish successful Entra consent, SharePoint role visibility or actual Edge lifecycle behavior.

## Pending external acceptance

- Actual Edge desktop Load unpacked, side panel, expanded view, keyboard navigation and service-worker suspension/restart.
- Entra registration, redirect/CORS behavior, user/admin consent, MFA, token redemption/refresh and sign-out against an authorized tenant.
- Owner/member/reader/denied/guest scenarios and actual paginated SharePoint collections compared with Microsoft's UI.
- SharePoint `AllSites.Read` and rights required by the selected role-assignment/effective-permission reads.
- Cross-tab navigation, multiple profiles and clipboard/download behavior on actual Edge.
- Store submission, review and publication. These require the owner's GO and have not occurred.

The hosted demo is tested separately in the available cloud Chromium browser when deployment is available. That checks the fictional web interface only. A screenshot cannot prove Microsoft integration or desktop Edge installation.
