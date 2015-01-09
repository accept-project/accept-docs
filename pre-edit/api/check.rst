Check
=====

.. _AcceptRequest:

AcceptRequest
-------------

To submit text for grammar corrections, spelling errors or style suggestions, a POST request can be issued with extra parameters to:

.. code-block:: none

   http://{[accept_portal_server]}/AcceptApi/Api/v{[api_version]}/AcceptRequest

where *[accept_portal_server]* is the name or IP address of the server running the ACCEPT portal and *[api_version]* is the version of the API (current is |version|).

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Parameter
     - Description
   * - Text
     - The input text to be checked.
   * - ApiKey
     - The caller’s API key (public key).
   * - Language
     - Language of the input text, it should be *en* for English and *fr* for French.
   * - RequestFormat
     - Format of the input content, it should be ‘Html’ for content containing HTML mark-up  or ‘Text’ for simple text.
   * - Rule
     - Optional rule set that should be used by the Acrolinx server to check the content. By default, a language-specific rule set will be used.
   * - Grammar
     - Set to *1* if grammar suggestions should be highlighted, and set to *0* if grammar should be ignored.
   * - Spell
     - Set to *1* if spelling mistakes should be highlighted, and set to *0* if the spelling errors should be ignored.
   * - Style
     - Set to *1* if text style should be checked and set to *0* if text style should be ignored.


.. todo:: Are we missing any parameter?

The response is returned in a JSON object and contains the following information:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Field
     - Description
   * - AcceptSessionCode
     - Each request is identified by a session code.
   * - ResponseStatus
     - Status of the Http request:
        *Ok*: The status of the request is Http 200.
        *Failed*: The status of the request is Http 500.
   * - State
     - State of the response associated to the session ID:
        *Done*: The response is ready.
        *Waiting* The response is still being built
        *Failed*: The response built failed.
   * - TimeStamp
     - Date and time (UTC) when the response was built.

.. todo:: Add processing to State

.. _GetResponseStatus:

GetResponseStatus
-----------------

To request the status of an API call, issue a GET request with a :makevar:`session` parameter to:

.. code-block:: none

   http://{[accept_portal_server]}/AcceptApi/Api/v{[api_version]}/GetResponseStatus

where *[accept_portal_server]* is the name or IP address of the server running the ACCEPT portal and *[api_version]* is the version of the API (current is |version|). The value of the :makevar:`session` parameter should correspond to an *AcceptSessionCode* value returned by an :ref:`AcceptRequest` request.

The response is returned in a JSON object and contains the following information:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Field
     - Description
   * - AcceptSessionCode
     - Each request is identified by a session code.
   * - ResponseStatus
     - Status of the Http request:
        Ok –  The status of the request is Http 200.
        Failed – The status of the request is Http 500.
   * - State
     - State of the response associated to the session ID:
        Done – The response is ready.
        Waiting – The response is still being built
        Failed – The response built failed.
   * - TimeStamp
     - Date and time (UTC) when the response was built.


.. _GetResponse:

GetResponse
-----------

To request a response, issue a GET request with a :makevar:`session` parameter to:

.. code-block:: none

   http://{[accept_portal_server]}/AcceptApi/Api/v{[api_version]}/GetResponse


where *[accept_portal_server]* is the name or IP address of the server running the ACCEPT portal and *[api_version]* is the version of the API (current is |version|). The value of the :makevar:`session` parameter should correspond to an *AcceptSessionCode* value returned by an :ref:`AcceptRequest` request.

The response is returned in a JSON object and contains the following information:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Field
     - Description
   * - session
     - The session code generated to identify the API request.
   * - responseStatus
     - Indicates whether or not the call was successfully completed.
   * - timeStamp
     - Date and time (UTC) when the Http response was built.
   * - response
     - The core of the API response, it may contain sets of result sets.
   * - resultset
     - The result set contains a collection of results that belong to the same context, for example, collection of results from Acrolinx API: grammar, spelling or styles suggestions.
   * - result
     - A result corresponds to an item from a result set, it can be for example a spelling error found in the text. Each result is then constituted by a header and a body, the header contains metadata about the result and the body contains the result details.
   * - header
     - The header of the result item contains all meta information. Each header may contain:
        Type – type of the flagged rule (grammar, spelling, style).
        Description – short information about a flagged rule.
        Rule – Name of the flagged rule.
        Contexttype – Can have the value 1 or 2 depending on the content flagged. If the context is referring to a single word its value should be 1, if the flagged content is composed of more than one word the value is 2.
   * - body
     - The details of a result item is currently composed of:
        Context – the piece of text flagged by the rule.
        Startpos – start index or position in the text where the matched context was flagged.
        Endpos – end index or position in the text where the matched content was flagged.
        Contextpieces – when there is more than one word in the flagged context the context piece can have more than one item. Each piece of the context has a start position and one end position.
        Suggestions – Collection of the suggestions returned.