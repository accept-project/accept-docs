.. _evaluationAPI:

Evaluation API
==============

Several methods have been created with a view to provide project administrators with as much flexibility as possible.

.. _questionsMethod:

Questions method
----------------

This is a GET method. The :makevar:`Questions` method will return a list of project-specific questions. These questions can be managed (added etc.) via the :guilabel:`Evaluation Projects` page. The URL for calling the Questions method is:

.. code-block:: none

   http://{[accept_portal_server]}/Api/v{[api_version]}/Evaluation/Questions/{[ID]}

where *[accept_portal_server]* is the ACCEPT portal server you want to target, *[api_version]* is the version of the API (current is |version|), and *[ID]* is the Evaluation project ID. For an evaluation project with an ID of *1* the call would be:

.. code-block:: none

   http://{[accept_portal_server]}/Api/v{[api_version]}/Evaluation/Questions/1


The following parameters can be passed on the URL:

.. list-table::
   :widths: 30 50 20
   :header-rows: 1

   * - Parameters
     - Details
     - Example
   * - key
     - This is a MANDATORY parameter. If this parameter is not passed then the API call will fail. This value can be found on the :guilabel:`Project` page.
     - key=21fdc25bebdf456db5c9e0993977bb12
   * - language
     - This is an OPTIONAL parameter. If this parameter is passed then only the questions in the specified language will be returned. Language code is based on RFC 4646. Examples of language codes are: *en*, *fr*, *en-us*, *fr-fr*
     - language=en_us
   * - category
     - This is an OPTIONAL parameter. If this parameter is passed then only the questions in the specified category will be returned.
     - category=1
   * - question
     - This is an OPTIONAL parameter. If this parameter is passed then only this question will be returned.
     - question=1

If no parameters (apart from Key) are passed then ALL questions will be returned. An example of an API call containing all parameters is:

.. code-block:: none

   http://{[accept_portal_server]}/Api/v{[api_version]}/Evaluation/Questions/1?key=21fdc25bebdf456db5c9e0993977bb12&language=en_us&category=1&question=1

where *[accept_portal_server]* is the ACCEPT portal server you want to target, *[api_version]* is the version of the API (current is |version|).


A JSON structure is returned (the example below has one question returned):

.. code-block:: javascript

    {
	"ResponseObject":[
		{
			"Name":"QualityQuestion",
			"LanguageQuestions":[
				{
					"Question":"What is your name?",
					"Action":"Submit",
					"Confirmation":”Thank You”,
					"Answers":[
						{
							"Name":"Rob",
							"Count":7,
							"Id":1
						},
						{
							"Name":"John",
							"Count":5,
							"Id":2
						}
					],
					"Language":
					{
						"Name":"English",
						"Code":"en_us",
						"Id":1
					},
					"Count":12,
					"Id":1
				}
			],
			"Count":0,
			"Id":1
		}
	],
	"ResponseStatus":"OK",
	"TimeStamp":"\/Date(1348482649898)\/",
    }

The response structure contains the following fields and values:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Field
     - Value
   * - ResponseStatus
     - Status of the API call. Possible options are:
       *OK* = Successful call.
       *FAILED*	= API call failed.
   * - ResponseObject
     - This is an array of question categories associated with the project. See below for more information on the structure. This will only be returned if :makevar:`ResponseStatus` is *OK*.
   * - TimeStamp
     - Datetime of API response.
   * - Exception
     - If the ResponseStatus is equal to *FAILED* then the :makevar:`Exception` item will also be returned. This will contain an explanation of why the call failed.
   * - Context
     - If the :makevar:`ResponseStatus` is equal to *FAILED* then the :makevar:`Context` item will also be returned. This will give some context as to where the call failed.


The :makevar:`ResponseObject` field contains an array of question categories.

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Field
     - Value
   * - Id
     - This is the *Id* of the Question Category.
   * - Name
     - The name of the category. This will only be returned if ResponseStatus is *OK*.
   * - LanguageQuestions
     - This is an array of questions associated with the Category. See below for more information on the structure. This will only be returned if :makevar:`ResponseStatus` is *OK*.
   * - Count
     - This is the current count for the number of times a :makevar:`Score` has been submitted for this Question Category. Note: The Count could be used to decide WHAT question to ask.

The :makevar:`LanguageQuestions` field contains an array of questions.

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Field
     - Value
   * - Id
     - This is the Id of the Question.
   * - Question
     - This is the text for the question
   * - Action
     - This is the text for an action that can take place related to this question. Examples could be: “Submit” or “Vote” or “Please click on this button to submit your vote!”
   * - Confirmation
     - This is the text for a confirmation message that can be displayed once a user has clicked on an action. Examples could be: “Thank you for voting!”
   * - Answers
     - This is an array of possible answers to the question. See below for more information on the structure.
   * - Language
     - This is the language associated with the question. See below for more information on the structure.
   * - Count
     - This is the current count for the number of times a Score has been submitted for this question.

The :makevar:`Answers` field contains an array of answers.

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Field
     - Value
   * - Id
     - This is the Id of the answer
   * - Name
     - This is the text for the answer
   * - Count
     - This is the current count for the number of times a score has been submitted for this answer.

The :makevar:`Language` field contains:

   * - Field
     - Value
   * - Id
     - This is the Id of the language.
   * - Name
     - This is the name of the language.
   * - Code
     - This is the short language code, e.g. *en*, *fr*, “en-us*

.. _scoreMethod:

Score method
------------

This is a POST method. The :makevar:`Score` method will submit the selected answer and other optional data. The URL for calling the :makevar:`Score` method is:

.. code-block:: none

   http://{[accept_portal_server]}/Api/v{[api_version]}/Evaluation/Score/{[ID]}

where *[accept_portal_server]* is the ACCEPT portal server you want to target, *[api_version]* is the version of the API (current is |version|), and *[ID]* is the Evaluation project ID. For an evaluation project with an ID of *1* the call would be:

.. code-block:: none

   http://{[accept_portal_server]}/Api/v{[api_version]}/Evaluation/Score/1

The following parameters can be passed as JSON in the Request body:

The following parameters can be passed on the URL:

.. list-table::
   :widths: 30 50 20
   :header-rows: 1

   * - Parameters
     - Details
     - Example
   * - key
     - This is a MANDATORY parameter. If this parameter is not passed then the API call will fail. This value can be seen on the Evaluation Project page.
     - key=21fdc25bebdf456db5c9e0993977bb12
   * - answer
     - This is a MANDATORY parameter. If this parameter is not passed then the API call will fail. This value is the ID of the answer.
     - answer=1
   * - param1
     - This is an OPTIONAL parameter. This value will accept content up to a length of 25,000 Unicode characters.
     - param1=This is the source text
   * - param2
     - This is an OPTIONAL parameter. This value will accept content up to a length of 25,000 Unicode characters.
     - param2= This is the target text
   * - param3
     - This is an OPTIONAL parameter. This value will accept content up to a length of 2,500 Unicode characters.
     - param3= This is a comment
   * - param4
     - This is an OPTIONAL parameter. This value will accept content up to a length of 250 Unicode characters.
     - param4=This is a username
   * - param5
     - This is an OPTIONAL parameter. This value will accept content up to a length of 250 Unicode characters.
     - param5=This is the browser language
   * - param6
     - This is an OPTIONAL parameter. This value will accept content up to a length of 250 Unicode characters.
     - param6=This is the browser type
   * - param7
     - This is an OPTIONAL parameter. This value will accept content up to a length of 25,000 Unicode characters.
     - param7=This is how long the user spent looking at the page before voting
   * - param8
     - This is an OPTIONAL parameter. This value will accept content up to a length of 250 Unicode characters.
     - param8=This is the OS type
   * - param9
     - This is an OPTIONAL parameter. This value will accept content up to a length of 250 Unicode characters.
     - param9=This is the user’s screen resolution
   * - param10
     - This is an OPTIONAL parameter. This value will accept content up to a length of 250 Unicode characters.
     - param10=This is the web application version number

.. note:: The optional parameter examples above are informative and are not meant to limit what can be posted.

 For the examples above the Request body should look similar to:

 .. code-block:: javascript

    {
	"key": "21fdc25bebdf456db5c9e0993977bb12",
	"answer": "2",
	"param1": "Hello world",
	"param2": "Bonjour le monde",
	"param3": "This is a very basic example",
	"param4": "Jacques",
	"param5": "fr-FR ",
	"param6": "Chrome",
	"param7": "1m 34sec ",
	"param8": "Win32 ",
	"param9":  "1440 × 900 pixels ",
	"param10": " My WebApp v1.0.1.2 "
    }

The :makevar:`Content-Type` must be set to *"application/json"*.

A JSON structure will be returned (the example below has one question returned):

 .. code-block:: javascript

    {
	"ResponseObject":
	{
		"ProjectID":1,
		"ScoreID":15,
		"Success":true
	},
	"ResponseStatus":"OK",
	"TimeStamp":"\/Date(1348494657358)\/",
    }

The response structure contains the following fields and values:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Field
     - Value
   * - ResponseStatus
     - Status of the API call. Possible options are:
       *OK* = Successful call.
       *FAILED*	= API call failed.
   * - ResponseObject
     - This is an array of question categories associated with the project. See below for more information on the structure. This will only be returned if :makevar:`ResponseStatus` is *OK*.
   * - TimeStamp
     - Datetime of API response.
   * - Exception
     - If the ResponseStatus is equal to *FAILED* then the :makevar:`Exception` item will also be returned. This will contain an explanation of why the call failed.
   * - Context
     - If the :makevar:`ResponseStatus` is equal to *FAILED* then the :makevar:`Context` item will also be returned. This will give some context as to where the call failed.


The :makevar:`ResponseObject` object contains information on the score submission.

 .. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Field
     - Value
   * - Success
     - This is a boolean that will indicate whether the submission was successful or not.
   * - ScoreId
     - This is the Id of the Score.  If Success is false then this value will be zero.
   * - ProjectId
     - This is the Id of the Project.  If Success is false then this value will be zero.

.. warning:: Currently each score submission is an insert so there is no possibility of going back (e.g. if a user changes their mind about a score). Also, it is currently not possible to submit multiple scores in one call (e.g. an array of scores).


.. note:: The use of the :makevar:`Questions` and :makevar:`Score` methods is illustrated in the following section: :ref:`evaluationClientExample`.

.. _contentChunksMethod:

ContentChunks method
--------------------

This is a GET method. The :makevar:`ContentChunks` method returns two lists: a list of chunks and list of questions.

.. code-block:: none

   http://{[accept_portal_server]}/AcceptApi/api/v{[api_version]}/Evaluation/ContentChunks/{[ID]}

where *[accept_portal_server]* is the ACCEPT portal server you want to target, *[api_version]* is the version of the API (current is |version|), and *[ID]* is the Evaluation project ID.
For an evaluation project with an ID of *1* the call would be:

.. code-block:: none

   http://{[accept_portal_server]}/AcceptApi/api/v{[api_version]}/Evaluation/ContentChunks/9


The following parameters can be passed on the URL:

.. list-table::
   :widths: 30 50 20
   :header-rows: 1

   * - Parameters
     - Details
     - Example
   * - key
     - This is a MANDATORY parameter. If this parameter is not passed then the API call will fail. This value can be found on the :guilabel:`Project` page.
     - key=21fdc25bebdf456db5c9e0993977bb12
   * - language
     - This is an OPTIONAL parameter. If this parameter is passed then only the questions in the specified language will be returned. Language code is based on RFC 4646. Examples of language codes are: *en*, *fr*, *en-us*, *fr_fr*
     - language=en_us
   * - question
     - This is an OPTIONAL parameter. If this parameter is passed then only this question will be returned.
     - question=1
   * - category
     - This is an OPTIONAL parameter. If this parameter is passed then only the questions in the specified category will be returned.
     - category=1

The list of questions returns the number of answers for each answer type while the list of chunks returns the segments (chunks) that should be evaluated (if they are active).

A response example is shown below:

.. literalinclude:: chunks_questions.json
   :language: javascript




Scores method
--------------

This is a GET method. The :makevar:`Scores` method returns a list of answers and associated metadata.

.. code-block:: none

   http://{[accept_portal_server]}/AcceptApi/api/v{[api_version]}/Evaluation/Scores/{[ID]}

where *[accept_portal_server]* is the ACCEPT portal server you want to target, *[api_version]* is the version of the API (current is |version|), and *[ID]* is the Evaluation project ID.
For an evaluation project with an ID of *1* the call would be:

.. code-block:: none

   http://{[accept_portal_server]}/AcceptApi/api/v{[api_version]}/Evaluation/Scores/9


The following parameters can be passed on the URL:

.. list-table::
   :widths: 30 50 20
   :header-rows: 1

   * - Parameters
     - Details
     - Example
   * - token
     - This is a MANDATORY parameter. If this parameter is not passed then the API call will fail. This value can be found on the :guilabel:`Project Details` page under :guilabel:`My Project Token`.
     - token=6845e9c2

A response example is shown below, with the actual question answer highlighted in yellow:

.. literalinclude:: scores_response.json
   :language: javascript
   :linenos:
   :emphasize-lines: 11
   :lines: 1-25



API security and authentication
-------------------------------

It is expected that the Evaluation API will commonly be called on the client side on the user's browser. This introduced a complication in implementing an authentication model for the API. API keys are being used but if they are being referenced client side, then a user could simply view the source of the web page and easily see the key that is being used.
To counter this, the concept of approved domains was introduced. All API keys have an approved domain associated with them. The intention here is that in addition to checking that the API key is valid, the system will also check to make sure that that key is valid for that domain. This domain verification is being performed by checking the Referrer URL within the HTTP request header for every API call. In practice the Referrer is the URL of the previous host which led to the current API request. This allows us to determine where an API call is coming from by grabbing the domain within the Referrer URL and comparing it with the one associated with the API key provided.
To enable this validation check when the API is being used from a script, we also check the UserHostAddress if the HTTP Referrer is empty. A flaw in this logic is that the HTTP Referrer can be easily spoofed. This is not of concern at the moment as the main focus at this point is to collect data and not to block unauthorized users.  Another issue is that the Referrer value can also be hidden or misspelled by a server for privacy concerns. In this case the authentication process will fail. In this scenario the UserHostAddress will be used as a fall-back.