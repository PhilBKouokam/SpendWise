# SpendWise

A full-stack personal finance application that helps users understand where their money goes through thoughtful transaction tracking, receipt management, and financial visualization.

[Live Demo](https://spendwise-two-navy.vercel.app) · [GitHub](https://github.com/PhilBKouokam/spendwise) · [Architecture Walkthrough](https://www.loom.com/share/75bc2eae927b4d0d9c22ff35297a09c1)

![React](https://img.shields.io/badge/React-20232A?style=flat&logo=react&logoColor=61DAFB)
![Express](https://img.shields.io/badge/Express-000000?style=flat&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat&logo=mongodb&logoColor=white)
![AWS S3](https://img.shields.io/badge/AWS_S3-569A31?style=flat&logo=amazons3&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat&logo=vercel&logoColor=white)

## 🎥 Architecture Walkthrough

https://www.loom.com/share/75bc2eae927b4d0d9c22ff35297a09c1

## 🌐 Live Demo

https://spendwise-two-navy.vercel.app

## 💻 Source Code

https://github.com/PhilBKouokam/spendwise

## The Problem

Understanding personal spending is often harder than recording it. Transactions become scattered across accounts and receipts, categories lose context, and a monthly total does not always explain which habits are shaping the bigger picture. SpendWise brings that information together so users can see what they spent, where it went, and how those decisions affect their financial position.

## Why I Built It

I built SpendWise to explore how thoughtful software can help people make better financial decisions. The product turns everyday financial activity into a clearer, more useful view of income, expenses, and spending patterns while also serving as an end-to-end full-stack engineering project.

Building it required decisions across the complete system: shaping an understandable interface, protecting user-owned data, defining reliable client-server boundaries, modeling financial records, and managing receipt files outside the primary database.

## Product Philosophy

Personal finance software should reduce uncertainty rather than add more information to sort through. SpendWise is designed around clarity, visibility, and trust: important totals are easy to find, transactions remain connected to their supporting details, and visual summaries make patterns easier to understand. Each part of the experience aims to help users feel informed without making financial tracking feel burdensome.

## Product Overview

SpendWise gives each user a private account for managing their day-to-day financial activity. Users can record income and expenses, organize and filter transactions, attach receipts, set their financial information in context through dashboard totals, and review visual breakdowns of spending and balance trends. The experience works across screen sizes and includes light and dark themes for comfortable ongoing use.

## 📸 Screenshots

<table>
  <tr>
    <td align="center" width="50%">
      <strong>Dashboard</strong><br />
      <img src="screenshots/SpendWise_Light_Dashboard.png" alt="SpendWise dashboard with balance cards and charts" width="420">
    </td>
    <td align="center" width="50%">
      <strong>Dark Mode Dashboard</strong><br />
      <img src="screenshots/SpendWise_Dark_Dashboard.png" alt="SpendWise dark mode dashboard" width="420">
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <strong>Transactions</strong><br />
      <img src="screenshots/SpendWise_Transactions_Page.png" alt="SpendWise transactions list with filters" width="420">
    </td>
    <td align="center" width="50%">
      <strong>Add Transaction and Receipt Upload</strong><br />
      <img src="screenshots/SpendWise_Add_Transaction_Form.png" alt="SpendWise add transaction form with receipt upload" width="420">
    </td>
  </tr>
  <tr>
    <td align="center" colspan="2">
      <strong>Edit Transaction</strong><br />
      <img src="screenshots/SpendWise_Edit_Transactio_Form.png" alt="SpendWise edit transaction form" width="420">
    </td>
  </tr>
</table>

## Key Features

### Personal Dashboard

- See total balance, income, and expenses in one place.
- Review recent financial activity without searching through individual records.

### Transaction Management

- Create, edit, and delete income and expense transactions.
- Filter transaction history by type, category, or description.

### Receipt Management

- Attach optional receipt images to transaction records.
- Keep supporting documentation connected to the expense it belongs to.

### Spending Insights

- Understand spending by category through visual breakdowns.
- Follow balance trends as transactions change over time.

### Secure Accounts

- Register and sign in to a personal account.
- Keep financial records scoped to the authenticated user.

### Responsive Experience

- Use the application across desktop and smaller screens.
- Switch between light and dark modes based on preference.

## 🏗 Architecture

SpendWise uses a layered client-server architecture that keeps presentation, application behavior, persistence, and file storage separate. This makes each part of the system easier to reason about and allows the interface, API, and infrastructure to evolve without taking on responsibilities that belong elsewhere.

`Frontend` → `REST API` → `Business Logic` → `Database` → `Cloud Storage`

- **Frontend:** Presents account, transaction, receipt, and visualization workflows while coordinating client-side state.
- **REST API:** Defines the boundary between the user interface and server-side capabilities through predictable resource-oriented requests.
- **Business Logic:** Handles authentication, authorization, validation, user-scoped transaction operations, and receipt upload coordination.
- **Database:** Persists users and financial records, including the relationship between each transaction and its owner.
- **Cloud Storage:** Stores receipt image files independently while the database retains the URL associated with each transaction.

The separation of concerns is especially important for sensitive, user-owned data. The frontend can guide the experience, but the backend remains responsible for enforcing access boundaries and deciding which records a request may read or change.

### Request Flow

`React UI` → `React Context` → `REST API` → `Express Controllers` → `MongoDB with Mongoose` → `API Response` → `React UI Update`

### Receipt Upload Flow

`React FormData Upload` → `Multer Middleware` → `AWS S3` → `Receipt URL Saved in MongoDB` → `React UI Update`

### Key User Flow

`Register` → `Login` → `Dashboard` → `Create Transaction` → `Upload Receipt` → `Edit Transaction` → `Delete Transaction` → `Charts Update Automatically`

## Engineering Decisions

### React Context for Shared Application State

Authentication and transaction data need to be available across related screens, but the application's state requirements do not justify the added conventions of a larger state-management library. React Context keeps shared state centralized with a smaller dependency and conceptual footprint. The tradeoff is that context must remain focused to avoid broad, unnecessary re-renders as the application grows.

### Express for a Focused API Layer

Express provides a small, flexible foundation for defining REST endpoints and composing authentication, upload, and error-handling middleware. Its minimal structure makes route-to-controller boundaries explicit, though it also means the project must establish and maintain those conventions rather than inheriting them from a more opinionated framework.

### MongoDB for Transaction-Oriented Records

SpendWise centers on user-owned transaction documents whose fields and receipt metadata map naturally to a document model. MongoDB supports straightforward iteration on that model, while Mongoose adds schemas and validation where consistency matters. This favors flexibility and developer clarity over the relational constraints that would be more valuable in a system with complex accounting relationships.

### JWT for Stateless Authentication

JWTs allow the API to authenticate requests without maintaining server-side session state, which fits a separately deployed frontend and backend. The tradeoff is that token handling and expiration require care, and access control must still be enforced on every protected server request.

### AWS S3 for Receipt Storage

Receipt images have different storage and delivery needs from transaction data. S3 keeps binary files out of the database and provides durable object storage, while MongoDB stores only the receipt URL associated with a record. This separation adds an external service and upload lifecycle to manage, but gives each storage system a responsibility suited to it.

### Recharts for Financial Visualization

Charts should make spending patterns easier to interpret without requiring a custom visualization system. Recharts provides responsive, composable chart primitives that work with the application's existing component model. It accelerates clear data presentation while still requiring deliberate choices about aggregation, labeling, and which comparisons are genuinely useful.

## Tech Stack

**Frontend:** React, React Router, React Context, JavaScript, Bootstrap, Recharts, Vite

**Backend:** Node.js, Express, JavaScript

**Database:** MongoDB, Mongoose

**Authentication:** JWT, bcrypt

**Cloud:** AWS S3, Multer

**Deployment:** Vercel frontend, Render backend

**Developer Tools:** npm, ESLint, Git, GitHub

## Development Workflow

AI-assisted development helped accelerate investigation, implementation, debugging, and documentation throughout the project. It supported faster exploration and iteration, while architecture decisions, code validation, testing, and final verification remained under human review. The workflow treated AI as an engineering aid rather than a substitute for judgment or ownership.

## API Overview

Deployed API: https://spendwise-backend-ple6.onrender.com

| Method | Route | Description | JWT Required |
| --- | --- | --- | --- |
| POST | `/api/auth/register` | Create a user, hash the password, and return a JWT. | No |
| POST | `/api/auth/login` | Validate credentials and return a JWT. | No |
| GET | `/api/trans` | Fetch transactions for the authenticated user. | Yes |
| POST | `/api/trans` | Create a transaction for the authenticated user. | Yes |
| GET | `/api/trans/:id` | Fetch one transaction owned by the authenticated user. | Yes |
| PATCH | `/api/trans/:id` | Update a transaction owned by the authenticated user. | Yes |
| DELETE | `/api/trans/:id` | Delete a transaction owned by the authenticated user. | Yes |
| POST | `/api/upload/receipt/:id` | Upload a receipt image to S3 and save the receipt URL on the transaction. | Yes |

## Local Development

### Prerequisites

- Node.js
- npm
- MongoDB connection string
- AWS S3 bucket and credentials for receipt uploads

### Backend

```bash
cd backend
npm install
npm run dev
```

Create `backend/.env` using `backend/.env.example`:

```bash
PORT=4600
MONGO_URI=mongodb+srv://<username>:<password>@<cluster-url>/<database-name>
JWT_SECRET=replace-with-a-long-random-development-secret
AWS_ACCESS_KEY_ID=replace-with-your-aws-access-key-id
AWS_SECRET_ACCESS_KEY=replace-with-your-aws-secret-access-key
AWS_REGION=us-east-1
AWS_BUCKET_NAME=replace-with-your-s3-bucket-name
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

Create `frontend/.env` using `frontend/.env.example`:

```bash
VITE_API_BASE_URL=http://localhost:4600
```

Open the frontend development server in your browser, register an account, and start tracking transactions.
