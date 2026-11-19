# NexTill Shop

NexTill Shop has two separate clients:

- The desktop website is the Vite app at the repository root.
- The mobile app is an Expo Router project in `mobile/`; application screens and logic use JavaScript/JSX.

Both apps save working data locally first. The desktop website uses IndexedDB through Dexie and caches its production app shell with a service worker; the mobile app uses SQLite. Product photos can be captured or selected from the desktop add-product form, and captured on mobile; they remain available offline. Mobile products can use multiple barcode aliases for the same stock unit, and deleting a product archives it so sales history remains intact. Mobile sales and product changes have a local outbox and sync to Supabase when available. Desktop data can be backed up to Supabase as snapshots and restored after a confirmation prompt. Cloud access is optional for local/offline use.

## Run locally

```powershell
npm install
npm run dev
```

```powershell
Set-Location mobile
npm install
npm start
```

## Supabase setup

1. Copy the root `.env.example` to `.env` and `mobile/.env.example` to `mobile/.env`.
2. Set the Supabase project URL and public anon/publishable key in both files. Never put a service-role key in either client.
3. Apply the SQL migrations in `supabase/migrations/` before enabling account registration, backup, or product photo sync. Product photos are stored in a private `product-images` bucket; only shop members can view them, and only owners or managers can upload or replace them.
4. Restart the Vite and Expo development servers after changing environment variables.

The mobile app attempts its preferred local backup time (06:00 by default) using OS background tasks and also checks on app open/reconnect. Product photos are captured and retained on-device for offline use, then uploaded to Supabase Storage when synced. The desktop website checks when it is open or reconnects. Neither browser nor mobile OS scheduling can guarantee execution at exactly 6 AM.

## Build

```powershell
npm run build
```

```powershell
Set-Location mobile
npm run doctor
npm run lint
npx expo export --platform ios
npx expo export --platform android
```

For store builds, configure the application identifiers and signing credentials, then use EAS Build with the profiles in `mobile/eas.json`. Supabase credentials and project configuration are required to validate live authentication, backup, and sync.
# NexTill-Shop
# NexTill-Shop-Web
