# Adding Passkey Login to Vue and express app with Hanko

This repo shows how you can add passkey login with Hanko Passkey API to your Vue-express app.

## Clone the repo

```bash
git clone https://github.com/teamhanko/passkeys-vue-express.git
```

## Add the environment variables

Add the following environment variables to `.env` file in the express-backend directory.

```sh
PASSKEYS_API_KEY=your-hanko-passkey-api-key
PASSKEYS_TENANT_ID=your-hanko-passkey-tenant-id
```

## Install Dependencies

To run up your project, you need to install dependencies for both the frontend and the backend. This project uses `pnpm` as the package manager.

### Frontend

1. Navigate to the frontend directory:

```bash
cd vue-frontend
```

1. Install the frontend dependencies:

```bash
pnpm install
```

### Backend

1. Navigate to the backend directory:

```bash
cd express-backend
```

2. Install the backend dependencies:

```bash
pnpm install
```
