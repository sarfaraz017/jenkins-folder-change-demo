# Jenkins Folder Change Pipeline Demo

This repo demonstrates a Jenkins pipeline that:
- Triggers only when files in `src/` change.
- Detects the latest changed file in `src/`.
- Processes that file in the pipeline.

## How it works
1. Jenkins job is configured with:
   - **Pipeline script from SCM**
   - **Included Regions** = `src/.*`
   - Webhook or Poll SCM trigger
2. When a commit modifies files under `src/`, Jenkins:
   - Finds the latest file changed in that folder
   - Exports it to `env.LATEST_FILE`
   - Runs processing steps on that file

## Quick Test
```bash
echo "hello" > src/test3.txt
git add src/test3.txt
git commit -m "Add test3.txt in src/"
git push origin main
