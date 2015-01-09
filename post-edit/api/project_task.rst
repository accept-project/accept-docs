Adding a task to a project
==========================

* Log into ACCEPT portal and click on the :guilabel:`PostEdit` tab.
* Click on the project name link or on the :guilabel:`Edit` button under the :guilabel:`Actions` column to navigate to the project detail page.
* When in the project detail page, click on :guilabel:`My Project Token` button to get your private project admin token. This value needs to be used in order to upload tasks using the ACCEPT API.
* Make use of any HTTP compliant client application (you can use any HTTP debugging proxy server application such as Fiddler) to:

   * Set the proper ACCEPT API URL to add tasks to projects. For example: :samp:`http://{[accept_portal_server]}/AcceptApi/Api/v{[api_version]}/PostEdit/AddDocumentToProject/?token={[token]}` where *[accept_portal_server]* is the ACCEPT portal server you want to target, *[api_version]* is the version of the API (current is |version|), and *[token]* is the project's admin token.
   * Add the following HTTP header to your request: :samp:`Content-Type : application/json`
   * Add the JSON that corresponds to your task in your request body and post it.