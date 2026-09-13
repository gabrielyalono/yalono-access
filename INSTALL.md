# Installation and live acceptance

## Install the preview locally

1. Download and extract the extension ZIP. Keep the extracted folder in a stable private location. `manifest.json` must be directly inside that folder.
2. In Microsoft Edge desktop, open `edge://extensions`, enable **Developer mode**, choose **Load unpacked**, and select that folder. Use Edge 124 or later, or a current supported Edge release. Enterprise policy may prohibit unpacked extensions; ask the administrator rather than bypassing policy.
3. Pin Yalono Access. Click its icon to open the side panel. Open Settings and copy the exact redirect URI shown there. It is derived from this installation's extension ID; do not guess it.

The package minimum is Chromium/Edge 124 because its bounded request handling uses `AbortSignal.any`. Side panel APIs require a compatible Chromium-based Edge build. Actual desktop Edge installation has not yet been exercised for this package. [Microsoft Edge sidebar documentation](https://learn.microsoft.com/en-us/microsoft-edge/extensions/developer-guide/sidebar).

## Microsoft Entra configuration — authorized tenant administrator/owner

No app registration was created automatically. Use an isolated test tenant/site with fictional documents and existing licenses; no paid tenant or subscription is required by the code.

1. Create a **single-tenant** Microsoft Entra application dedicated to Yalono Access. Keep its Directory (tenant) ID and Application (client) ID. Do not create a client secret.
2. Register the **exact redirect URI displayed by the extension** as a **Single-page application (SPA)** redirect. The authorization flow uses PKCE S256 and the browser identity API; implicit grant is not used. The extension never adds/spoofs an Origin header. This registration and actual browser token redemption must pass the live test; stop and investigate any platform/CORS mismatch instead of changing token security assumptions.
3. Add Microsoft Graph **delegated** permissions: `User.Read`, `Files.Read`, `Sites.Read.All`. OIDC `openid`, `profile`, `offline_access` are requested for identity and session refresh. Apply the tenant's standard consent process. If approval is required, an authorized administrator must grant it.
4. For optional role/inheritance/effective-permission details, add the **SharePoint delegated `AllSites.Read`** permission and approve only under your normal policy. The extension requests this audience separately with **Enable SharePoint role & inheritance details**. Do not add Write, FullControl, Directory.Read.All, application permissions or legacy SharePoint ACS credentials.
5. In Settings enter tenant ID, client ID and the exact HTTPS site URL, for example `https://yourtenant.sharepoint.com/sites/Finance`. Saving asks Edge for access to that exact hostname, clears old authentication/history and saves the configuration locally.
6. Connect Microsoft 365, choose the authorized test account, complete Microsoft MFA/consent, select a library, then explicitly select a folder/file. If needed, enable the separate SharePoint details consent and select the object again.

PKCE requires a matching registration and redirect URI. Edge unpacked extension IDs can change if installed from a different location or profile. A Store package receives a stable ID; update the registration before testing that package. Never put tenant-specific configuration, tokens, private site URLs or test-person data in the public repository. [Microsoft authorization code flow](https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow), [Microsoft user-consent policy](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/configure-user-consent).

## Required real Microsoft 365 acceptance

This is a procedure, **not a record of tests already passed**. Record account role, tested object, timestamp, Edge version, result and redacted evidence privately.

| Scenario | Expected observation |
| --- | --- |
| Owner / member / reader | Returned visibility may differ; no claim of all users merely from page completion |
| Site/library/folder/file with inherited rights | SharePoint confirms inherited where authorized; Graph absence does not decide |
| Object with unique roles/custom role | Exact role names; unique only after `HasUniqueRoleAssignments=true` |
| Direct user / SP group / unreadable or nested directory group | Explain verified paths; unreadable and nested paths unknown |
| Anonymous / organization / specific people / existing access links | Scope and returned recipients match Manage Access; no link is opened automatically |
| Future/past/no configured/absent expiration | Exact returned timestamp; null/missing remains unavailable |
| 401, 403, consent denied, admin approval, session refresh failure | No stale detail or invented empty inventory |
| Switch account/site/object/tab during request | Previous details disappear; delayed response cannot repopulate them |
| Paginated library and permission collections | Explicit lazy pages; bounded collection; incomplete status on limit |
| Throttling and cancellation | Retry-After honored; user can stop; no recursive tenant scan |
| Snapshot followed by lost access | Historical details hidden; export requires a new check |
| Close browser, reopen; service-worker suspension | Session handling and re-selection work; no background surveillance |

Do not test write permissions by writing data: Yalono Access never performs a permission change. Compare against Microsoft's own UI using the same account and exact object. Keep sharing URLs and raw tokens out of screenshots, bug reports and logs.

## Troubleshooting

- Empty configuration: enter the dedicated app and tenant IDs; the demonstration is a separate file.
- `CONSENT_REQUIRED` / `ADMIN_APPROVAL_REQUIRED`: complete the tenant's approval process. This cannot be bypassed by Yalono Access.
- Graph works, SharePoint details unavailable: confirm the separate delegated audience/consent and actual role visibility. Keep Graph's partial view; do not broaden to FullControl for convenience.
- `SESSION_EXPIRED`: sign in again. Silent renewal is bounded and does not keep long-term credentials on disk.
- `STALE_CONTEXT`: reselect or refresh the object. The UI expires observations after 60 seconds.
- 403 on sharing information: no returned rows is not evidence that nobody has access.
- Large libraries: explore folders on demand. Scans check at most 10 direct children.

## Remove and clean up

Use Settings → Clear local history and Sign out; then Remove in `edge://extensions`. Delete any exported files and extracted package you no longer need. Remove the dedicated application's consent/app registration through normal Entra governance if no longer used. Removing the extension does not revoke any person's document permissions and does not change SharePoint sharing links.
