..  include:: /Includes.rst.txt
..  index:: Content objects; TEXT
..  _cobj-text:

====
TEXT
====

A content object of the type "TEXT" can be used to output static text or HTML.

stdWrap properties are available under the property "value" and on the
very rootlevel of the object.

..  note::
    Gifbuilder also has a :ref:`TEXT object <gifbuilder-text>` -
    do not mix that one up with the cObject described here; both are
    different objects.

..  contents::
    :local:

..  index:: TEXT; Properties
..  _cobj-text-properties:

Properties
==========

..  _cobj-text-value:

value
-----

..  confval:: value

    :Data type: :ref:`data-type-string` / :ref:`stdWrap <stdwrap>`

    Text, which you want to output.


..  _cobj-text-stdWrap:

stdWrap
-------

..  confval:: stdWrap

    :Data type: :ref:`stdWrap <stdwrap>`

    stdWrap properties are available on the very root level of the
    object. This is non-standard! You should use these stdWrap
    properties consistently to those of the other cObjects by
    accessing them through the property "stdWrap".


..  _cobj-text-cache:

cache
-----

..  confval:: cache

    :Data type: :ref:`cache <cache>`

    See :ref:`cache function description <cache>` for details.


Examples
========

..  code-block:: typoscript
    :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

    10 = TEXT
    10.value = This is a text in uppercase
    10.stdWrap.case = upper

The above example uses the stdWrap property :ref:`case <stdwrap-case>`. It
returns "THIS IS A TEXT IN UPPERCASE".

..  code-block:: typoscript
    :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

    10 = TEXT
    10.value.field = title
    10.stdWrap.wrap = <strong>|</strong>

The above example gets the header of the current page (which is
stored in the database field :sql:`title`). The header is then wrapped in
:html:`<strong>` tags, before it is returned.


Now let us have a look at an extract from a more complex example:

..  code-block:: typoscript
    :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

    10 = TEXT
    10.value.field = bodytext
    10.stdWrap.parseFunc < lib.parseFunc_RTE
    10.stdWrap.dataWrap = <div>|</div>

The above example returns the content, which was found in the field
:sql:`bodytext` of the current record from $cObj->data-array. Here that
shall be the current record from the database table :sql:`tt_content`. This
is useful inside :ref:`COA <cobj-coa>` objects.

Here is the same example in its context:

..  code-block:: typoscript
    :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

    10 = CONTENT
    10 {
      table = tt_content
      select {
        where.dataWrap = irre_parentid  = {field:uid}
        begin = 0
      }

      renderObj = COA
      renderObj {
        stdWrap.if.isTrue.data = field:bodytext
        10 = TEXT
        10.value.field = bodytext
        10.stdWrap.parseFunc < lib.parseFunc_RTE
        10.stdWrap.dataWrap = <div>|</div>
       }
    }

Here we use the cObject :ref:`CONTENT <cobj-content>` to return all content elements
(records from the database table "tt_content"), which are on the
current page. (These content elements have the corresponding value in
:sql:`irre_parentid`.) They are rendered using a :ref:`COA <cobj-coa>` cObject, which only
processes them, if there is content in the field :sql:`bodytext`.

The resulting records are each rendered using a TEXT object:

The TEXT object returns the content of the field :sql:`bodytext` of the
according :sql:`tt_content` record. (Note that the property "field" in this
context gets content from the table :sql:`tt_content` and not - as in
the example above - from :sql:`pages`. See the description for the
data type :ref:`getText/field <data-type-gettext-field>`!) The resulting content
is then parsed with :ref:`parsefunc` and finally wrapped in :html:`<div>` tags
before it is returned.
