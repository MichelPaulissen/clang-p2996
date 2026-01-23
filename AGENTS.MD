## Release binaries (GitHub Actions)
Goal: build clang-only Linux binaries and attach them to a GitHub release.

Workflow: `.github/workflows/release-binaries.yml` (customized for forks).

Steps:
1) Ensure workflow permissions are "Read and write" in GitHub repo settings.
2) Update `p2996` branch as needed and push.
3) Tag the release name (example `260122`) and push tag:
   - `git -C /home/michel/projects/lag/dependencies/clang-p2996 tag -f 260122`
   - `git -C /home/michel/projects/lag/dependencies/clang-p2996 push -f origin 260122`
4) Run the workflow:
   - `gh workflow run release-binaries.yml -R MichelPaulissen/clang-p2996 -r p2996 -F release-version=260122 -F upload=true -F runs-on=ubuntu-22.04`
5) Monitor:
   - `gh run list -R MichelPaulissen/clang-p2996 --workflow release-binaries.yml --limit 1`
   - `gh run view <RUN_ID> -R MichelPaulissen/clang-p2996 --log-failed`
6) Download and unpack the release:
   - `gh release download 260122 -R MichelPaulissen/clang-p2996 -p LLVM-260122-Linux-X64.tar.xz -D /home/michel/projects/lag/dependencies/clang-p2996/releases/260122`
   - `tar -xJf /home/michel/projects/lag/dependencies/clang-p2996/releases/260122/LLVM-260122-Linux-X64.tar.xz -C /home/michel/projects/lag/dependencies/clang-p2996/releases/260122`

Local binaries:
- Binaries live under `releases/<tag>/bin` and `releases/<tag>/lib/clang`.
- Do not commit binaries to git; use GitHub Releases.
- Helper script: `scripts/fetch_release.py` (downloads and unpacks a release).
