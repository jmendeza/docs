:is-up-to-date: True
:last-updated: 4.0.0

.. index:: Asset Processing

.. _asset-processing-config:

==============================
Asset Processing Configuration
==============================

Asset processing allows you to define transformations for static assets (currently only images), through a series of processor pipelines that are executed when the assets are uploaded to Studio.

To modify the Asset Processing configuration, click on |projectTools| from the bottom of the Sidebar, then click on **Configuration** and select **Asset Processing** from the dropdown list.

.. image:: /_static/images/site-admin/config-open-asset-proc-config.jpg
    :alt: Configurations - Open Asset Processing Configuration
    :width: 65 %
    :align: center

------
Sample
------

.. code-block:: xml
    :caption: *CRAFTER_HOME/data/repos/sites/SITENAME/sandbox/config/studio/asset-processing/asset-processing-config.xml*
    :linenos:

    <!--

    	Asset processing configuration file. You can define in this file one or multiple pipelines to process static assets when they're
    	uploaded. A pipeline configuration supports the following elements:

    	<pipeline>
    		<inputPathPattern/>
    		<keepOriginal/>
    		<processors>
    			<processor>
    				<type/>
    				<params/>
    				<outputPathFormat/>
    			</processor>
    		</processors>
    	</pipeline>

        - inputPathPattern: regex that the assets need to match in order to be processed by the pipeline. Groups that are captured by this
        regex are available later to the outputPathFormat.
        - keepOriginal (optional): if the original asset (without changes) should be saved.
        - type: the type of the processor. Right now 2 types are supported: ImageMagickTransformer and TinifyTransformer.
    		- ImageMagickTransformer: runs ImageMagick from the command line, with params.options as the command line params.
    		- TinifyTransformer: uses the Java client of TinyPNG to compress JPEG/PNG images (see https://tinypng.com/developers/reference).
		The Tinify API key must be specified in the studio-config-overrides.yaml.
	    - outputPathFormat (optional): the format of the output path. Variables that have a dollar sign ($) and an index are later replaced
	    by groups that resulted during input path matching, to form the final output path. If not specified, then the same input path is used
	    as the output path.

    -->
    <assetProcessing>
        <pipelines>

            <!-- Web transformer pipeline -->
            <pipeline>
                <inputPathPattern>^/static-assets/images/upload/(.+)\.jpg$</inputPathPattern>
                <keepOriginal>false</keepOriginal>
                <processors>
                    <processor>
                        <type>ImageMagickTransformer</type>
                        <params>
                            <options>-level 0,100%,1.3 -gaussian-blur 0.05 -quality 20% -strip</options>
                        </params>
                        <outputPathFormat>/static-assets/images/compressed/web/$1-compressed.jpg</outputPathFormat>
                    </processor>
                </processors>
            </pipeline>

            <!-- Mobile transformer pipeline -->
            <pipeline>
                <inputPathPattern>^/static-assets/images/upload/(.+)\.jpg$</inputPathPattern>
                <keepOriginal>false</keepOriginal>
                <processors>
                    <processor>
                        <type>ImageMagickTransformer</type>
                        <params>
                            <options>-level 0,100%,1.3 -gaussian-blur 0.05 -quality 20% -strip -resize 226x164</options>
                        </params>
                        <outputPathFormat>/static-assets/images/compressed/mobile/$1-compressed.png</outputPathFormat>
                    </processor>
                    <processor>
                        <type>TinifyTransformer</type>
                    </processor>
                </processors>
            </pipeline>

        </pipelines>
    </assetProcessing>

For more details on asset processing, see :ref:`asset-processing`
