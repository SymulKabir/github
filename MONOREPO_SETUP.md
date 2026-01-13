## Github monorepo setup
---

#### Create a New Empty Monorepo

```bash
mkdir monorepo
cd monorepo
```
#### Initialize git

```bash
git init
```
#### So create the folder:

```bash
mkdir apps
```


#### Typical monorepo structure:

```markup
monorepo/
   apps/
      projectA/
      projectB/ 
```
#### For each existing repository, run:

```bash
git submodule add <REPO_URL> apps/projectA
git submodule add <REPO_URL> apps/projectB
```
Example:

```bash
git submodule add https://github.com/user/projectA.git apps/projectA
```

#### Commit the Submodule Setup

```bash
git add .
git commit -m "Added 5 projects as submodules"
```

#### How to Update All Submodules
To fetch latest code from all repos:

```bash
git submodule update --remote --merge
```

#### Update one repo

```bash
cd apps/projectA
git pull origin main
```
Switch branch inside submodule

```bash
cd apps/projectA
git checkout dev
```

#### Clone monorepo with submodules later
```bash
git clone <monorepo_url> --recurse-submodules
```










