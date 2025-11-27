# Wazo PBX Docker Stack

This is a Portainer-compatible Docker Compose stack for Wazo PBX, configured for:
- **Zadarma SIP Trunking**
- **Node.js AI Receptionist Integration** (via ARI)
- **Android SIP Extensions** (via Wazo UI)

## Prerequisites
- Docker & Docker Compose
- Portainer (optional, but recommended)
- Git

## Deployment

### Option 1: Portainer (Git Stack)
1.  In Portainer, go to **Stacks** > **Add stack**.
2.  Select **Repository**.
3.  Enter the URL of your GitHub repository containing these files.
4.  **Important**: Since `.env` is not in the repo (for security), you must manually set the Environment variables in Portainer:
    - `LOCAL_GIT_REPOS`: `.`
    - `SIP_USERNAME`: `04188`
    - `SIP_PASSWORD`: `kK3mMoBmZ0`
    - `SIP_DOMAIN`: `sip.zadarma.com`
    - **Recommended**: Clone this repo to your server manually, create a `.env` file from `.env.example`, and use **Local** stack in Portainer.

### Option 2: Manual Deployment
1.  Clone this repository:
    ```bash
    git clone <your-repo-url> wazo-pbx
    cd wazo-pbx
    ```
2.  Copy the example environment file:
    ```bash
    cp .env.example .env
    ```
3.  Edit `.env` and add your Zadarma credentials.
4.  Ensure submodules/dependencies are present.
5.  Run:
    ```bash
    docker compose up -d
    ```

## Configuration

### 1. Zadarma SIP Trunk
Your credentials are in `.env` for reference.
To configure the trunk in Wazo:
1.  Log in to **Wazo UI** (https://<your-ip>:8443). Default creds: `root` / `secret` (or check `variables.env`).
2.  Go to **Trunks** > **SIP Protocol**.
3.  Create a new trunk:
    - **Authentication Username**: `04188`
    - **Password**: `kK3mMoBmZ0`
    - **Server**: `sip.zadarma.com`
    - **Register**: Yes

### 2. Node.js AI Integration
Your Node.js app can connect to Asterisk ARI using:
- **URL**: `http://<wazo-ip>:5039/ari`
- **WebSocket**: `ws://<wazo-ip>:8088/ari/events`
- **Username**: `wazo-user`
- **Password**: `wazo-password` (Change this in `etc/asterisk-ari.conf`)

### 3. Android Extensions
1.  In Wazo UI, go to **Users**.
2.  Create a user.
3.  Add a **Line** (SIP).
4.  Use the generated credentials (Username/Password) in your Android SIP client (e.g., Zoiper, GS Wave).
