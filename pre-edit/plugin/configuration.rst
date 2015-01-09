Configuration
=============

================= ====== ======================
Name              Type   Value
================= ====== ======================
configurationType String Default: "contextMenu"
================= ====== ======================

Describes how the plug-in should behave. This parameter can only receive two values, *contextMenu* or *tinyMCEEmbedded*.

================= ====== ======================
Name              Type   Value
================= ====== ======================
AcceptServerPath  String Default: ""
================= ====== ======================

The URL for the ACCEPT API.

================= ====== ======================
Name              Type   Value
================= ====== ======================
Lang              String Default: "en"
================= ====== ======================

Language that will be used for the input text. Can be *fr* for French, *en* for English or *de* for German.

================= ====== ======================
Name              Type   Value
================= ====== ======================
imagesPath        String Default: "../css/images"
================= ====== ======================

The path to the directory that contains all the images used by the plug-in.

================= ====== ======================
Name              Type   Value
================= ====== ======================
tinyMceUrl        String Default: "extra/tiny_mce/tiny_mce.js"
================= ====== ======================

The path to the tiny MCE JavaScript file. This option is mandatory.

================= =================== ======================
Name              Type                Value
================= =================== ======================
LoadInputText     JavaScript Function Default: see code below
================= =================== ======================

.. code-block:: javascript

   var inputText = "";
   inputText = settings.requestFormat == 'TEXT' ? inputText = $("#" + acceptObjectId).val() : inputText = $("#" + acceptObjectId).html();
   return inputText;


Customize the way to load the input text. This parameter is consumed as a function, this means is expected that a function be passed, this function should return the text to check. Example:

.. code-block:: javascript

     function () {
        var inputText =  /* INPUT TEXT or HTML */
        return inputText;
    }

================= =================== ======================
Name              Type                Value
================= =================== ======================
SubmitInputText   JavaScript Function Default: see code below
================= =================== ======================

.. code-block:: javascript

     settings.requestFormat == 'TEXT' ? $('#' + acceptObjectId).val(text) : $('#' + acceptObjectId).html(text);

Customize the way the input text is submitted from the dialog box back into the text editor. This parameter is also consumed as a function, in this case the plug-in expects to pass the text as an input parameter. Example:

.. code-block:: javascript

    function (textParameter) { /* SEND THE TEXT OR HTML BACK TO THE TEXT INPUT AREA */ }

================= ====== ======================
Name              Type   Value
================= ====== ======================
languageUi        String Default: "en"
================= ====== ======================

Language to use for the UI labels.

Currently the following languages are supported:

  *  *en* = English
  *  *fr* = French
  *  *de* = German

================= ====== ======================
Name              Type   Value
================= ====== ======================
requestFormat     String Default: "TEXT"
================= ====== ======================

Format of the text input, which can be *TEXT* for text content or *HTML* for text containing markup language.

================= ====== ======================
Name              Type   Value
================= ====== ======================
Rule              String Default: The language-specific rule set.
================= ====== ======================

Optional rule set that should be used by the Acrolinx server to check the content. By default, a language-specific rule set will be used.

================= ============ ======================
Name              Type         Value
================= ============ ======================
checkingLevels    String Array Default: []
================= ============ ======================

Instead of defining the :make:var`Rule` setting above it is also possible to define multiple rule sets by using the :makevar:`checkingLevels` setting.
The rule names provided will then be interpreted as checking levels where the first rule name provided is used to perform the first
content check. Subsequent checks can then be triggered by clicking the remaining check level buttons.

================= ======== ======================
Name              Type     Value
================= ======== ======================
rightClickEnable  Boolean  Default: false
================= ======== ======================

Indicates whether the right-click context menu should be active.

================= ======= ======================
Name              Type    Value
================= ======= ======================
showFixAll        Boolean Default: false
================= ======= ======================

Indicates whether the :guilabel:`Replace All` button should be active.

================= ======= ======================
Name              Type    Value
================= ======= ======================
isModal           Boolean Default: true
================= ======= ======================

Indicates whether the dialog box that shows the results should behave as a modal.

================= ======= ======================
Name              Type    Value
================= ======= ======================
isModal           Boolean Default: true
================= ======= ======================

Indicates whether the dialog box that shows the results can be dragged.

================= ============= ======================
Name              Type   Value
================= ============= ======================
dialogHeight      Number/String Default: "auto"
================= ============= ======================

Height (in pixels) of the dialog box that shows the results. Alternatively the value can set to the string *auto*.

================= ============= ======================
Name              Type   Value
================= ============= ======================
dialogWidth       Number/String Default: "auto"
================= ============= ======================

Width (in pixels) of the dialog box that shows the results. Alternatively the value can set to the string *auto*.

==================== ============= ======================
Name                 Type          Value
==================== ============= ======================
placeHolderMaxHeight Number/String Default: $(window).height()
==================== ============= ======================

Maximum height (in pixels) of the dialog box that shows the results.

==================== ============= ======================
Name                 Type          Value
==================== ============= ======================
placeHolderMaxWidth  Number/String Default: $(window).width()
==================== ============= ======================

Maximum width (in pixels) of the dialog box that shows the results.

==================== ============= ======================
Name                 Type          Value
==================== ============= ======================
placeHolderMinHeight Number/String Default: 100
==================== ============= ======================

Minimum height (in pixels) of the dialog box that shows the results.

==================== ============= ======================
Name                 Type          Value
==================== ============= ======================
placeHolderMinWidth  Number/String Default: 380
==================== ============= ======================

Minimum width (in pixels) of the dialog box that shows the results.

==================== ============= ======================
Name                 Type          Value
==================== ============= ======================
showManualCheck      Boolean       Default: false
==================== ============= ======================

When set to *true* this button allows the user to manual re-check the content.


==================== ============= ======================
Name                 Type          Value
==================== ============= ======================
styleSheetPath       String        Default: "../css"
==================== ============= ======================

Path to the :file:`Accept.css` file. This path needs to be correctly set in order to inject the necessary styles in the tinyMCE iframe (rendered within the dialog).


==================== ============= ======================
Name                 Type          Value
==================== ============= ======================
htmlBlockElements    String        Default: "p ,h1 ,h2 ,h3 ,h4 ,h5 ,h6 ,ol ,ul ,li ,pre ,address ,blockquote ,dl ,div ,fieldset ,form ,hr ,noscript ,table"
==================== ============= ======================

List of block level HTML elements the plug-in should consider. A control node is added after each HTML element in this list to simulate a line break in the content to check. These nodes are removed before the content is applied back to the source placeholder.


===================== ============= ======================
Name                  Type          Value
===================== ============= ======================
refreshStatusAttempts Number        Default: 5
===================== ============= ======================

Number of attempts the plug-in will take to check if the response containing the results for the content sent to check are ready. If the attempts' value reaches the threshold limit, subsequent manual triggers might be needed by the user.


==================== ============= ======================
Name                 Type          Value
==================== ============= ======================
editorWidth          Number/String Default: "380px"
==================== ============= ======================

Initial width of the dialog inline editor.


==================== ============= ======================
Name                 Type          Value
==================== ============= ======================
editorHeight         Number/String Default: "80px"
==================== ============= ======================

Initial height of the dialog inline editor.


==================== =================== ===================================
Name                 Type                Value
==================== =================== ===================================
getSessionUser       JavaScript/Function Default: function () { return ""; }
==================== =================== ===================================

It is possible to configure the plug-in to search for end user information (login name, etc...) and attach that info as part of the metadata collected.
This can be achieved by writing the necessary code to search and then mandatorily return a string value representative of the desired information.


==================== ====================== ===================================
Name                 Type                   Value
==================== ====================== ===================================
injectSelector       jQuery Selector/String Default: null
==================== ====================== ===================================

When the plug-in is configured in the :ref:`externalCall` mode, this setting combined with the :makevar:`injectContent` setting is used to inject extra HTML code on the page.
The idea here is to trigger the content check from one element injected by the combination of these properties.
The value expected is a `jQuery selector <http://api.jquery.com/category/selectors/>`_,
the selector being used to find at least one existing DOM element where the new HTML code (provided via the :makevar:`injectContent` setting value) is injected.


==================== =================== ===================================
Name                 Type                Value
==================== =================== ===================================
injectContent         HTML/String              Default: null
==================== =================== ===================================

The HTML content that is injected in the element(s) found by the jQuery selector provided in the setting :makevar:`injectSelector`.


==================== =================== ===================================
Name                 Type                Value
==================== =================== ===================================
injectWaitingPeriod  Number              Default: 100
==================== =================== ===================================

Value in milliseconds the plug-in should wait to inject the HTML content from the :makevar:`injectContent` setting into the DOM elements matched in the jQuery selector provided by the :makevar:`injectSelector` setting.


==================== ====================== ===================================
Name                 Type                   Value
==================== ====================== ===================================
triggerCheckSelector jQuery Selector/String Default: null
==================== ====================== ===================================

Expects a jQuery selector. This value is used to find the DOM element from where a mouse click will trigger the content check.


==================== ====================== ===================================
Name                 Type                   Value
==================== ====================== ===================================
timeoutWaitingTime   Number                 Default: 7000
==================== ====================== ===================================

Value in milliseconds that Ajax requests will take before fall into a timeout exception.

