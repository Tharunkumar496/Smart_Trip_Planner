# SmartTrip AI — from prototype to production

The attached `smarttrip-ai.jsx` is a fully working front-end prototype: real recommendation
scoring, budget math, itinerary generation, weather-aware reordering, an editable chat
assistant, and a session-only "My Trips" list — all running on a small curated dataset of
five Telangana destinations (Warangal, Yadadri, Nagarjuna Sagar, Bhadrachalam, Vikarabad) so
every part of the flow actually behaves correctly end to end.

To turn this into the full product described in your spec, here's what each remaining piece
needs and roughly where it plugs in.

## 1. Real destination discovery + Gemini AI
- Replace the hard-coded `DESTINATIONS` array and `scoreDestination` logic with a server-side
  call to the Gemini API (via Cloud Functions/Cloud Run, never from the browser).
- Send the user's form inputs, get back candidate destinations + reasoning, and keep your own
  scoring formula (budget fit, distance, interest match, time, weather) applied to whatever
  the model returns — that keeps the score explainable and tunable.

## 2. Google Maps Platform
- Needed for: geocoding the starting location, live distance/duration (Distance Matrix API),
  Places API for real attraction/restaurant/hotel data and photos, and an embedded map
  (Maps JavaScript API) on the trip detail page.
- Call these from a backend (Cloud Function) that holds the API key via Secret Manager, and
  return only the fields the front end needs — never ship the key to the browser unrestricted.
  
- The "Route" and "Get Directions" buttons in the prototype are placeholders; swap them for
  a Directions API request plus a `https://www.google.com/maps/dir/?api=1&...` deep link.

## 3. Weather API
- Any provider (OpenWeatherMap, Tomorrow.io, Google Weather API) works. Fetch a daily
  forecast for the destination's coordinates for the trip dates, and feed it into the same
  `buildItinerary` reordering logic already in the prototype — that part doesn't need to
  change, only the data source does.

## 4. Firebase Authentication + Firestore
- Auth: enable Email/Password and Google sign-in in the Firebase console; wrap the app in
  `onAuthStateChanged` and add a login screen.
- Firestore: use the `users/{userId}` and `trips/{tripId}` structure from your spec. The
  prototype's "Save trip" button already produces the exact object shape you'd write to
  `trips/{tripId}` — just swap the `setSavedTrips` call for a Firestore `addDoc`.
- Security rules: restrict `trips/{tripId}` reads/writes to `request.auth.uid == resource.data.userId`.

## 5. Hosting + backend
- Firebase Hosting for the static front end.
- Cloud Functions or Cloud Run for anything that needs a secret key (Maps, Weather, Gemini) —
  the front end calls your function, your function calls the third-party API.
- Store all keys in Google Cloud Secret Manager and inject them as environment variables at
  deploy time, not in client code.

## 6. Suggested build order
1. Firebase project + Hosting + Auth (get login working first).
2. One Cloud Function that wraps Gemini for destination discovery.
3. Swap in real Places/Maps data for the attractions and hotel cards.
4. Add the Weather API call.
5. Firestore save/load for "My Trips".
6. Polish: skeleton loading states, error handling, rate limiting on the Cloud Functions.

Nothing in the prototype's UI or state logic needs to be thrown away for this — the data
sources get swapped underneath, but the scoring, budget, itinerary and chat logic can stay.
