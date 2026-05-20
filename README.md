<div align="center">

<br />
<br />

<h1>NightCode</h1>

<p>A terminal-based AI coding agent.</p>

<p>Plan, chat, and build inside your local project with a Bun-powered CLI, Hono API, Prisma ORM, Clerk auth, and AI SDK streaming.</p>

<br />

<p>
  <a href="https://cwa.run/bun?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=badge_bun"><img src="https://img.shields.io/badge/Bun-000000?style=for-the-badge&logo=bun&logoColor=white" alt="Bun" /></a>&nbsp;
  <a href="https://cwa.run/opentui?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=badge_opentui"><img src="https://img.shields.io/badge/OpenTUI-111111?style=for-the-badge" alt="OpenTUI" /></a>&nbsp;
  <a href="https://cwa.run/react?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=badge_react"><img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React" /></a>&nbsp;
  <a href="https://cwa.run/hono?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=badge_hono"><img src="https://img.shields.io/badge/Hono-E36002?style=for-the-badge&logo=hono&logoColor=white" alt="Hono" /></a>&nbsp;
  <a href="https://cwa.run/neon?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=badge_neon"><img src="https://img.shields.io/badge/Neon-00E599?style=for-the-badge&logo=neon&logoColor=black" alt="Neon" /></a>&nbsp;
  <a href="https://cwa.run/clerk?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=badge_clerk"><img src="https://img.shields.io/badge/Clerk-6C47FF?style=for-the-badge&logo=clerk&logoColor=white" alt="Clerk" /></a>&nbsp;
  <a href="https://cwa.run/polar?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=badge_polar"><img src="https://img.shields.io/badge/Polar-000000?style=for-the-badge&logo=polar&logoColor=white" alt="Polar" /></a>&nbsp;
  <a href="https://cwa.run/sentry?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=badge_sentry"><img src="https://img.shields.io/badge/Sentry-362D59?style=for-the-badge&logo=sentry&logoColor=white" alt="Sentry" /></a>&nbsp;
  <a href="https://cwa.run/railway?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=badge_railway"><img src="https://img.shields.io/badge/Railway-0B0D0E?style=for-the-badge&logo=railway&logoColor=white" alt="Railway" /></a>
</p>

</div>

<br />

## Tutorial

Each chapter has a matching branch so you can check out the code at any point in the tutorial:

| Branch | Chapter |
|--------|---------|
| `main` | Final project |
| `01-project-setup-component-architecture` | Project setup and component architecture |
| `02-ui-infrastructure` | Terminal UI infrastructure |
| `03-routing-screen-layout` | Routing and screen layout |
| `04-server-shared-database` | Server, shared package, and database |
| `05-ai-chat-streamiing` | AI chat streaming |
| `06-session-management-config` | Session management and configuration |
| `07-tool-calling` | Tool calling |
| `08-user-experience` | User experience polish |
| `09-billing` | Billing and credit metering |
| `10-client-side-tool-execution` | Client-side tool execution |
| `11-the-end` | Final tutorial state |

```bash
git checkout 07-tool-calling  # example: jump to tool calling
```

## Features

- **Terminal AI Chat** - Run an AI coding assistant directly in your terminal with an OpenTUI and React interface
- **Plan and Build Modes** - Use read-only planning tools or enable write, edit, and shell execution tools for implementation
- **Streaming Responses** - Stream model output through the AI SDK with persisted session history
- **Local Project Tools** - Read files, list directories, glob, grep, write files, edit files, and run shell commands inside the current project
- **Multi-Model Support** - Use supported Anthropic and OpenAI chat models from a shared model registry
- **Persistent Sessions** - Store authenticated user sessions and messages in Postgres via Prisma
- **Clerk OAuth** - Authenticate the CLI through a browser-based Clerk OAuth flow
- **Usage Billing** - Meter AI usage as credits through Polar before allowing session and chat actions

## Getting Started

### Prerequisites

- [Bun](https://cwa.run/bun?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=prerequisites_bun) installed
- PostgreSQL database, such as [Neon](https://cwa.run/neon?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=prerequisites_neon)
- [Clerk](https://cwa.run/clerk?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=prerequisites_clerk) application configured for OAuth
- Anthropic and/or OpenAI API key
- [Polar](https://cwa.run/polar?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=prerequisites_polar) account and credits meter

### 1. Clone and install

```bash
git clone git@github.com:code-with-antonio/nightcode.git
cd nightcode
bun install
```

### 2. Configure environment

```bash
cp .env.example .env
```

Fill in the required values:

```bash
API_URL=http://localhost:3000
DATABASE_URL=

ANTHROPIC_API_KEY=
OPENAI_API_KEY=

CLERK_FRONTEND_API=
CLERK_OAUTH_CLIENT_SECRET=
CLERK_OAUTH_CLIENT_ID=
CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=
JWT_SECRET=jwt-secret

POLAR_ACCESS_TOKEN=
POLAR_PRODUCT_ID=
POLAR_SERVER=sandbox
POLAR_CREDITS_METER_ID=
```

### 3. Set up Clerk OAuth

NightCode authenticates the CLI through a browser-based Clerk OAuth flow. The CLI opens Clerk authorization in the browser, Clerk redirects to the server at `/auth/callback`, and the server forwards the authorization code back to the local CLI callback server.

In your [Clerk](https://cwa.run/clerk?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=setup_clerk) dashboard:

1. Go to **Configure > Developers > OAuth applications**.
2. Click **Add OAuth application**.
3. Name it anything, for example `NightCode`.
4. Select these four scopes: `openid`, `email`, `profile`, and `offline_access`.
5. Turn on **Public**. This is required for the Authorization Code with PKCE flow used by the CLI.
6. Turn on **Consent screen** so users can approve the requested scopes.
7. Add `http://localhost:3000/auth/callback` as a redirect URI for local development.
8. Add your deployed callback URL as another redirect URI for production, for example `https://your-deployment.com/auth/callback`.

You can keep both local and production redirect URIs on the same OAuth application.

Copy the generated application credentials into `.env`:

| Environment variable | Clerk value |
|----------------------|-------------|
| `CLERK_OAUTH_CLIENT_ID` | OAuth application Client ID |
| `CLERK_OAUTH_CLIENT_SECRET` | OAuth application Client Secret |
| `CLERK_FRONTEND_API` | Clerk frontend API URL |
| `CLERK_PUBLISHABLE_KEY` | Clerk publishable key |
| `CLERK_SECRET_KEY` | Clerk secret key |

### 4. Set up Polar billing

NightCode uses Polar credits to gate new work and bill completed AI usage. The server checks the user's active meter balance before creating sessions or sending chat requests, then ingests usage events after AI responses finish.

In your [Polar](https://cwa.run/polar?utm_source=github&utm_medium=readme&utm_campaign=nightcode&utm_content=setup_polar) dashboard, use sandbox mode for local development and create a meter with these exact settings:

| Setting | Value |
|---------|-------|
| Meter name | `nightcode_credits` |
| Filter | Name equals `nightcode_usage` |
| Aggregation | Sum |
| Aggregation property | `credits` |

The event name and metadata key must match exactly. The server sends usage events like this:

```ts
{
  name: "nightcode_usage",
  metadata: { credits }
}
```

Next, create a meter credits benefit and attach it to a one-time purchase product:

1. Create a benefit using the `nightcode_credits` meter.
2. Set the credited units, for example `1000` credits.
3. Create a one-time purchase product, for example `$20` for `1000` credits.
4. Attach the credits benefit to that product.
5. Set the customer portal visibility to private so purchases happen through API-generated checkout links.

Then copy the required Polar values into `.env`:

| Environment variable | Where to find it |
|----------------------|------------------|
| `POLAR_ACCESS_TOKEN` | Polar developer settings token |
| `POLAR_PRODUCT_ID` | Product ID from the credits product |
| `POLAR_SERVER` | Use `sandbox` locally, `production` for live billing |
| `POLAR_CREDITS_METER_ID` | Meter ID from the meter URL |

The CLI upgrade flow calls `/billing/checkout`, which opens a Polar checkout URL. The usage flow calls `/billing/portal`, which opens the customer's Polar portal.

### 5. Set up the database

Generate the Prisma client:

```bash
bun run --cwd packages/database db:generate
```

Apply your Prisma schema to the configured Postgres database using your preferred Prisma workflow.

### 6. Run the server

```bash
bun run dev:server
```

The API runs on `http://localhost:3000`.

### 7. Run the CLI

In another terminal:

```bash
bun run dev:cli
```

To build and link the local CLI binary:

```bash
bun run link:cli
nightcode
```

## Project Structure

```
packages/
├── cli/                         # OpenTUI + React terminal client
│   ├── bin/                     # nightcode executable shim
│   └── src/
│       ├── components/          # Terminal UI components, dialogs, messages
│       ├── hooks/               # Chat and UI hooks
│       ├── layouts/             # Root terminal layouts
│       ├── lib/                 # API client, auth, OAuth, local tool execution
│       ├── providers/           # Dialog, keyboard, prompt, theme, toast providers
│       └── screens/             # Home, new session, and session screens
├── database/                    # Prisma schema, generated client, database exports
├── server/                      # Hono API for auth, billing, sessions, and chat
└── shared/                      # Shared schemas, tool contracts, and model registry
```

## Scripts

| Command | Description |
|---------|-------------|
| `bun run dev:cli` | Start the CLI in watch mode |
| `bun run dev:server` | Start the Hono server with hot reload |
| `bun run build:cli` | Build the CLI package |
| `bun run link:cli` | Build and link the `nightcode` executable |
| `bun run --cwd packages/database db:generate` | Generate the Prisma client |

## Packages

| Package | Description |
|---------|-------------|
| `@nightcode/cli` | Terminal UI and client-side tool execution |
| `@nightcode/server` | Hono API, AI streaming, auth checks, and billing ingestion |
| `@nightcode/database` | Prisma client and database schema |
| `@nightcode/shared` | Shared Zod schemas, AI tool contracts, and model definitions |
