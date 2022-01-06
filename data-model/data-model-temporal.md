#   Timezone

    A time zone is an area that observes a uniform standard time for legal, commercial and social purposes.
    
    Time zones tend to follow the boundaries between countries and their subdivisions instead of strictly following longitude, because it is convenient for areas in frequent communication to keep the same time.

#   UTC
    
    All time zones are defined as offsets from Coordinated Universal Time (UTC), ranging from UTC−12:00 to UTC+14:00. 
    
    The offsets are usually a whole number of hours, but a few zones are offset by an additional 30 or 45 minutes, such as in India, South Australia and Nepal.
    
    Some areas of higher latitude use daylight saving time for part of the year, typically by adding one hour to local time during spring and summer.

#   Points

    Formally a point in time has no duration; it simply refers to a particular position in the timeline under discussion. We can talk of the point in time at which some event begins or ends.

#   Duration: 
    A duration refers to a number of time quanta; for example, a week, two months, three years and 10 seconds are all durations. 
    
    A duration refers to a particular magnitude (size) of a part of a timeline, but not the direction (so whether we talk of a week ago or a week beginning in three days' time, we are still referring to a length of time of a week).

#   Interval: 
    An interval has a start time point and an end time point.
    Using more formal notation, we can refer to an interval I(s, e) with start point and end point, and for which all points referring to time from s to e (inclusive) make up the interval.
    
    Note that there is an assumption (constraint) that the timepoint does not occur after the timepoint â€˜eâ€™ (an interval of zero time quanta would have a start point and end point that were equal).

#   Timeline:
    Conceptually we can often imagine time as moving along a line in one direction.
    
    When graphically representing time, it is usual to draw a line (often with an arrow to show time direction), where events shown towards the end of the timeline have occurred later than those shown towards the beginning of the line. 
    
    Often a graphical timeline is draw like an axis on a graph (by convention, the X-axis represents time) and the granularity of the time units is marked (and perhaps labelled) along the X-axis.

#   date
    YYYY-MM-DD

    UPDATE rental
    SET return_date = '2019-09-17 15:30:00'
    WHERE rental_id = 99999;

#   datetime
    YYYY-MM-DD HH:MI:SS

    SELECT CURRENT_DATE(),  CURRENT_TIMESTAMP();

#   timestamp
    YYYY-MM-DD HH:MI:SS

#   time
    HHH:MI:SS

##   Conversion Functions ##

    Convert from iso format string to date
    SELECT CAST('2019-09-17' as date) as date_field; -- Working

    Convert custom format date into timestamp
    select from_unixtime(unix_timestamp('12-03-2010' , 'dd-MM-yyyy'))

    Convert 
    select cast(to_date(from_unixtime(unix_timestamp('12-03-2010','dd-MM-yyyy'))) as date)

    select cast(to_date(from_unixtime(unix_timestamp('2021-03-27', 'yyyy-MM-dd'))) as date)

#   Idempotent (String -> String)

    SELECT from_unixtime(to_unix_timestamp('2018/06/05 15:25:42.23','yyyy/MM/dd HH:mm:ss')

#   ISO format 
    SELECT cast('2018-06-05' as date); 
    

#   Pattern
    select cast(to_date(from_unixtime(unix_timestamp('05-06-2018', 'dd-MM-yyyy'))) as date)


#   Convert ISO8601 to a date type:
    select cast(to_date(from_unixtime(unix_timestamp(regexp_replace('2018-06-05T08:02:59Z', 'T',' ')))) as date);


#   Convert String/Timestamp/Date to DATE

    SELECT cast(date_format('2018-06-05 15:25:42.23','yyyy-MM-dd') as date);

#   
    SELECT cast(date_format(current_date(),'yyyy-MM-dd') as date);
    
    SELECT cast(date_format(current_timestamp(),'yyyy-MM-dd') as date);

#   Convert String/Timestamp/Date to BIGINT_TYPE

    SELECT to_unix_timestamp('2018/06/05 15:25:42.23','yyyy/MM/dd HH:mm:ss');

    SELECT to_unix_timestamp(current_date(),'yyyy/MM/dd HH:mm:ss');
    
    SELECT to_unix_timestamp(current_timestamp(),'yyyy/MM/dd HH:mm:ss');

#   Convert String/Timestamp/Date to STRING

    SELECT date_format('2018-06-05 15:25:42.23','yyyy-MM-dd');
    
    SELECT date_format(current_timestamp(),'yyyy-MM-dd');

    SELECT date_format(current_date(),'yyyy-MM-dd');

#   Convert BIGINT unixtime to STRING
    
    SELECT to_date(from_unixtime(unixtime,'yyyy/MM/dd HH:mm:ss'));
#   Convert String to BIGINT unixtime
    
    SELECT unix_timestamp('2018-06-05 15:25:42.23','yyyy-MM-dd') as TIMESTAMP;

#   Convert String to TIMESTAMP
    
    SELECT cast(unix_timestamp('2018-06-05 15:25:42','yyyy-MM-dd') as TIMESTAMP);


#   Manipulating Temporal Data
    SELECT DATE_ADD(CURRENT_DATE(), 5);

#   DAYS

    #last days data

    SELECT LAST_DAY('2019-09-17');

#   date difference
    
    SELECT DATEDIFF('2019-09-03', '2019-06-21');
    SELECT DATEDIFF('2019-09-03 23:59:59', '2019-06-21 00:00:01');
    SELECT DATEDIFF('2019-06-21', '2019-09-03');

#   *********************YEARS*********************

    **year from date** 
    
    SELECT EXTRACT(YEAR FROM '2019-09-18 22:19:05');







