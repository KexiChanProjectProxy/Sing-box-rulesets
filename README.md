# Sing-box-rulesets

This repository contains JSON rule-set sources for [sing-box](https://sing-box.sagernet.org/).

## GitHub Actions workflow

Every push to `main` or pull request that updates files in the `list/` directory triggers the
[Compile sing-box rule sets](.github/workflows/compile-rule-sets.yml) workflow. The pipeline:

1. Installs the latest `sing-box` binary using the official installation script.
2. Compiles each JSON rule-set in `list/` into a corresponding `.srs` file inside the repository's `ruleset/` directory, preserving any sub-directory structure.
3. Commits and pushes the updated `ruleset/` directory back to the repository on `main`.

You can also run the compile step locally with:

```bash
curl -fsSL https://sing-box.app/install.sh | sudo bash
mkdir -p ruleset
find list -type f -name '*.json' -print0 | while IFS= read -r -d '' file; do
  rel="${file#list/}"
  output="ruleset/${rel%.json}.srs"
  mkdir -p "$(dirname "${output}")"
  sing-box rule-set compile --output "${output}" "${file}"
done
```
#