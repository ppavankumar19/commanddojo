# 🥋 CommandDojo

CommandDojo is an interactive educational platform to learn **Linux, Windows, and Git commands** using real terminal practice, Asciinema demos, and explanation videos — all managed through a secure admin dashboard.

This project is built with modern full-stack technologies and designed to scale.

---

## 🚀 Features (Implemented)

### 👨‍🎓 User Features
- Browse commands by section:
  - `/linux`
  - `/windows`
  - `/git`
- Command detail page includes:
  - command description
  - **syntax (required)**
  - Asciinema terminal recording
  - “Practice this command” button
  - explanation video (YouTube, Vimeo, or any URL)
- Dedicated practice terminal:
  - `/practice`
- Always-fresh data across multiple tabs (no stale cache)

---

### 🛠 Admin Features
- Secure admin authentication
- Admin dashboard:
  - create commands
  - edit commands
  - publish/unpublish
- Required fields enforced:
  - syntax
  - Asciinema URL
  - explanation video URL
- Live embed preview while typing:
  - Asciinema iframe preview
  - YouTube / Vimeo auto-embed
  - Any other video URL with a fallback link
- Tags support
- Lesson steps for guided practice
- Automatic cache revalidation after save

---

### 🔐 Authentication & Roles
- Credentials-based authentication using **NextAuth**
- Roles:
  - `ADMIN`
  - `USER`
- Passwords securely hashed using **bcrypt**

---

## 🧱 Tech Stack

### Frontend
- **Next.js 16 (App Router)**
- **React 19**
- **TypeScript**
- **Tailwind CSS**
- **Framer Motion**
- **Lucide Icons**

### Backend
- **Next.js Server Actions**
- **Prisma ORM**
- **PostgreSQL**

### Authentication
- **NextAuth**
- **bcrypt**

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
│   │           └── actions.ts         # Update command
│   │
│   ├── api/
│   │   └── auth/
│   │       └── [...nextauth]/route.ts
│   │
│   ├── linux/
│   │   ├── page.tsx
│   │   └── [slug]/page.tsx
│   │
│   ├── windows/
│   ├── git/
│   ├── practice/
│   │   └── page.tsx
│   │
│   ├── login/
│   ├── signup/
│   ├── layout.tsx
│   └── page.tsx
│
├── components/
│   ├── admin/
│   │   ├── EmbedPreview.tsx
│   │   └── CreateCommandForm.tsx
│   └── TopNav.tsx
│
├── lib/
│   ├── prisma.ts
│   └── authOptions.ts
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
  id                   String   @id @default(cuid())
  sectionId            String
  slug                 String
  title                String
  shortSummary          String
  description           String
  syntax                String
  asciinemaUrl          String
  explanationVideoUrl   String
  tags                  Json?
  lessonSteps           Json?
  published             Boolean  @default(false)
  createdAt             DateTime @default(now())
  updatedAt             DateTime @updatedAt

  section  Section @relation(fields: [sectionId], references: [id])
  progress Progress[]
}
```

### Progress (Prepared for future use)

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
git clone <your-repo-url>
cd commanddojo
npm install
```

### 2️⃣ Environment Variables

Create `.env`:

```env
DATABASE_URL="postgresql://user:password@localhost:5432/commanddojo"
NEXTAUTH_SECRET="your-secret"
NEXTAUTH_URL="http://localhost:3000"
```

### 3️⃣ Database Setup

```bash
npx prisma migrate dev
npx prisma generate
```

### 4️⃣ (Optional) Seed Admin

```bash
npm run prisma:seed
```

### 5️⃣ Run Development Server

```bash
npm run dev
```

---

## 🔑 Admin Management (CLI)

### View Admin Users

```sql
SELECT email, role FROM "User" WHERE role = 'ADMIN';
```

### Promote User to Admin

```sql
UPDATE "User"
SET role = 'ADMIN'
WHERE email = 'user@example.com';
```

---

## 🧠 Caching Strategy

* All dynamic pages use:

``` ts
export const dynamic = "force-dynamic";
export const revalidate = 0;
```

* Admin server actions call:

``` ts
revalidatePath(...)
```

➡ ensures fresh data across tabs and sessions.

---

## 🎥 Supported Video Sources

* YouTube
* Vimeo
* Direct video files (`.mp4`, `.webm`)
* Any external URL (NotebookLM, etc.)

  * iframe attempted
  * fallback “Open video” link always available

---

## 📌 Current Status

* ✅ MVP complete
* ✅ Stable
* ✅ Ready for expansion

---

## 🧭 Planned (Paused)

* User progress UI
* Search & tag filters
* XP / streaks
* Admin analytics
* Deployment (Docker / VPS / Vercel)

---

## 👨‍💻 Author

**Pavan Kumar**

---

## 🥋 Philosophy

> Learn commands by doing — not memorising.

```
