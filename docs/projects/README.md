# Platform Service Case Studies

These writeups describe engineering lessons from public-safe services and infrastructure work inside the Tempest platform.

## Home Media Platform

[Read the case study](home-media-platform.md)

The media platform is a practical service build that grew into a full operations story:

- Local, VPN, and public access paths.
- Media server app support across phones, desktops, tablets, and TVs.
- File-sync drop folders for imports.
- Metadata cleanup for movies, TV, and anime.
- Automation for loose files, folders, extras, duplicates, and incomplete uploads.
- Monitoring for origin, proxy, public route, and disk pressure.

The case study focuses on the issues that show up when a media server stops being a toy and starts behaving like a supported service.

## Bookfolio

[Read the case study](bookfolio.md)

Bookfolio is a product-style application for cataloging physical books with real-world intake and cleanup workflows:

- Barcode, ISBN, title, author, and manual entry paths.
- User-selected cover images when metadata is incomplete.
- Admin controls for user lifecycle management.
- Bulk actions for managing large collections.
- Public access through a hardened service edge.
- Monitoring and operations notes like any other supported platform service.

The case study focuses on product workflow, messy metadata, user management, and operating a small public-facing app safely.
