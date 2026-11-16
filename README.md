# Multi-Step Form

A three-step registration form in React that validates every field as you type, keeps your data when you move between steps and submits to a mock REST API. After submitting, it routes to a posts feed served by the same mock API.

I built it as a take-home frontend assignment. It's a compact example of form state, validation and API mocking without a form library.

## Features

- **Three steps with shared state.** Each step saves into one parent state object, so going *Back* or *Next* never loses input.
- **Live per-field validation.** Errors appear as you type and are checked again on *Save*, *Next* and *Submit*:
  - Step 1: email format, plus a password with at least 2 uppercase, 2 lowercase, 2 digits and 2 special characters
  - Step 2: first name (letters only, 2–50 characters), optional last name, address (10+ characters)
  - Step 3: country code (`+91` or `+1`), a 10-digit phone number, terms accepted
- **Save without advancing.** Each step has a *Save* action with a toast confirmation (react-toastify).
- **Mock API with MSW.** `POST /submit` and `GET /posts` are intercepted by a Mock Service Worker, and the posts are generated with Faker.
- **Posts page.** Loading, error and success states for the fetched feed.
- **Responsive layout** with Tailwind CSS. Show/hide toggle on the password field.

## Tech stack

React 18 · Vite · React Router 6 · Tailwind CSS · MSW · Faker · react-toastify · Heroicons · ESLint · Prettier

## Getting started

```bash
git clone https://github.com/hemantpatil135/multi-step-form.git
cd multi-step-form
npm install
npm run dev        # http://localhost:5173
```

| Script | What it does |
|---|---|
| `npm run dev` | Start the dev server with the mock API turned on |
| `npm run build` | Production build |
| `npm run preview` | Preview the production build |
| `npm run lint` | ESLint |
| `npm run format` / `format:check` | Prettier |

No environment variables are needed.

> **Note:** the mock API only starts in development (`main.jsx` skips MSW when `import.meta.env.PROD` is true). A production build has no backend to submit to, so run it with `npm run dev`.

## Project structure

```
src/
├── components/
│   ├── MultiStepFormWrapper.jsx   # owns form state and the current step
│   ├── FormStep1.jsx              # email + password
│   ├── FormStep2.jsx              # name + address
│   ├── FormStep3.jsx              # phone + terms, submits to the API
│   └── common/ToastNotification.jsx
├── pages/
│   ├── Root.jsx                   # layout and outlet
│   └── Posts.jsx                  # fetched posts feed
├── mocks/
│   ├── handlers.js                # MSW request handlers
│   └── browser.js                 # MSW worker setup
├── utils/validation.js            # pure validation functions, one per field
├── Router.jsx                     # createBrowserRouter config
└── main.jsx                       # starts MSW in dev, then renders the app
```

## How it works

`MultiStepFormWrapper` holds a single `formData` object and the current step number. Each step keeps its own local field state, checks it with the pure functions in `utils/validation.js`, and calls `onSave` to merge its fields into the parent. Step 3 posts the merged payload (without the terms flag) and then navigates to `/posts`.

## Possible improvements

- Turn on MSW in production builds (or add a small serverless endpoint) so a hosted demo works end to end
- Move the validation functions into a schema (e.g. Zod) shared by every step
- Add unit tests for `utils/validation.js` and an interaction test for the step flow
- Migrate to TypeScript
