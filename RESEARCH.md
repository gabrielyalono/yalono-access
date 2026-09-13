# Product decisions from Microsoft community reports

Research window: September 2016–September 2026. Reviewed 13 September 2026. This is a qualitative sample of dated Microsoft Q&A and Tech Community discussions, not an exhaustive corpus, a prevalence estimate, or evidence that every reported behavior remains reproducible today. No posts, accounts or emails were contacted. Technical rules are checked against Microsoft's current API documentation in FEASIBILITY.md; forum replies are not treated as current API contracts.

| Date / source | Reported difficulty | Decision in v0.1 |
| --- | --- | --- |
| 21 Oct 2016, [Tech Community: document and folder inheritance](https://techcommunity.microsoft.com/discussions/sharepoint_general/sharepoint-online---document-and-folder-permissions-inheritance/23981) | Confusion about moved files, unique permissions and the mental model of folders; later corrections in the thread show why old answers need caution. | Verify the exact current object; never infer inheritance from location. |
| 16 May 2020, [Microsoft Q&A: Inherit permissions SharePoint](https://learn.microsoft.com/en-us/answers/questions/5067450/inherit-permissions-sharepoint) | Difficulty finding permission controls and understanding site/group boundaries. | Plain-language access summary, visible scope and verified access paths. |
| 21 Apr 2023, [Tech Community: permissions and inheritance make no sense](https://techcommunity.microsoft.com/discussions/sharepoint_general/sharepoint-permissions-and-inheritance-make-no-sense/3802394) | Site/subsite and reused-group permissions appear contradictory to the author. | Separate object inheritance from group membership; preserve unknown paths and explain Limited Access. |
| 2 Mar 2024, [Microsoft Q&A: inherit edit permissions but not view permissions](https://learn.microsoft.com/en-us/answers/questions/5304868/sharepoint-inherit-edit-permissions-but-not-view-p) | A confidential team area needs more understandable visibility boundaries. | Show source, exact scope and caller limitations; no universal security score. |
| 4 Jul 2025, [Microsoft Q&A: parent shared links and subfolders](https://learn.microsoft.com/en-us/answers/questions/2338518/sharepoint-online-shared-links-retain-access-to-su) | The author reports unexpected access via a shared link after changing inheritance. This is a reported scenario, not a reproduced finding here. | Display sharing links separately from role assignments. Do not claim unique permissions mean complete isolation. |
| 17 Jan 2026, [Microsoft Q&A: very large libraries](https://learn.microsoft.com/en-us/answers/questions/5722529/we-are-encountering-challenges-in-sharepoint-when) | Managing rights and navigation in a library above 100,000 records is cumbersome. | Lazy pagination and explicit checks bounded to 10 direct children; no tenant-wide crawling. |
| 24 Apr 2026, [Tech Community: SharePoint Permissions Management](https://techcommunity.microsoft.com/discussions/sharepoint_general/sharepoint-permissions-management/4514582) | The author struggles to rediscover unique permissions, obtain spreadsheet observations and understand the limited scope of site-level views. | Folder exploration, redacted exports, honest visibility labels and opt-in local observations. |

## Priority and feasibility

The strongest fit for a browser companion is **clarity at the exact selected object**, with a direct route back to Microsoft. Site owners and business users should see readable role labels, returned sharing scopes/recipients/expiration and why some information is unknown. The product must be useful without claiming tenant-wide access knowledge.

Local comparisons and group paths are constrained by API visibility. The comparison engine is shipped and demonstrated, but the live view suppresses historical details when complete comparable coverage cannot be established. Directory expansion, tenant audit, automated remediation and background monitoring would require a separate scope and authorization assessment; they are not hidden promises in this release.

## Free / Pro direction — proposal only

Free could focus on the current authorized object, readable sharing links and explicit source/visibility. A future Pro offer could add richer local workflows and validated comparison/export convenience. The current release has no paywall, subscription, activation key, payment processor or online license enforcement. Pricing, commercial terms, marketing automation and customer outreach belong to phase 2 after a separate GO. No revenue is assumed or promised.
