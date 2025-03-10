..  include:: /Includes.rst.txt
..  index::
    Functions; imageLinkWrap
    imageLinkWrap
..  _imagelinkwrap:

=============
imageLinkWrap
=============


..  contents::
    :local:

..  index:: imageLinkWrap; Properties

Properties
==========

..  _imagelinkwrap-enable:

enable
------

..  confval:: imageLinkWrap.enable

    :Data type: :ref:`data-type-boolean` / :ref:`stdwrap`

    :Default: 0

    Whether or not to link the image. Must be set to True to make
    :typoscript:`imageLinkWrap` do anything at all.


..  _imagelinkwrap-file:

file
----

..  confval:: imageLinkWrap.file

    :Data type: :ref:`stdwrap`

    Apply :ref:`stdwrap` functionality to the file path.


..  _imagelinkwrap-width:

width
-----

..  confval:: imageLinkWrap.width

    :Data type: :ref:`data-type-positive-integer` / :ref:`stdwrap`

    :Default: 0

    Width of the image to be shown in pixels. If you add "m" to
    :typoscript:`width` or :typoscript:`height` or both then the width and
    height parameters will be interpreted as maximum and proportions of the
    image will be preserved.


..  _imagelinkwrap-height:

height
------

..  confval:: imageLinkWrap.height

    :Data type: :ref:`data-type-positive-integer` / :ref:`stdwrap`

    :Default: 0

    Width of the image to be shown in pixels. If you add "m" to
    :typoscript:`width` or :typoscript:`height` or both then the width and
    height parameters will be interpreted as maximum and proportions of the
    image will be preserved.


..  _imagelinkwrap-effects:

effects
-------

..  confval:: imageLinkWrap.effects

    :Data type: like :ref:`gifbuilder-effect` of :ref:`GIFBUILDER`

    :Default: 0

    Apply image effects to the preview image.

Example for effects
~~~~~~~~~~~~~~~~~~~

..  code-block:: typoscript

    imageLinkWrap {
        effects = gamma=1.3 | sharpen=80 | solarize=70
           # effects only works when directImageLink is FALSE
        directImageLink = 0
           # at most 800 pixels wide. Keep proportions.
        width = 800m
           # at most 600 pixels wide. Keep proportions.
        height = 600m
    }


..  _imagelinkwrap-sample:

sample
------

..  confval:: imageLinkWrap.sample

    :Data type: :ref:`data-type-positive-integer` / :ref:`stdwrap`

    :Default: 0

    :typoscript:`sample` is a switch which determines how the image
    processor (often GraphicsMagick or ImageMagick) calculates the preview
    image. If :typoscript:`sample` is true then `- sample` is used with
    GraphicsMagick or ImageMagick instead of `- geometry` to calculate the
    preview image. `sample` does not use antialiasing and is therefore
    much faster than the `geometry` procedure of
    GraphicsMagick or ImageMagick.


..  _imagelinkwrap-title:

title
-----

..  confval:: imageLinkWrap.title

    :Data type: :ref:`data-type-string` / :ref:`stdwrap`

    Specifies the html-page-title of the preview window.
    Needs :typoscript:`JSwindow = 1`.


..  _imagelinkwrap-bodyTag:

bodyTag
-------

..  confval:: imageLinkWrap.bodyTag

    :Data type: :ref:`data-type-tag` / :ref:`stdwrap`

    This is the `<body>`-tag of the preview window.
    Needs :typoscript:`JSwindow = 1`.

Example setting a bodytag for the preview window
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  code-block:: typoscript

    # "onBlur" closes the window automatically if it looses focus
    imageLinkWrap.JSwindow = 1
    imageLinkWrap.bodyTag (
        <body class="jsWindow someOtherClass"
              onBlur="self.close()">
    )


..  _imagelinkwrap-wrap:

wrap
----

..  confval:: imageLinkWrap.wrap

    :Data type: :ref:`data-type-wrap`

    This wrap is placed around the `<img>`-tag in the preview window.
    Needs :typoscript:`JSwindow = 1`.


..  _imagelinkwrap-target:

target
------

..  confval:: imageLinkWrap.target

    :Data type: :ref:`data-type-target` / :ref:`stdwrap`

    :Default: :typoscript:`thePicture`

    This specifies the `target` attribute of the link. The attribute
    will only be created if the current :ref:`Doctype <setup-config-doctype>`
    allows it. Needs :typoscript:`JSwindow = 1`. Default: 'thePicture'.

Example: Use an alternative target for the JavaScript Window
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  code-block:: typoscript

    # (1) to produce:  <a target="preview" ... >
    imageLinkWrap.target = preview

    # (2) to use a new window for each image
    # let there be:  <a target="<hash-code>" ... >
    imageLinkWrap.JSwindow = 1
    imageLinkWrap.JSwindow.newWindow = 1


..  _imagelinkwrap-JSwindow:

JSwindow
--------

..  confval:: imageLinkWrap.JSwindow

    :Data type: :ref:`data-type-boolean` / :ref:`stdwrap`

    :Default: 0

    If true (:typoscript:`JSwindow = 1`) Javascript will be used to open
    the image in a new window. The window is automatically resized to match
    the dimensions of the image.


..  _imagelinkwrap-JSwindow-expand:

JSwindow.expand
---------------

..  confval:: imageLinkWrap.JSwindow.expand

    :Data type: :typoscript:`x`, :typoscript:`y` / :ref:`stdwrap`

    :Default: 0

    :typoscript:`x` and :typoscript:`x` are of data type
    :ref:`data-type-integer`. The values are added to the width and height
    of the preview image when calculating the width and height of the
    preview window.


..  _imagelinkwrap-JSwindow-newWindow:

JSwindow.newWindow
------------------

..  confval:: JSwindow.newWindow

    :Data type: :ref:`data-type-boolean` / :ref:`stdwrap`

    :Default: 0

    If the :ref:`Doctype <setup-config-doctype>` allows the :ref:`data-type-target`
    attribute then the image will be opened in a window with the name given
    by `target`. If that windows is kept open and the next image with the
    same :ref:`data-type-target` attribute is to be shown then it will appear
    in the same preview window.
    If :typoscript:`JSwindow.newWindow` is set to True,
    then a unique hash value is used as `target` value for each image.
    This guarantees that each image is opened in a new window.


..  _imagelinkwrap-JSwindow-altUrl:

JSwindow.altUrl
---------------

..  confval:: imageLinkWrap.JSwindow.altUrl

    :Data type: :ref:`data-type-string` / :ref:`stdwrap`

    If this returns anything then it is used as URL of the preview window.
    Otherwise the default "showpic" script will be used.


..  _imagelinkwrap-JSwindow-altUrl-noDefaultParams:

JSwindow.altUrl\_noDefaultParams
--------------------------------

..  confval:: imageLinkWrap.JSwindow.altUrl_noDefaultParams

    :Data type: :ref:`data-type-boolean` / :ref:`stdwrap`

    :Default: 0

    If true (:typoscript:`JSwindow.altUrl_noDefaultParams = 1`) then the
    image parameters are not automatically appended to the
    :typoscript:`altUrl`. This is useful if you want to add them yourself
    in a special way.


..  _imagelinkwrap-typolink:

typolink
--------

..  confval:: imageLinkWrap.typolink

    :Data type: :ref:`typolink` / :ref:`stdwrap`

    If this returns anything it will be used as link and override
    everything else.


..  _imagelinkwrap-directImageLink:

directImageLink
---------------

..  confval:: imageLinkWrap.directImageLink

    :Data type: :ref:`data-type-boolean` / :ref:`stdwrap`

    :Default: 0

    If true (:typoscript:`directImageLink = 1`) then a link will be
    generated that points directly to the image file. This means that no
    "showpic" script will be used.


..  _imagelinkwrap-linkParams:

linkParams
----------

..  confval:: imageLinkWrap.linkParams

    :Data type: :ref:`typolink` / :ref:`stdwrap`

    When the direct link for the preview image is calculated all
    attributes of :typoscript:`linkParams` are used as settings for the
    :ref:`typolink` function. In other words: Use the same parameters
    for :typoscript:`linkParams` that you would use for :ref:`typolink`.
    Needs :typoscript:`JSwindow = 0`.

Example: Use alternative parameters for the a-tag
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This way it is possible to use a lightbox and to display
resized images in the frontend. More complete examples are
:ref:`imageLinkWrap-example-fancybox` and
:ref:`imageLinkWrap-example-topup`.

..  code-block:: typoscript

    JSwindow = 0
    directImageLink = 1
    linkParams.ATagParams.dataWrap (
      class="{$styles.content.imgtext.linkWrap.lightboxCssClass}"
      rel="{$styles.content.imgtext.linkWrap.lightboxRelAttribute}"
    )


..  _imagelinkwrap-stdWrap:

stdWrap
-------

..  confval:: imageLinkWrap.stdWrap

    :Data type: :ref:`stdwrap`

    This adds :ref:`stdwrap` functionality to the almost final
    result.

What it does
============

:typoscript:`imageLinkWrap = 1`

If set to True (:typoscript:`= 1`) then this function attaches a link to an image
that opens a special view of the image. By default the link points to
the a "showpic" script that knows how to deal with several parameters.
The script checks an md5-hash to make sure that the parameters are unchanged.
See :ref:`imageLinkWrap-basic-example-showpic`.

There is an alternative. You may set :typoscript:`directImageLink` to True
(:typoscript:`= 1`). In that case the link will directly point to the image
- no intermediate script is involved. This method can well be used to display
images in a lightbox. See :ref:`imageLinkWrap-basic-example-directImageLink`
and the lightbox examples on this page.

If :typoscript:`JSwindow` is True (:typoscript:`= 1`) more fancy
features are available since the preview now is opened by Javascript.
Then the Javascript window title, size, background-color and more can be set to
special values.

Implementation
==============

-   `imageLinkWrap <https://github.com/TYPO3/typo3/blob/83d36733d7700a49a2d312d09c93ab4d87953e9a/typo3/sysext/frontend/Classes/ContentObject/ContentObjectRenderer.php#L939>`__ in API,
-   method `imageLinkWrap` in
-   class :php:`ContentObjectRenderer` in
-   namespace :php:`namespace TYPO3\CMS\Frontend\ContentObject;` in
-   file :file:`ContentObjectRenderer.php` in
-   folder :file:`typo3/sysext/frontend/Classes/ContentObject`.

..  _imagelinkwrap-examples:

Examples for imageLinkWrap
==========================

..  _imageLinkWrap-basic-example-showpic:

Basic example: Create a link to the showpic script
--------------------------------------------------

..  code-block:: typoscript

    10 = IMAGE
    10 {
       # point to the image
       file = fileadmin/demo/lorem_ipsum/images/a4.jpg
       # make it rather small
       file.width = 80
       # add a link to tx_cms_showpic.php that shows the original image
       imageLinkWrap = 1
       imageLinkWrap {
          enable = 1
          # JSwindow = 1
       }
    }

..  _imageLinkWrap-basic-example-directImageLink:

Basic example: Link directly to the original image
--------------------------------------------------

..  code-block:: typoscript

    10 = IMAGE
    10 {
       file = fileadmin/demo/lorem_ipsum/images/a4.jpg
       file.width = 80
       imageLinkWrap = 1
       imageLinkWrap {
          enable = 1
          # link directly to the image
          directImageLink = 1
          # JSwindow = 1
       }
    }


..  _imageLinkWrap-example-popup-window:

Example: Larger display in a popup window
-----------------------------------------

..  code-block:: typoscript

    page = PAGE
    page.10 = IMAGE
    page.10 {
       # the relative path to the image
       # find the images in the 'lorem_ipsum' extension an copy them here
       file = fileadmin/demo/lorem_ipsum/images/b1.jpg
       # let's make the normal image small
       file.width = 80
       # yes, we want to have a preview link on the image
       imageLinkWrap = 1
       imageLinkWrap {
          # must be TRUE for anything to happen
          enable = 1
          # "m" = at most 400px wide - keep proportions
          width = 400m
          # "m" = at most 300px high - keep proportions
          height = 300
          # let's use fancy Javascript features
          JSwindow = 1
          # black background
          bodyTag = <body style="background-color:black; margin:0; padding:0;">
          # place a Javascript "close window" link onto the image
          wrap = <a href="javascript:close();"> | </a>
          # let there be a new and unique window for each image
          JSwindow.newWindow = 1
          # make the preview window 30px wider and 20px higher
          # than what the image requires
          JSwindow.expand = 30,20
       }
    }


..  _imageLinkWrap-example-printlink:

Example: Printlink
------------------

..  code-block:: typoscript

    5 = IMAGE
    5 {
       file = fileadmin/images/printlink.png
       imageLinkWrap = 1
       imageLinkWrap {
          enable = 1
          typolink {
             target = _blank
             parameter.data = page:alias // TSFE:id
             additionalParams = &type=98
          }
       }
       altText = print version
       titleText = Open print version of this page in a new window
       params = class="printlink"
    }



..  _imageLinkWrap-example-fancybox:

Example: Images in lightbox "fancybox"
--------------------------------------

Let's follow this `lightbox.ts example <https://github.com/georgringer/modernpackage/blob/master/Resources/Private/TypoScript/content/lightbox.ts>`__
and use `fancybox <http://fancybox.net>`_:

..  code-block:: typoscript

    # Add the CSS and JS files
    page {
       includeCSS {
          file99 = fileadmin/your-fancybox.css
       }
       includeJSFooter {
          fancybox = fileadmin/your-fancybox.js
       }
    }

    # Change the default rendering of images to match lightbox requirements
    tt_content.image.20.1.imageLinkWrap {
       JSwindow = 0
       directImageLink = 1
       linkParams.ATagParams {
          dataWrap = class= "lightbox" data-fancybox-group="lightbox{field:uid}"
       }
    }

..  _imageLinkWrap-example-topup:

Example: Images in lightbox "TopUp"
-----------------------------------

In this `blog post <https://www.interaktionsdesigner.de/2009/typo3-klickvergrossern-durch-eine-jquery-lightbox-ersetzen>`__
(German) Paul Lunow shows a way to integrate the
`jQuery <https://jquery.com/>`__
`TopUp lightbox <https://jquery-plugins.net/topup-jquery-lightbox-pop-up-plugin>`__:

..  code-block:: typoscript

    tt_content.image.20.1.imageLinkWrap >
    tt_content.image.20.1.imageLinkWrap = 1
    tt_content.image.20.1.imageLinkWrap {
       enable = 1
       typolink {
          # directly link to the recent image
          parameter.cObject = IMG_RESOURCE
          parameter.cObject.file.import.data = TSFE:lastImageInfo|origFile
          parameter.cObject.file.maxW = {$styles.content.imgtext.maxW}
          parameter.override.listNum.stdWrap.data = register : IMAGE_NUM_CURRENT
          title.field = imagecaption // title
          title.split.token.char = 10
          title.if.isTrue.field = imagecaption // header
          title.split.token.char = 10
          title.split.returnKey.data = register : IMAGE_NUM_CURRENT
          parameter.cObject = IMG_RESOURCE
          parameter.cObject.file.import.data = TSFE:lastImageInfo|origFile
          ATagParams = target="_blank"
       }
    }
