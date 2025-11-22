## `DateTime` functions and properties

### `DateTime.Now`

*(Formerly known as `GetTime`, which is now obsolete)*

```ags
readonly static DateTime* DateTime.Now;
```

Gets the current system time. You could use this for timing a loop, or
for effects like telling the player to go to bed, and so on.

A *DateTime* object is returned, which contains various properties that
you can use.

Note that the DateTime object that you get will not be kept up to date
with the current time; it will remain static with the time at which you
called DateTime.Now.

Example:

```ags
DateTime *dt = DateTime.Now;
Display("The date is: %02d/%02d/%04d", dt.DayOfMonth, dt.Month, dt.Year);
Display("The time is: %02d:%02d:%02d", dt.Hour, dt.Minute, dt.Second);
```

will display the current date and time in 24-hour format

*See also:* [`DateTime.DayOfMonth`](DateTime#datetimedayofmonth),
[`DateTime.Hour`](DateTime#datetimehour),
[`DateTime.Millisecond`](DateTime#datetimemillisecond),
[`DateTime.Minute`](DateTime#datetimeminute),
[`DateTime.Month`](DateTime#datetimemonth),
[`DateTime.RawTime`](DateTime#datetimerawtime),
[`DateTime.Second`](DateTime#datetimesecond),
[`DateTime.Year`](DateTime#datetimeyear)

---

### `DateTime.CreateFromDate`

```ags
static DateTime* DateTime.CreateFromDate(int year, int month, int day, int hour = 0, int minute = 0, int second = 0);
```

Returns a DateTime set as created.

Because of how DateTime is implemented in AGS engine, it only supports a range of dates between January 1970 to January 2038 (inclusive).
It returns invalid object if year is below 1970 or above 2038, or if any other value is invalid.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`DateTime.Now`](DateTime#datetimenow),
[`DateTime.CreateFromRawTime`](DateTime#datetimecreatefromrawtime)

---

### `DateTime.CreateFromRawTime`

```ags
static DateTime* DateTime.CreateFromRawTime(int rawTime);
```

Creates DateTime object from the provided raw time value (in seconds). 
It returns invalid object if rawTime is negative.

This function may be useful, for example, if you write "raw time" from a DateTime.Now in a file, and load it back later.

**NOTE:** the raw time mentioned here is sometimes also known as UNIX time, stored as a 32-bit signed integer.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`DateTime.RawTime`](DateTime#datetimerawtime),
[`DateTime.Now`](DateTime#datetimenow),
[`DateTime.CreateFromDate`](DateTime#datetimecreatefromdate)

---

### `DateTime.DayOfMonth`

```ags
readonly int DateTime.DayOfMonth;
```

Gets the day of the month represented by the DateTime object. This will
be from 1 to 31, representing the current day within the month.

Example: For an example, see [`DateTime.Now`](DateTime#datetimenow).

*See also:* [`DateTime.Now`](DateTime#datetimenow)

---

### `DateTime.Hour`

```ags
readonly int DateTime.Hour;
```

Gets the hour represented by the DateTime object. This will be from 0 to
23, representing the hour in 24-hour format.

Example: For an example, see [`DateTime.Now`](DateTime#datetimenow).

*See also:* [`DateTime.Now`](DateTime#datetimenow)

---

### `DateTime.Millisecond`

```ags
readonly int DateTime.Millisecond;
```

Gets the millisecond represented by the DateTime object. This will be from 0
to 999, meaning the milliseconds passed since the last second.

Millisecond property can be used to measure time periods in combination with [RawTime](DateTime#datetimerawtime), which reports a number of seconds passed since certain zero point. This may be useful to calculate duration of events in game when you need to know such duration in a higher precision.

Example:

```ags
DateTime* old_time = DateTime.Now;

// imagine some actions are performed here

DateTime* new_time = DateTime.Now;

int duration_ms = ((new_time.RawTime - old_time.RawTime) * 1000) + (new_time.Millisecond - old_time.Millisecond);
System.Log(eLogDebug, "Time passed: %d ms", duration_ms);
```

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`DateTime.Now`](DateTime#datetimenow),
[`DateTime.RawTime`](DateTime#datetimerawtime)

---

### `DateTime.Minute`

```ags
readonly int DateTime.Minute;
```

Gets the minute represented by the DateTime object. This will be from 0
to 59, representing the minute in 24-hour format.

Example: For an example, see [`DateTime.Now`](DateTime#datetimenow).

*See also:* [`DateTime.Now`](DateTime#datetimenow)

---

### `DateTime.Month`

```ags
readonly int DateTime.Month;
```

Gets the month represented by the DateTime object. This will be from 1
to 12, representing the month of the year.

Example: For an example, see [`DateTime.Now`](DateTime#datetimenow).

*See also:* [`DateTime.Now`](DateTime#datetimenow)

---

### `DateTime.RawTime`

*(Formerly known as `GetRawTime`, which is now obsolete)*

```ags
readonly int DateTime.RawTime;
```

This function returns the raw system time, as the number of seconds
since January 1970. While this value is not useful in itself, you can
use it to calculate time differences by getting the value at the start
of the game, for example, and then getting the value later on, and the
difference between the two is how much time has elapsed.

**NOTE:** Because this accesses the real-time clock on the users'
system, it is not a good idea to use this for long term timing tasks,
since if the user saves the game and then restores it later, the Time
value returned by this function will obviously include the difference
when they were not playing.

Example:

```ags
DateTime *dt = DateTime.Now;
int start_time = dt.RawTime;
Wait(120);
dt = DateTime.Now;
Display("After the wait it is now %d seconds later.", dt.RawTime - start_time);
```

should display that 3 seconds have elapsed.

*See also:* [`DateTime.Now`](DateTime#datetimenow),
[`SetTimer`](Globalfunctions_General#settimer)

---

### `DateTime.Second`

```ags
readonly int DateTime.Second;
```

Gets the second represented by the DateTime object. This will be from 0
to 59, representing the second.

Example: For an example, see [`DateTime.Now`](DateTime#datetimenow).

*See also:* [`DateTime.Now`](DateTime#datetimenow)

---

### `DateTime.Year`

```ags
readonly int DateTime.Year;
```

Gets the year represented by the DateTime object. This is the full year,
for example *2005*.

Example: For an example, see [`DateTime.Now`](DateTime#datetimenow).

*See also:* [`DateTime.Now`](DateTime#datetimenow)
