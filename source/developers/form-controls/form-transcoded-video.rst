:is-up-to-date: False
:last-updated: 4.0.0

:orphan:

.. document does not appear in any toctree, this file is referenced
   use :orphan: File-wide metadata option to get rid of WARNING: document isn't included in any toctree for now

.. index:: Form Controls; Transcoded Video

.. _form-transcoded-video:

========================
Transcoded Video Control
========================

-------
Example
-------
.. image:: /_static/images/form-controls/form-control-transcoded-video-example.png
    :width: 80%
    :alt: Form Control Transcoded Video Example
    :align: center

|

.. todo: update image above

-------------
Configuration
-------------
.. image:: /_static/images/form-controls/form-control-transcoded-video.png
    :width: 40%
    :alt: Form Control Transcoded Video
    :align: center

|

.. include:: /includes/form-controls/form-control-field-basics.rst

+------------------------+-----------------------------------------------------------------------+
|| Description/Purpose   || Transcoded Video selector from Video Transcoding Data Source.        |
+------------------------+-----------------------------------------------------------------------+
|| Properties            || * Data Source: Source that will populate the transcoded video picker.|
||                       || * Read Only: Make field read-only (can't be changed by the author).  |
+------------------------+-----------------------------------------------------------------------+
|| Constraints           || * Required: Make field required to fill out.                         |
+------------------------+-----------------------------------------------------------------------+
|| Related Data Sources  || * |mediaConvertTranscode|                                            |
+------------------------+-----------------------------------------------------------------------+

.. |mediaConvertTranscode| replace:: :ref:`Video Transcoding From S3 Repository <form-source-mediaconvert-transcode>`
