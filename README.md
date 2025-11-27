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
4.  **Important**: Ensure `LOCAL_GIT_REPOS` is set in the Environment variables section of the stack, or use the `.env` file.
    - If deploying from Git, Portainer might not handle relative paths in `volumes` correctly if the submodules aren't checked out.
    - **Recommended**: Clone this repo to your server manually, then use **Local** stack in Portainer pointing to the `docker-compose.yml`.

### Option 2: Manual Deployment
1.  Clone this repository:
    ```bash
    git clone <your-repo-url> wazo-pbx
    cd wazo-pbx
    ```
2.  Ensure submodules/dependencies are present (the `wazo-auth-keys` and `xivo-config` directories).
3.  Run:
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
