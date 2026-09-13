# Feasibility and evidence matrix

Reviewed 13 September 2026. **Implemented** below means code exists and is exercised against fixtures; it does not mean an actual Microsoft tenant has passed acceptance. No Entra application ID, authorized test tenant or Edge runtime was available in this session.

| Capability | Actual source and fields | Delegated consent / additional visibility | Behavior when unavailable | Status |
| --- | --- | --- | --- | --- |
| Identity | Signed Entra ID token (`tid`, `oid`) and Graph `/me?$select=id` | `openid profile offline_access User.Read` | Sign-in/configuration error; no borrowed page credentials | Implemented; live validation required |
| Site/library discovery | Graph `/sites/{hostname}:/{path}` and `/sites/{siteId}/drives` | `Sites.Read.All`; tenant consent policy may require administrator | Exact URL verified; no drive list inferred from DOM | Implemented; simulated |
| Folder/file context | Graph `/drives/{driveId}/items/{itemId}` and `/children`, identifiers + metadata only | `Files.Read` and caller's object access | Object unavailable; explicit selection | Implemented; simulated |
| Sharing permissions | Graph driveItem `/permissions`, `roles`, `link`, V2 granted identities | `Files.Read`; returned set depends on caller | Caller-limited label even after all returned pages | Implemented; simulated |
| Inheritance | SharePoint list/list-item `HasUniqueRoleAssignments` | Separate SharePoint `AllSites.Read`; actual role visibility still required | Unknown; Graph's missing `inheritedFrom` never proves direct access | Implemented; tenant validation required |
| Actual/custom roles | SharePoint `RoleAssignments`, `Member`, `RoleDefinitionBindings/Name` | Separate SharePoint consent and authorization to enumerate assignments | Partial/denied; fall back to visible Graph sharing entries with unknown origin | Implemented; tenant validation required |
| Current user's capabilities | SharePoint `EffectiveBasePermissions` High/Low | SharePoint consent and readable object | Unknown; group names never imply owner/edit privileges | Implemented; simulated masks |
| Why access | Returned current user ID, current-user SharePoint groups and returned role bindings | Only already readable group data | Unresolved/nested/directory paths remain unknown | Limited implementation |
| Visible SP group membership | `_api/web/sitegroups(id)/users` | Group readable by caller | Incomplete/unavailable; no directory-wide scopes | Implemented; simulated |
| Entra/M365/nested membership, guest status | Not requested in v0.1 | Would require individually justified directory permissions | Unknown; never guessed from name or email domain | Intentionally unavailable |
| Share link scope and recipients | Graph `link.scope`, `grantedToIdentitiesV2` | Only what Graph returns for caller | Missing recipients explicitly unavailable | Implemented; simulated |
| Expiration | Graph `expirationDateTime` | Returned field and documented semantics | Missing/null/invalid = unavailable; explicit MinValue = no expiration | Implemented; simulated |
| Download limitation | Graph `link.preventsDownload` or returned `blocksDownload` link type | Returned field/type | No inference when absent | Implemented; simulated |
| Link usability or guest lifetime | No request; no link traversal | Requires separate actual access/policy information | Never labeled Active; expiration does not guarantee usability | Not claimed |
| Can manage | EffectiveBasePermissions ManagePermissions bit | Current user's rights, not a name-based owner assumption | Unknown unless bit verified; app itself remains read-only | Implemented; simulated |
| Open Microsoft UI | Canonical current item `webUrl` | Normal Microsoft authorization | No guessed Manage Access deep link | Implemented; click required |
| Local differences | Two normalized observations | Exact identity/scope/source/full comparable visibility and current revalidation | Historical details/deltas withheld when incomplete | Demo works; live Graph coverage gates it off in v0.1 |
| Scan | Explicit request, first 10 direct children | Same delegated object visibility | Unknown/unavailable for unverified children | Implemented; simulated |

## Important limits

Graph sharing permissions are not a universal SharePoint ACL. An owner/co-owner may receive more results than an ordinary user, and sensitive link properties may be withheld. The extension does not try to promote a result to a full inventory merely because pagination ends. [Microsoft: list driveItem permissions](https://learn.microsoft.com/en-us/graph/api/driveitem-list-permissions?view=graph-rest-1.0).

For SharePoint and OneDrive for Business, Graph does not return `inheritedFrom`. The extension obtains inheritance from SharePoint when possible. Microsoft documents `DateTime.MinValue` as no expiration; absent/null is treated separately. [Microsoft: permission resource](https://learn.microsoft.com/en-us/graph/api/resources/permission?view=graph-rest-1.0).

Scope describes who is eligible under the sharing link. Returned authorized recipients are neither an email delivery list nor a usage log. Existing-access links do not grant new access. [Microsoft: sharingLink resource](https://learn.microsoft.com/en-us/graph/api/resources/sharinglink?view=graph-rest-1.0).

`Sites.Read.All` is used for site-by-path discovery; `Files.Read` is used for file metadata and sharing permissions. No application permission or write scope is requested. This read-only scope combination still requires review under the tenant's consent policy. [Site discovery](https://learn.microsoft.com/en-us/graph/api/site-getbypath?view=graph-rest-1.0), [consent policies](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/configure-user-consent).

The SharePoint REST assignment structure is documented in Microsoft's REST examples. Those legacy examples also contain mutations and add-in authentication, which this extension does not use. Successful Entra delegated `AllSites.Read` access to every selected property/collection must be verified on the intended tenant. A 403 will not trigger a request for FullControl. [REST structure reference](https://learn.microsoft.com/en-us/sharepoint/dev/sp-add-ins/set-custom-permissions-on-a-list-by-using-the-rest-interface), [permission mask enum](https://learn.microsoft.com/en-us/dotnet/api/microsoft.sharepoint.client.permissionkind?view=sharepoint-csom).

Graph site `/permissions` is not used to enumerate ordinary site users; that endpoint has a different purpose. No undocumented Microsoft endpoint is used to obtain access data.
