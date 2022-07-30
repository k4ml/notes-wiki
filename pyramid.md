# Pyramid

### Running pshell with custom shell

`pshell etc/development.ini --list-shells` only list `python` even though ipython has been installed. Looking at the [source code](https://github.com/Pylons/pyramid/blob/2.0-branch/src/pyramid/scripts/pshell.py#L232), it actually looking for entry-point `pyramid.pshell_runner`. Looking for `pyramid.pshell_runner` lead us to [pyramid-ipython](https://pypi.org/project/pyramid\_ipython/) on PyPI.
