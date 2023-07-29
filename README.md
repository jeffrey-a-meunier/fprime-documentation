# FPrime documentation

How to compile and view the documentation:

1. Install Sphinx. https://www.sphinx-doc.org/en/master/usage/installation.html
2. Fetch this repo.
3. Open a terminal window in the top-level of this directory tree.
4. Execute the command `make html`.
5. Use Python to serve the web pages: `python3 -m http.server -d _build/html/ 8000`
6. Navigate to http://localhost:8000
