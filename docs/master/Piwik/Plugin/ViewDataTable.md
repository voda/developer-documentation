<small>Piwik\Plugin</small>

ViewDataTable
=============

This class is used to load (from the API) and customize the output of a given DataTable.

Description
-----------

The main() method will create an object implementing ViewInterface
You can customize the dataTable using the disable* methods.

You can also customize the dataTable rendering using row metadata:
- &#039;html_label_prefix&#039;: If this metadata is present on a row, it&#039;s contents will be prepended
                       the label in the HTML output.
- &#039;html_label_suffix&#039;: If this metadata is present on a row, it&#039;s contents will be appended
                       after the label in the HTML output.

Example:
In the Controller of the plugin VisitorInterest
&lt;pre&gt;
   function getNumberOfVisitsPerVisitDuration( $fetch = false)
 {
       $view = ViewDataTable::factory( &#039;cloud&#039; );
       $view-&gt;init( $this-&gt;pluginName,  __FUNCTION__, &#039;VisitorInterest.getNumberOfVisitsPerVisitDuration&#039; );
       $view-&gt;setColumnsToDisplay( array(&#039;label&#039;,&#039;nb_visits&#039;) );
       $view-&gt;disableSort();
       $view-&gt;disableExcludeLowPopulation();
       $view-&gt;disableOffsetInformation();

       return $this-&gt;renderView($view, $fetch);
   }
&lt;/pre&gt;


Constants
---------

This abstract class defines the following constants:

- [`ID`](#ID)
- [`CONFIGURE_FOOTER_ICONS_EVENT`](#CONFIGURE_FOOTER_ICONS_EVENT)

Properties
----------

This abstract class defines the following properties:

- [`$config`](#$config)
- [`$requestConfig`](#$requestConfig)

### `$config` <a name="config"></a>

#### Signature

- It is a **public** property.
- It is a(n) `Piwik\ViewDataTable\Config` value.

### `$requestConfig` <a name="requestConfig"></a>

#### Signature

- It is a **public** property.
- It is a(n) `Piwik\ViewDataTable\RequestConfig` value.

Methods
-------

The abstract class defines the following methods:

- [`__construct()`](#__construct) &mdash; Default constructor.
- [`getDefaultConfig()`](#getDefaultConfig)
- [`getDefaultRequestConfig()`](#getDefaultRequestConfig)
- [`getViewDataTableId()`](#getViewDataTableId) &mdash; Returns the viewDataTable ID for this DataTable visualization.
- [`isViewDataTableId()`](#isViewDataTableId)
- [`getDataTable()`](#getDataTable) &mdash; Returns the DataTable loaded from the API
- [`setDataTable()`](#setDataTable) &mdash; To prevent calling an API multiple times, the DataTable can be set directly.
- [`getIdsWithInheritance()`](#getIdsWithInheritance) &mdash; Returns the viewDataTable IDs of a visualization&#039;s class lineage.
- [`render()`](#render) &mdash; Convenience function.
- [`getDefaultDataTableCssClass()`](#getDefaultDataTableCssClass)
- [`getVisualizationInfoFor()`](#getVisualizationInfoFor) &mdash; Returns an array mapping visualization IDs with information necessary for adding the visualizations to the footer of DataTable views.
- [`getOverridableProperties()`](#getOverridableProperties) &mdash; Returns the list of view properties that can be overriden by query parameters.
- [`isRequestingSingleDataTable()`](#isRequestingSingleDataTable)

### `__construct()` <a name="__construct"></a>

Default constructor.

#### Signature

- It is a **public** method.
- It accepts the following parameter(s):
    - `$controllerAction`
    - `$apiMethodToRequestDataTable`
- It does not return anything.

### `getDefaultConfig()` <a name="getDefaultConfig"></a>

#### Signature

- It is a **public static** method.
- It does not return anything.

### `getDefaultRequestConfig()` <a name="getDefaultRequestConfig"></a>

#### Signature

- It is a **public static** method.
- It does not return anything.

### `getViewDataTableId()` <a name="getViewDataTableId"></a>

Returns the viewDataTable ID for this DataTable visualization.

#### Description

Derived classes
should declare a const ID field with the viewDataTable ID.

#### Signature

- It is a **public static** method.
- It returns a(n) `string` value.
- It throws one of the following exceptions:
    - [`Exception`](http://php.net/class.Exception)

### `isViewDataTableId()` <a name="isViewDataTableId"></a>

#### Signature

- It is a **public** method.
- It accepts the following parameter(s):
    - `$viewDataTableId`
- It does not return anything.

### `getDataTable()` <a name="getDataTable"></a>

Returns the DataTable loaded from the API

#### Signature

- It is a **public** method.
- It returns a(n) [`DataTable`](../../Piwik/DataTable.md) value.
- It throws one of the following exceptions:
    - [`Exception`](http://php.net/class.Exception) &mdash; if not yet defined

### `setDataTable()` <a name="setDataTable"></a>

To prevent calling an API multiple times, the DataTable can be set directly.

#### Description

It won&#039;t be loaded again from the API in this case

#### Signature

- It is a **public** method.
- It accepts the following parameter(s):
    - `$dataTable`
- _Returns:_ $dataTable DataTable
    - `void`

### `getIdsWithInheritance()` <a name="getIdsWithInheritance"></a>

Returns the viewDataTable IDs of a visualization&#039;s class lineage.

#### See Also

- `self::getVisualizationClassLineage`

#### Signature

- It is a **public static** method.
- It accepts the following parameter(s):
    - `$klass`
- It returns a(n) `array` value.

### `render()` <a name="render"></a>

Convenience function.

#### Description

Calls main() &amp; renders the view that gets built.

#### Signature

- It is a **public** method.
- _Returns:_ The result of rendering.
    - `string`

### `getDefaultDataTableCssClass()` <a name="getDefaultDataTableCssClass"></a>

#### Signature

- It is a **public** method.
- It does not return anything.

### `getVisualizationInfoFor()` <a name="getVisualizationInfoFor"></a>

Returns an array mapping visualization IDs with information necessary for adding the visualizations to the footer of DataTable views.

#### Signature

- It is a **public static** method.
- It accepts the following parameter(s):
    - `$visualizations`
- It returns a(n) `array` value.

### `getOverridableProperties()` <a name="getOverridableProperties"></a>

Returns the list of view properties that can be overriden by query parameters.

#### Signature

- It is a **public** method.
- It returns a(n) `array` value.

### `isRequestingSingleDataTable()` <a name="isRequestingSingleDataTable"></a>

#### Signature

- It is a **public** method.
- It does not return anything.
