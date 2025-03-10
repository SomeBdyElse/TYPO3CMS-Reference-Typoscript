..  include:: /Includes.rst.txt
..  index::
    Functions; select
    Database; select
..  _select:

======
select
======

This object generates an SQL-select statement to select records
from the database.

Some records are hidden or timed by start- and end-times. This is
automatically added to the SQL-select by looking for "enablefields"
in the :php:`$GLOBALS['TCA']`.

..  warning::

    Do not use GET or POST data like GPvar directly with this object!
    Avoid :ref:`SQL injections <t3coreapi:security-sql-injection>`! Don't trust
    any external data! Secure any unknown data, for example with
    :ref:`stdwrap-intval`.

..  contents::
    :local:

..  index:: select; Properties
..  _select-properties:

Properties
==========

..  _select-uidInList:

uidInList
---------

..  confval:: uidInList

    :Data type: *list of record\_ids* / :ref:`stdWrap`

    Comma-separated list of record uids from the according database table.
    For example when the select function works on the table `tt_content`, then
    this will be uids of `tt_content` records.

    **Note:** :typoscript:`this` is a *special keyword* and replaced with the id of the
    *current record*.

    ..  attention::
        :ref:`select_pidInList` defaults to :typoscript:`this`.
        Therefore by default only records
        from the current page are available for :typoscript:`uidInList`. If records
        should be fetched globally, :typoscript:`pidInList = 0` should also be set.

    ..  rubric:: Example

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        select {
           uidInList = 1,2,3
           pidInList = 0
        }

        select.uidInList = this

..  _select_pidInList:

pidInList
---------

..  confval:: pidInList

    :Data type: *list of page\_ids* / :ref:`stdWrap`
    :Default: :typoscript:`this`

    Comma-separated list of pids of the record. This will be page uids (pids). For
    example when the select function works on the table tt_content, then this
    will be pids of tt_content records, the parent pages of these records.

    Pages in the list, which are not visible for the website user, *are
    automatically removed* from the list. Thereby no records from hidden,
    timed or access-protected pages will be selected! Nor will be records
    from recyclers. Exception: The hidden pages will be listed in *preview mode*.

    **Special keyword:** :typoscript:`this`
        Is replaced with the id of the current page.

    **Special keyword:** :typoscript:`root`
        Allows to select records from the root-page level (records with pid=0,
        e.g. useful for the table "sys_category" and others).

    **Special value:** :typoscript:`-1`
        Allows to select versioned records in workspaces directly.

    **Special value:** :typoscript:`0`
        Allows to disable the :sql:`pid` constraint completely. Requirements:
        :typoscript:`uidInList` *must* be set or the table *must* have the prefix
        "static\_\*".

    ..  note::
        Check the doktype of your backend page. If you are trying to fetch records from
        a sys_folder for example, the :php:`$cObj->checkPid_badDoktypeList` method will insert the
        following SQL into your query:

        ..  code-block:: sql

            [...]WHERE (`your_requested_table_name`.`uid` = 0) AND [...]

        Which might result in an empty query result, depending on your records.


    ..  rubric:: Example

    Fetch related `sys_category` records stored in the MM intermediate table:

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        10 = CONTENT
        10 {
           table = sys_category
           select {
              pidInList = root,-1
              selectFields = sys_category.*
              join = sys_category_record_mm ON sys_category_record_mm.uid_local = sys_category.uid
              where.data = field:_ORIG_uid // field:uid
              where.intval = 1
              where.wrap = sys_category_record_mm.uid_foreign=|
              orderBy = sys_category_record_mm.sorting_foreign
              languageField = 0 # disable translation handling of sys_category
           }
        }


..  _select-recursive:

recursive
---------

..  confval:: recursive

    :Data type: :ref:`data-type-integer` / :ref:`stdWrap`
    :Default: 0

    Number of recursive levels for the pidInList.


..  _select-orderBy:

orderBy
-------

..  confval:: orderBy

    :Data type: *SQL-orderBy* / :ref:`stdWrap`

    ORDER BY clause without the words "ORDER BY".

    ..  rubric:: Example

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        orderBy = sorting, title


..  _select-groupBy:

groupBy
-------

..  confval:: groupBy

    :Data type: *SQL-groupBy* / :ref:`stdWrap`

    GROUP BY clause without the words "GROUP BY".

    ..  rubric:: Example

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        groupBy = CType


..  _select-max:

max
---

..  confval:: max

    :Data type: :ref:`data-type-integer` + :ref:`objects-calc` +"total" / :ref:`stdWrap`

    Max records

    **Special keyword:** "total" is substituted with :php:`count(*)`.


..  _select-begin:

begin
-----

..  confval:: begin

    :Data type: :ref:`data-type-integer` + :ref:`objects-calc` +"total" / :ref:`stdWrap`

    Begin with record number *value*.

    **Special keyword:** :typoscript:`total`
        Is substituted with :php:`count(*)`.


..  _select-where:

where
-----

..  confval:: where

    :Data type: *SQL-where* / :ref:`stdWrap`

    WHERE clause without the word "WHERE".

    ..  rubric:: Example

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        where = (title LIKE '%SOMETHING%' AND NOT doktype)

    Use `{#fieldname}` to make the database
    framework quote these fields:

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        where = ({#title} LIKE {#%SOMETHING%} AND NOT {#doktype})


..  _select-languageField:

languageField
-------------

..  confval:: languageField

    :Data type: :ref:`data-type-string` / :ref:`stdWrap`

    This defaults to whatever is defined in TCA "ctrl"-section in the
    "languageField". Change it to overwrite the behaviour in your query.

    By default all records that have language-relevant information in the
    TCA "ctrl"-section are translated on translated pages.

    This behaviour can be disabled by setting :typoscript:`languageField = 0`.


..  _select-includeRecordsWithoutDefaultTranslation:

includeRecordsWithoutDefaultTranslation
---------------------------------------

..  confval:: includeRecordsWithoutDefaultTranslation

    :Data type: :ref:`data-type-boolean` / :ref:`stdWrap`
    :Default: 0

    If content language overlay is activated and the option :typoscript:`languageField` is not disabled,
    :typoscript:`includeRecordsWithoutDefaultTranslation` allows to additionally fetch records,
    which do **not** have a parent in the default language.


..  _select-selectFields:

selectFields
------------

..  confval:: selectFields

    :Data type: :ref:`data-type-string` / :ref:`stdWrap`
    :Default: \*

    List of fields to select, or :php:`count(*)`.

    If the records need to be localized, please include the
    relevant localization-fields (uid, pid, languageField and
    transOrigPointerField). Otherwise the TYPO3 internal localization
    will not succeed.


..  _select-join:

join, leftjoin, rightjoin
-------------------------

..  confval:: join, leftjoin, rightjoin

    :Data type: :ref:`data-type-string` / :ref:`stdWrap`

    Enter the JOIN clause without :sql:`JOIN`, :sql:`LEFT OUTER JOIN` and :sql:`RIGHT OUTER JOIN`
    respectively.

    ..  rubric:: Example

    Fetch related `sys_category` records stored in the MM intermediate table:

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        10 = CONTENT
        10 {
           table = sys_category
           select {
              pidInList = root,-1
              selectFields = sys_category.*
              join = sys_category_record_mm mm ON mm.uid_local = sys_category.uid
              # ....
            }
        }

    See :ref:`select_pidInList` for more examples.


..  _select-markers:

markers
-------

..  confval:: markers

    :Data type: *(array of markers)*

    The markers defined in this section can be used, wrapped in the usual
    ###markername### way, in any other property of select. Each value is
    properly escaped and quoted to prevent SQL injection problems. This
    provides a way to safely use external data (e.g. database fields,
    GET/POST parameters) in a query.

    Available sub-properties:

    <markername>.value (value)
       Sets the value directly.

    <markername>.commaSeparatedList (:ref:`data-type-boolean`)
       If set, the value is interpreted as a comma-separated list of values.
       Each value in the list is individually escaped and quoted.

    (stdWrap properties ...)
       All stdWrap properties can be used for each markername.


    ..  warning::

        Since TYPO3 v8 there is a problem combining orderBy with markers caused
        by the quoting of the fields, see :issue:`87799`.

    ..  rubric:: Example

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        page.60 = CONTENT
        page.60 {
            table = tt_content
            select {
                pidInList = 73
                where = header != ###whatever###
                markers {
                    whatever.data = GP:first
                }
            }
        }

    This example selects all records from table tt_content, which are on page 73 and
    which don't have the header set to the value provided by the Get/Post variable
    "first".

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        page.60 = CONTENT
        page.60 {
            table = tt_content
            select {
                pidInList = 73
                where = header != ###whatever###
                markers {
                    whatever.value = some
                    whatever.wrap = |thing
                }
            }
        }


    This examples selects all records from the table tt_content which are on page 73
    and which don't have a header set to a value constructed by whatever.value and
    whatever.wrap ('something').


..  _selectQuotingOfFields:

Quoting of fields
=================

It is possible to use `{#fieldname}` to make the database
framework quote these fields (see :doc:`ext_core:Changelog/8.7/Important-80506-DbalCompatibleFieldQuotingInTypoScript`):

..  code-block:: typoscript
    :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

    select.where = ({#title} LIKE {#%SOMETHING%} AND NOT {#doktype})

This applies to:

*   :typoscript:`select.where`

but not to:

*   :typoscript:`select.groupBy`
*   :typoscript:`select.orderBy`

as these parameters already follow a stricter syntax that allow automatic parsing and
quoting.

Example
=======

See PHP source code for
:php:`\TYPO3\CMS\Frontend\ContentObject\ContentObjectRenderer`,
:php:`ContentObjectRenderer::getQuery()`,
:php:`ContentObjectRenderer::getWhere()`.

Condensed form:

..  code-block:: typoscript
    :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

    10 = CONTENT
    10 {
       table =
       select {
          uidInList =
          pidInList =
          recursive =
          orderBy =
          groupBy =
          max =
          begin =
          where =
          languageField =
          includeRecordsWithoutDefaultTranslation =
          selectFields =
          join =
          leftjoin =
          rightjoin =
       }
    }

See also:

*   :ref:`cobj-content`: for more complete examples with :typoscript:`select`
    and rendering the output with :typoscript:`renderObj`
*   :ref:`data-type-wrap`: enclosing results within text, used in some of the
    examples above
*   :ref:`stdWrap`: for more functionality, can be used in some of the properties,
    such as :typoscript:`pidInList`, :typoscript:`selectFields` etc.
