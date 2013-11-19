# Visualizing Report Data

<!-- Meta (to be deleted)
Purpose: - explain how to display report data (explain how to use ViewDataTable)
- explain how visualizations work (& what visualizations are available)
- explain how new visualizations are created
- explain how to make widgets that are displayed embeddable in the dashboard

Audience: plugin developers who want to display new reports or want to create new visualizations

Expected Result: plugin developers who understand how ViewDataTable can be used and fits in w/ Piwik. who know what types of visualizations are available. and devs who know how to make new visualizations.

Notes: should introduce concept of creating new visualization? or perhaps the heading will be enough.

What's missing? (stuff in my list that was not in when I wrote the 1st draft)
-->

## About this guide

**Read this guide if**

* you'd like to know **how to display your reports the way existing reports are displayed**
* you'd like to know **the different ways your reports can be displayed**
* you'd like to know **how to get your report to display in the dashboard**
* you'd like to know **how to create new visualizations**

**Guide assumptions**

This guide assumes that you:

* can code in PHP,
* have a general understanding of extending Piwik (if not, read our [Getting Started](#) guide),
* and understand the purpose of [Piwik controllers](#) and [Piwik APIs](#).

## Displaying Analytics Reports

In Piwik, an analytics report is just a set of two-dimensional data (stored internally in [DataTable](#) instances). They are returned by API methods. Controller methods can output visualizations of these reports by using the [ViewDataTable](#) class.

### Using ViewDataTable

Report visualizations can be in any format. The **sparkline** visualization for example outputs an image. Most visualizations, however, will output an HTML display that allows users to switch between different visualizations. You've very likely seen this display before:

TODO: image of datatable view

There are two ways to output this display for a report. The most succinct way is to call the [ViewDataTable\Factory::renderReport](#) method:

    // controller method for the MyPlugin.myReport report
    public function myReport($fetch = false)
    {
        return \Piwik\ViewDataTable\Factory::renderReport($this->pluginName, __FUNCTION__, $fetch);
    }

[renderReport](#) will create a new [ViewDataTable](#) instance and render it. The report can be configured via the [ViewDataTable.configure](#) event.

The other way to output the display is to manually create and configure a [ViewDataTable](#) instance:

    // controller method for the MyPlugin.myReport report
    public function myReport($fetch = false)
    {
        $view = \Piwik\ViewDataTable\Factory::build(
            $defaultType = 'table',
            $apiAction = 'MyPlugin.myReport',
            $controllerMethod = 'MyPlugin.myReport',
        );
        $view->config->show_limit_control = false;
        $view->config->show_search = false;
        $view->config->show_goals = true;

        // ... do some more configuration ...

        if ($fetch) {
            return $view->render();
        } else {
            echo $view->render();
        }
    }

The visualization type is specified in the first argument to [\Piwik\ViewDataTable\Factory::build](#).

When [ViewDataTable::render](#) is called, the [ViewDataTable](#) instance will load a report through the reporting API (using the [Piwik\API\Request](#) class) and output an HTML display using Twig templates.

To learn about the different ways to configure ViewDataTable instances, read [the ViewDataTable class docs](#).

_Note: The display generated by ViewDataTable will reload the report using AJAX, which means there must be a way to get the display for just the report in question. **Therefore, you must create a dedicated controller method for each of your reports.** You can use a ViewDataTable instance outside of a controller method, but it must still reference such a controller method._

### Displaying reports on a page

Once there exists a controller method for a report, displaying it on a page in Piwik is straightforward. Assuming you've [exposed a controller method as a menu item](#), you can then [reuse your report's controller method](#) to include the report display in the menu item page:

    // controller method exposed as a menu item
    public function index()
    {
        $view = new View("@MyPlugin/index.twig");
        $view->myReport = $this->myReport($fetch = true);
        echo $view->render();
    }

    // report method
    public function myReport($fetch = false)
    {
        return \Piwik\ViewDataTable\Factory::renderReport($this->pluginName, __FUNCTION__, $fetch);
    }

The **index.twig** template will look like this:

    <h1>My Report</h1>

    {{ myReport }}

### Displaying reports in the Dashboard

After a controller method is created for a report, the report can be made available to the dashboard by using the [WidgetsList.addWidgets](#) event:

    // event handler for the WidgetsList.addWidgets event in the MyPlugin/MyPlugin.php file
    public function addWidgets()
    {
        WidgetsList::add('My Category Name', 'My Report Title', 'MyPlugin', 'myReport');
    }

Piwik users will then be able to see and select **myReport** in the widget selector.

_Note: Any controller method can be embedded in the dashboard, not just reports. So if you have a popup that you'd like to make available as a dashboard widget, you can use [WidgetsList.addWidgets](#) to do so. This is exactly how we made the [Visitor Profile](#) available in the dashboard._

### About visualizations

A report visualization is a class that extends from [ViewDataTable](#). Every visualization has a unique ID that is specified by a class constant named **ID**. They are referred to by this ID, not their class name, because they must be able to be specified in the **viewDataTable** query parameter. Using a fully qualified class name would be too verbose.

The **viewDataTable** query parameter is set by [ViewDataTable](#)'s associated JavaScript code. It represents the user's choice of view (made by clicking on a visualization's footer icon) and thus will override any visualization type specified in code.

Users can switch visualizations by clicking on one of the icons displayed below the visualization itself:

TODO: image

These icons are called **footer icons**. Not all visualizations have to be available in this manner. For example, the [Visitor Log](#) uses its own visualization, but since it can only use data from one report ([Live.getLastVisitsDetails](#)) it is not available in the footer.

To learn more about the controls that surround a visualization in most report displays, [read the user docs for report visualizations](#).

### Core Visualizations

The following are a set of visualizations, called **Core Visualizations**, that come pre-packaged with Piwik (visualizations are listed by their **viewDataTable** ID:

TODO: IDs should link to images?

* **table**: This is the main visualization used to view report data. It is essentially a table of rows and columns.
* **tableAllColumns**: The same as **table** except a couple extra processed metrics are calculated and displayed using the report data.
* **tableGoals**: The same as **table** except goal metrics are processed and displayed for the report and each goal of the website the report is for.
* **graphVerticalBar**: Displays report data as a vertical bar graph. _Uses jqPlot._
* **graphPie**: Displays report data as a pie graph. _Uses jqPlot._
* **graphEvolution**: Displays a set of reports over time as a line graph. Metrics are displayed as different series.
* **cloud**: Displays report labels in a tag cloud using the values of a specified metric as significance.
* **sparkline**: Displays the values of one metric over time as a line graph. Outputs an image as opposed to HTML.

You can use these visualizations when creating [ViewDataTable](#) instances without having to install third-party plugins.

## Creating new Visualizations

Plugins can provide their own visualizations, either for use just within the plugin or as a new visualization that can be applied to any report. The [Treemap Visualization](#) plugin is an example of this:

TODO: treemap image

This section will explain how [ViewDataTable](#) works and everything you can do when extending it.

### Extending ViewDataTable

The first step in creating a new visualization is to create a class that descends from [ViewDataTable](#). If you are creating a visualization that will not output HTML, then extend directly from [ViewDataTable](#). If your visualization will be in HTML, extend from [Visualization](#).

[Visualization](#) is a direct descendant of [ViewDataTable](#) that provides common behavior you see in all core visualizations. That is, it shows the footer icons, the limit selector, the report documentation, the search box and more.

**Setting ViewDataTable metadata**

In your new class that derives from [ViewDataTable](#) or [Visualization](#) the first thing you'll want to do is set the necessary visualization metadata. All visualization metadata are stored as class constants. You can set the following constants:

* **ID**: _(required)_ The unique ID for this visualization. This is what gets supplied in the **viewDataTable** query paremeter.
* **TEMPLATE\_FILE**: _(required)_ A reference to a Twig template file, eg, `"@MyPlugin/_myDataTableViz.twig"`.
* **FOOTER\_ICON**: A path from Piwik's root directory to an icon to display in the footer of the visualization.
* **FOOTER\_ICON\_TITLE**: The tooltip to use when the user hovers over the footer icon. Can be a [translation token](#).

An example:

    class MyVisualization extends Visualization
    {
        const ID = 'myvisualization';
        const TEMPLATE_FILE = '@MyPlugin/_myVisualization.twig';
        const FOOTER_ICON = 'plugins/MyPlugin/images/myvisualization.png';
        const FOOTER_ICON_TITLE = 'My Visualization';
    }

**Adding new display properties**

Report visualizations can be configured by setting display properties.

**Changing what data is loaded**

**Manipulating report data before displaying**

**Making your visualization available to Piwik**

### Adding custom JavaScript

### Adding extra UI controls

### Adding new UI widgets

### Styling your visualization

### Making your visualization theme-able

<!-- detail process (server side + client side) (graph, metrics picker, adding UI widgets to datatable display, ???) -->

## Learn more

<!-- link to All about analytics -->
<!-- show treemap as example of new visualization -->