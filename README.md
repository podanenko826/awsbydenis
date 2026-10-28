# AWS by Denis (Tech & Cloud Blog)

A modern, full-stack technical blog and publishing platform built with Next.js 15 (App Router), TypeScript, Supabase (PostgreSQL), and AWS services (CDK, SES, Lambda).

## Features

- 📝 **Rich MDX Content & Live Editing**: Write and publish interactive articles using `@mdxeditor/editor` with live preview, syntax highlighting, and custom MDX components.
- 👥 **Multi-Author Profiles**: Dedicated author pages, bio, social links, role-based metadata, and author post filtering.
- 🏷️ **Tagging & Content Discovery**: Categorization by tags, pagination, adjacent post navigation, and view counters.
- 📚 **Books & Resources Showcase**: Dedicated section to highlight published books, study guides, and external purchase links.
- 📬 **Newsletter & Subscription System**: Built-in subscriber management with preference categories (articles, products, services) and automated email notifications.
- 💬 **Interactive Community**: Comments powered by Giscus (GitHub Discussions integration).
- 🛡️ **Secure Admin Dashboard**: Dedicated `/admin` suite to manage posts, tags, authors, media, and subscribers, protected by IP-whitelisting middleware.
- ⚡ **Optimized Performance & SEO**: Server-side rendering, dynamic OpenGraph/metadata generation, and responsive layouts.
- 🐳 **Docker Ready**: Containerized deployment support with multi-stage Docker configuration.

## Tech Stack

- **Framework**: [Next.js 15](https://nextjs.org/) (App Router)
- **Language**: [TypeScript](https://www.typescriptlang.org/)
- **UI & Styling**: React 18, Bootstrap 5, React-Bootstrap, Emotion / Styled Components, Material UI (MUI Icons), Sass
- **Database**: [Supabase](https://supabase.com/) (PostgreSQL) via `@supabase/supabase-js`
- **Markdown & MDX**: MDX Editor (`@mdxeditor/editor`), `@next/mdx`, `next-mdx-remote`, `remark`, `rehype`, `highlight.js`
- **State Management**: [Zustand](https://github.com/pmndrs/zustand)
- **Cloud & Infrastructure**: AWS CDK, AWS SES (Simple Email Service), AWS Lambda, AWS SDK v3
- **Comments**: [Giscus](https://giscus.app/) (GitHub Discussions)

---

## Getting Started

### Prerequisites

- **Node.js**: v18.0.0 or higher
- **npm** (or `pnpm` / `yarn`)
- A [Supabase](https://supabase.com/) project (with `posts`, `authors`, `tags`, and `subscribers` tables)
- (Optional) AWS Account configured for SES and CDK email deployments

### Installation

1. **Clone the repository**:

```bash
git clone <repository-url>
cd awsbydenis
```

2. **Install dependencies**:

```bash
npm install
```

3. **Configure environment variables**:

Create a `.env.local` file in the root directory (refer to [Environment Variables](#environment-variables)):

```bash
cp .env.example .env.local  # or create .env.local manually
```

4. **Start the development server**:

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

---

### Build for Production

To create an optimized production build:

```bash
npm run build
```

To run the production server:

```bash
npm run start
```

### Run with Docker

Build and run the container locally:

```bash
# Build Docker image
docker build -t awsbydenis .

# Run Docker container
docker run -p 3000:3000 --env-file .env.local awsbydenis
```

---

## Environment Variables

Configure the following variables in your `.env.local` file:

| Variable | Required | Description |
| :--- | :---: | :--- |
| `NEXT_PUBLIC_SUPABASE_URL` | Yes | Supabase project API URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Yes | Supabase public anonymous API key |
| `SUPABASE_SERVICE_ROLE_KEY` | Yes | Supabase service role key (for server/admin queries) |
| `NEXT_PUBLIC_API_BASE_URL` | No | Base URL for internal API fetches (defaults to `http://localhost:3000`) |
| `ALLOWED_ADMIN_IPS` | No | Comma-separated list of allowed IPs for `/admin` access |
| `NEXT_PUBLIC_AWS_REGION` | No | AWS region for SES email delivery (e.g., `eu-central-1`) |
| `NEXT_PUBLIC_ACCESS_KEY_ID` | No | AWS IAM Access Key for SES |
| `NEXT_PUBLIC_SECRET_ACCESS_KEY` | No | AWS IAM Secret Key for SES |
| `NEXT_PUBLIC_SENDER_EMAIL` | No | Verified AWS SES sender email address |

---

## How to Use

### 1. Public Reader Experience

- **Homepage (`/`)**: Browse recent posts, featured articles, and quick-filter by categories.
- **Article Reader (`/[slug]`)**: Read full articles rendered with responsive typography, code highlighting, and Giscus comments.
- **Adjacent Navigation**: Seamlessly navigate between previous and next articles.
- **Authors (`/author/[handle]`)**: View author bio, credentials, social links, and associated posts.
- **Books (`/books`)**: Discover published technical books and external purchase links.
- **Newsletter (`/subscribe`, `/preferences`, `/unsubscribe`)**: Manage email subscription preferences.

### 2. Admin Dashboard (`/admin`)

The admin dashboard provides full editorial and management capabilities:

- **Posts Manager (`/admin/posts`)**: Create, draft, edit, and publish posts using the interactive MDX visual editor.
- **Authors Manager (`/admin/authors`)**: Create and manage contributor profiles and social links.
- **Media & Images (`/admin/images`)**: Manage article assets and upload cover images.
- **Subscribers (`/admin/subscribers`)**: Monitor subscriber growth and email notification preferences.
- **Tags (`/admin/tags`)**: Organize taxonomy and topics.

> [!NOTE]
> **Route Protection**: The `/admin` routes are secured by Next.js middleware in `src/middleware.ts`. Unauthorized IPs will receive a `404 Not Found` response to keep the admin portal hidden. In local development (`NODE_ENV=development`), `localhost` is allowed by default.

---

## Project Structure

```text
awsbydenis/
├── src/
│   ├── app/                         # Next.js App Router
│   │   ├── (user)/                  # Public customer-facing routes
│   │   │   ├── [slug]/              # Article reader & comments
│   │   │   ├── author/              # Author profile views
│   │   │   ├── books/               # Books showcase
│   │   │   ├── imprint/             # Legal imprint page
│   │   │   ├── privacy-policy/      # Privacy policy
│   │   │   ├── preferences/         # Newsletter preference center
│   │   │   └── page.tsx             # Blog homepage
│   │   ├── (admin)/admin/           # Protected admin dashboard
│   │   │   ├── posts/               # MDX post editor & manager
│   │   │   ├── authors/             # Author profiles management
│   │   │   ├── images/              # Image & media manager
│   │   │   ├── subscribers/         # Subscriber list & filters
│   │   │   └── tags/                # Content tags manager
│   │   ├── api/                     # Next.js Route Handlers (REST API)
│   │   │   ├── posts/               # Posts CRUD & view increments
│   │   │   ├── author/              # Author endpoints
│   │   │   ├── subscriber/          # Newsletter subscription API
│   │   │   └── image/               # Image upload handlers
│   │   └── middleware.ts            # Admin IP authorization middleware
│   │
│   ├── components/                  # Reusable UI components
│   │   ├── EditorComponent.tsx      # MDX rich-text editor
│   │   ├── NavBar.tsx               # Site navigation header
│   │   ├── Footer.tsx               # Site footer
│   │   ├── PostCard/                # Post card preview components
│   │   └── Modals/                  # Management modals
│   │
│   ├── lib/                         # Clients & core business logic
│   │   ├── supabase.ts              # Supabase client initialization
│   │   ├── posts.ts                 # Post data access layer
│   │   ├── authors.ts               # Author data access layer
│   │   ├── subscribers.ts           # Subscriber data access layer
│   │   └── email-stack.ts           # AWS CDK Email Stack definition
│   │
│   ├── lambda/                      # Serverless functions
│   │   └── send-email.ts            # AWS Lambda handler for SES emails
│   │
│   ├── services/                    # Email templates & markdown utilities
│   │   ├── sendEmailsOnPost.ts      # SES notification orchestration
│   │   └── updateEmailTemplate.ts   # HTML email template generator
│   │
│   └── types/                       # Shared TypeScript interfaces
│       └── index.ts                 # Author, Post, Tag, Subscriber types
│
├── scripts/                         # Database and maintenance scripts
│   ├── seed/                        # Seeding legal pages and initial posts
│   └── verify/                      # Connectivity and data health checks
│
├── Dockerfile                       # Production container setup
├── package.json                     # Scripts & project dependencies
└── tsconfig.json                    # TypeScript compiler configuration
```

---

## Database & Maintenance Scripts

Custom utility scripts can be run directly from the root using npm:

- **Seed Legal Pages**:
  ```bash
  npm run db:seed
  ```
- **Check Post Records & DB Health**:
  ```bash
  npm run db:check
  ```

---

## AWS Infrastructure & Email Service

This project integrates AWS CDK for serverless email notification workflows:

- **AWS SES**: Sends transactional and newsletter broadcast emails to active subscribers.
- **AWS Lambda**: Node.js 18 serverless function defined in `src/lambda/send-email.ts` to dispatch messages with IAM policies.
- **AWS CDK Stack**: Defined in `src/lib/email-stack.ts` to deploy and configure the email notification pipeline.

---

## License

This project is licensed under the [MIT License](LICENSE).

## Contributing

Contributions, issues, and feature requests are welcome! Feel free to open an issue or submit a pull request.
