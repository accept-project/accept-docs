Dependencies
============

The ACCEPT plug-in is written in JavaScript and uses the jQuery and jQuery UI libraries.
These libraries need to be included on any page using the ACCEPT plug-in.

The following versions of JQuery and jQueryUI have been tested:

.. list-table::
   :widths: 20 80
   :header-rows: 1

   * - Library
     - Version
   * - jQuery
     - All versions above v1.3.2
   * - jQueryUI
     - All versions above v1.7.3

Both are included as part of the download package, but you can always download the latest version of `jQuery <http://jquery.com/>`_ core library and the last version of `jQuery UI <http://jqueryui.com/>`_.

.. tip:: We recommend jQuery v1.3 or above due to the significant speed increases in several areas.

The jQuery UI library used should match the version of jQuery selected.

.. list-table::
   :widths: 20 80
   :header-rows: 1

   * - JQuery version
     - JQueryUI version
   * - jQuery v1.3.2 or above
     - jQuery UI version 1.8.21 (Stable Version)
   * - jQuery 1.3.2
     - jQuery UI for jQuery 1.3.2 (Stable Version)

The ACCEPT plug-in uses the ACCEPT |pre| API to perform the actual checks. For more information, refer to the :ref:`preEditApi` documentation.

.. note:: Always remember that in order to use the ACCEPT plug-in, JavaScript needs to be set to "Enabled" at the browser level.