# hiveImpScript

## To convert unix time into timestamp 
	select from_unixtime(cast(tag_time as bigint)) from tags limit 10;
