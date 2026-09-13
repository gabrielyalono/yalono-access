# Security and privacy

Yalono Access reads the metadata and permission details that Microsoft permits the signed-in account and dedicated application to read. It cannot extend that account's authorization. No AI or remote application backend is part of the product.

## Data handled

Names, principal identifiers, group names, role names, site/object identifiers, object titles/URLs and returned sharing-link metadata can be personal or confidential data. The current view remains in the browser. Returned share-link URLs are held only for the current observation and may be copied explicitly; they are excluded from snapshots and both export formats. JSON exports can still contain the canonical object/site URL, identifiers and personal names. Store exports as confidential information and share them only with authorized recipients.

No document body, file download, page cookie, SharePoint internal token, password, hidden DOM selection or Microsoft Graph directory-wide inventory is collected. No telemetry, remote logging, analytics, external font, AI service or error-reporting endpoint is included.

## Authentication controls

- Dedicated single-tenant delegated OAuth2 code flow, PKCE S256, random state and nonce, exact callback origin/path and code cardinality checks.
- JOSE verifies RS256 ID-token signature from the configured tenant's JWKS, issuer, audience, required claims, expiry and nonce. Graph `/me` must match the signed object ID. A separate SharePoint sign-in must match the same tenant and object ID.
- Graph tokens go only to `graph.microsoft.com`; SharePoint tokens go only to the configured HTTPS SharePoint hostname and permitted site path. No client secret is used. No auth material is sent to GitHub, Cloudflare or the demo.
- Tokens live in trusted `chrome.storage.session`; sign-out and configuration change clear them. Refresh is per audience and bounded. They are not logged or exposed through UI RPC.

## Read and UI boundaries

Actual Microsoft data clients accept only GET requests to allowlisted metadata/permission endpoints, not content or mutations. Redirects and cross-object pagination are rejected. Schemas reject malformed responses; errors and incomplete pagination are carried through as evidence. Strings are rendered through React escaping, never `dangerouslySetInnerHTML`. CSV cells with formula-leading characters are neutralized, including whitespace/control prefixes.

Snapshots are opt-in, local, redacted and bounded. On current access loss they cannot be retrieved by the UI. Comparison requires the same account/site/drive/item/mode/source with complete, available comparable coverage for every included component. Graph sharing results are conservatively caller-limited, so live historical comparisons are gated off in this release. A snapshot is an observation at a stated time, never a continuous audit record or a confirmed revocation event.

The local browser profile and authorized user can inspect local storage. This release does not encrypt local records with a separate user key, promise tamper resistance, certify effective permissions under every policy, or provide an administrator audit service. Browser-session suspension/recovery and end-to-end Entra consent are pending real Edge/tenant acceptance.

## Manifest justification

| Permission | Purpose |
| --- | --- |
| `sidePanel` | Edge sidebar |
| `storage` | Local configuration/opt-in snapshots and session-only tokens |
| `identity` | Microsoft interactive authorization redirect |
| `activeTab` | Active context invalidation after a user gesture |
| `https://graph.microsoft.com/*` | Delegated metadata and sharing reads |
| `https://login.microsoftonline.com/*` | Entra authorize/token/JWKS |
| Optional `https://*.sharepoint.com/*` | Declares possible hosts; Settings requests only the chosen origin |

No `tabs` history permission, `<all_urls>`, cookies, scripting, content scripts, external messages, web-accessible resources, downloads permission or remote executable code. The UI uses a normal local Blob for a user-requested export.

## Demo hosting

Demo accounts, organizations, objects, groups and permissions are fictional. Its build has no live auth/adapter code. The demo's snapshots last only in tab memory. It includes no application tracking. GitHub Pages logs visitor IP addresses for host security, as described in [GitHub's Pages documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages). The offline HTML makes no server requests and can be inspected without a Microsoft account.

Before external distribution, the owner must choose a public privacy/support contact, verify the Store declarations against this implementation and complete real tenant tests. No Store submission or legal/compliance certification is implied.
