<div align="center">

# KalingaRx

**A multi-branch pharmacy system for sales, stock with expiry tracking, and prescriptions.**

[![Live site](https://img.shields.io/badge/Live_site-kalingarx.onrender.com-0b7a5a?style=for-the-badge&logo=render&logoColor=white)](https://kalingarx.onrender.com)

![Node.js](https://img.shields.io/badge/Node.js-20+-5FA04E?style=flat-square&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-5-000000?style=flat-square&logo=express&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8-4479A1?style=flat-square&logo=mysql&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-vanilla-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Swagger](https://img.shields.io/badge/Swagger-API_docs-85EA2D?style=flat-square&logo=swagger&logoColor=black)
![Tests](https://img.shields.io/badge/API_tests-58-0b7a5a?style=flat-square)

[Introduction](#introduction) · [Features](#features) · [Installation](#installation) · [Commands](#commands) · [Deployment](#deployment) · [Contributing](#contributing) · [Tech Stack](#tech-stack)

</div>

---

## Introduction

Pharmacies need accurate records of medicine stock, expiry dates, sales, and prescriptions. Keeping these by hand is slow, and mistakes are easy to make. KalingaRx puts them in one place for every branch. It records sales through a point of sale (POS), deducts stock automatically, tracks batches and expiry dates, and handles prescription review and dispensing.

|                |                                                                                             |
| -------------- | ------------------------------------------------------------------------------------------- |
| **School**     | FAITH Colleges · College of Computing and Information Technology                            |
| **Course**     | ITELEC1: Integrative Programming and Techniques · Full-Stack Information System Development |
| **Members**    | Dimapilis, James Harriet M. · Gutierrez, Ahron E. · Lat, Gio Angelo L. · Palmero, Jerome L. |
| **Instructor** | Mr. Bryan Christian Briones                                                                 |

Three roles, each with only what they need:

| Role | What they do |
| --- | --- |
| **Manager** | Runs a branch: medicines and prices, deliveries, sending stock to another branch, voiding sales, reports. A manager with **no branch** is the **owner**: they see every branch and also manage staff accounts and branches. |
| **Pharmacist** | Sells, checks and verifies prescriptions, receives deliveries, and removes expired or damaged stock. Their PRC license is saved on their account. |
| **Cashier** | Sells, gives out prescriptions a pharmacist has already checked, and adds customers. Can record a prescription, but a pharmacist must verify it. |

## Features

- **Selling with receipts**: Search or scan, pay by cash, GCash or card, and print the receipt. Senior Citizen and PWD discounts (20% off, VAT-exempt). Medicines marked Rx need a checked prescription.
- **Real-time stock**: Stock goes down with every sale.
- **Expiry tracking**: Every delivery is saved with its lot number and expiry date, with warnings for expired, expiring and low stock.
- **Prescriptions**: Record what the paper says, a pharmacist checks it, then give it out at Sell. Every prescription has a printable record (patient, doctor and PRC no., medicines, the verifying pharmacist and their PRC no., and what was given out).
- **Close the day**: Count the cash drawer (bills and coins) against the day's cash sales, see Balanced, Short or Over, and print a closing slip to sign.
- **Reports**: Sales summary, best-selling medicines, and stock and expiry. Each can be printed or downloaded for Excel.
- **Notifications**: A bell on every page, checked every minute, shows what each role needs to act on: prescriptions to check, checked or rejected prescriptions, voided sales, expiring PRC licenses, and stock problems.

The menu has 8 pages (cashiers see 6):

| Page | What it is for |
| --- | --- |
| **Home** | Big buttons for common jobs, today's sales, and one **Needs attention** list: expired first, then expiring soon, then running low. |
| **Sell** | The point of sale. |
| **Sales** | Past sales. Reprint a receipt; managers can void a sale and the stock goes back on the shelf. **Close the day** lives here. |
| **Prescriptions** | Record, verify, and print prescription records. A pharmacist can **Save and verify** in one step. |
| **Medicines & Stock** | One list with filter chips (Running low, Expiring soon, Expired, Out of stock). Open a medicine to see its stock by expiry date, receive a delivery, remove stock, send stock to another branch, and see its history. |
| **Customers** | Name, phone, and Senior Citizen or PWD ID, plus their purchases and prescriptions. |
| **Reports** | Sales, best sellers, and stock and expiry. |
| **Settings** | Staff accounts, and Branches for the owner. |

Behind the scenes, the safety rules:

- Sell always takes the stock that expires first, and expired stock can never be sold. An expired lot can't be re-dated, and a lot that has sold units can only have its expiry moved earlier.
- Prescriptions are valid for 180 days from the date written.
- A failed sale saves nothing.
- Every stock change is recorded with who did it and why, and voided sales keep who voided them and the reason.

New staff can start with the [User guide](docs/USER_GUIDE.md).

---

## Project Structure

```
Pharmacy-Management-System/
├── .github/workflows/ci.yml    # CI: lint, API tests against MySQL, PR title check, automatic releases
├── database/
│   ├── pharmacy_pms.sql        # Complete database in one file (import in phpMyAdmin)
│   ├── schema.sql              # The 10 tables, primary/foreign keys, constraints
│   └── seed.sql                # Sample data (branches, users, medicines, sales)
├── docs/
│   ├── API.md                  # Every endpoint, request body, and status code
│   ├── ERD.md                  # Database design and entity-relationship diagram
│   ├── TESTING.md              # Test results and demo checklist
│   ├── DESIGN.md               # UI design decisions and why the system was simplified
│   ├── USER_GUIDE.md           # Step-by-step guide for pharmacists, cashiers and managers
│   └── pharmacy.postman_collection.json
├── public/                     # Frontend (served by Express)
│   ├── index.html              # Main app page
│   ├── login.html              # Sign-in page
│   ├── css/                    # Theme colors, base styles, components, layout
│   ├── icons/                  # Favicon and icon sprite
│   └── js/
│       ├── app.js              # Starts the app, sidebar, navigation by role
│       ├── api.js              # Fetch wrapper for calling the REST API
│       ├── router.js           # Page switching (#/home, #/pos, #/stock, ...)
│       ├── ui/                 # Reusable parts: dialogs, tables, forms, toasts
│       └── views/              # One file per page: dashboard (Home), pos (Sell), stock, prescriptions, settings, ...
├── scripts/
│   ├── db-setup.js             # Creates the database (npm run db:setup)
│   ├── generate-seed.js        # Builds seed.sql
│   ├── build-sql.js            # Builds pharmacy_pms.sql from schema + seed
│   └── generate-postman.js     # Builds the Postman collection
├── src/                        # Backend (Node.js + Express)
│   ├── server.js               # Starts the server
│   ├── app.js                  # Express setup: security, sessions, routes
│   ├── config/                 # .env settings, MySQL connection, VAT/discount rates
│   ├── routes/                 # API endpoints and who can use them
│   ├── middleware/             # Login check, role check, validation, error handling
│   ├── validators/             # Input rules for every form
│   ├── controllers/            # Handle each request and send the response
│   ├── models/                 # SQL queries
│   ├── services/               # Checkout, stock (earliest expiry first), sending stock, reports
│   ├── docs/openapi.js         # OpenAPI spec shown by Swagger UI at /api-docs
│   └── utils/                  # Shared helpers (responses, pricing, receipt numbers)
├── tests/                      # Automated API tests (npm test)
├── .env.example                # Copy to .env and set your MySQL details
├── package.json
└── README.md
```

A request flows like this:

```
Browser (fetch) → routes → middleware → controller → model / service → MySQL
```

## Installation

### Requirements

- [Node.js](https://nodejs.org) 20 or newer
- [XAMPP](https://www.apachefriends.org) (or any MySQL 8 / MariaDB 10.4+ server)

### Steps

1. **Start MySQL**

   Open the XAMPP control panel and start **MySQL**. Also start **Apache** if you want to use phpMyAdmin.

2. **Clone the repository and install packages**

   ```bash
   git clone https://github.com/luxmoncoeur/Pharmacy-Management-System.git
   cd Pharmacy-Management-System
   npm install
   ```

3. **Create your `.env` file**

   ```bash
   cp .env.example .env
   ```

   On Windows Command Prompt, use `copy .env.example .env` instead.

   | Setting | Purpose | Default |
   | --- | --- | --- |
   | `DB_HOST`, `DB_PORT` | Where MySQL runs | `localhost`, `3306` |
   | `DB_USER`, `DB_PASSWORD` | MySQL login | `root`, empty (XAMPP default) |
   | `DB_NAME` | Database name | `pharmacy_pms` |
   | `SESSION_SECRET` | Secures login sessions; required in production | none |
   | `PORT` | Web server port | `3000` |
   | `DB_SSL` | Set to `true` for cloud MySQL that requires SSL | off |
   | `TRUST_PROXY` | Set to `1` when running behind a host's proxy (for example Render) | off |

4. **Create the database with sample data**

   Choose one:

   - **Terminal:** `npm run db:setup`
   - **phpMyAdmin:** open <http://localhost/phpmyadmin>, click the **Import** tab (with no database selected), choose `database/pharmacy_pms.sql`, and click **Import**.

   Either way you get the `pharmacy_pms` database with all tables and sample data. Doing it again resets everything to fresh sample data.

5. **Start the system**

   ```bash
   npm start
   ```

   Open <http://localhost:3000> and sign in.

### Demo accounts

All accounts use the password **`Pharmacy123!`**. The sample data is fictional.

| Username           | Role                       | Branch       |
| ------------------ | -------------------------- | ------------ |
| `manager`          | Owner (manager, no branch) | All branches |
| `manager.main`     | Manager                    | Main Branch  |
| `pharmacist.main`  | Pharmacist                 | Main Branch  |
| `cashier.main`     | Cashier                    | Main Branch  |
| `manager.north`    | Manager                    | North Branch |
| `pharmacist.north` | Pharmacist                 | North Branch |
| `cashier.north`    | Cashier                    | North Branch |

### Common problems

| Problem                             | Fix                                                                                                          |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `Cannot connect to MySQL`           | Make sure MySQL is running in XAMPP and `.env` has the right user and password, then run `npm run db:setup`. |
| Port 3000 is already in use         | Set `PORT=3001` in `.env`.                                                                                   |
| Signed out after `npm run db:setup` | Normal: the reset clears login sessions. Sign in again.                                                      |

## Commands

| Command | Description |
| --- | --- |
| `npm install` | Install all dependencies |
| `npm run db:setup` | Create or reset the database with sample data |
| `npm start` | Start the server at `localhost:3000` |
| `npm run dev` | Start the server and restart it when files change |
| `npm test` | Run the 58 automated API tests on a separate test database |
| `npm run lint` | Check the code with ESLint |

## Deployment

The live site runs on [Render](https://render.com) (free web service) with the database on [TiDB Cloud](https://tidbcloud.com) (free, MySQL-compatible). Every push to `main` redeploys automatically.

| Render setting | Value |
| --- | --- |
| Build command | `npm ci --ignore-scripts` (add `&& npm run db:setup` once to load the database; remove it after, or every deploy resets the data) |
| Start command | `npm start` |
| Environment | `NODE_ENV=production`, `TRUST_PROXY=1`, `DB_SSL=true`, `SESSION_SECRET`, `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`, `DB_NAME=pharmacy_pms` |

The free instance sleeps after 15 minutes without visitors, so the first visit after that takes about a minute to load.

## Documentation

- [API reference](docs/API.md): every endpoint, request body, and status code
- **Swagger UI:** with the server running, open <http://localhost:3000/api-docs> to browse and try every endpoint
- [Database design and ERD](docs/ERD.md): 10 tables, and what was simplified
- [Testing and results](docs/TESTING.md)
- [Design notes, including why and how the system was simplified](docs/DESIGN.md)
- [User guide for pharmacy staff](docs/USER_GUIDE.md)
- [Postman collection](docs/pharmacy.postman_collection.json)
- [Commit message cheat sheet](docs/conventional-commits.md)

## Contributing

Create a branch from `main` and open a pull request; don't push directly to `main`.

```bash
git checkout main && git pull
git checkout -b feat/short-description
git add [file or folder]
git commit -m "feat: describe your change"
git push -u origin feat/short-description
```

**Commit messages decide the version** ([cheat sheet](docs/conventional-commits.md)): `feat: …` is a minor release, `fix: …` a patch, `feat!: …` or a `BREAKING CHANGE:` footer a major one, and `docs`, `chore`, `test`, `ci` or `refactor` release nothing. `npm install` sets up a git hook that checks each commit message.

### CI/CD

GitHub Actions runs [.github/workflows/ci.yml](.github/workflows/ci.yml) on every pull request and every push to `main`, using the reusable workflows from [ci-cd-template](https://github.com/luxmoncoeur/ci-cd-template) (pinned to v1.1.0).

| Job | What it does |
| --- | --- |
| `checks` | `npm run lint` (ESLint), from the template |
| `test` | Starts a MySQL 8.4 container, builds the test database from `schema.sql` and `seed.sql`, and runs `npm test` |
| `release-preview` | Pull requests only: checks that the PR title is a Conventional Commit and comments which version merging it would release |
| `ci` | The single required check: passes only when the jobs above passed |
| `release` | On `main` after `ci` passes: semantic-release bumps the version in `package.json`, updates `CHANGELOG.md`, tags `vX.Y.Z`, and creates a GitHub Release |

**One-time GitHub setup:** in Settings → General → Pull Requests, allow only squash merging and set the default commit message to "Pull request title and description". Then add a branch ruleset for `main` that requires a pull request and the `ci` status check. If `main` is protected this way, the release job needs a deploy key to push its release commit; see [Protecting main](https://github.com/luxmoncoeur/ci-cd-template#protecting-main) in the template README.

---

## Tech Stack

| Area | Tools |
| --- | --- |
| Frontend | HTML, CSS, vanilla JavaScript (Fetch API, async/await) |
| Backend | [Node.js](https://nodejs.org), [Express 5](https://expressjs.com) |
| Database | MySQL (XAMPP / MariaDB locally, [TiDB Cloud](https://tidbcloud.com) live) via `mysql2` |
| API | REST with JSON; [Swagger UI](https://swagger.io/tools/swagger-ui/) at `/api-docs` from an OpenAPI 3 spec |
| Security | Session login, `bcryptjs` password hashing, `helmet`, rate limiting, parameterized queries |
| Validation | [Zod](https://zod.dev) on the server, plus browser-side checks |
| Testing | Node test runner, `supertest`, Postman |
| Hosting | [Render](https://render.com) |
| CI/CD | GitHub Actions, ESLint, Conventional Commits, semantic-release |

## Limitations

- No online ordering or delivery, no insurance or hospital integration, and no automatic ordering from suppliers.
- The system supports, but does not replace, the professional judgment of a licensed pharmacist.
- Receipts are for demonstration and are not BIR-accredited official receipts.

---

<div align="center">

**KalingaRx**: pharmacy sales, stock and prescriptions, in one place.

Built as a final project for ITELEC1 at FAITH Colleges.

</div>
