Configuration
=============

Dependencies
------------

Since the plug-in is written on top of the jQuery and jQuery UI libraries those are both a mandatory requirement for deployment in any Web-based environment.

Contents
--------

The ACCEPT |post| plug-in consists of the following files:

    * A core JavaScript file.
    * A core CSS file which contains all UI settings.
    * An :file:`images` folder.

.. todo:: Clarify the names of these files.

Initialisation
--------------


The ACCEPT |post| plug-in has the following initialisation options:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Field
     - Description 
   * - dialogHeight
     - Height for the dialog window where the plug-in will be triggered
   * - dialogWidth
     - Width for the dialog window where the plug-in will be triggered
   * - leftPaneWidth
     - Width for the left hand-side navigation pane.
   * - leftPaneHeight
     - Height for the left hand-side navigation pane.
   * - leftPaneFontSize
     - Font size for the text in the left hand-side navigation pane.
   * - textAreasWidth
     - Width for the right hand-side pane.
   * - textAreasHeight
     - Height for the right hand-side pane.
   * - imagesPath
     - URL path for all plug-in images
   * - acceptServerPath
     - URL path for the ACCEPT API
   * - textIdContainer
     - Attribute name that might contain the Post Editing text identifier
   * - userIdSelector
     - jQuery selector for the DOM element that contains the user identifier
   * - userIdContainer
     - Attribute name that might contain the Post Editing user identifier
   * - preEditApiKey
     - API key for the ACCEPT |pre| plug-in
   * - preEditImagesPath
     - URL path for the |pre| plug-in images
   * - preEditWith
     - Width for the |pre| plug-in dialog
   * - preEditHeight
     - Height for the |pre| plug-in dialog
   * - preEditingLanguageUI
     - UI language for the |pre| plug-in

.. todo:: Is this exhaustive?

.. _pe-client-example:

Example
-------

.. warning:: Since the |post| plug-in is currently not available for download, this example makes use of the ACCEPT Content Delivery Network (CDN) to make the |post| plug-in functionality available.

The following JavaScript and HTML snippets demonstrate how the |post| plug-in may be used to trigger on each item of a list:

.. code-block:: html
    :emphasize-lines: 11

    <head>

     <link href="|project-portal-url|/Plugin/PostEdit/v1.0/css/Accept.css" rel="stylesheet" type="text/css" />
     <link href="http://www.accept-portal.eu/Plugin/PostEdit/v1.0/css/PostEdit.css" rel="stylesheet" type="text/css" />
     <link href="http://www.accept-portal.eu/Plugin/PostEdit/v1.0/css/jquery-ui-1.8.17.customPost.css" rel="stylesheet" type="text/css" />

     <script  src="http://www.accept-portal.eu/Plugin/jquery-1.5.1.min.js" type="text/javascript"></script>
     <script  src="http://www.accept-portal.eu/Plugin/jquery-ui-1.8.24.custom.min.js" type="text/javascript"></script>
     <script  src="http://www.accept-portal.eu/Plugin/tiny_mce/tiny_mce.js" type="text/javascript"></script>
     <script  src="http://www.accept-portal.eu/Plugin/tiny_mce/jquery.tinymce.js" type="text/javascript"></script>
     <script  src="http://www.accept-portal.eu/Plugin/accept-jquery-plugin-1.1.js" type="text/javascript"></script>
     <script  src="http://www.accept-portal.eu/Plugin/PostEdit/v1.0/js/accept-postedit-jquery-plugin-1.0.js" type="text/javascript"></script>


    <script type="text/javascript">
     //waiting DOM to be loaded
     $(document).ready(function () {
        $('#app-list').contents().find(".left").AcceptPostEdit({
            textIdContainer: 'title',
            dialogHeight: 650,
            dialogWidth: 850,
            acceptServerPath: 'http://[accept_portal_server]/AcceptApi/Api/v[api_version]',
            userIdSelector: 'div.userContainer',
            userIdContainer: 'title',
            imagesPath: 'http://[accept_portal_server]/Plugin/PostEdit/v1.0/css/images',
        });
    });
    </script>

     </head>
     <body>

    <h3>List of tasks:</h3>
    <ul id="app-list">
    <li>
    <div class="container">
        <div class="left" title="unique_task_id_1" style="cursor:pointer;">
            <div class="userContainer" style="display:none" title="[user_name]"></div>
            <span style="font-size:xx-small;vertical-align:middle;font-size:large;">Click to edit: Task 1</span>
         </div>
        <div class="middle">  </div>
    </div>
    </li>
    <li>
    <div class="container">
        <div class="left" title="unique_task_id_2" style="cursor:pointer;">
            <div class="userContainer" style="display:none" title="[user_name]"></div>
            <span style="font-size:xx-small;vertical-align:middle;font-size:large;">Click to edit: Task 2</span>
        </div>
        <div class="middle">  </div>
    </div>
    </li>
    <li>
    <div class="container">
        <div class="left" title="unique_task_id_3" style="cursor:pointer;">
            <div class="userContainer" style="display:none" title="[user_name]"></div>
            <span style="font-size:xx-small;vertical-align:middle;font-size:large;">Click to edit: Task 3</span>
        </div>
        <div class="middle">  </div>
    </div>
    </li>
    </ul>
    </body>

where *[user_name]* is the (ACCEPT Portal) user name of the user that should be performing the post-editing task, *[accept_portal_server]* is the name or IP address of the server running the ACCEPT portal and *[api_version]* is the version of the API (current is |version|).

.. note:: In the HTML example, the line highlighted in yellow is only required if the |post| plug-in integrates the |pre| plug-in.

In the example above, a jQuery selector is used to locate a post-editing task container (HTML element):

.. code-block:: javascript

    $('#app-list').contents().find(".left").AcceptPostEdit({

The container is the *ul* element whose ID is *app-list*. The actual post-editing tasks are contained within DIV elements whose CSS class name is *left*. Each of these DIV elements also contains a unique ID, which can be found in the value of its *title* attribute (e.g. *unique_task_id_3*):

.. code-block:: javascript

    textIdContainer: 'title'

Finally, a jQuery Selector is provided to identify the HTML element that contains the unique *user* identifier:

.. code-block:: javascript

    userIdSelector: 'div.userContainer',
    userIdContainer: 'title',



.. warning:: If two or more users edit a task using the same user name, some edits are likely to be overwritten.
