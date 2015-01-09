.. _evaluationClientExample:

Client example
==============

In order to demonstrate how the API may be used, a Demo page has been created on the `ACCEPT Portal <http://www.accept-portal.eu/AcceptPortal/en/Evaluation/Demo>`_. The code snippets below, which are those used on this Demo page, show how to call the :ref:`questionsMethod` to ask for feedback. The HTML shown below contains two example content records in an HTML table. The first field contains a **<div>** tag with the name *voting*. The second field contains some example content.

.. code-block:: html

    <!-- An example table containing two records -->
    <table border="0">
        <tr>
        <td>
        <div class="lia-message-view message-uid-147" id="messageview">
            <table border=1>
            <tr>
            <td><div name="voting" /></td>
            <td>
                <div class="content-source">This is a test sentence!</div>
            </td>
            <td>
                <div class="content-mt"> C'est une test phrase!</div>
            </td>
            </tr>
            </table>
        </div>
        </td>
        </tr>
        <tr>
        <td>
        <div class="lia-message-view message-uid-148" id="messageview">
            <table border=1>
            <tr>
            <td><div name="voting" /></td>
            <td>
                <div class="content-source">This is another test sentence!</div>
            </td>
            <td>
                <div class="content-mt">C'est une autre test phrase!</div>
            </td>
            </tr>
            </table>
        </div>
        </td>
        </tr>
    </table>

We want to replace the **<div name="voting" />** with our questions. This requires two things:

  1. Get the list of questions by making an API call
  2. Process the returned data to display the questions

To get the list of questions, the following jQuery AJAX method may be used, and the returned data may be processed in the success function:

.. code-block:: html

    <script type="text/javascript" language="javascript">
        $(document).ready(function () {
            $.ajax({
                url: 'http://www.accept-portal.com/Api/v1/Evaluation/Questions/1?key=<api_key>&language=en_us&category=1&question=1',
                contentType: "application/json",
                type: "GET",
                async: false,
                cache: false,
                dataType: 'jsonp',
                success: function (data) {
                    var text = '';
                    var q = data.ResponseObject[0];
                    for (var i = 0; i < q.LanguageQuestions.length; i++) {
                        question = q.LanguageQuestions[i].Question;
                        questionId = q.LanguageQuestions[i].Id;
                        language = q.LanguageQuestions[i].Language.Code;
                        languageid = q.LanguageQuestions[i].Language.Id
                        action = q.LanguageQuestions[i].Action;

                        $("div[class^='lia-message-view message-uid-']").each(function () {
                            var id = $(this).attr("class");
                            var PostID = $(this).attr("class").replace("lia-message-view message-uid-", "");
                            var frmName = "voting_" + PostID;
                            text = '<form id="' + frmName + '"><table border=1><tr><td bgcolor=lightgray><b>' + question + '</b></td></tr>';
                            text += '<tr><td><div id="voteoptions">';
                            for (var x = 0; x < q.LanguageQuestions[i].Answers.length; x++) {
                                answer = q.LanguageQuestions[i].Answers[x];
                                if (x == 0)
                                    text += '<input id="radioCount_' + answer.Id + '" type="radio" name="choice" value="' + answer.Id + '" checked>' + answer.Name;
                                else
                                    text += '<input id="radioCount_' + answer.Id + '" type="radio" name="choice" value="' + answer.Id + '">' + answer.Name;
                            }
                            text += '<br/>';
                            text += '<input  value="' + action + '" type="button" onclick="SubmitVote(\'' + frmName + '\');" />';
                            text += '</div>';
                            text += '<div id="voteresult" style="display:none">';
                            text += '<font color="green">RESULT: ' + q.LanguageQuestions[i].Confirmation + '</font>';
                            text += '</div>';
                            text += '</td></tr>';
                            text += '</table></form>';
                            $(this).find('div[name="voting"]').html(text);
                        });
                    }
                },
                error: function (jqXHR, textStatus, errorThrown) {
                    alert(errorThrown);
                }
            });

        });
    </script>

The Submission javascript code is as follows:

.. code-block:: html

    <script type="text/javascript" language="javascript">
        function SubmitVote(frmName) {
                var PostID = frmName.replace("voting_", "")
                $('#' + frmName + " input:radio").each(function () {
                    if ($(this).is(':checked')) {
                        debugger;
                        var answerID = $(this).val();
                        $.ajax({
                            url: 'http://www.accept-portal.com/Api/v1/Evaluation/Score/1',
                            dataType: 'json',
                            contentType: "application/json",
                            type: 'post',
                            data: '{ "key": "<api_key>", "answer": ' + answerID + ', "param1": ' + PostID + '  }',
                            success: function (e) {
                                $('#' + frmName).find('#voteoptions').css("display", "none");
                                $('#' + frmName).find('#voteresult').css("display", "block");
                            },
                            error: function (jqXHR, textStatus, errorThrown) {
                                //alert(errorThrown);
                            }
                        });
                    }
                });
            }
    </script>