==========================
Dateformat Function Syntax
==========================

The syntax of the ^DateFormat function is as follows:

 

*^DateFormat("expression", "pattern")*,

 

where

-  expression defines the expression which should be assessed to obtain
   the specific date. The possible values of this expression are:
   
   -  today: represents today’s date on the system.
   -  yesterday: represents yesterday’s date.
   -  today - n: represents the date corresponding to n days before the
      current date. Therefore, yesterday is equivalent to today - 1.
	  
-  pattern determines the format of the text string this function
   returns. This format is constructed by combining the following
   letters:
   
   -  y: represents a year digit.
   -  M: represents a month digit.
   -  d: represents a digit of the day of the month.

 

For example, to obtain news from **news.acme.com** on today’s date,
knowing that on this Web site the news has links of the form
(supposing that today’s date is 2004/12/29):
   
   \http://news.acme.com /news/2004/12/29/weblog/1104290407.html

 
in which the news date and, thus, number varies, the following has to
be entered in the regular expression field of the link filter:
   
   \http://news.acme.com/news/^DateFormat("today","yyyy/MM/dd")/weblog/(.)+html
