# hiveImpScript

## To convert unix time into timestamp 
	select from_unixtime(cast(tag_time as bigint)) from tags limit 10;

## Sentimental analysis on drugs data using AFFIN dictonary
	create external table scores
	(
	word string, 
	score double
	)
	row format delimited
	fields terminated by '\t'
	lines terminated by '\n'
	location '/user/root/pankaj/sentimental/dict/';
	tblproperties("skip.header.line.count"="1");

	create temporary table drugs_words as select drug_id,split(review, " ") from drugs;
	create temporary table drugs_words_exploded as select drug_id, words from drugs_words lateral view explode(`_c1`) explodedVal as words;
	create temporary table words_scores as select a.drug_id, a.words, b.score from drugs_words_exploded a inner join scores b on a.words = b.word;
	create temporary table final as select drug_id, sum(score) from words_scores group by drug_id;
	create temporary table final_reviews as select a.drug_id as drug_id, a.`_c1` as score, b.review as review from final a inner join drugs b on a.drug_id = b.drug_id;


