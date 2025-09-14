📌 Habit Tracker — Specification
1. Overview

The Habit Tracker App is a web application that allows users to create, track, and maintain personal habits while adding a social layer (friends and streaks). It is designed as a modern full-stack project using:

Frontend: Next.js 14 (App Router), TailwindCSS, shadcn/ui

Backend: Next.js API Routes (optionally NestJS as a separate service)

Database: PostgreSQL (via Prisma ORM)

Auth: NextAuth (JWT + OAuth in the future)

Deployment: Vercel (frontend & backend), Neon.tech or Supabase (Postgres)

The app should be simple, clean, and mobile-friendly.

2. Core Features
🔹 Authentication

User registration & login (email + password).

Session management (JWT).

Profile settings (username, avatar).

🔹 Habits Management

Create a habit (name, description, frequency: daily/weekly).

Edit or delete habits.

Track progress (mark as “done” for today).

Show streak count (e.g., 🔥 12 days in a row).

Calendar view of completed days.

🔹 Dashboard

List of all habits in cards (with progress bar & “Mark Done” button).

Quick stats: active habits, longest streak, completion % this week.

🔹 Statistics

Graphs:

Weekly completion rate.

Longest streaks.

KPI cards: Avg. Completion %, Total Habits Created.

🔹 Social (Friends)

Search and add friends.

See friend’s streaks.

Leaderboard (sorted by streaks).

3. Nice-to-Have Features (Future Scope)

Notifications / Reminders (via email or push).

AI Integration (OpenAI API): AI coach that suggests new habits.

Gamification: badges, rewards, levels.

PWA Support: installable app on mobile.

4. Technical Stack
Frontend

Next.js 14 (App Router, Server Components).

shadcn/ui for UI components (cards, tables, modals).

TailwindCSS for styling.

lucide-react for icons.

recharts for graphs (stats page).

Backend

Next.js API Routes (first version).

(Optional) NestJS backend for enterprise separation.

Database

PostgreSQL hosted on Supabase or Neon.tech (free tier).

Prisma ORM for schema, migrations, and queries.

Auth

NextAuth with Credentials provider (JWT).

Later: Google/GitHub OAuth.

Deployment

Vercel for frontend + backend.

Supabase/Neon for database.

5. Database Schema (Prisma)
model User {
  id        String   @id @default(uuid())
  email     String   @unique
  password  String
  name      String?
  avatarUrl String?
  habits    Habit[]
  friends   Friendship[] @relation("UserFriends")
}

model Habit {
  id        String   @id @default(uuid())
  userId    String
  name      String
  frequency String   // daily, weekly
  createdAt DateTime @default(now())
  progress  Progress[]
  user      User     @relation(fields: [userId], references: [id])
}

model Progress {
  id       String   @id @default(uuid())
  habitId  String
  date     DateTime
  done     Boolean  @default(false)
  habit    Habit    @relation(fields: [habitId], references: [id])
}

model Friendship {
  id        String   @id @default(uuid())
  userId    String
  friendId  String
  createdAt DateTime @default(now())
  
  user   User @relation("UserFriends", fields: [userId], references: [id])
  friend User @relation("UserFriends", fields: [friendId], references: [id])
}

6. Pages & UI Components
Pages

/login — Sign In form.

/register — Sign Up form.

/dashboard — List of habits, quick stats.

/habits/[id] — Habit detail with calendar.

/stats — Graphs + KPIs.

/friends — Friend list + leaderboard.

Components

HabitCard (with streak, progress, button).

Sidebar (navigation).

StatsCard (completion %, streak).

FriendListItem.

Calendar (completed days).

7. Example User Flow

User registers → logs in.

Creates first habit “🏃 Run every morning.”

Marks it as done for 3 days → streak shows 🔥 3.

Dashboard shows “2 active habits, avg. completion 80%.”

User adds a friend → sees leaderboard.

⚡ With this spec, you have:

A clear MVP scope.

Database schema.

Tech stack.

UI + pages.

Room for future “wow” features (AI, gamification).