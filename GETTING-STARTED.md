# How to publish this as a GitHub repo

1. Extract the ZIP on your computer.
2. On GitHub choose **New repository**, name it `aws-solutions-architect-associate-notes`, and keep it public only if your notes are safe to share.
3. **Option A — GitHub web UI:** use **Add file → Upload files** to upload the extracted repository contents (not the parent folder), then commit.
4. **Option B — local Git:** from the extracted repository directory:

```bash
git init
git add .
git commit -m "docs: initialize SAA-C03 learning portfolio"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/aws-solutions-architect-associate-notes.git
git push -u origin main
```

5. Open `README.md` in the GitHub interface and check navigation.
6. Edit `PROGRESS.md`, fill one topic at a time, and add verified sources.

**Important:** The ZIP contains **no** `.git` directory. No repository has been created or published on your behalf.
