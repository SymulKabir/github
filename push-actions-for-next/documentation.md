#### 🧩 Step 1: Create workflow file
Inside your project, create this path and file:

```
.github/workflows/deploy.yml
```
---
#### ⚙️ Step 2: Add the workflow config
Here’s a minimal Next.js build + deploy workflow that runs whenever you push to `master` branch:
```
name: CI/CD for Next.js with Docker

on:
  push:
    branches: ["master"]

jobs:
  test-build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 21

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test --if-present

      - name: Build Next.js
        run: npm run build
  deploy:
    runs-on: ubuntu-latest
    needs: test-build
    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Deploy to server via SSH
        uses: appleboy/ssh-action@v0.1.10
        with:
          host: ${{ secrets.SERVER_IP }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.DEPLOY_KEY }}
          # port: 24
          script: | 
            cd /var/website/somachar/frontend
            if [ ! -d .git ]; then
              git clone git@github.com:SymulKabir/somachartv-next.js.git .
            else
              git pull origin master
            fi
          

            docker build -t somachar-frontend ./
            docker stop somachar-frontend || true
            docker rm somachar-frontend || true
            docker run -d -p 3001:3001 --name somachar-frontend somachar-frontend
            docker logs somachar-frontend
```
---

#### Setup all `secrets` and `SSH access` in Hosting Server for GitHub Actions

**1. Go to your GitHub `repo → Settings → Secrets and variables → Actions → Secrets`**

Click `New repository secret`:
- Name = SERVER_IP
- Secret = <your server IP>

Click `New repository secret`:
- Name = SERVER_USER
- Secret = <your server user>

Click `New repository secret`:
- Name = DEPLOY_KEY
- Secret = <your private SSH key>

**2. Login in server by SSH** (To collect `<your private SSH key>`)

On your server:

```bash

ssh-keygen -t rsa -b 4096 -C "github-deploy" -f ~/.ssh/github_deploy -N ""
cat ~/.ssh/github_deploy.pub >> ~/.ssh/authorized_keys

```

Now copy the private key:

```bash
cat ~/.ssh/github_deploy

```
Copy the output and use in github secret as `DEPLOY_KEY`

#### ✅ Step 4: Push your code
Now just push your code to the master branch:

```bash
git add .
git commit -m "Deploy update"
git push origin master
```
GitHub Actions will automatically start the workflow, build your Next.js app, and deploy it.
 
