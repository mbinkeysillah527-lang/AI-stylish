# AI Fashion Studio architecture

The repository now contains three layers:

- `src/` — the existing MonetizeAI web application.
- `flutter_app/` — the Android/iOS Flutter client for AI Fashion Studio.
- `backend/` — the Django REST API and Celery job worker.

## First mobile vertical slice

1. Discover loads products from `GET /api/catalog/products/`.
2. Wardrobe loads only the authenticated user's saved items from `GET /api/wardrobe/items/`.
3. Try-on selects a garment and a private person-photo path, then queues `POST /api/try-on/jobs/`.
4. Celery owns the durable job lifecycle. A GPU worker can later consume the queued job and write the generated result URL.

## Security boundaries

- Catalog endpoints are public and expose active products only.
- Wardrobe and try-on endpoints require JWT authentication.
- Querysets are scoped to `request.user` on every private viewset.
- AI credentials belong in backend environment variables, never in Flutter.
- Uploaded image handling should use signed S3/Cloudinary URLs before production rollout.

## Next production slices

- Add a dedicated upload endpoint with MIME/size validation and private object storage.
- Add a GPU worker service for IDM-VTON or CatVTON with a status callback.
- Add Stripe webhook reconciliation before showing paid entitlements.
- Add social graph, orders, messaging, and creator analytics as separate Django apps.
