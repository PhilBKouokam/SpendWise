# SpendWise

A full-stack personal finance tracker for managing authenticated income and expense records, visualizing spending patterns, and securely uploading receipt images.

## 🎥 2-Minute Walkthrough

https://www.loom.com/share/75bc2eae927b4d0d9c22ff35297a09c1

## 🌐 Live Demo

https://spendwise-two-navy.vercel.app

## 💻 Source Code

https://github.com/PhilBKouokam/spendwise

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

## Why I Built This

I built SpendWise to practice the engineering patterns behind production-style CRUD applications: account-based authentication, protected user data, client-server request flows, persistent database records, and cloud-backed file uploads.

From a product perspective, the goal was to make personal finance tracking more useful than a static spreadsheet. Users can register, log transactions, attach receipt images, filter their history, and immediately see how income and expenses affect their balance and spending charts.

The project gave me practical experience connecting a React frontend to an Express API, securing routes with JWT authentication, modeling user-owned data in MongoDB, and integrating AWS S3 for receipt storage.

## Features

- Register and log in with JWT-based authentication.
- Create income and expense transactions tied to the authenticated user.
- View dashboard balance cards for total balance, income, and expenses.
- Visualize spending by category and balance trends with Recharts.
- Filter transactions by type, category, and description.
- Edit and delete existing transactions.
- Upload optional receipt images for transaction records.
- Store receipt images in AWS S3 and save receipt URLs in MongoDB.
- Protect dashboard and transaction pages from unauthenticated access.
- Toggle between light and dark mode.

## Engineering Highlights

- JWT authentication for stateless user sessions.
- bcrypt password hashing before user records are stored.
- Protected React Router routes for authenticated pages.
- React Context state management for authentication and transactions.
- REST API architecture with separate route and controller layers.
- MongoDB and Mongoose for structured user and transaction persistence.
- User-scoped database queries to protect private financial records.
- AWS S3 receipt uploads using the AWS SDK.
- Multer multipart uploads with in-memory file handling.
- Recharts data visualization for spending and balance trends.
- Client-server architecture with separate frontend and backend deployments.
- Responsive Bootstrap UI with dark mode support.

## 🏗 Architecture

SpendWise follows a client-server architecture where the React frontend owns the user experience and the backend owns authentication, authorization, persistence, and receipt storage.

`React UI` → `React Context` → `REST API` → `Express Controllers` → `MongoDB with Mongoose` → `API Response` → `React UI Update`

For receipt uploads:

`React FormData Upload` → `Multer Middleware` → `AWS S3` → `Receipt URL Saved in MongoDB` → `React UI Update`

## Key User Flows

`Register` → `Login` → `Dashboard` → `Create Transaction` → `Upload Receipt` → `Edit Transaction` → `Delete Transaction` → `Charts Update Automatically`

## Tech Stack

**Frontend:** React, React Router, React Context, JavaScript, Bootstrap, Recharts, Vite

**Backend:** Node.js, Express, JavaScript

**Database:** MongoDB, Mongoose

**Authentication:** JWT, bcrypt

**Cloud:** AWS S3, Multer

**Deployment:** Vercel frontend, Render backend

**Developer Tools:** npm, ESLint, Git, GitHub

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
