Getting project information
===========================

To get information about a project, the following GET method can be used:

.. code-block:: none

   http://[accept_portal_server]/AcceptApi/Api/v[api_version]/Admin/ProjectInfo?token={token}

where *[accept_portal_server]* is the ACCEPT portal server you want to target, *[api_version]* is the version of the API (current is |version|), and *[token]* is the project's admin token.

The request must include the following in the HTTP header:

.. code-block:: none

   Content-Type:application/json

This method returns a JSON object whose **ResponseObject** value is composed of two lists: a list of tasks and a list of users.

