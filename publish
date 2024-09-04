#! /bin/bash

# --- begin runfiles.bash initialization v3 ---
# Copy-pasted from the Bazel Bash runfiles library v3.
set -uo pipefail; set +e; f=bazel_tools/tools/bash/runfiles/runfiles.bash
# shellcheck disable=SC1090
source "${RUNFILES_DIR:-/dev/null}/$f" 2>/dev/null || \
source "$(grep -sm1 "^$f " "${RUNFILES_MANIFEST_FILE:-/dev/null}" | cut -f2- -d' ')" 2>/dev/null || \
source "$0.runfiles/$f" 2>/dev/null || \
source "$(grep -sm1 "^$f " "$0.runfiles_manifest" | cut -f2- -d' ')" 2>/dev/null || \
source "$(grep -sm1 "^$f " "$0.exe.runfiles_manifest" | cut -f2- -d' ')" 2>/dev/null || \
{ echo>&2 "ERROR: cannot find $f"; exit 1; }; f=; set -e
# --- end runfiles.bash initialization v3 ---

runfiles_export_envvars

TARGET=$1

if [ "$TARGET" == "" ]; then
  echo "Usage: $0 (prod|dev)"
  exit 1
fi

if [ "$TARGET" == "prod" ]; then
    remote="https://github.com/elefant-ai/docs-prod.git"
fi

if [ "$TARGET" == "dev" ]; then
    remote="https://github.com/elefant-ai/docs-dev.git"
fi

docs_src=$(rlocation "_main/docs/public")

echo "Publishing docs to $remote"
temp_dir=$(mktemp -d)
echo "temp_dir: $temp_dir"

cp -r -L $docs_src/* $temp_dir
cd $temp_dir
git init
git add .
git commit -m "Publishing docs"
git remote add origin $remote
git push -f origin master
