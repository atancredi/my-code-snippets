
- [**GMT**](http://greenwich%20mean%20time/) (1675), Greenwich Mean Time is the mean solar time at Greenwich (London) Royal Observatory, it has been used to assess longitudes and is then related to [Time Zones](https://en.wikipedia.org/wiki/Time_zone) definitions.
- [**Coordinated Universal Time**](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (1963), aka UTC, is one of the current accurate time standard we can use. It is the successor of GMT and it is not adjusted for Day Saving Time. Its measure is based on [Atomic Time](https://en.wikipedia.org/wiki/International_Atomic_Time), it ticks SI seconds. It introduces a [leap second](https://en.wikipedia.org/wiki/Leap_second) to keep correspondence with [Universal Time](https://en.wikipedia.org/wiki/Universal_Time#Versions) UT1 which is based on celestial observations.
- [**ISO-8601**](https://en.wikipedia.org/wiki/ISO_8601) (1988), is a standard from the ISO group providing unambiguous and well-defined method of representing dates and times. ISO-8601 has been defined to keep the complete order of timestamps when stored as string (provided years are all positive and there is no mixed time zone).

# DST Problem
There is a major issue with DST, it breaks **continuity** twice a year and **causality** once a year:

- From Winter Time to Summer Time, it **introduces a gap** of one hour around midnight (last Sunday in October). In the switch day an hour like 02:30:00 cannot happen in any CET and CEST (continuity).
- From Summer Time to Winter Time, it **introduces an overlap** of one hour around midnight (last Sunday in March). It makes timestamps like 2019-10-27T02:30:00 **ambiguous** as they can happen both in CET and CEST (continuity and causality). When you collect data periodically, you will have duplicated timestamps in this range, and it is undecidable without any extra information what timestamp happened first.

Even worse, in addition of breaking continuity and causality, **it is not deterministic**. DST changes are subject to political decisions and then relies on almanac. It forces us to keep track of decisions around the world and constantly update the almanac.

Also notice that some language that supports Time Zones may not support Day Saving Time automatically. Some does, another does not. Typically if your language exposes a Time Zone called CEST (CET + DST active), it is likely you have to keep track when DST occurs by yourself. If you need Time Zones support, you better choose a language that also support DST natively and automatically.

# Best practices
- Don't use custom formats to represent timestamps, use ISO-8601 or RFC-3339 instead.
- avoid 2-digit date, as it creates confusion between 19xx and 20xx. All years should be displayed as 4-digit value.
- watch the day and month order, as well as month abbreviations (Dic. vs Dec.)
- Don't Reinvent the Wheel: Never create a custom object for time unless there is no standard already available for what you do and you exactly know what you are implementing - in this case test and document.
- Never store timestamps without knowing if it stands for the beginning or the end of period unless it is not relevant, in both cases document it;
- Never store timestamps without knowing on which time zone it belongs (including DST). If you cannot store it with those information avoid Local Time, force UTC and document it;
- Never let the user choose its own time zone for storage unless you can fully keep track of it (including DST). Instead allow user to choose its time zone for ingestion and extraction. The conversion task is then on the service provider;
- Avoid mixed time zones (including DST) if you cannot properly keep track of it;
- Avoid mixed granularities if you have not properly defined how to resample time series;

We could summarize our point as follow: always prefer UTC as a reference with ad hoc conversions instead of naïve Local Time, if you need to address localization do use Time Zone aware timestamps with DST support and force ISO-8601 or RFC-3339 for data exchange. Most of modern languages offer support for it, so why not using them? With those requirements you should be able to ease and standardize your developments. And when a tricky issue about timestamps opens, it will save your day!# ISO 8601 Standard

- System dates should be formatted using the ISO 8601 standards, be in UTC time, and have the _at suffix.
- Other dates should use the less precise formats from ISO 8601 unless the use case would benefit from a more specialized format.
- Consider the timezone of the event and whether it needs to be communicated separately through the API.
- Avoid assuming the user's cultural context. Instead, design APIs to be as inclusive as possible

# ISO 8601 Time Format
	YYYY-MM-DDTHH:MM:SS.XXX
	YYYY-MM-DDTHH:MM:SSZ # Z indicates UTC time

- it's important to consider leading zeros
- if the dates are saved as strings, they are still sorted properly!

## including timezone information vs UTC time
Normalizing dates to UTC works well for dates in the past. However, for scheduled events in the future, UTC is not a good fit since timezones and timezone offsets can change in the meantime and the resulting date may not be what the user expected.

Having a separate field for timezone also makes sense where it is important to display to the user. Examples include a calendar event for a virtual meeting where the attendees live in different countries or a flight itinerary where the arrival and departure airports are in different cities.

An API with a separate field for timezone may look like this:

```json
{
  "departure_time": "2023-09-24T11:00:00",
  "departure_timezone": "Australia/Sydney"
}
```

It's best practice to format timezones as values from the [IANA timezone database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) instead of reinventing the wheel. For example, `Australia/Sydney`. This ensures widespread compatibility with different systems.

### Encode the timezone offset with the date field

If "Australia/Sydney" refers to a timezone, the timezone offset would be Australian Eastern Standard Time (AEST), or UTC+10:00. Alternatively, it may be Australian Daylight Saving Time (AEDT), which is UTC+11:00.

Since the timezone offset can change throughout the year for a given timezone, it is not appropriate to have a timezone offset as a standalone field. However, there are certain situations where it makes sense to encode the offset as part of the date field.

An example might be an album of sunset photos taken around the world. A user would expect each photo to be taken around the same time of day, i.e. dusk. It would be strange if some photos were displayed at 2 am due to timezone conversion. Here it doesn't make sense to normalize all photo timestamps to UTC, or even a single timezone. Yet it is also important to convey the precise time each photo was taken.

```json
{
  "taken_at": "2023-01-020T18:30:01-05:00"
}
```

This format allows both the local time to be easily obtained and the absolute point in time to be calculated.

### Separate fields for local time and UTC time

Often it can be simpler for the developer to have multiple fields available to them, for each specific use case:

```json
{
    "taken_at_utc": "2023-01-020T18:30:01-05:00"
    "taken_at_local": "2023-01-020T23:30:01Z"
}
```