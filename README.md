Perfect place to pause 👍
Below is a **complete, production-quality `README.md`** that documents **everything you’ve built so far** — architecture, tech stack, features, file structure, setup commands, and workflows.

You can **copy-paste this directly** into `README.md` at the project root.

---

````md
# 🥋 CommandDojo

**CommandDojo** is an interactive learning platform to master **Linux, Windows, and Git commands** using:
- real terminal practice
- Asciinema demos
- explanation videos
- admin-curated lessons
- progress-ready architecture

Built with modern full-stack technologies and designed for scalability.

---

## 🚀 Features Implemented (Current Status)

### 👨‍🎓 User Side
- Browse commands by section:
  - `/linux`
  - `/windows`
  - `/git`
- Command detail page includes:
  - command description
  - **syntax (required)**
  - Asciinema terminal demo
  - practice button (shared terminal)
  - explanation video (any provider supported)
- Dedicated **practice terminal** (`/practice`)
- No stale cache across tabs (always fresh data)

### 🛠 Admin Side
- Secure admin login
- Admin dashboard:
  - create commands
  - edit commands
  - publish/unpublish
- Required fields enforced:
  - syntax
  - asciinema URL
  - explanation video URL
- **Live embed preview** while typing:
  - Asciinema iframe
  - YouTube / Vimeo embeds
  - Any other video URL with a fallback link
- Tags support
- Lesson steps for guided practice
- Cache revalidation after save (other tabs update instantly)

### 🔐 Authentication
- Credentials-based auth using **NextAuth**
- Roles:
  - `ADMIN`
  - `USER`
- Secure password hashing (bcrypt)

---

## 🧱 Tech Stack

### Frontend
- **Next.js 16 (App Router)**
- **React 19**
- **TypeScript**
- **Tailwind CSS**
- **Framer Motion** (animations)
- **Lucide Icons**

### Backend
- **Next.js Server Actions**
- **Prisma ORM**
- **PostgreSQL**

### Auth
- **NextAuth (Credentials Provider)**
- bcrypt password hashing

---

## 📂 Project Structure

```text
commanddojo/
├── app/
│   ├── admin/
│   │   └── commands/
│   │       ├── page.tsx              # Admin command list
│   │       ├── new/
│   │       │   └── page.tsx           # Create command
│   │       └── [id]/
│   │           ├── page.tsx           # Edit command
│   │           └── actions.ts         # Update command (server action)
│   │
│   ├── api/
│   │   └── auth/
│   │       └── [...nextauth]/route.ts
│   │
│   ├── linux/
│   │   ├── page.tsx                  # Linux command list
│   │   └── [slug]/page.tsx           # Linux command detail
│   │
│   ├── windows/
│   ├── git/
│   ├── practice/
│   │   └── page.tsx                  # Shared practice terminal
│   │
│   ├── login/
│   │   └── page.tsx
│   ├── signup/
│   │   └── page.tsx
│   ├── layout.tsx
│   └── page.tsx                      # Home
│
├── components/
│   ├── admin/
│   │   ├── EmbedPreview.tsx           # Live iframe preview
│   │   └── CreateCommandForm.tsx
│   └── TopNav.tsx
│
├── lib/
│   ├── prisma.ts                      # Prisma client
│   └── authOptions.ts                 # NextAuth config
│
├── prisma/
│   ├── schema.prisma
│   ├── migrations/
│   └── seed.ts
│
├── public/
├── .env
├── package.json
└── README.md
````

---

## 🗄 Database Models (Prisma)

### User

```prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  password  String
  role      Role     @default(USER)
  createdAt DateTime @default(now())

  progress  Progress[]
}
```

### Section

```prisma
model Section {
  id       String    @id @default(cuid())
  key      String    @unique
  title    String
  commands Command[]
}
```

### Command

```prisma
model Command {
  id                    String   @id @default(cuid())
  sectionId             String
  slug                  String
  title                 String
  shortSummary           String
  description            String
  syntax                 String
  asciinemaUrl           String
  explanationVideoUrl    String
  tags                   Json?
  lessonSteps            Json?
  published              Boolean  @default(false)
  createdAt              DateTime @default(now())
  updatedAt              DateTime @updatedAt

  section  Section @relation(fields: [sectionId], references: [id])
  progress Progress[]
}
```

### Progress (Ready for future use)

```prisma
model Progress {
  id        String   @id @default(cuid())
  userId    String
  commandId String
  completed Boolean  @default(false)
  createdAt DateTime @default(now())

  user    User    @relation(fields: [userId], references: [id])
  command Command @relation(fields: [commandId], references: [id])

  @@unique([userId, commandId])
}
```

---

## 🔧 Setup Instructions

### 1️⃣ Clone & Install

```bash
git clone <repo-url>
cd commanddojo
npm install
```

### 2️⃣ Environment Variables

Create `.env`:

```env
DATABASE_URL="postgresql://commanddojo_user:password@localhost:5432/commanddojo"
NEXTAUTH_SECRET="your-secret"
NEXTAUTH_URL="http://localhost:3000"
```

### 3️⃣ Database Setup

```bash
npx prisma migrate dev
npx prisma generate
```

### 4️⃣ Seed Admin (optional)

```bash
npm run prisma: seed
```

### 5️⃣ Run Dev Server

```bash
npm run dev
```

---

## 🔑 Admin Management (CLI)

### View Admins

```sql
SELECT email, role FROM "User" WHERE role='ADMIN';
```

### Promote User to Admin

```sql
UPDATE "User" SET role='ADMIN' WHERE email='you@example.com';
```

---

## 🧠 Caching Strategy

* All dynamic pages use:

```ts
export const dynamic = "force-dynamic";
export const revalidate = 0;
```

* Server actions call:

``` ts
revalidatePath(...)
```

➡ ensures **no stale data across tabs**

---

## 🎯 Supported Video Types

* YouTube
* Vimeo
* Direct `.mp4 / .webm`
* NotebookLM or any external URL

  * iframe attempted
  * fallback “Open video” link always shown

---

## 📌 Current Status

✅ MVP complete
✅ Stable
✅ Ready for feature expansion

---

## 🧭 Planned Next Phases (Paused)

* User progress UI
* XP / streaks
* Search & filters
* Admin analytics
* Deployment (Docker / VPS / Vercel)

---

## 👨‍💻 Author

Built with care by **Pavan Kumar**
Assisted by ChatGPT

---

## 🥋 Philosophy

> *Learn commands by doing — not memorising.*
