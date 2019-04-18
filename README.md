# hiveImpScript

## To convert unix time into timestamp 
  select from_unixtime(cast(tag_time/1000 as bigint)) from tags limit 10;
