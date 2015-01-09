Project data
============

Once users have started working on a |post| project, project administrators can export post-editing activity data at user, document or project level. The data is exported in an `XLIFF <http://docs.oasis-open.org/xliff/v1.2/cs02/xliff-core.html>`_ format.

The following example shows the type of usage data that is captured by the system and made available in the :makevar:`header` of the XLIFF report.

.. literalinclude:: simple_xliff_example.xml
   :language: xml
   :linenos:
   :emphasize-lines: 6-7,14, 25-29
   :lines: 3-38

Thanks to the information present in the :makevar:`header` element, the steps that were taken during the post-editing process can be retraced. The :makevar:`phase-group` element contains a number of phases corresponding to processes that were used to interact with the target text. The first of these phases (in chronological order) actually occurred before the post-editing process since it corresponds to the actual automatic translation of the source text using a specific Machine Translation tool.

The first post-editing phase is the one whose **phase-name** attribute has a *start_pe* value. The starting time of this phase is indicated in the value of the **date** attribute. The times indicate that the start of this phase coincided with the start of another phase,  whose **phase-name** attribute has a *t1.1* value. The syntax of this value is based on the following parts:

    * phase_type: *r* (for revision) or *t* (for thinking)
    * translation unit ID: starting at 1
    * iteration ID for a given translation unit: starting at 1

The *t1.1* value therefore means that the first iteration of a *thinking* phase started for the first translation unit of the file. During this phase, some countable events occurred. These events are captured in a number of :makevar:`count` elements with a matching **phase-name** attribute value. In this example, the phase lasted just over 16 seconds (as indicated by the element with the *x-think-time* value). The element with the *x-start_source_switch* value indicates that the UI displayed the source text when the phase was started. However, this was changed twice by the user, as indicated by the element with the *x-source_switch* value where the value of **unit** is *instance*. Other elements also indicate precisely when the UI was changed.

In short, a thinking phase is used to capture events that happened in the |post| client when these events are not directly related to the editing of the target text. For instance, if the user had commented on the quality of the target text, a *note* child element would have been attached to the :makevar:`phase` element.

Based on the times present in the report, it is possible to determine that the *t1.1* phase was not immediately followed by the *t1.2* phase, since the difference between the starting time of the *t1.2* phase and that of the *t1.1* phase is 24 seconds (whereas the *t1.1* phase lasted just over 16 seconds). This means that the user closed the client application and re-opened it 8 seconds later. As soon as the client application was re-opened, the *t1.2* phase started and lasted over 6 seconds. This time, however, the *t1.2* phase was immediately followed by another phase in the client application, the *r1.1* phase (which was a revision phase).

Multiple events occurred during the *r1.1* phase, including the generation of two comments by the user (as indicated by the two child :makevar:`note` elements) on lines 11 and 12. Other events got captured in :makevar:`count` elements, including how long the editing of the target text lasted (as indicated by the *x-editing-time* value). In this example, the *x-editing-time* value is the same as the *x-typing-time* value, indicating that the user started editing the target text using a keyboard key (instead of using a contextual translation option). Various numbers of pressed keys are available for this phase, thus allowing for a detailed analysis of the type of post-editing that was conducted.

.. warning:: *x-editing-time* and *x-typing-time* values must be analyzed carefully. For practical reasons, the calculation of these times may not fully correspond to the reality. For instance, an *x-typing-time* value of 50 seconds does not necessarily mean that the user was typing for 50 seconds. It means that the user started typing and that the phase ended 50 seconds later. It is possible, however, that the user typed for 10 seconds, thought for 2 seconds, typed again for 12 seconds, etc... Trying to capture and analyze this data in a sane manner is extremely challenging, which is why the calculations are currently simplified.

Since an editing revision happened, the target text is likely to have changed in the first translation unit. This is confirmed when examining the first :makevar:`trans-unit` element of the :makevar:`body` element:

.. literalinclude:: simple_xliff_example.xml
   :language: xml
   :linenos:
   :emphasize-lines: 7-15
   :lines: 39-55

In this example, it is possible to see that the current *target* text corresponds to what was produced in the *r1.1* phase, relegating the translation from the *mt_baseline* phase to an :makevar:`alt-trans` element.

.. note:: Thinking phases do not have any impact on :makevar:`trans-unit` elements since the target text does not get modified during these phases.

Special Notes
-------------

Special notes associated with a paraphrasing service, an interactive check or provided translation options may be included in :makevar:`phase` elements, as shown below.

.. literalinclude:: xliff_example_notes.xml
   :language: xml
   :linenos:
   :emphasize-lines: 2,13-14,16
   :lines: 7-22

Such notes have a descriptive **from** attribute value, such as:


======================== ================================================================================== ===========================
Value                    Description                                                                        :makevar:`note` value format
======================== ================================================================================== ===========================
paraphrasing_replacement When a paraphrasing suggestion is used to replace a substring in the original text paraphrasing_trigger_timestamp|||substring_selected|||paraphrasing_suggestion|||substring_index|||paraphrasing_suggestion_position|||paraphrasing_suggestion_use_timestamp|||original_text
paraphrasing_display     When paraphrasing suggestions are displayed and dismissed                          paraphrasing_trigger_timestamp|||substring_selected|||substring_index|||original_text
paraphrasing_empty       When no paraphrasing suggestions are returned by the service                       paraphrasing_trigger_timestamp|||substring_selected|||substring_index|||original_text
paraphrasing_failure     When the paraphrasing service fails                                                paraphrasing_trigger_timestamp|||substring_selected|||substring_index|||original_text
interactive_check        When an interactive check's suggestion replaces a substring in the original text   interactive_check_menu_display_timestamp|||substring_marked|||interactive_check_suggestion|||substring_index|||interactive_check_suggestion|||interactive_check_suggestion_use_timestamp|||original_text
trans_options            When a translation option is used to to replace a substring in the original text   highlighted_substring|||translation_option_used|||substring_index|||original_text
======================== ================================================================================== ===========================


.. warning:: Interactive check usage is incremented every time the user triggers a text-level check so the actual value of a phase-level :makevar:`count` element, whose **count-type** attribute has an *x-checking-usage* value, may not match the number of notes with a *interactive_check* **from** value. On the other hand, paraphrasing usage is incremented every time a paraphrasing request is triggered (regardless of success and failures) so the actual value of a phase-level :makevar:`count` element, whose **count-type** attribute has an *x-paraphrasing-usage* value, will match the number of notes with a **from** value starting with *paraphrasing_*. An additional phase-level :makevar:`count` element, whose **count-type** attribute has an *x-paraphrasing-failures* value, may be included (*x-paraphrasing-failures* being a subset of *x-paraphrasing-usage*).