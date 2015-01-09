Usage
=====

Logging usage information
-------------------------

.. _AuditAction:

AuditAction
^^^^^^^^^^^

This method can be used to log user actions and interactions with the API results, such as ignored rules or time spent using the API.  To log a user action, a POST call can be issued to:

.. code-block:: none

   http://{[accept_portal_server]}/AcceptApi/Api/v{[api_version]}/AuditAction

where *[accept_portal_server]* is the name or IP address of the server running the ACCEPT portal and *[api_version]* is the version of the API (current is |version|).

This method expects the following parameters:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Field
     - Description
   * - SessionCodeId
     - The session identifier.
   * - AuditTypeId
     - The type of Audit that is being logged:
        1. Acrolinx Request: The body of the Acrolinx API request.
        2. Acrolinx Raw Result: the body of the Acrolinx API (raw) result.
        3. Acrolinx Response: the body of the Acrolinx API (JSON) response.
        4. Accept Response: The body of the Accept API response.
        5. Acrolinx Response Status: the (JSON) response body of the Acrolinx API response status.
        6. Accept Response Status: the body of the Accept API Response Status.
        7. Accept Rule Used: rule or suggestion used by user.
        8. Accept Rule Ignored: rule or suggestion Ignored by the user.
        9. Original And Final Text:	initial and  final text.
   * - RuleName
     - Name of rule in the audit context (if there is one).
   * - AuditContext
     - Piece of text in the context.
   * - TextBefore
     - Piece of text immediately before the text in context, set to 40 characters by default.
   * - TextAfter
     - Piece of text immediately after the text in context, set to 40 characters by default.
   * - StartTime
     - Timestamp when the action was initiated.
   * - EndTime
     - Timestamp when the action was finalized.

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
        Request Ok: The status of the request is HTTP 200.
        Request Failed: The status of the request is HTTP 500.
   * - TimeStamp
     - Date and time (UTC) when the response was built.


Retrieving usage information
----------------------------

There are two GET methods to retrieve some usage information associated with |pre| instances. These methods are :makevar:`SimpleGlobalSessionDomain` and :makevar:`GlobalSessionDomain`.

A global session corresponds to what happens between the time when a |pre| client is opened and when it is closed by a user. A global session may have more than one child session (e.g. auto-check).

Global session information can be retrieved from a given date up until the present day for a given |pre| client instance using the following request (which will return a response in JSON format):

.. code-block:: none

   http://{[accept_portal_server]}/AcceptApi/Api/v{[api_version]}/Core/SimpleGlobalSessionDomain/?id={[instance_id]}&start={[date]}&end={[date]}&userKey={[user_key]}


where *[accept_portal_server]* is the name or IP address of the server running the ACCEPT portal and *[api_version]* is the version of the API (current is |version|). *[instance_id]*, *[date]* and *[user_key]* correspond to values for the following parameters:

 * id: API key used by one or more client instance
 * start: starting date for range in `UTC <http://www.w3.org/TR/1998/NOTE-datetime-19980827>`_ format
 * end: end date for range in `UTC <http://www.w3.org/TR/1998/NOTE-datetime-19980827>`_ format
 * userKey: key of user who created the client instance

.. note:: This request may return usage data for multiples languages (e.g. *EN*, *FR* or *DE*) if the API key is shared by multiple client instances. To obtain specific usage data (say, for a given language), you may need to filter on rule set names (rather than language information which is unfortunately not reliable).

If you prefer to export the data in CSV format, the *format* parameter may be used as follows:

.. code-block:: none

   http://{[accept_portal_server]}/AcceptApi/Api/v{[api_version]}/Core/SimpleGlobalSessionDomain/?id={[instance_id]}&start={[date]}&end={[date]}&userKey={[user_key]}&format=csv

If you prefer to export the data in Excel format, the *format* parameter may be used as follows:

.. code-block:: none

   http://{[accept_portal_server]}/AcceptApi/Api/v{[api_version]}/Core/SimpleGlobalSessionDomain/?id={[instance_id]}&start={[date]}&end={[date]}&userKey={[user_key]}&format=excel

.. note:: The :makevar:`SimpleGlobalSessionDomain` method only generates global session data. To get the data including child sessions info, the :makevar:`GlobalSessionDomain` may be used as follows:

.. code-block:: none

    http://{[accept_portal_server]}/AcceptApi/Api/v{[api_version]}/Core/GlobalSessionDomain/?id={[instance_id]}&start={[date]}&end={[date]}&userKey={[user_key]}

The :makevar:`GlobalSessionDomain` method does not accept any *format* parameter, its only response format being a JSON format.

Response
^^^^^^^^

A request made against the :makevar:`GlobalSessionDomain` method returns a response in JSON format. A response example is shown below.

.. literalinclude:: global_session_domain_response.json
   :language: javascript
   :linenos:
   :emphasize-lines: 6,8,13-14,17-19,23-29,35

When a request is successful, the **ResponseObject** value of the response contains the following information:

  * When the checking session started, as indicated by the **GlobalStartTime** value
  * When the checking session ended, as indicated by the **GlobalEndTime** value
  * The original text sent for checking, as indicated by the **Input** value
  * The final text at the end of checking sessions, as indicated by the **Output** value
  * The language configuration used by the language checking provider, as indicated by the **Language** value in **Metadata**
  * The rule set used by the language checking provider, as indicated by the **RuleSet** value in **Metadata**
  * A unique anonymised value of the user that triggered the checking session, as indicated by the **User** value in **Metadata**
  * An array of **ChildSessions** containing precise information about at least one specific check performed on the text present in the **Context** value. Three sets of results are available for each check:
    * **ProviderResults**, which correspond to the detailed output generated by the language checking provider
    * **ClientResults**, which correspond to actions performed in the :guilabel:`Settings` window, as explained below.
    * **Results**, which correspond to actions performed in the main window, as explained below.


.. literalinclude:: global_session_domain_response2.json
   :language: javascript
   :lines: 7-20,23-80
   :emphasize-lines: 3,5-6,9,12,25,27-30,33,35-36,39,50,55,66-68
..   :linenos:

In the example above, the *Results* section of the report allows us to understand that the user (among other things):
  *  Decided to learn the 6-letter word *distro*, which had been flagged between indexes 294 and 300 (but not including index 300) as a spelling error
  *  Decided to ignore the *sentence_too_long* rule, based on an instance starting at index 86
  *  Displayed the recommendation tooltip for the *wrong_sequence_of_words* rule at index 0 but decided not to ignore the rule
  *  Decided to accept the *test* suggestion for the mispelt word *testt* at index 40.

In the example above, the *ClientResults* section of the report allows us to understand that the user:
  * Removed the word *structred* that they had previously learnt
  * Removed the *use_comma_after_introductory_phrase* rule that they had previously decided to ignore.







