<small>Piwik\DataTable\Filter</small>

ExcludeLowPopulation
====================

Deletes all rows for which a specific column has a value that is lower than specific minimum threshold value.

Description
-----------

**Basic usage examples**

    // remove all countries from UserCountry.getCountry that have less than 3 visits
    $dataTable = // ... get a DataTable whose queued filters have been run ...
    $dataTable-&gt;filter(&#039;ExcludeLowPopulation&#039;, array(&#039;nb_visits&#039;, 3));

    // remove all countries from UserCountry.getCountry whose percent of total visits is less than 5%
    $dataTable = // ... get a DataTable whose queued filters have been run ...
    $dataTable-&gt;filter(&#039;ExcludeLowPopulation&#039;, array(&#039;nb_visits&#039;, false, 0.05));

    // remove all countries from UserCountry.getCountry whose bounce rate is less than 10%
    $dataTable = // ... get a DataTable that has a numerical bounce_rate column ...
    $dataTable-&gt;filter(&#039;ExcludeLowPopulation&#039;, array(&#039;bounce_rate&#039;, 0.10));


Constants
---------

This class defines the following constants:

- [`MINIMUM_SIGNIFICANT_PERCENTAGE_THRESHOLD`](#MINIMUM_SIGNIFICANT_PERCENTAGE_THRESHOLD)

Methods
-------

The class defines the following methods:

- [`__construct()`](#__construct) &mdash; Constructor.
- [`filter()`](#filter) &mdash; See [ExcludeLowPopulation](#).

### `__construct()` <a name="__construct"></a>

Constructor.

#### Signature

- It is a **public** method.
- It accepts the following parameter(s):
    - `$table` ([`DataTable`](../../../Piwik/DataTable.md))
    - `$columnToFilter`
    - `$minimumValue`
    - `$minimumPercentageThreshold`
- It does not return anything.

### `filter()` <a name="filter"></a>

See [ExcludeLowPopulation](#).

#### Signature

- It is a **public** method.
- It accepts the following parameter(s):
    - `$table`
- It does not return anything.
