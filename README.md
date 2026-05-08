# Vexa AI + Make.com Deployment Guide

This guide will help you deploy Vexa Lite on Railway and connect it to your Google Calendar via Make.com.

## Phase 1: Deploying to Railway

1. **Prerequisites**:
   - A [Railway](https://railway.app) account.
   - A Vexa Transcriber API Key from [vexa.ai](https://staging.vexa.ai/dashboard/transcription).

2. **Deployment Steps**:
   - Open your terminal in this `vexa-deployment` folder.
   - Run `railway login` (if you haven't already).
   - Run `railway init` and follow the prompts.
   - Run `railway up`.
   - In the Railway Dashboard, add the following Environment Variables:
     - `TRANSCRIBER_API_KEY`: (Your key from vexa.ai)
     - `ADMIN_API_TOKEN`: (The content of `admin_token.txt`)
     - `DATABASE_URL`: (Railway will automatically provide this if you add a PostgreSQL service, or you can link the one in the docker-compose).

3. **Verify**:
   - Once deployed, your Vexa API will be available at `https://your-project-name.up.railway.app`.

---

## Phase 2: Make.com Setup (Calendar Sync)

1. **Create a New Scenario** in Make.com.
2. **Trigger**: Search for **Google Calendar** -> **Watch Events**.
   - Connect to `teamflc786@gmail.com`.
   - Set to watch your primary calendar.
3. **Filter**: Add a filter after the Google Calendar node.
   - **Condition**: `Location` or `Hangout Link` **contains** `meet.google.com`.
4. **Action**: Search for **HTTP** -> **Make a Request**.
   - **Method**: `POST`
   - **URL**: `https://your-project-name.up.railway.app/api/v1/bots`
   - **Headers**:
     - `Authorization`: `Bearer YOUR_ADMIN_API_TOKEN`
     - `Content-Type`: `application/json`
   - **Body Type**: `Raw`
   - **Content Type**: `JSON (application/json)`
   - **Request Content**:
     ```json
     {
       "meeting_url": "{{1.hangoutLink}}",
       "bot_name": "FLC Assistant",
       "transcription_enabled": true,
       "recording_enabled": true
     }
     ```
5. **Schedule**: Set the scenario to run every 5 or 15 minutes.

---

## Your Admin Token
The generated token for your instance is stored in `admin_token.txt`. Keep this secret as it allows full control over your Vexa bot.
