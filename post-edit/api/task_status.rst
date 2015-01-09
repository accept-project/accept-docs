Checking task status
====================

To check the status of a task, the following GET method can be used:

.. code-block:: none

   http://[accept_portal_server]/AcceptApi/Api/v[api_version]/PostEdit/GetTaskStatus?textId={text_id}


where *[accept_portal_server]* is the ACCEPT portal server you want to target, *[api_version]* is the version of the API (current is |version|), and *[text_id]* is the task's text ID.

The request must include the following in the HTTP header:

.. code-block:: none

   Content-Type:application/json

This method returns a JSON object in the form of:

.. code-block:: javascript

   "ResponseObject": {
            "TextId": "[UniqueTaskID]",
            "UserId": "ExtDeVUser1",
            "Status": 1,
            "LastUpdate": "2014-09-11T09:45:31.000Z"
        }

The **status** values are:

  * *0* : task not started by user
  * *1* : task started but not finished by user
  * *2* : task completed by user


