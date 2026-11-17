# Deploy Your English Classroom Hub Outside Arena (permanent link + teacher editing)

This project is a complete Next.js + PostgreSQL app. Once deployed you get your **own permanent website address**
(for example `https://english-classroom-hub.vercel.app`) where:

- **You** sign in as teacher and **add / edit / delete** activities, games, Canva projects, worksheets and links.
- **Students** open the same link (no login) and play everything.
- Everything is stored in a real database, so changes are visible to every student on every device.

You need two **free** accounts: **Neon** (database) and **Vercel** (website hosting).

---

## Step 1 — Get the project file

In the website click **Share Hub → Deploy Online** and press **Download Project ZIP**
(file name: `english-classroom-hub-source.zip`).

Unzip it on your computer. You should see `package.json`, `src`, `public`, etc.

---

## Step 2 — Create a free PostgreSQL database (Neon)

1. Go to <https://neon.tech> → **Sign up** (free).
2. Click **New Project** → choose any name → **Create**.
3. On the project dashboard click **Connect** and copy the **connection string**.
   It looks like:

   ```
   postgresql://neondb_owner:XXXXXXXX@ep-xxxx-xxxx.eu-central-1.aws.neon.tech/neondb?sslmode=require
   ```

4. Keep this string safe — it is your `DATABASE_URL`.

---

## Step 3 — Deploy to Vercel

### Option A —Vercel CLI (fastest, no GitHub needed)

1. Install Node.js from <https://nodejs.org> (LTS version).
2. Open a terminal / command prompt **inside the unzipped project folder** and run:

   ```bash
   npm install
   npx vercel login
   npx vercel
   ```

   Accept the default answers (press Enter).
3. Add the database password as an environment variable:

   ```bash
   npx vercel env add DATABASE_URL production
   ```

   Paste your Neon connection string when asked, then press Enter.
4. Go live:

   ```bash
   npx vercel --prod
   ```

   Vercel prints your public address, e.g. `https://english-classroom-hub.vercel.app`.

### Option B — GitHub + Vercel website (no terminal)

1. Create a GitHub account, then a **new repository** and upload the unzipped project files.
2. Go to <https://vercel.com> → **Add New → Project** → **Import** your repository.
3. Before clicking Deploy, open **Environment Variables** and add:
   - **Name:** `DATABASE_URL`
   - **Value:** your Neon connection string
4. Click **Deploy**.

---

## Step 4 — Create the tables (one click, once)

Open this address in your browser (replace with your own domain):

```
https://YOUR-SITE.vercel.app/api/setup
```

You should see:

```json
{ "success": true, "message": "Database ready..." }
```

This creates the tables and loads the starter English activities.

---

## Step 5 — Sign in as teacher

1. Open your new site.
2. Click **Teacher Sign In** (top right).
3. Enter the default passcode: **`teacher123`**
4. Open **Settings** and set your **own secret passcode**.

Now you can **Add Activity**, **Edit**, **Publish**, and **Delete** — from any device, anywhere in the world.

---

## Step 6 — Share with students

1. In **Share Hub → Links & QR Code**, paste your Vercel address into **Custom Web Domain** and press **Save**.
   From now on the copy-link buttons and the printable **QR code** point to your permanent site.
2. Share `/api/setup` finished site with parents, or print the QR card for tablets.

---

## Updating the site later

After changing code on your computer:

```bash
npx vercel --prod
```

Your database content stays untouched — only the app code updates.

---

## Notes

- **Audio / speech** works best on real devices (iPad, Chrome, Edge). On iPhone, turn the side **silent switch off**.
- **Canva websites** (`*.my.canva.site`) block being embedded in other pages, so Canva cards open in a new tab.
  Gemini HTML games run inside the classroom with a **Full Screen / Bigger Screen** button.
- **Uploaded PDF / DOCX / PPTX** files are stored inside the database as attachments.
  If you upload very large files, consider hosting them on Google Drive and adding them as a Link instead.
- Free Neon + Vercel plans are enough for a classroom. Both can be upgraded later without changing anything.

---

### Environment variables used

| Name           | Required | Example                                                              |
| -------------- | -------- | -------------------------------------------------------------------- |
| `DATABASE_URL` | Yes      | `postgresql://user:pass@host/db?sslmode=require`                     |

### Useful commands

```bash
npm run dev     # run locally on http://localhost:3000
npm run build   # production build
npm start       # run the production build
```

### Manual schema push (optional)

If you prefer Drizzle migrations instead of `/api/setup`:

```bash
npx drizzle-kit push --dialect postgresql --schema ./src/db/schema.ts --url "YOUR_DATABASE_URL"
```
