# DonorPulse

> A responsive, accessible health dashboard for tracking voluntary blood donation history, haemoglobin trends, and donor analytics — built with React, Vite, and Recharts.

![React](https://img.shields.io/badge/React-18-149eca?logo=react&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-8-646cff?logo=vite&logoColor=white)
![Recharts](https://img.shields.io/badge/Recharts-2-0088cc?logo=recharts&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6%2B-f7df1e?logo=javascript&logoColor=black)
![Node.js](https://img.shields.io/badge/Node.js-20.19%2B-339933?logo=node.js&logoColor=white)
![ESLint](https://img.shields.io/badge/Lint-ESLint-4b32c3?logo=eslint&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-0f766e)

### 🌐 [Live Demo](https://saravanansaranraj27.github.io/donor-pulse)

DonorPulse helps voluntary donors monitor their health metrics over time. It turns raw donation records into insights: haemoglobin stability analysis, donation frequency patterns, and a predicted date for the next donation.

The app is a frontend-only static build with no backend. It supports dark and light themes, custom SVG animations, and charts built with Recharts, and it deploys to GitHub Pages.

## Contents

- [Dashboard content](#dashboard-content)
- [Features](#features)
- [Quick start](#quick-start)
- [Using the app](#using-the-app)
- [Technical highlights](#technical-highlights)
- [Project structure](#project-structure)
- [Technology](#technology)
- [Development commands](#development-commands)
- [Deployment](#deployment)
- [Linting](#linting)
- [Troubleshooting](#troubleshooting)
- [Complete command reference](#complete-command-reference)
- [Limitations](#limitations)
- [Contributing](#contributing)
- [License](#license)

## Dashboard content

The dashboard is a single page with a hero and **5 navigable sections**, all rendered from the records in `src/data/donor.js`.

### Sections — 5 anchors

| Section   | Anchor       | Content                                                                            |
| --------- | ------------ | ---------------------------------------------------------------------------------- |
| Summary   | `#summary`   | Total donations, average Hb, highest and lowest readings                           |
| Trend     | `#trend`     | Interactive area chart of haemoglobin across donations                             |
| Analytics | `#analytics` | Consistency status, average donation gap, next donation estimate, yearly bar chart |
| Rhythm    | `#rhythm`    | Interactive donation timeline with keyboard-accessible markers                     |
| Record    | `#record`    | Full table of every donation: S.No, date, haemoglobin, place, and status           |

### Consistency statuses

The consistency check reads the last three haemoglobin readings and returns one of these:

| Status           | Condition                                                    |
| ---------------- | ------------------------------------------------------------ |
| Building Data    | Fewer than 3 records                                         |
| Attention Needed | Any recent reading below the 12.5 g/dl eligibility threshold |
| Variable         | Coefficient of variation above 5% across recent readings     |
| Declining Trend  | Three consecutive drops                                      |
| Stable           | None of the above                                            |

### Analytics utilities

| Function                    | Purpose                                                          |
| --------------------------- | ---------------------------------------------------------------- |
| `sortRecordsByDate`         | Sorts records chronologically                                    |
| `calculateDonationStats`    | Computes the summary statistics                                  |
| `calculateYearlyStats`      | Groups donations by year for the bar chart                       |
| `calculateAverageGap`       | Computes the average gap between donations                       |
| `calculateNextDonationDate` | Adds the average gap (90 days by default) to the latest donation |
| `calculateConsistency`      | Returns the consistency status                                   |
| `getRecordTier`             | Classifies a reading as low, mid, or high                        |

## Features

| Area               | Capabilities                                                                                      |
| ------------------ | ------------------------------------------------------------------------------------------------- |
| Health dashboard   | Summarizes total donations, average Hb, highest and lowest readings, and consistency status       |
| Data visualization | Shows an area chart for Hb trends and a bar chart for annual donation frequency, using Recharts   |
| Donor analytics    | Calculates the average donation gap and estimates the next donation date                          |
| Section navigation | Provides a sticky top bar with scroll-spy highlighting, smooth scrolling, and a mobile menu       |
| Theming            | Switches dark and light mode with CSS variable transitions and glassmorphism effects              |
| Accessibility      | Supports keyboard navigation, ARIA labels on interactive elements, and reduced-motion preferences |
| Responsive design  | Uses a mobile-first layout for all device sizes                                                   |
| Loading state      | Shows a skeleton loader for 800ms on first paint                                                  |
| Back to top        | Shows a floating button after scrolling past 300px                                                |
| Custom icons       | Hand-rolled SVG icons in `Icons.jsx`, with the rest re-exported from `lucide-react`               |
| GitHub Pages ready | Builds with a `/donor-pulse/` base path and a `deploy` script                                     |
| ESLint             | Lints with a flat config, React Hooks rules, and React Refresh rules                              |

## Quick start

### Requirements

- Node.js 20.19 or newer (or 22.12 or newer), as required by Vite 8
- npm
- Visual Studio Code is recommended

### Clone the repository

```bash
git clone https://github.com/saravanansaranraj27/donor-pulse.git
cd donor-pulse
```

### Install dependencies

```bash
npm install
```

`gh-pages` is already listed in `devDependencies`, so no separate install is needed for deployment.

### Verify Node.js

```bash
node --version
```

### Verify the installation

```bash
npm run lint
npm run build
```

## Using the app

The development server serves the app under the `/donor-pulse/` base path.

### Start the development server

```powershell
npm run dev
```

The app runs at:

```text
http://localhost:5173/donor-pulse/
```

### Workflow

1. **Dashboard:** View the hero section for a snapshot of total donations and current health status
2. **Navigation:** Use the sticky top bar, or the mobile menu on small screens, to jump between sections. The active link highlights as you scroll
3. **Trend analysis:** Hover over the haemoglobin trend chart to see the reading for each donation
4. **Analytics:** Check the Donor Analytics section for donation consistency, the average gap, and the estimated next donation date
5. **Rhythm strip:** View your donation timeline in the Donation Rhythm section, and focus or hover over markers for quick stats
6. **Full record:** Scroll to the bottom for a table of every recorded visit
7. **Theme toggle:** Use the button in the top bar to switch between light and dark mode
8. **Back to top:** After scrolling down, use the floating button to jump back to the top

### Preview the production build

```powershell
npm run build
npm run preview
```

## Technical highlights

- **Modular architecture:** Code is split into `pages`, `layouts`, `components`, `hooks`, `data`, and `utils`. The `Dashboard` page is a thin orchestrator that composes focused `components/dashboard/*` pieces through two hooks
- **Custom hooks:** `useDashboardData` derives sorted records, stats, yearly stats, the average gap, the next donation estimate, and consistency data in one place. `useScrollNavigation` owns scroll-spy, smooth scrolling, and the back-to-top button. `useTheme` holds the theme state and syncs it to `<html data-theme>`
- **Performance:** Heavy components such as `HbChart` and `YearlyChart` are wrapped in `React.memo` to avoid unnecessary re-renders during theme switches
- **Custom data logic:** `utils/date.js` provides `parseDate`, and `utils/donationAnalytics.js` builds on it for sorting, averages, yearly stats, gaps, predictions, and consistency
- **Custom icon set:** `Icons.jsx` hand-rolls a few SVG icons and re-exports the rest from `lucide-react`
- **Advanced CSS:** Theme tokens, base styles, and animations are split across `variables.css`, `themes.css`, and `globals.css`, using CSS custom properties, `backdrop-filter`, and keyframe animations
- **Accessibility first:** The Rhythm Strip markers use `role="button"`, `tabIndex`, and descriptive `aria-label` values so they work with the keyboard and screen readers

## Project structure

```text
donor-pulse/
├── src/
│   ├── assets/
│   │   ├── hero.png                         # Image asset
│   │   ├── react.svg                        # Default Vite template asset
│   │   └── vite.svg                         # Default Vite template asset
│   ├── components/
│   │   ├── charts/
│   │   │   ├── HbChart.jsx                  # Haemoglobin trend area chart
│   │   │   └── YearlyChart.jsx              # Annual donation frequency bar chart
│   │   ├── common/
│   │   │   └── Loader.jsx                   # Skeleton loading state
│   │   └── dashboard/
│   │       ├── DashboardHeader.jsx          # Sticky top bar, theme toggle, mobile menu
│   │       ├── DashboardHero.jsx            # Hero section with pulse SVG and quick stats
│   │       ├── DonationSummary.jsx          # Summary stat cards
│   │       ├── HemoglobinTrend.jsx          # Trend section wrapping HbChart
│   │       ├── DonorAnalytics.jsx           # Consistency, average gap, next donation, yearly chart
│   │       ├── DonationRhythm.jsx           # Interactive donation timeline
│   │       ├── DonationRecordTable.jsx      # Full donation record table
│   │       └── SectionHeader.jsx            # Shared section heading
│   ├── data/
│   │   ├── donor.js                         # Donor profile and donation records
│   │   └── navigation.js                    # Section navigation links
│   ├── hooks/
│   │   ├── useDashboardData.js              # Derives stats, gaps, and consistency data
│   │   ├── useScrollNavigation.js           # Scroll-spy, smooth scroll, back to top
│   │   └── useTheme.js                      # Theme state synced to <html data-theme>
│   ├── layouts/
│   │   └── MainLayout.jsx                   # Top-level layout wrapper
│   ├── pages/
│   │   └── Dashboard/
│   │       └── Dashboard.jsx                # Page orchestrator
│   ├── styles/
│   │   ├── variables.css                    # Design tokens
│   │   ├── themes.css                       # Light and dark theme values
│   │   └── globals.css                      # Global styles, layout, animations
│   ├── utils/
│   │   ├── date.js                          # parseDate utility
│   │   └── donationAnalytics.js             # Stats, gaps, predictions, consistency logic
│   ├── App.jsx                              # Root component, renders MainLayout
│   ├── Icons.jsx                            # Custom SVG icons and lucide-react re-exports
│   └── main.jsx                             # Entry point
│
├── public/
│   ├── favicon.svg                          # Site favicon
│   └── icons.svg                            # Static SVG icons
├── index.html                               # HTML shell, fonts, and early theme script
├── vite.config.js                           # Vite config with the /donor-pulse/ base path
├── eslint.config.js                         # ESLint flat configuration
├── .gitignore                               # Git exclusions for generated files
├── package.json                             # Dependencies and npm scripts
├── package-lock.json                        # Locked npm dependency versions
└── README.md                                # Project documentation
```

## Technology

- [React 18](https://react.dev/) — UI components and hooks
- [Vite 8](https://vite.dev/) — dev server and production build
- [`@vitejs/plugin-react`](https://www.npmjs.com/package/@vitejs/plugin-react) — React support for Vite
- [Recharts 2](https://recharts.org/) — composable charts built on React
- [Lucide React](https://lucide.dev/) — icon set, re-exported through `Icons.jsx`
- [Google Fonts](https://fonts.google.com/) — Inter and Sora
- CSS3 — CSS variables, `backdrop-filter`, and keyframe animations, with no CSS framework
- [ESLint](https://eslint.org/) — flat config with `eslint-plugin-react-hooks` and `eslint-plugin-react-refresh`
- [`gh-pages`](https://www.npmjs.com/package/gh-pages) — static deployment to GitHub Pages

## Development commands

### Application

```powershell
npm run dev
npm run build
npm run preview
```

### Linting

```powershell
npm run lint
```

### Deployment

```powershell
npm run deploy
```

## Deployment

### Deploy to GitHub Pages

Build the project and publish the `dist` folder:

```powershell
npm run build
npm run deploy
```

The `deploy` script runs `gh-pages -d dist`.

The app is served under the `/donor-pulse/` base path configured in `vite.config.js`. If you rename the repository, update `base` to match.

### Manual deployment

Create a production build:

```powershell
npm run build
```

The built files are located in the `dist/` directory and can be deployed to any static hosting service configured for the `/donor-pulse/` base path.

## Linting

The project uses ESLint with a flat configuration covering `.js` and `.jsx` files, with the React Hooks and React Refresh plugins. The `dist` folder is ignored.

```powershell
npm run lint
```

## Troubleshooting

### Node.js version error or build failure

Vite 8 needs Node.js 20.19 or newer (or 22.12 or newer). Check your version:

```powershell
node --version
```

Install a supported version, then run `npm install` again.

### `Cannot find native binding` during build

This happens when `node_modules` was installed on a different operating system. Delete `node_modules` and reinstall:

```powershell
Remove-Item -Recurse -Force node_modules
npm install
```

### The page is blank or returns 404 in development

The app uses the `/donor-pulse/` base path. Open:

```text
http://localhost:5173/donor-pulse/
```

### Port 5173 is already in use

Stop the other process, or start the server on another port:

```powershell
npm run dev -- --port 5174
```

### Assets fail to load on GitHub Pages

Confirm `base` in `vite.config.js` matches the repository name, then rebuild and redeploy:

```powershell
npm run build
npm run deploy
```

### ESLint errors

Run:

```powershell
npm run lint
```

### The theme does not stay after a reload

This is expected. The theme follows the OS `prefers-color-scheme` on load, and the toggle applies to the current session only.

## Complete command reference

```powershell
git clone https://github.com/saravanansaranraj27/donor-pulse.git
npm install
node --version
npm run dev
npm run dev -- --port 5174
npm run build
npm run preview
npm run lint
npm run deploy
```

## Limitations

- This is a frontend-only demonstration. Data is hard-coded in `src/data/donor.js` for presentation purposes.
- No backend database or user authentication is implemented.
- The theme toggle is session-only. It is not saved to `localStorage`, so the app follows the OS `prefers-color-scheme` on every reload.
- No automated tests are included in the project.
- The app is a personal portfolio showcase for UI/UX and React performance skills, and it is not medical advice.

## Contributing

Bug reports, improvements, and pull requests are welcome.

Keep changes focused and verify them before opening a pull request:

```powershell
npm run lint
npm run build
```

## License

This project is licensed under the MIT License.
