.. _preEditAll:

Pre-Edit
========

.. only:: full

    Portal
    ------

    .. warning:: The Pre-Edit demo section of the ACCEPT Portal can only be used to check text that is manually entered by a user (either by typing text or copying/pasting). This section does not allow you to:

     * configure a language checking service provider
     * create projects
     * upload files to check
     * download modified files


    .. toctree::
       :maxdepth: 2

       portal/api_keys
       portal/demo

.. only:: pre_edit_plugin or full

    .. _preEditPlugin:

    Plug-in
    -------

    .. toctree::
       :maxdepth: 2

       plugin/introduction
       plugin/target_audience
       plugin/dependencies
       plugin/supported_browsers
       plugin/downloading
       plugin/installation
       plugin/configuration
       plugin/theme
       plugin/examples

.. only:: full

    .. _preEditApi:

    API
    ---

    .. note:: The ACCEPT |pre| API is used by any instance of the |pre| plug-in (including the demo section of the Portal). This API currently only supports the `Acrolinx <http://infocenter.acrolinx.com/dev/index.jsp>`_ language checking technology, but other technologies may be added in the future (e.g. to cover additional languages). This API, which can be also leveraged by custom tools, can be easily described as a piece of software built as a wrapper over the Acrolinx services. The ACCEPT |pre| API combines the main features available within the Acrolinx services in an independent REST (Representational State Transfer) API.

    The following functionality is available via the API:

    .. toctree::
       :maxdepth: 2

       api/check
       api/usage