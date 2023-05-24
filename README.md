# Documentation
This contains the new Sphinx documentation for Discover.


### Making changes
Any new docs that you make should be placed in the `docs/source` directory, and should be connected to the toctree in `index.rst`. Connections can either be direct, this happens by adding the filename without the extension to the toctree, or can be indirect. To do this the new files must be within the toctree of a file that is within the toctree of `index.rst`. Documentation can be added in either markdown or reStructuredText. Additionally, documentation can be generated from python modules, if and only if, the modules contain [numpy docstrings](https://numpydoc.readthedocs.io/en/latest/format.html).

### Rebuilding
There should already be docs built in the `docs/build/html` directory.
To rebuild the docs after making changes, run the following:
```
cd docs && make html
```

### Viewing
Once the docs have been built, you should find the updated/new docs in `docs/build/html`
