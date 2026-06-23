# 🌿 Plant Nursery Project

A full‑stack web application for a plant nursery that showcases a catalog of plants with detailed information, pricing, and a direct contact mechanism for purchases. Built with **Next.js** (frontend + backend) and designed with a **microservices‑oriented architecture** for scalability and maintainability.

---

## 📌 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
  - [Microservices Design](#microservices-design)
  - [Next.js as Full‑Stack Platform](#nextjs-as-full-stack-platform)
- [Potential Challenges & Mitigations](#potential-challenges--mitigations)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Variables](#environment-variables)
  - [Running the Project](#running-the-project)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

---

## 📖 Overview

The **Plant Nursery Project** is a responsive web application that allows users to:

- Browse a curated collection of plants with images, scientific names, care instructions, and prices.
- Filter and search plants by categories (e.g., indoor, outdoor, flowering, succulents).
- View detailed plant profiles.
- Contact the nursery directly via email or phone to place orders or ask questions.
- Submit inquiries through a contact form that stores messages for nursery staff.

The system is built with **Next.js** to leverage its hybrid rendering capabilities (SSR, SSG, ISR) and API routes for backend logic. To ensure future extensibility and maintainability, the backend logic is organized following **microservices principles**—even within the Next.js ecosystem.

---

## ✨ Features

- **Plant Catalog** – Display plants with image, name, price, description, and care tips.
- **Filtering & Search** – Quick search and category filters (e.g., light requirements, water needs).
- **Plant Detail Page** – Dedicated page for each plant with in‑depth information.
- **Contact & Inquiry** – Contact form (email/phone) to request purchases or ask questions; data stored in database.
- **Admin Dashboard** (optional) – Manage plant inventory, view incoming inquiries (protected route).
- **Responsive UI** – Optimised for mobile, tablet, and desktop.
- **Performance** – Static generation (SSG) for plant list and detail pages where possible.
- **API Endpoints** – RESTful APIs for plant data, inquiries, and admin operations.

---

## 🛠️ Tech Stack

| Layer           | Technology                                                                                          |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **Frontend**    | Next.js (React), TypeScript, Tailwind CSS (or any CSS framework), Framer Motion (animations)        |
| **Backend**     | Next.js API Routes (Node.js) – organized as microservices                                           |
| **Database**    | PostgreSQL (or MongoDB) – with Prisma ORM / Mongoose                                                |
| **Authentication** | NextAuth.js (for admin access)                                                                  |
| **Email**       | Nodemailer / SendGrid API for contact emails                                                        |
| **Image Hosting**| Cloudinary / AWS S3 (or Next.js built‑in Image optimization)                                       |
| **Validation**  | Zod / Yup                                                                                           |
| **Testing**     | Jest, React Testing Library                                                                         |
| **CI/CD**       | GitHub Actions, Vercel / Docker                                                                     |

---

## 🏗️ Architecture

### Microservices Design

Even though the project runs on a single Next.js codebase, the backend logic is **decoupled into domain‑specific services** to mimic a microservices approach:

- **Plant Service** – Handles CRUD operations for plant catalog, search, and filtering.
- **Inquiry Service** – Manages customer inquiries, email notifications, and storage.
- **User Service** – (Optional) Manages admin authentication and roles.

Each service is implemented as a **separate set of API routes** (e.g., `/api/plants/*`, `/api/inquiries/*`, `/api/auth/*`) and communicates via a **service layer** that abstracts database access. This structure:

- Makes the codebase easier to maintain and scale.
- Allows independent evolution of each domain.
- Facilitates potential migration to separate deployable microservices in the future (e.g., using Docker, Kubernetes) without rewriting the entire app.

> **Note:** For a true microservices deployment, each service would run as a separate container. However, for a small‑scale project, the logical separation within Next.js provides many of the same benefits at a lower operational cost.

### Next.js as Full‑Stack Platform

Next.js provides a unified environment for both frontend and backend:

- **Frontend**: Pages and components are rendered using React with support for SSG, SSR, and ISR.
- **Backend**: API routes (`/pages/api/*` or `/app/api/*`) handle server‑side logic, database interactions, and external integrations.

This simplifies development, deployment, and debugging, especially for MVP and mid‑sized projects.

---

## ⚠️ Potential Challenges & Mitigations

Using Next.js for both frontend and backend—especially when aiming for a microservices architecture—comes with certain challenges. Below are the common ones and our strategies to address them:

| Challenge                                                                 | Mitigation                                                                                               |
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| **API routes are monolithic by default** – All routes run in the same serverless function, making it hard to scale individual services. | Organise routes by domain (service folders) and use a **service‑oriented folder structure**. For heavy workloads, consider deploying each service as a separate Next.js app or using a dedicated backend framework. |
| **Cold starts & execution limits** – Serverless functions (Vercel) have timeouts and cold start latency. | Use **ISR/SSG** for static pages to reduce API calls. For long‑running tasks, offload to background jobs (e.g., queues) or use edge functions where appropriate. |
| **Database connection management** – Each serverless invocation may open a new DB connection, leading to exhaustion. | Implement a **connection pool** and reuse the client across invocations (singleton pattern). Use a database proxy (e.g., PgBouncer, Prisma Accelerate). |
| **Deployment complexity** – Microservices often require separate builds and deployments. | For this project, we start with a **monorepo** (single codebase) with clear service boundaries. If needed, we can split into separate Next.js apps later using tools like **Turborepo** or **Nx**. |
| **Environment configuration** – Different services may need different env vars. | Use a single `.env` file with prefixes (e.g., `PLANT_SERVICE_DB_URL`) and a configuration module that loads settings per service. |
| **Inter‑service communication** – If services need to talk to each other, they must call each other’s API endpoints, which can introduce latency and coupling. | Minimise direct calls by using a **shared data layer** (database) or **event‑driven** patterns (e.g., using Redis Pub/Sub or message queues). For simplicity, we keep services loosely coupled and rely on the database for state sharing. |

> **Recommendation:** For a small‑scale plant nursery project, the logical microservices approach within Next.js is sufficient. If the project grows, we can easily extract each service into separate backend services (e.g., Express, Fastify, or NestJS) while keeping Next.js as the frontend BFF (Backend for Frontend).

---

## 🚀 Getting Started

### Prerequisites

- Node.js (v18+)
- npm / yarn / pnpm
- PostgreSQL (or MongoDB) – local or cloud (e.g., Supabase, Neon)
- Git

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/plant-nursery.git
   cd plant-nursery
   ```

2. **Install dependencies**
   ```bash
   npm install
   # or
   yarn install
   ```

3. **Set up environment variables** – see [Environment Variables](#environment-variables).

4. **Set up the database**
   - Run migrations (if using Prisma):
     ```bash
     npx prisma migrate dev --name init
     ```
   - Seed the database with sample plants (optional):
     ```bash
     npm run seed
     ```

### Environment Variables

Create a `.env.local` file in the root directory and add the following:

```env
# Database
DATABASE_URL="postgresql://user:password@localhost:5432/plantnursery"

# Authentication (for admin)
NEXTAUTH_SECRET="your-secret"
NEXTAUTH_URL="http://localhost:3000"

# Email (for contact form)
EMAIL_HOST="smtp.example.com"
EMAIL_PORT="587"
EMAIL_USER="your-email@example.com"
EMAIL_PASS="your-password"

# Optional: Cloudinary for image uploads
CLOUDINARY_CLOUD_NAME="your-cloud-name"
CLOUDINARY_API_KEY="your-api-key"
CLOUDINARY_API_SECRET="your-api-secret"
```

### Running the Project

```bash
# Development mode
npm run dev

# Build for production
npm run build
npm start
```

The application will be available at `http://localhost:3000`.

---

## 📦 Deployment

The project is optimised for deployment on **Vercel** (zero‑config for Next.js). However, you can also deploy to any Node.js hosting platform (e.g., AWS, DigitalOcean, Heroku) using Docker.

**Vercel Deployment Steps:**

1. Push the repository to GitHub.
2. Import the project in Vercel.
3. Add the environment variables in the Vercel dashboard.
4. Deploy – Vercel will automatically detect Next.js and build accordingly.

**Docker Deployment** (for custom microservices):

- A `Dockerfile` and `docker-compose.yml` are provided to run the application along with a PostgreSQL container.

---

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository and create your feature branch (`git checkout -b feature/amazing-feature`).
2. Commit your changes (`git commit -m 'Add some amazing feature'`).
3. Push to the branch (`git push origin feature/amazing-feature`).
4. Open a Pull Request.

Please ensure your code adheres to the project's coding standards and includes relevant tests.

---

## 📄 License

This project is licensed under the MIT License – see the [LICENSE](LICENSE) file for details.

---

## 🧑‍🌾 Acknowledgments

- Icons and images from [Unsplash](https://unsplash.com) / [Pexels](https://pexels.com)
- Plant data sourced from public botanical databases

---

**Happy Planting! 🌱**  
*For any questions or support, please open an issue or contact the maintainers.*