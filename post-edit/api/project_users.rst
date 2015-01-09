Adding project users
====================

To add users to a project, the following POST method can be used:

.. code-block:: none

   http://[accept_portal_server]/AcceptApi/Api/v[api_version]/Admin/AddUserProject

where *[accept_portal_server]* is the ACCEPT portal server you want to target and *[api_version]* is the version of the API (current is |version|).

The request must include the following in the HTTP header:

.. code-block:: none

   Content-Type:application/json

The request must include a JSON body based on the following format:

.. code-block:: javascript

    {
        "userName": "…",
        "token": "…"
    }

where **token** is the project's admin token and **userName** the name of the user who will be allowed to work on the task (as specified during the initialisation of the |post| plug-in, as described in :ref:`pe-client-example`).
