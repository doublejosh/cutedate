## PHP function : cuteDate() ##

Receives an integer time/date value; created easily with mktime() or pulled from a database, then compares it against today’s date().

Most importantly it returns a user-friendly text string representation of the difference, **with the format chosen based on the distance of time!**

You might see outputs like:
  * "a moment ago" -- if it was just done.
  * "4 hours ago" -- if it was rather recent.
  * “last Thursday” -- if it's less than a week old
  * “yesterday at 3:15pm” -- because that's how we talk.
  * “a week ago” -- if its within the 24 period of the same day last week.
  * "11 days ago" -- if within two weeks.
  * “a moment ago” -- if it's less two minutes old.
  * “October 3rd” -- if it's over a month old but within a year.
  * “Mar. 26th ‘05” -- if it's been a long time.

**Works for future dates too.**


---


Since I'm a SVN noob (I'm a Git person) I'm going to post this here for now since it's a small snippet...


```
<?php

// -------------------- RETURN USER FRIENDLY DATE/TIME STRING -------------

function cuteDate($compared) {
 if(!is_numeric($compared) || strlen(time())!=10) return 'date unknown'; // proper format check
 $today= time(); // get it
 $diff= $today-$compared; // subtract it
 // return the cute string
 switch(true) {
  case $diff < 0 : { // ------------------------------- IN THE FUTURE -------------------
    switch (true) {
	  case $diff > -120 : // ----------------------- Seconds (within 2 minutes)
	   return 'in just a moment'; break;
	  case $diff > -3600 : // ----------------------- Minutes
	   return 'in '.number_format($diff/-60).' minutes'; break;
	  case $diff > -64800 : // ----------------------- Hours
	   return 'in '.number_format($diff/-3600).' hours ('.date("g:i",$compared).')'; break;
	  case $diff > -172800 : // ----------------------- Yesterday (over 18 hours ago)
	   return 'tomorrow at '.date("g:ia",$compared); break;
	  case $diff > -604800 : // ----------------------- Day in the next week
	   return 'next '.date('D',$compared).'. at '.date("g:ia",$compared); // (lowercase L = long / uppercase D = short) break;
	  case $diff > -691200 : // ----------------------- One week from today
	   return 'in a week at '.date("g:ia",$compared); break;
	  case $diff > -1123200 : // ----------------------- Within two weeks
	   return 'in '.number_format($diff/-86400).' days ('.date('n/j',$compared).')'; break;
	  case $diff > -31449600 : // ----------------------- This year
	   return date('F jS',$compared); break; // (uppercase M = short / uppercase F = long)
	  default : // ----------------------- Over a year from now
	   return date("M jS, 'y",$compared); break; // (uppercase M = short / uppercase F = long)
	}
  }
  break;
  case $diff >= 0 : { // ------------------------------- IN THE PAST --------------------
    switch (true) {
	  case $diff < 120 : // ----------------------- Seconds (up to 2 minutes)
	   return 'a moment ago'; break;
	  case $diff < 3600 : // ----------------------- Minutes
	   return number_format($diff/60).' minutes ago'; break;
	  case $diff < 65880 : // ----------------------- Hours
	   return number_format($diff/3600).' hours ago ('.date("g:i",$compared).')'; break;
	  case $diff < 172800 : // ----------------------- Yesterday (over 18 hours ago)
	   return 'yesterday at '.date("g:ia",$compared); break;
	  case $diff < 518400 : // ----------------------- Days still in the week
	   return 'last '.date('D',$compared).'. at '.date("g:ia",$compared); // (lowercase L = long / uppercase D = short) break;
	  case $diff < 604800 : // ----------------------- One week ago today
	   return 'a week ago at '.date("g:ia",$compared); break;
	  case $diff < 1123200 : // ----------------------- Within two weeks
	   return number_format($diff/86400).' days ago ('.date('n/j',$compared).')'; break;
	  case $diff < 1209600 : // ----------------------- Two weeks ago today
	   return 'two weeks ago'; break;
	  case $diff < 31449600 : // ----------------------- This year
	   return date('F jS',$compared); break; // (uppercase M = short / uppercase F = long)
	  case $diff < 31536000 : // ----------------------- Exactly a year ago
	   return 'a year ago today'; break;
	  default : // ----------------------- Over a year ago
	   return date("M jS, 'y",$compared); // (uppercase M = short / uppercase F = long)
    }
  }
 } // main switch
}

?>
```