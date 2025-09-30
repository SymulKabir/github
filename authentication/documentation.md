### Github Authentication
Github Authentication with any pc to clone and and push any privet or public repository. 


#### Step 1 — Generate the SSH key
```bash
ssh-keygen -t ed25519 -C "server@mydomain" -f ~/.ssh/github-auth-key -N ""
```
- This creates:
  - Private key: `~/.ssh/github-auth-key`
  - Public key: `~/.ssh/github-auth-key.pub`

#### Step 2 — Copy the public key
```bash
cat ~/.ssh/github-auth-key.pub
```
Copy the full output.

#### Step 3 — Add the key to GitHub
- Go to **GitHub → Settings → SSH and GPG keys → New SSH key**
- **Title:** Something like `Server Key`
- **Key:** Paste the copied public key
- Click **Add SSH key**

#### Step 4 — Test SSH connection
```bash
ssh -i ~/.ssh/github-auth-key -T git@github.com
```
You should see:
```bash
Hi <username>! You've successfully authenticated...
```
If yes → key works.

#### Step 5 — Make it easy for future commands
- Start the SSH agent:

```bash
eval "$(ssh-agent -s)"
```
- Add your key to the agent:
```bash
ssh-add ~/.ssh/github-auth-key
```
Now `git` will automatically use this key.
- Clone normally:
```bash
git clone git@github.com:OWNER/PRIVATE-REPO.git
```
