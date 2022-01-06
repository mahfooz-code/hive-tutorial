#   Function tips for date and string

#   to_date()
    The to_date() function removes hours, minutes, and seconds from a date.
    This is useful when we need to check whether the values of date/time type columns are within the data range 
    such as to_date(update_datetime) between 2014-11-01 and 2014-11-31.
    
    SELECT TO_DATE(FROM_UNIXTIME(UNIX_TIMESTAMP())) as currentdate;
