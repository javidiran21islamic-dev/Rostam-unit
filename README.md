# Rostam-unit
name: Extract ROSTAM UNIT project

on:
  workflow_dispatch:

permissions:
  contents: write

jobs:
  extract:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Extract project ZIP
        run: |
          ZIP_FILE=$(find . -maxdepth 2 -type f -iname "*.zip" | head -n 1)
          if [ -z "$ZIP_FILE" ]; then
            echo "Project ZIP not found"
            exit 1
          fi
          mkdir -p /tmp/rostam-unpack
          unzip -o "$ZIP_FILE" -d /tmp/rostam-unpack
          cp -a /tmp/rostam-unpack/rostam_next/. .
          rm -f "$ZIP_FILE"
          rm -rf /tmp/rostam-unpack

      - name: Save extracted files
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add -A
          git commit -m "Extract ROSTAM UNIT web project" || exit 0
          git push
