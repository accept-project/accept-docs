Adding a task to a project
==========================

* Log in to the ACCEPT portal and click the :guilabel:`PostEdit` tab.
* Click the project name link or the :guilabel:`Post-Edit` link button in the :guilabel:`Actions` column to navigate to the project's detail page.
* In the project's detail page click :guilabel:`Choose File` button to upload a file from your local machine. Keep in mind that the file must follow a specific :ref:`json_format` and it be UTF-8 encoded.
* After choosing a file, click the :guilabel:`Upload Task` button to upload your task to the system.
* If the task was successfully uploaded, it should be listed in the tasks table. Otherwise you will get an error message.


.. _json_format:

JSON format
-----------

To upload a post-editing task, the `JSON <http://json.org/>`_ file must contain the following data:

* **text_id**: string corresponding to a unique string identifier (e.g. hash value of the source text).
* **src_sentences**: Array of objects, which are key/value pairs of sentences, in the form: **text**: *sentence* (where *sentence* is a string)
* **tgt_sentences**: Array of objects, each containing a key/value sentence pair, in the form: **text**: *sentence* (where *sentence* is a string). Each object may also contain an optional **options** pair, in the form: **options**: *option_array* (where *option_array* is an array of objects). Each object in the *option_array* should contain a pair of tokens (in the form **context**: *token*, where *token* corresponds to a substring from **sentence**) and a **values** pair which should contain an array of objects. Each of these objects contains an alternative phrase pair, in the form **phrase**: *token*.

.. note:: You will not be able to upload a file with a **text_id** value that is already in use in the project. The number of objects in **src_sentences** must be equal to the number of objects in **tgt_sentences**.

Additionaly, the JSON file may also contain the following metadata:

 * **MtContactEmail**: String representing the email address of the person who generated the translation output
 * **MtDate**: String representing a date in `UTC <http://www.w3.org/TR/1998/NOTE-datetime-19980827>`_ format
 * **MtTool**: String representing the name of the MT system used
 * **MtToolId**: String representing the ID or version of the MT system used
 * **SourceLanguage**: String representing the language code of the source language based on `RFC 4646 <http://www.ietf.org/rfc/rfc4646.txt>`_
 * **TargetLanguage**: String representing the language code of the target language based on `RFC 4646 <http://www.ietf.org/rfc/rfc4646.txt>`_
 * **Category**: String representing the type of text category (e.g. *IT*, *Health*)
 * **DataType**: String representing the input file format (e.g. “text”)
 * **ProductName**: String representing the name of the product described by the text

.. warning:: There is currently no verification on language codes and the **Category**, **DataType** and **ProductName** information is currently not used in any way by the Portal.

Finally, the JSON file may also contain an optional **tgt_templates** array to influence the display of the target sentences in the post-editing client. Templates were developed to define the layout of
the post-editing tasks and how they are displayed in the editor. Standard HTML **DIV** elements may be included within the **tgt_templates** array and each **DIV** element may contain any CSS style information
within or around it. Each **DIV** corresponds to one segment in the editor, which means that the display of a paragraph can be defined individually. Each **DIV** element must include a *@TARGET@* sequence, which defines the position of the respective target sentence in the target text. The number of segments in **tgt_templates** needs to match the number of segments in the **src_sentences** and **tgt_sentences** arrays
in order for the templates to be mapped correctly to the segments to be edited. To build a paragraph, for example, the sequence *style="display:inline"* can be included in front of the placeholder for the actual segment. Special characters must be `encoded <http://www.w3schools.com/tags/ref_urlencode.asp>`_ (e.g. **<** as **%3C**).


Examples
--------

Simple:

.. literalinclude:: json_example.json
   :language: javascript

With translation options:

.. literalinclude:: options_example.json
   :language: javascript

.. warning:: The order of the elements in the **options** array matters since overlapping substrings will be ignored. In the example above, both *translation* and *automatic* will be ignored because the **context** *translation automatic* appears first in the array.

With target templates:

.. literalinclude:: markup_example.json
   :language: javascript