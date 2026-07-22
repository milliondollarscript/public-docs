---
slug: media-and-draft-recovery
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [media, uploads, drafts, recovery, orders]
---

# Placement Media And Draft Recovery

Customers can upload placement artwork while completing an order. Million Dollar Script validates the file, saves a temporary server-side draft for the verified order, and updates the grid preview without requiring the completed placement to be saved first.

## Supported Images

Placement uploads accept JPEG, PNG, GIF, and WebP images. WordPress and the web server still control the maximum request and file size. The **Maximum Upload Width** and **Maximum Upload Height** settings can apply smaller image-dimension limits; a value of `0` leaves that dimension to the server and WordPress limits.

Use **Cover** when the image should fill the purchased area and may be cropped. Use **Contain** when the whole image must remain visible and empty space around it is acceptable. The order form updates its preview when this choice changes.

## Saved Progress

Text fields and selections are stored in the browser for up to seven days. If an unfinished order is opened again in the same browser, a **Restore details** or **Restore order** action appears when recoverable progress is available.

Selected images are uploaded immediately after local validation. A successful upload creates a temporary WordPress media attachment tied to the verified order, grid selection, and a private draft token. This lets the same image be restored after a refresh. A file that never finished uploading cannot be recovered from browser storage; choose it again.

Temporary image drafts expire after three days unless they are consumed by a saved placement. Expired, replaced, removed, or released drafts are deleted when they are no longer used by a placement.

## Replace Or Remove Artwork

Choose another image to replace the current draft. Million Dollar Script saves the replacement first and then removes the previous unused draft. Use the remove action to delete the current draft without replacing it.

Order identifiers, signed order keys, request nonces, and draft tokens are checked before draft media can be read, replaced, or removed. Do not share an order management link publicly.

## Troubleshooting

- If the browser reports that an image cannot be restored, the original upload did not complete or the temporary draft expired.
- If an upload is rejected, check the file type, WordPress upload limit, PHP request limit, and configured dimension limits.
- If the preview uses the wrong crop, switch between Cover and Contain before saving.
- If server storage is constrained, reduce upload dimensions and confirm WordPress cron can clean stale drafts.
- If large grid rendering exceeds shared-host resources, use the ImageGrid Rendering extension rather than increasing PHP limits indefinitely.
