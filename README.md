accept-doc
==========

This repository contains the source files (in [reStructuredText format](http://docutils.sourceforge.net/rst.html)) of the reference documentation for the ACCEPT framework's components, including:

* [Portal (Web application)](https://github.com/accept-project/accept-portal)
* [API](https://github.com/accept-project/accept-api)
* [Pre-edit client](https://github.com/accept-project/accept-pre-edit)
* [Post-edit client](https://github.com/accept-project/accept-post-edit)

The relationships between these components are shown in the following architecture diagram:

![Architecture](https://raw.githubusercontent.com/accept-project/accept-docs/master/_static/ACCEPT_Architecture.jpg)

Two documentation sets can be built: a full set and a set pertaining solely to the standard pre-edit client. Building is achived using [Sphinx,](http://sphinx-doc.org/index.html) and by default the [ReadTheDocs theme](https://github.com/snide/sphinx_rtd_theme). Both can be installed using Python and  [pip](https://pypi.python.org/pypi/pip):

```bash
    $ pip install sphinx
    $ pip install sphinx_rtd_theme
```

Before building the documentation, you should edit the `conf.py` file to use relevant configuration values (under the line starting with `TODO`). Examples of values to edit include:

```python
    portal_base_url = '' # Base URL (e.g. 'https://myexample.com')
    portal_instance_type = '' # Web application root (e.g. 'Portal' or 'PortalStaging')
```

The following are examples of build commands for the full set (multiple HTML pages) and pre-edit documentation set (single HTML page) respectively:

```bash
    $ sphinx-build -t full . _build
    $ sphinx-build -b singlehtml -t pre_edit_plugin -D html_title="The ACCEPT Pre-Edit plug-in" -D master_doc=index_pre_edit_plugin . _builds-pre-
```



