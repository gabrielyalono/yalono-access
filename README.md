# Yalono Access

**Understand your SharePoint access, one object at a time.**

A clean, read-only Microsoft Edge side panel for the permissions and sharing links visible to your Microsoft 365 account.

[**Explore the interactive demo →**](https://gabrielyalono.github.io/yalono-access/)

> **DEMO — fictional data.** The public demo makes no Microsoft connection. Choose Owner, Member, Reader or Access denied to see how the view handles different visibility. No Microsoft 365 tenant has been validated for this development preview.

## A clearer view of access

- Readable people and group roles, with confirmed inheritance where authorized.
- Sharing-link scopes, returned recipients and exact expiration information.
- Folder exploration, search, filters and sorting.
- Read-only exports and opt-in local observations with strict visibility checks.
- English and French. No AI, document-body collection or background tenant monitoring.

Microsoft remains the authority. An absent field or empty result is never presented as proof that no access exists. The live comparison view remains gated when complete comparable visibility cannot be established.

## Try the demo

Open the demo, inspect **People & groups**, switch to **Share links**, then explore **Folder tree**. In **Changes**, save an observation, use **Simulate a change** in the demo toolbar, and save a second observation. Switch to **Access denied** to see historical detail disappear. The data and names are fictional and reset when you reload.

## Documentation

[Installation and Entra setup](INSTALL.md) · [Feasibility and limits](FEASIBILITY.md) · [Security and privacy](SECURITY.md) · [Research](RESEARCH.md) · [Test status](TESTING.md)

Version **0.1.0**, development preview. TypeScript, lint, build and automated fixture tests pass. Real Edge installation, Entra consent and an authorized tenant acceptance run are still required before external release. No Store submission has been made.

## Project boundaries

This public repository contains documentation and the compiled fictional demonstration only. Commercial application source, authentication and Microsoft adapters are maintained in a separate private repository. The demo shares presentation components with the extension; it cannot connect to Microsoft or access your documents.

The demo is static, with no backend, payments, analytics or marketing automation. GitHub Pages may keep its standard security access logs; see [privacy details](SECURITY.md). No Yalono Trader service or resource is modified. No custom domain or paid infrastructure is required.

Independent product; not affiliated with or certified by Microsoft. Microsoft, SharePoint and Edge are trademarks of their respective owners. Third-party runtime licenses are included in [THIRD_PARTY_NOTICES.txt](THIRD_PARTY_NOTICES.txt).
