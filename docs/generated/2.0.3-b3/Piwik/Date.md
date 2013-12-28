<small>Piwik\</small>

Date
====

Utility class that wraps date/time related PHP functions.

Using this class can
be easier than using `date`, `time`, `date_default_timezone_set`, etc.

### Performance concerns

The helper methods in this class are instance methods and thus `Date` instances
need to be constructed before they can be used. The memory allocation can result
in noticeable performance degradation if you construct thousands of Date instances,
say, in a loop.

### Examples

**Basic usage**

    $date = Date::factory('2007-07-24 14:04:24', 'EST');
    $date->addHour(5);
    echo $date->getLocalized("%longDay% the %day% of %longMonth% at %time%");

Methods
-------

The class defines the following methods:

- [`factory()`](#factory) &mdash; Creates a new Date instance using a string datetime value.
- [`getDatetime()`](#getdatetime) &mdash; Returns the current timestamp as a string with the following format: `'YYYY-MM-DD HH:MM:SS'`.
- [`getDateStartUTC()`](#getdatestartutc) &mdash; Returns the start of the day of the current timestamp in UTC.
- [`getDateEndUTC()`](#getdateendutc) &mdash; Returns the end of the day of the current timestamp in UTC.
- [`setTimezone()`](#settimezone) &mdash; Returns a new date object with the same timestamp as `$this` but with a new timezone.
- [`adjustForTimezone()`](#adjustfortimezone) &mdash; Converts a timestamp in a from UTC to a timezone.
- [`getTimestampUTC()`](#gettimestamputc) &mdash; Returns the Unix timestamp of the date in UTC.
- [`getTimestamp()`](#gettimestamp) &mdash; Returns the unix timestamp of the date in UTC, converted from the current timestamp timezone.
- [`isLater()`](#islater) &mdash; Returns `true` if the current date is older than the given `$date`.
- [`isEarlier()`](#isearlier) &mdash; Returns `true` if the current date is earlier than the given `$date`.
- [`toString()`](#tostring) &mdash; Converts this date to the requested string format.
- [`__toString()`](#__tostring) &mdash; See [toString()](/api-reference/Piwik/Date#tostring).
- [`compareWeek()`](#compareweek) &mdash; Performs three-way comparison of the week of the current date against the given `$date`'s week.
- [`compareMonth()`](#comparemonth) &mdash; Performs three-way comparison of the month of the current date against the given `$date`'s month.
- [`compareYear()`](#compareyear) &mdash; Performs three-way comparison of the month of the current date against the given `$date`'s year.
- [`isToday()`](#istoday) &mdash; Returns `true` if current date is today.
- [`now()`](#now) &mdash; Returns a date object set to now in UTC (same as [today()](/api-reference/Piwik/Date#today), except that the time is also set).
- [`today()`](#today) &mdash; Returns a date object set to today at midnight in UTC.
- [`yesterday()`](#yesterday) &mdash; Returns a date object set to yesterday at midnight in UTC.
- [`yesterdaySameTime()`](#yesterdaysametime) &mdash; Returns a date object set to yesterday with the current time of day in UTC.
- [`setTime()`](#settime) &mdash; Returns a new Date instance with `$this` date's day and the specified new time of day.
- [`setDay()`](#setday) &mdash; Returns a new Date instance with `$this` date's time of day and the day specified by `$day`.
- [`setYear()`](#setyear) &mdash; Returns a new Date instance with `$this` date's time of day, month and day, but with a new year (specified by `$year`).
- [`subDay()`](#subday) &mdash; Subtracts `$n` number of days from `$this` date and returns a new Date object.
- [`subWeek()`](#subweek) &mdash; Subtracts `$n` weeks from `$this` date and returns a new Date object.
- [`subMonth()`](#submonth) &mdash; Subtracts `$n` months from `$this` date and returns the result as a new Date object.
- [`subYear()`](#subyear) &mdash; Subtracts `$n` years from `$this` date and returns the result as a new Date object.
- [`getLocalized()`](#getlocalized) &mdash; Returns a localized date string using the given template.
- [`addDay()`](#addday) &mdash; Adds `$n` days to `$this` date and returns the result in a new Date.
- [`addHour()`](#addhour) &mdash; Adds `$n` hours to `$this` date and returns the result in a new Date.
- [`addHourTo()`](#addhourto) &mdash; Adds N number of hours to a UNIX timestamp and returns the result.
- [`subHour()`](#subhour) &mdash; Subtracts `$n` hours from `$this` date and returns the result in a new Date.
- [`addPeriod()`](#addperiod) &mdash; Adds a period to `$this` date and returns the result in a new Date instance.
- [`subPeriod()`](#subperiod) &mdash; Subtracts a period from `$this` date and returns the result in a new Date instance.
- [`secondsToDays()`](#secondstodays) &mdash; Returns the number of days represented by a number of seconds.

<a name="factory" id="factory"></a>
<a name="factory" id="factory"></a>
### `factory()`

Creates a new Date instance using a string datetime value.

The timezone of the Date
result will be in UTC.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$dateString` (`string`|`int`) &mdash;

      <div markdown="1" class="param-desc"> `'today'`, `'yesterday'`, `'now'`, `'yesterdaySameTime'`, a string with `'YYYY-MM-DD HH:MM:SS'` format or a unix timestamp.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   <li>
      <div markdown="1" class="parameter">
      `$timezone` (`string`) &mdash;

      <div markdown="1" class="param-desc"> The timezone of the result. If specified, `$dateString` will be converted from UTC to this timezone before being used in the Date return value.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a [`Date`](../Piwik/Date.md) value.
- It throws one of the following exceptions:
    - [`Exception`](http://php.net/class.Exception) &mdash; If `$dateString` is in an invalid format or if the time is before Tue, 06 Aug 1991.

<a name="getdatetime" id="getdatetime"></a>
<a name="getDatetime" id="getDatetime"></a>
### `getDatetime()`

Returns the current timestamp as a string with the following format: `'YYYY-MM-DD HH:MM:SS'`.

#### Signature

- It returns a `string` value.

<a name="getdatestartutc" id="getdatestartutc"></a>
<a name="getDateStartUTC" id="getDateStartUTC"></a>
### `getDateStartUTC()`

Returns the start of the day of the current timestamp in UTC.

For example,
if the current timestamp is `'2007-07-24 14:04:24'` in UTC, the result will
be `'2007-07-24'`.

#### Signature

- It returns a `string` value.

<a name="getdateendutc" id="getdateendutc"></a>
<a name="getDateEndUTC" id="getDateEndUTC"></a>
### `getDateEndUTC()`

Returns the end of the day of the current timestamp in UTC.

For example,
if the current timestamp is `'2007-07-24 14:03:24'` in UTC, the result will
be `'2007-07-24 23:59:59'`.

#### Signature

- It returns a `string` value.

<a name="settimezone" id="settimezone"></a>
<a name="setTimezone" id="setTimezone"></a>
### `setTimezone()`

Returns a new date object with the same timestamp as `$this` but with a new timezone.

See [getTimestamp()](/api-reference/Piwik/Date#gettimestamp) to see how the timezone is used.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$timezone` (`string`) &mdash;

      <div markdown="1" class="param-desc"> eg, `'UTC'`, `'Europe/London'`, etc.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a [`Date`](../Piwik/Date.md) value.

<a name="adjustfortimezone" id="adjustfortimezone"></a>
<a name="adjustForTimezone" id="adjustForTimezone"></a>
### `adjustForTimezone()`

Converts a timestamp in a from UTC to a timezone.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$timestamp` (`int`) &mdash;

      <div markdown="1" class="param-desc"> The UNIX timestamp to adjust.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   <li>
      <div markdown="1" class="parameter">
      `$timezone` (`string`) &mdash;

      <div markdown="1" class="param-desc"> The timezone to adjust to.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>

<ul>
  <li>
    <div markdown="1" class="parameter">
    _Returns:_  (`int`) &mdash;
    <div markdown="1" class="param-desc">The adjusted time as seconds from EPOCH.</div>

    <div style="clear:both;"/>

    </div>
  </li>
</ul>

<a name="gettimestamputc" id="gettimestamputc"></a>
<a name="getTimestampUTC" id="getTimestampUTC"></a>
### `getTimestampUTC()`

Returns the Unix timestamp of the date in UTC.

#### Signature

- It returns a `int` value.

<a name="gettimestamp" id="gettimestamp"></a>
<a name="getTimestamp" id="getTimestamp"></a>
### `getTimestamp()`

Returns the unix timestamp of the date in UTC, converted from the current timestamp timezone.

#### Signature

- It returns a `int` value.

<a name="islater" id="islater"></a>
<a name="isLater" id="isLater"></a>
### `isLater()`

Returns `true` if the current date is older than the given `$date`.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$date` ([`Date`](../Piwik/Date.md)) &mdash;

      <div markdown="1" class="param-desc"></div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a `bool` value.

<a name="isearlier" id="isearlier"></a>
<a name="isEarlier" id="isEarlier"></a>
### `isEarlier()`

Returns `true` if the current date is earlier than the given `$date`.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$date` ([`Date`](../Piwik/Date.md)) &mdash;

      <div markdown="1" class="param-desc"></div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a `bool` value.

<a name="tostring" id="tostring"></a>
<a name="toString" id="toString"></a>
### `toString()`

Converts this date to the requested string format.

See [http://php.net/date](http://php.net/date)
for the list of format strings.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$format` (`string`) &mdash;

      <div markdown="1" class="param-desc"></div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a `string` value.

<a name="__tostring" id="__tostring"></a>
<a name="__toString" id="__toString"></a>
### `__toString()`

See [toString()](/api-reference/Piwik/Date#tostring).

#### Signature


<ul>
  <li>
    <div markdown="1" class="parameter">
    _Returns:_  (`string`) &mdash;
    <div markdown="1" class="param-desc">The current date in `'YYYY-MM-DD'` format.</div>

    <div style="clear:both;"/>

    </div>
  </li>
</ul>

<a name="compareweek" id="compareweek"></a>
<a name="compareWeek" id="compareWeek"></a>
### `compareWeek()`

Performs three-way comparison of the week of the current date against the given `$date`'s week.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$date` ([`Date`](../Piwik/Date.md)) &mdash;

      <div markdown="1" class="param-desc"></div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>

<ul>
  <li>
    <div markdown="1" class="parameter">
    _Returns:_  (`int`) &mdash;
    <div markdown="1" class="param-desc">Returns `0` if the current week is equal to `$date`'s, `-1` if the current week is earlier or `1` if the current week is later.</div>

    <div style="clear:both;"/>

    </div>
  </li>
</ul>

<a name="comparemonth" id="comparemonth"></a>
<a name="compareMonth" id="compareMonth"></a>
### `compareMonth()`

Performs three-way comparison of the month of the current date against the given `$date`'s month.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$date` ([`Date`](../Piwik/Date.md)) &mdash;

      <div markdown="1" class="param-desc"> Month to compare</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>

<ul>
  <li>
    <div markdown="1" class="parameter">
    _Returns:_  (`int`) &mdash;
    <div markdown="1" class="param-desc">Returns `0` if the current month is equal to `$date`'s, `-1` if the current month is earlier or `1` if the current month is later.</div>

    <div style="clear:both;"/>

    </div>
  </li>
</ul>

<a name="compareyear" id="compareyear"></a>
<a name="compareYear" id="compareYear"></a>
### `compareYear()`

Performs three-way comparison of the month of the current date against the given `$date`'s year.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$date` ([`Date`](../Piwik/Date.md)) &mdash;

      <div markdown="1" class="param-desc"> Year to compare</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>

<ul>
  <li>
    <div markdown="1" class="parameter">
    _Returns:_  (`int`) &mdash;
    <div markdown="1" class="param-desc">Returns `0` if the current year is equal to `$date`'s, `-1` if the current year is earlier or `1` if the current year is later.</div>

    <div style="clear:both;"/>

    </div>
  </li>
</ul>

<a name="istoday" id="istoday"></a>
<a name="isToday" id="isToday"></a>
### `isToday()`

Returns `true` if current date is today.

#### Signature

- It returns a `bool` value.

<a name="now" id="now"></a>
<a name="now" id="now"></a>
### `now()`

Returns a date object set to now in UTC (same as [today()](/api-reference/Piwik/Date#today), except that the time is also set).

#### Signature

- It returns a [`Date`](../Piwik/Date.md) value.

<a name="today" id="today"></a>
<a name="today" id="today"></a>
### `today()`

Returns a date object set to today at midnight in UTC.

#### Signature

- It returns a [`Date`](../Piwik/Date.md) value.

<a name="yesterday" id="yesterday"></a>
<a name="yesterday" id="yesterday"></a>
### `yesterday()`

Returns a date object set to yesterday at midnight in UTC.

#### Signature

- It returns a [`Date`](../Piwik/Date.md) value.

<a name="yesterdaysametime" id="yesterdaysametime"></a>
<a name="yesterdaySameTime" id="yesterdaySameTime"></a>
### `yesterdaySameTime()`

Returns a date object set to yesterday with the current time of day in UTC.

#### Signature

- It returns a [`Date`](../Piwik/Date.md) value.

<a name="settime" id="settime"></a>
<a name="setTime" id="setTime"></a>
### `setTime()`

Returns a new Date instance with `$this` date's day and the specified new time of day.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$time` (`string`) &mdash;

      <div markdown="1" class="param-desc"> String in the `'HH:MM:SS'` format.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>

<ul>
  <li>
    <div markdown="1" class="parameter">
    _Returns:_  ([`Date`](../Piwik/Date.md)) &mdash;
    <div markdown="1" class="param-desc">The new date with the time of day changed.</div>

    <div style="clear:both;"/>

    </div>
  </li>
</ul>

<a name="setday" id="setday"></a>
<a name="setDay" id="setDay"></a>
### `setDay()`

Returns a new Date instance with `$this` date's time of day and the day specified by `$day`.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$day` (`int`) &mdash;

      <div markdown="1" class="param-desc"> The day eg. `31`.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a [`Date`](../Piwik/Date.md) value.

<a name="setyear" id="setyear"></a>
<a name="setYear" id="setYear"></a>
### `setYear()`

Returns a new Date instance with `$this` date's time of day, month and day, but with a new year (specified by `$year`).

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$year` (`int`) &mdash;

      <div markdown="1" class="param-desc"> The year, eg. `2010`.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a [`Date`](../Piwik/Date.md) value.

<a name="subday" id="subday"></a>
<a name="subDay" id="subDay"></a>
### `subDay()`

Subtracts `$n` number of days from `$this` date and returns a new Date object.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$n` (`int`) &mdash;

      <div markdown="1" class="param-desc"> An integer > 0.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a [`Date`](../Piwik/Date.md) value.

<a name="subweek" id="subweek"></a>
<a name="subWeek" id="subWeek"></a>
### `subWeek()`

Subtracts `$n` weeks from `$this` date and returns a new Date object.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$n` (`int`) &mdash;

      <div markdown="1" class="param-desc"> An integer > 0.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a [`Date`](../Piwik/Date.md) value.

<a name="submonth" id="submonth"></a>
<a name="subMonth" id="subMonth"></a>
### `subMonth()`

Subtracts `$n` months from `$this` date and returns the result as a new Date object.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$n` (`int`) &mdash;

      <div markdown="1" class="param-desc"> An integer > 0.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>

<ul>
  <li>
    <div markdown="1" class="parameter">
    _Returns:_  ([`Date`](../Piwik/Date.md)) &mdash;
    <div markdown="1" class="param-desc">new date</div>

    <div style="clear:both;"/>

    </div>
  </li>
</ul>

<a name="subyear" id="subyear"></a>
<a name="subYear" id="subYear"></a>
### `subYear()`

Subtracts `$n` years from `$this` date and returns the result as a new Date object.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$n` (`int`) &mdash;

      <div markdown="1" class="param-desc"> An integer > 0.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a [`Date`](../Piwik/Date.md) value.

<a name="getlocalized" id="getlocalized"></a>
<a name="getLocalized" id="getLocalized"></a>
### `getLocalized()`

Returns a localized date string using the given template.

The template should contain tags that will be replaced with localized date strings.

Allowed tags include:

- **%day%**: replaced with the day of the month without leading zeros, eg, **1** or **20**.
- **%shortMonth%**: the short month in the current language, eg, **Jan**, **Feb**.
- **%longMonth%**: the whole month name in the current language, eg, **January**, **February**.
- **%shortDay%**: the short day name in the current language, eg, **Mon**, **Tue**.
- **%longDay%**: the long day name in the current language, eg, **Monday**, **Tuesday**.
- **%longYear%**: the four digit year, eg, **2007**, **2013**.
- **%shortYear%**: the two digit year, eg, **07**, **13**.
- **%time%**: the time of day, eg, **07:35:00**, or **15:45:00**.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$template` (`string`) &mdash;

      <div markdown="1" class="param-desc"> eg. `"%shortMonth% %longYear%"`</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>

<ul>
  <li>
    <div markdown="1" class="parameter">
    _Returns:_  (`string`) &mdash;
    <div markdown="1" class="param-desc">eg. `"Aug 2009"`</div>

    <div style="clear:both;"/>

    </div>
  </li>
</ul>

<a name="addday" id="addday"></a>
<a name="addDay" id="addDay"></a>
### `addDay()`

Adds `$n` days to `$this` date and returns the result in a new Date.

instance.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$n` (`int`) &mdash;

      <div markdown="1" class="param-desc"> Number of days to add, must be > 0.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a [`Date`](../Piwik/Date.md) value.

<a name="addhour" id="addhour"></a>
<a name="addHour" id="addHour"></a>
### `addHour()`

Adds `$n` hours to `$this` date and returns the result in a new Date.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$n` (`int`) &mdash;

      <div markdown="1" class="param-desc"> Number of hours to add. Can be less than 0.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a [`Date`](../Piwik/Date.md) value.

<a name="addhourto" id="addhourto"></a>
<a name="addHourTo" id="addHourTo"></a>
### `addHourTo()`

Adds N number of hours to a UNIX timestamp and returns the result.

Using
this static function instead of [addHour()](/api-reference/Piwik/Date#addhour) will be faster since a
Date instance does not have to be created.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$timestamp` (`int`) &mdash;

      <div markdown="1" class="param-desc"> The timestamp to add to.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   <li>
      <div markdown="1" class="parameter">
      `$n` (`Piwik\number`) &mdash;

      <div markdown="1" class="param-desc"> Number of hours to add, must be > 0.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>

<ul>
  <li>
    <div markdown="1" class="parameter">
    _Returns:_  (`int`) &mdash;
    <div markdown="1" class="param-desc">The result as a UNIX timestamp.</div>

    <div style="clear:both;"/>

    </div>
  </li>
</ul>

<a name="subhour" id="subhour"></a>
<a name="subHour" id="subHour"></a>
### `subHour()`

Subtracts `$n` hours from `$this` date and returns the result in a new Date.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$n` (`int`) &mdash;

      <div markdown="1" class="param-desc"> Number of hours to subtract. Can be less than 0.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a [`Date`](../Piwik/Date.md) value.

<a name="addperiod" id="addperiod"></a>
<a name="addPeriod" id="addPeriod"></a>
### `addPeriod()`

Adds a period to `$this` date and returns the result in a new Date instance.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$n` (`int`) &mdash;

      <div markdown="1" class="param-desc"> The number of periods to add. Can be negative.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   <li>
      <div markdown="1" class="parameter">
      `$period` (`string`) &mdash;

      <div markdown="1" class="param-desc"> The type of period to add (YEAR, MONTH, WEEK, DAY, ...)</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a [`Date`](../Piwik/Date.md) value.

<a name="subperiod" id="subperiod"></a>
<a name="subPeriod" id="subPeriod"></a>
### `subPeriod()`

Subtracts a period from `$this` date and returns the result in a new Date instance.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$n` (`int`) &mdash;

      <div markdown="1" class="param-desc"> The number of periods to add. Can be negative.</div>

      <div style="clear:both;"/>

      </div>
   </li>
   <li>
      <div markdown="1" class="parameter">
      `$period` (`string`) &mdash;

      <div markdown="1" class="param-desc"> The type of period to add (YEAR, MONTH, WEEK, DAY, ...)</div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a [`Date`](../Piwik/Date.md) value.

<a name="secondstodays" id="secondstodays"></a>
<a name="secondsToDays" id="secondsToDays"></a>
### `secondsToDays()`

Returns the number of days represented by a number of seconds.

#### Signature

-  It accepts the following parameter(s):

   <ul>
   <li>
      <div markdown="1" class="parameter">
      `$secs` (`int`) &mdash;

      <div markdown="1" class="param-desc"></div>

      <div style="clear:both;"/>

      </div>
   </li>
   </ul>
- It returns a `float` value.
