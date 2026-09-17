# Tea Collector

A simple offline-first mobile app for a tea leaf collection center. Built with
Expo (React Native) + SQLite so it works fully offline, with no login needed.

## Features

- **Home** — today's total weight received from all farmers (tap it to see previous days), search, farmer list, add-farmer button
- **Add Farmer** — name + phone, blocks duplicate names
- **Farmer Detail** — this month's running total, add today's weight (price auto-applied), list of this month's daily records, "Mark Month as Paid"
- **Daily History** — total weight/amount received on each previous day, across all farmers
- **Price** — update the current price per kg (tap the price chip on Home). New entries use the new price; past entries keep the price they were recorded with.

All data is stored locally on the device with SQLite (`expo-sqlite`), so the
app works with no internet connection at all — perfect for a collection
center with poor signal.

## Run it

```bash
npm install
npx expo start
```

This prints a QR code in the terminal. Install the **Expo Go** app on your
phone (Play Store / App Store), then scan the QR code — the app opens
directly on your phone. Every time you edit a file and save, the app
updates automatically.

This project is pinned to **Expo SDK 54**, which matches the current
published version of Expo Go, so scanning the QR code should just work.

## Project structure

```
App.js                     — loads the database, then renders the navigator
src/
  db/database.js           — SQLite schema + all queries (farmers, entries, price, monthly totals, daily totals)
  navigation/AppNavigator.js
  screens/
    HomeScreen.js
    AddFarmerScreen.js
    FarmerDetailScreen.js
    DailyHistoryScreen.js
    PriceScreen.js
  components/
    PrimaryButton.js
    EmptyState.js
  theme/theme.js            — colors, spacing, typography used across the app
```

## Notes on the price rule

Each entry stores its own `price_per_kg` at the moment it's saved
(`src/db/database.js` → `addEntry`). Changing the price in the Price screen
only changes `settings.price_per_kg`, which is read for the *next* entry —
it never touches rows already saved in the `entries` table. That's what
keeps past totals frozen even after the price changes.

## Next steps you might want

- An "Export month to PDF/share" button for handing farmers a printed slip
- A password/PIN to open the app, since it holds farmer payment data
- Backup/restore (e.g. export the SQLite file, or sync to a backend when online)
