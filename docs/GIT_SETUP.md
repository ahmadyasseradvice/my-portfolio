# Git and GitHub setup

```powershell
cd C:\Users\ahmad\.gemini\antigravity\scratch\ahmad-portfolio
git init -b main
git config user.name "Ahmad Yasser Morsy"
git config user.email "ahmadyasseradvice@gmail.com"

# Access your GitHub repos (pick one)
gh auth login                      # GitHub CLI (recommended): browser login
# or SSH: ssh-keygen -t ed25519 -C "ahmadyasseradvice@gmail.com"
#         then add ~/.ssh/id_ed25519.pub at github.com/settings/keys

gh repo create ahmad-portfolio --private --source . --remote origin
# or: git remote add origin https://github.com/<user>/ahmad-portfolio.git

git add . && git commit -m "chore: initial structure"
git push -u origin main
```

- Never commit `.env.local` (it is in `.gitignore`).
- Branching: work in `feat/*` branches and open PRs. Vercel gives a preview URL for each PR.
- Deploy: vercel.com → Import the GitHub repo → add the env vars → every push to `main` deploys.
- To list your existing repos once logged in, run `gh repo list`.
