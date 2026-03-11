# Supabase Database Setup Guide

This guide explains how to set up the backend database for the FRC Scouting Dashboard using Supabase. Supabase provides a free PostgreSQL database, authentication, and file storage all in one place.

## Step 1: Create a Project
1. Go to [Supabase.com](https://supabase.com/) and create a free account.
2. Click **New Project** and select your organization.
3. Give your project a name (e.g., `TeamXXXX-Scouting`), generate a secure database password, and select a region closest to your competition area.
4. Click **Create new project**. Note: It will take a few minutes for the database to provision.

## Step 2: Initialize the Database Schema
Once your project is ready, you need to create the tables, storage buckets, and security policies.

1. On the left sidebar, click the **SQL Editor** icon.
2. Click **New Query**.
3. Copy the entire contents of the `schema.sql` file provided with this dashboard.
4. Paste the SQL code into the editor and click **Run** (or press Cmd/Ctrl + Enter).
5. Wait for the Success message. Your tables, triggers, and storage buckets are now fully configured.

## Step 3: Configure Offline Persistence (Crucial for FRC)
Cellular networks at FRC events are highly unreliable. By default, Supabase logs users out after 1 hour if it cannot connect to the internet to refresh their token. We need to extend this to last the entire weekend so scouters can save data offline.

1. On the left sidebar, click the **Authentication** icon (the users icon).
2. Under the Authentication menu, click **Configuration** (the gear icon) and select **Advanced**.
3. Scroll down to the **Sessions** section.
4. Change the **Refresh token reuse interval** to `10` seconds.
5. Click **Save changes**.

## Step 4: Get Your API Keys
To connect your HTML dashboard to this new database, you need two specific keys.

1. On the left sidebar, click the **Project Settings** icon (the gear at the bottom).
2. Click **API** under the Configuration section.
3. Under Project URL, copy the **URL** string.
4. Under Project API keys, copy the **anon public** key string.

## Step 5: Connect the App
1. Open your `index.html` file in a code editor.
2. Scroll to the bottom of the file to locate the `APP_CONFIG` JavaScript object.
3. Paste your Supabase URL and Anon Key into the respective fields:

```javascript
const APP_CONFIG = {
    teamNumber: "XXXX",
    teamName: "YOUR_TEAM_NAME",
    appTitle: "Scouting Dashboard",
    supabaseUrl: 'PASTE_YOUR_PROJECT_URL_HERE',
    supabaseAnonKey: 'PASTE_YOUR_PUBLISHABLE_KEY_HERE',
    ownerEmailLock: "leadscout@yourteam.com",
    defaultEventYear: "2026"
};
```

## Step 6: Configure Initial Event Settings
Before your scouters can log in, you must define your Team Code and The Blue Alliance (TBA) key in your database.

1. Go back to your Supabase dashboard and click the **Table Editor** icon.
2. Click on the `api_keys` table.
3. Edit the `teamCode` row to set the secret password your scouters will use to register new accounts.
4. Edit the `tba` row and paste in your Read API Key from [The Blue Alliance](https://www.thebluealliance.com/account).

Your backend is now fully operational and securely connected to your dashboard.