
## Parse and format datetime objects

### strftime
```python
datetime.now().strftime(format)
```

### strptime
```python
datetime.now().strptime(format)
```

### format codes
Code	Meaning
- `%a`	Weekday as Sun, Mon
- `%A`	Weekday as full name as Sunday, Monday
- `%w`	Weekday as decimal no as 0,1,2...
- `%d`	Day of month as 01,02
- `%b`	Months as Jan, Feb
- `%B`	Months as January, February
- `%m`	Months as 01,02
- `%y`	Year without century as 11,12,13
- `%Y`	Year with century 2011,2012
- `%H`	24 Hours clock from 00 to 23
- `%I`	12 Hours clock from 01 to 12
- `%p`	AM, PM
- `%M`	Minutes from 00 to 59
- `%S`	Seconds from 00 to 59
- `%f`	Microseconds 6 decimal numbers
