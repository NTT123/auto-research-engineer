---
name: paper-download
description: Download an arxiv paper (PDF, HTML, TeX source) into the project's ./paper/ directory. Use when you need to fetch a paper by arxiv ID or URL.
---

Download an arxiv paper into the project's `./paper/` directory.

## Steps

1. Extract the arxiv ID from whatever was provided.
   Strip any URL prefix — the ID is the `YYMM.NNNNN` part (or old-style `category/NNNNN`).

2. Create `./paper/` if it doesn't exist.

3. Download in this order:

   **HTML** (preferred for reading):
   ```bash
   curl -L -o ./paper/{id}.html "https://arxiv.org/html/{id}"
   ```
   Check if the result is a valid HTML page (not a 404 or redirect to abstract). Some older papers don't have HTML — that's fine, skip it.

   **PDF** (always download):
   ```bash
   curl -L -o ./paper/{id}.pdf "https://arxiv.org/pdf/{id}.pdf"
   ```

   **TeX source** (optional — download if the user specifically wants it or the PDF lacks implementation detail):
   ```bash
   curl -L -o ./paper/{id}.tar.gz "https://arxiv.org/src/{id}"
   mkdir -p ./paper/src && tar -xzf ./paper/{id}.tar.gz -C ./paper/src/
   ```

4. Report what was downloaded and where it's stored.
