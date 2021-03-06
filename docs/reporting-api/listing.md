# Reporting API Reference

This is the Piwik API Reference. It lists all functions that can be called, documents the parameters, and links to examples for every call in the various formats.

## API Request

### Standard API parameters

*   **idSite**

    *   the integer id of your website, eg. _idSite=1_
    *   you can also specify a list of idSites comma separated, eg. _idSite=1,4,5,6_
    *   if you want to get data for all websites, set _idSite=all_

*   **period** &mdash; the period you request the statistics for. Can be any of: _day_, _week_, _month_, _year_ or _range_. All reports are returned for the dates based on the website's time zone.

    *   **day** returns data for a given day.
    *   **week** returns data for the week that contains the specified 'date'
    *   **month** returns data for the month that contains the specified 'date'
    *   **year** returns data for the year that contains the specified 'date'
    *   **range** returns data for the specified 'date' range.

    For example to request a report for the range Jan 1st to Feb 15th you would write _&period=range&date=2011-01-01,2011-02-15_

*   **date**

    *   standard format = _YYYY-MM-DD_
    *   magic keywords = _today_ or _yesterday_. These are relative the website timezone. For example, for a website with UTC+12 timezone, "date=today" for an API request at 5PM UTC on 2010-01-01 will return the reports for 2010-01-02.
    *   range of dates

        *   _lastX_ for the last X periods including today (eg &date=last10&period=day would return an entry for each of the last 10 days including today). This is relative to the website timezone.
        *   _previousX_ returns the last X periods before today (eg. &date=last52&period=week will return an entry for each of the 52 weeks before this week). This is relative to the website timezone.
        *   _YYYY-MM-DD,YYYY-MM-DD_ for every period (day, week, month or year) in the date range

        *   Note: if you set 'period=range' to request data for a custom date range, the API will return the sum of data for the specified date range.
When 'period=range', the following keywords are supported for the parameter 'date':

        *   _lastX_
        *   _previousX_
        *   _YYYY-MM-DD,YYYY-MM-DD_, or _YYYY-MM-DD,today_ or _YYYY-MM-DD,yesterday_

*   **segment** &mdash; defines the Custom Segment you wish to filter your reports to.

    *   for example, 'referrerName==twitter.com' will return the requested API report, processed for the subset of users coming from twitter.com
    *   segments can be combined in AND and OR operations. For example, to filter for "Visits where (Referrer name is Google OR Referrer name is Bing) AND Country is India", you would write:
_referrerName==Google,referrerName==Bing;country==IN_

        *   see [segmentation documentation](/reporting-api/segmentation) for the list of available dimensions & metrics, example values for each, and more information about the custom segment parameter

*   **format**; defines the format of the output

    *   xml
    *   json (if you want to do [cross domain request in ajax](http://bob.pythonmac.org/archives/2005/12/05/remote-json-jsonp/) and get json data, you can wrap the json data around a function call by using the **jsoncallback** parameter)
    *   csv (comma-separated values)
    *   tsv (tab-separated values, similar to CSV but loads properly in Excel)
    *   html
    *   php; when you export in PHP format it is serialized by default (set _serialize=0_ to get the raw php data structure). You can have a visual output of the data by setting _prettyDisplay=1_
    *   rss (when **date** is a range for example date=last10 or date=previous15)
    *   original; to fetch the original PHP data structure. This is useful when you call the Piwik API [internally using the PHP code](/querying-the-reporting-api)

### Optional API parameters

Each API call can contain parameters that do not appear in the list of parameters, but act as "filters". Filters can be presentation filters (eg. specify the language for internationalization), or act as data helpers (sort results, search for a dataset subset, fetch children of a given entity).

Here is an overview of the parameters you can add to any API request, where applicable:

*   **language**; if specified, returns data strings that can be internationalized and will be translated. For example, dates and times returned by the Live API can be translated into the specified language. Expected value is the 2 language letters code, eg. en, fr, de, es, etc. You can get the available list of language by calling the LanguagesManager API.

*   **idSubtable** is used to request a subtable of a given row. In Piwik, some rows are linked to a sub-table. For example, each row in the Referers.getSearchEngines response have an "idsubdatatable" field. This integer idsubdatatable is the idSubtable of the table that contains all keywords for this search engine. You can then request the keywords for this search engine by calling Referers.getKeywordsFromSearchEngineId with the parameter idSubtable=X (replace X with the idsubdatatable value found in the Referers.getSearchEngines response, for the search engine you are interested in).

*   **expanded**; some API functions have a parameter 'expanded'. If 'expanded' is set to 1, the returned data will contain the first level results, as well as all sub-tables. See, for example, the [returned dataset for the Actions.getDownloads API function](http://demo.piwik.org/?module=API&method=Actions.getDownloads&idSite=7&period=month&date=today&format=xml&token_auth=anonymous&expanded=1).

*   **flat**; some API functions have a parameter 'expanded', which means that the data is hierarchical. For such API function, if 'flat' is set to 1, the returned data will contain the flattened view of the table data set. The children of all first level rows will be aggregated under one row. This is useful for example to see all Custom Variables names and values at once, for example, [Piwik forum user status](http://demo.piwik.org/index.php?module=API&method=CustomVariables.getCustomVariables&idSite=7&period=month&date=yesterday&format=xml&token_auth=anonymous&flat=1), or to see the full URLs not broken down by directory or structure.

*   **label**; this parameter can be used to search only for the row matching a given label. When specified, the report data will be filtered and return only the rows where the row label matches the specified parameter. For example you can set &label=Nice%20Keyword to keep only the row with a label "Nice Keyword".
There are also generic filters you can choose to apply on all APIs that return web analytics reports. For example, there is a filter for sorting by column, define start and number of rows to return, a filter to only return rows matching a given string,

*   **filter\_offset**; defines the offset of the starting row being returned
*   **filter\_limit**; defines the number of rows to be returned. Set to -1 to return all rows. By default, only the top 100 rows are returned.
*   **filter\_truncate**; if set, will truncate the table after $filter\_truncate rows. The last row will be named 'Others' (localized in the requested language) and the columns will be an aggregate of statistics of all truncated rows.
*   **filter\_pattern**; defines the text you want to search for in the **filter\_column**. Only the row with the given column matching the pattern will be returned.
*   **filter\_column** ; defines the column that we want to search for a text (see **filter\_pattern**). If not specified, defaults to 'label' (from Piwik 1.7)
*   **filter\_sort_order**; defines the order of the results, asc or desc
*   **filter\_sort\_column**; defines the column to be sorted by
*   **filter\_excludelowpop**; defines the column to use for the threshold of value **filter\_excludelowpop\_value**; only the columns with a value greater than **filter\_excludelowpop\_value**; will be returned
*   **filter\_excludelowpop\_value**; defines the minimum value for the **filter_excludelowpop** column
*   **hideColumns**; a comma separated list of columns. If set, removes those columns from the result. This can be used to reduce the amount of data transferred.
*   **showColumns**; a comma separated list of columns. If set, removes all columns in the result that are not found in this list. This can be used to reduce the amount of data transferred.

*   **filter\_column\_recursive**; defines the column to be searched for when recursively searching for a pattern _filter\_pattern\_recursive_
*   **filter\_pattern\_recursive**; defines the text you are searching for. Only the matching rows are returned. This filter is applied to recursive tables (Actions/Downloads/Outlinks tables)

*   **disable\_generic\_filters**; if set to 1, all the generic filters above will not be applied. This can be useful to disable the filters above which are otherwise applied with default values.
*   **disable\_queued\_filters**; if set to 1, all the filters that are mostly presentation filters (replace a column name, apply callbacks on the column to add new information such as the browser icon URL, etc.) will not be applied.

### Passing an array of data as a parameter

Some parameters can optionally accept arrays. For example, the urls parameter of SitesManager.addSite, SitesManager.addSiteAliasUrls, and SitesManager.updateSite allows for an array of urls to be passed. To pass an array add the bracket operators and an index to the parameter name in the get request. So, to call SitesManager.addSite with two urls you would use the following array:

[http://demo.piwik.org/?module=API&method=SitesManager.addSite&siteName=new%20example%20website&urls[0]=http://example.org&urls[1]=http://example-alias.org](http://demo.piwik.org/?module=API&method=SitesManager.addSite&siteName=new%20example%20website&urls[0]=http://example.org&urls[1]=http://example-alias.org)

### Advanced Users: Send multiple API Requests at once

Sometimes it is necessary to call the Piwik API a few times to get the data needed for a report or custom application. When you need to call many API functions simultaneously or if you just don't want to issue a lot of HTTP requests, you may want to consider using a **Bulk API Request**. This feature allows you to call several API methods with one HTTP request (either a GET or POST).

To issue a bulk request, call the API.getBulkRequest method and pass the API methods & parameters (each request must be [URL Encoded](http://php.net/manual/en/function.urlencode.php)) you wish to call in the 'urls' query parameter. For example, to call VisitsSummary.get & VisitorInterest.getNumberOfVisitsPerVisitDuration at the same time, you can use:

    [http] http://demo.piwik.org/?module=API&method=API.getBulkRequest&format=json&urls[0]=method%3dVisitsSummary.get%26idSite%3d1%26date%3d2012-03-06%26period%3dday&urls[1]=method%3dVisitorInterest.getNumberOfVisitsPerVisitDuration%26idSite%3d1%26date%3d2012-03-06%26period%3dday

Notice that urls[0] is the url-encoded call to VisitsSummary.get by itself and that urls[1] is what you would use to call VisitorInterest.getNumberOfVisitsPerVisitDuration by itself. The &format is specified only once (format=xml and format=json are supported for bulk requests).

The API Response will be an array containing the formatted result of each individual API method, in this case VisitsSummary.get and VisitorInterest.getNumberOfVisitsPerVisitDuration.

## Authenticate to the API via token_auth parameter

In the example above, the request works because the statistics are public (the _anonymous_ user has a _view_ access to the website). By default in Piwik your statistics are private. In the case that you cannot have your statistics to be public:

*   when you access your Piwik installation you are requested to log in
*   when you call the API over http you need to authenticate yourself
This is done by adding a secret parameter in the URL. This parameter is as secret as your login and password!

You can get this token in the _Manage Users_ admin area.

Then you simply have to add the parameter **&token\_auth=YOUR\_TOKEN** at the end of your API call URL.

## API Response: Metric Definitions

Here is a list of metrics returned by the API and their definition.

#### General Metrics

*   nb\_uniq\_visitors - Number of unique visitors
*   nb\_visits - Number of Visits (30 min of inactivity considered a new visit)
*   nb\_actions - Number of actions (page views, outlinks and downloads)
*   sum\_visit\_length - Total time spent, in seconds
*   bounce\_count - Number of visits that bounced (viewed only one page)
*   max\_actions - Maximum number of actions in a visit
*   nb\_visits\_converted - Number of visits that converted a goal
*   nb\_conversions - Number of goal conversions
*   revenue - Total revenue of goal conversions

#### Metrics Specific to Page URLs and Page Titles Reports

*   nb\_hits - Number of views on this page
*   entry\_nb\_visits - Number of visits that started on this page
*   entry\_nb\_uniq_visitors - Number of unique visitors that started their visit on this page
*   entry\_nb\_actions - Number of page views for visits that started on this page
*   entry\_sum\_visit_length - Time spent, in seconds, by visits that started on this page
*   entry\_bounce\_count - Number of visits that started on this page, and bounced (viewed only one page)
*   exit\_nb\_visits - Number of visits that finished on this page
*   exit\_nb\_uniq_visitors - Number of unique visitors that ended their visit on this page
*   sum\_time\_spent - Total time spent on this page, in seconds
*   sum\_daily\_nb_uniq_visitors - Sum of daily unique visitors over days in the period. Piwik doesn't process unique visitors across the full period.
*   sum\_daily\_entry\_nb\_uniq\_visitors - Sum of daily unique visitors that started their visit on this page
*   sum\_daily\_exit\_nb\_uniq\_visitors - (deprecated) Same as sum\_daily\_entry\_nb\_uniq\_visitors
*   exit\_bounce\_count - (deprecated) Same as entry\_bounce\_count

#### Processed metrics, appearing in the Metadata API response

*   avg\_time\_on\_page - Average time spent, in seconds, on this page
*   bounce\_rate - Ratio of visitors leaving the website after landing on this page
*   exit\_rate - Ratio of visitors that do not view any other page after this page

#### Ecommerce metrics, appearing in the Ecommerce Goals.getItems* API calls only:

*   revenue - The total revenue generated by Product sales. Excludes tax, shipping and discount.
*   quantity - Quantity is the total number of products sold for each Product SKU/Name/Category.
*   orders - It is the total number of Ecommerce orders which contained this Product SKU/Name/Category at least once.
*   abandoned_carts - This value is only set if the request contains '&abandonedCarts=1'. In this case, "orders" metrics will not be returned. It is the total number of abandoned carts which contained this Product SKU/Name/Category at least once.
*   avg\_price - The average revenue for this Product/Category.
*   avg\_quantity - The average quantity for this Product/Category.
*   nb\_visits - This value appears only if you have set up ['Ecommerce Product/Category page tracking'](http://piwik.org/docs/ecommerce-analytics/#toc-tracking-product-page-views-category-page-views-optional). The number of visits on the Product/Category page.
*   conversion\_rate - This value appears only if you have set up ['Ecommerce Product/Category page tracking'](http://piwik.org/docs/ecommerce-analytics/#toc-tracking-product-page-views-category-page-views-optional). The conversion rate is the number of orders (or abandoned\_carts if the request contains '&abandonedCarts=1') containing this product/category divided by number of visits on the product/category page.

## API Method List

[include url="http://demo.piwik.org/?module=API&action=listAllMethods&prefixUrl=http://demo.piwik.org/&idSite=7"]