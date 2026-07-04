# Welcome to your Expo app 👋

This is an [Expo](https://expo.dev) project created with [`create-expo-app`](https://www.npmjs.com/package/create-expo-app).

## Get started

1. Install dependencies

   ```bash
   npm install
   ```

2. Start the app

   ```bash
   npx expo start
   ```

In the output, you'll find options to open the app in a

- [development build](https://docs.expo.dev/develop/development-builds/introduction/)
- [Android emulator](https://docs.expo.dev/workflow/android-studio-emulator/)
- [iOS simulator](https://docs.expo.dev/workflow/ios-simulator/)
- [Expo Go](https://expo.dev/go), a limited sandbox for trying out app development with Expo

You can start developing by editing the files inside the **app** directory. This project uses [file-based routing](https://docs.expo.dev/router/introduction).

## Get a fresh project

When you're ready, run:

```bash
npm run reset-project
```

This command will move the starter code to the **app-example** directory and create a blank **app** directory where you can start developing.

### Other setup steps

- To set up ESLint for linting, run `npx expo lint`, or follow our guide on ["Using ESLint and Prettier"](https://docs.expo.dev/guides/using-eslint/)
- If you'd like to set up unit testing, follow our guide on ["Unit Testing with Jest"](https://docs.expo.dev/develop/unit-testing/)
- Learn more about the TypeScript setup in this template in our guide on ["Using TypeScript"](https://docs.expo.dev/guides/typescript/)

## Learn more

To learn more about developing your project with Expo, look at the following resources:

- [Expo documentation](https://docs.expo.dev/): Learn fundamentals, or go into advanced topics with our [guides](https://docs.expo.dev/guides).
- [Learn Expo tutorial](https://docs.expo.dev/tutorial/introduction/): Follow a step-by-step tutorial where you'll create a project that runs on Android, iOS, and the web.

## Join the community

Join our community of developers creating universal apps.

- [Expo on GitHub](https://github.com/expo/expo): View our open source platform and contribute.
- [Discord community](https://chat.expo.dev): Chat with Expo users and ask questions.

---

## mcall backend integration

This client talks to the **`vectory`** NestJS backend (sibling repo). No UI is
wired up yet — this note documents the API surface so the client work can start.

**Base URL:** the `vectory` server (default `http://localhost:3000` in dev).

**Auth (stub):** every request except `GET /directory/:phoneNumber` must send an
`X-User-Id: <id>` header. This is temporary stub auth; it will be replaced by a
real token later, so keep the user id resolution behind a single client helper.

**Android capability note:** the MVP is designed around the Android 10+
`CallScreeningService` + `ROLE_CALL_SCREENING` role (caller ID + spam screening
for unknown numbers, live, without `READ_CALL_LOG`). Reading historical call
logs requires the heavier default-Phone-handler / Play exception route and is
deferred. See `vectory/README.md` → "Platform constraints & risks".

**Endpoints:**

| Purpose | Call |
|---------|------|
| Quick-save a lead | `POST /contacts/quick-save` `{ phoneNumber, name?, tag?, source? }` |
| List / get / delete contacts | `GET /contacts`, `GET /contacts/:id`, `DELETE /contacts/:id` |
| Add a call note | `POST /notes` `{ phoneNumber, body, callRecordId? }` |
| Notes for a number | `GET /notes?phoneNumber=` |
| Look up a number's label | `GET /directory/:phoneNumber` |
| Contribute / vote on a label | `POST /directory`, `POST /directory/:id/vote` `{ value: 1 \| -1 }` |
| Search history | `GET /search?q=` |
| Reminders | `POST /reminders`, `GET /reminders`, `GET /reminders/due`, `PATCH /reminders/:id` |
| Export / delete my data | `GET /me/export`, `DELETE /me` |

Full contracts live in `vectory/README.md`.
