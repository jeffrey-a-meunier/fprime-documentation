# FPrime documentation

How to compile and view the documentation:

#. Install Sphinx. https://www.sphinx-doc.org/en/master/usage/installation.html
#. Fetch this repo.
#. Open a terminal window in the top-level of this directory tree.
#. Execute the command `make html`.
#. Use Python to serve the web pages: `python3 -m http.server -d _build/html/ 8000`
#. Navigate to http://localhost:8000
