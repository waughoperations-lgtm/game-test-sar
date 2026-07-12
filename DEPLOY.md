# Putting the games online with Railway

This gets your games onto a real web link you can share with friends. You only
have to do it once; after that, updates go live automatically.

The games are plain web pages, plus a tiny helper (`server.js`) that hands them
out. Railway runs that helper for you.

---

## What you need
- A free **GitHub** account (the code already lives there).
- A **Railway** account (you'll make one — it's free to start).

## Step by step

1. Go to **https://railway.app** and click **Login** → **Login with GitHub**.
   Approve the sign-in.

2. Click **New Project** → **Deploy from GitHub repo**.

3. If Railway asks for permission to see your GitHub repositories, click
   **Configure GitHub App** and give it access to the **game-test-sar**
   repository, then come back.

4. Pick the **game-test-sar** repository from the list.

5. Railway will ask which **branch** to deploy. Choose the branch your latest
   work is on (for example `main`, or `claude/visibility-check-xsp3gg`).

6. Railway now builds it automatically. It sees the `package.json`, and runs
   the game server for you. Wait until the deployment shows a **green /
   "Success"** status (about a minute).

7. Open the project's **Settings** → **Networking** → click
   **Generate Domain**. Railway gives you a public link like
   `something.up.railway.app`.

8. Open that link in your browser — you should see the **Spooky Town** menu.
   That's your games, live on the internet!

9. **Share the link** with friends. They open it, pick a game, and:
   - **Domino Night / Multiplayer:** use **Quick Play** to match with anyone,
     or **Create/Join a room** by code to play privately (with voice).

---

## Good to know
- **No settings or passwords needed** — there's nothing to configure. The games
  connect players directly (peer-to-peer) using free public services.
- **Cost:** Railway gives free starting credit; after that the Hobby plan is a
  few dollars a month. A tiny file server like this uses very little.
- **Updating the games:** whenever new changes are pushed to the branch Railway
  is watching, it **redeploys automatically** — the live link updates on its own.
- **The link is public** — anyone who has it can play. That's what makes it
  shareable.

## What this does NOT include (yet)
- **Big-scale matchmaking** (many simultaneous public games) and a **relay
  server** for players on strict networks — those need extra pieces we can add
  later. Quick Play works today for small numbers of players.
