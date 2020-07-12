---
layout: post
author: Michael Tsikerdekis
permalink: /:categories/:year/:month/:day/:title/
title: Bayesian Test of Independence
---

<h1 id="hn_Bayesian_Test_of_Independence" style="font-weight: 300; font-stretch: normal; font-size: 18pt; line-height: 19pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; margin: 0pt; color: rgb(153, 51, 85); padding-bottom: 5px; border-style: none; text-shadow: rgb(221, 221, 221) 1px 1px 0px; background-color: transparent;">
	<a class="heading" href="http://tsikerdekis.wuwcorp.com/BayesianTestofIndependence#hn_Introduction" style="font-size: 15pt; line-height: 18pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 10px; padding-bottom: 10px; background-color: transparent;">Introduction</a></h1>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">The following test behaves alot like the chi-square test of independence. It can work with ordinal, categorical and even dichotomous variables (any case that can give you a contingency table).</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	&nbsp;</p>

<h2 id="hn_References" style="font-weight: 300; font-stretch: normal; font-size: 15pt; line-height: 18pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; margin: 0pt; color: rgb(153, 51, 85); padding-top: 10px; padding-bottom: 10px; text-shadow: rgb(238, 238, 238) 1px 1px 0px; background-color: transparent;">
	<a class="heading" href="http://tsikerdekis.wuwcorp.com/BayesianTestofIndependence#hn_References" style="font-size: 15pt; line-height: 18pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 10px; padding-bottom: 10px; background-color: transparent;">References</a></h2>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">This method is based on the book "</span><a class="ext" href="http://bayes.bgsu.edu/bcwr/" style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; text-align: justify;" target="\&quot;_new\&quot;">Bayesian Computation With R</a><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">" by Jim Albert. If you want to learn more about the model and the code you can read the book or the article.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">For the procedure you need R and the&nbsp;</span><a href="http://tsikerdekis.wuwcorp.com/LearnBayes" style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; text-align: justify;">LearnBayes</a><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">&nbsp;package that can be installed in R using the command</span><em style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; color: rgb(68, 68, 68); font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">install.packages('<a href="http://tsikerdekis.wuwcorp.com/LearnBayes" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;">LearnBayes</a>')</em><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	&nbsp;</p>

<h2 id="hn_Procedure_Highlights" style="font-weight: 300; font-stretch: normal; font-size: 15pt; line-height: 18pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; margin: 0pt; color: rgb(153, 51, 85); padding-top: 10px; padding-bottom: 10px; text-shadow: rgb(238, 238, 238) 1px 1px 0px; background-color: transparent;">
	<a class="heading" href="http://tsikerdekis.wuwcorp.com/BayesianTestofIndependence#hn_Procedure_Highlights" style="font-size: 15pt; line-height: 18pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 10px; padding-bottom: 10px; background-color: transparent;">Procedure Highlights</a></h2>

<h3 id="hn_Input" style="font-stretch: normal; font-size: 12pt; line-height: 15pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; font-weight: 300; margin: 0pt; color: rgb(170, 170, 170); padding-top: 5px; padding-bottom: 5px; background-color: transparent;">
	<a class="heading" href="http://tsikerdekis.wuwcorp.com/BayesianTestofIndependence#hn_Input" style="font-size: 12pt; line-height: 15pt; width: auto; color: rgb(170, 170, 170); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 5px; padding-bottom: 5px; background-color: transparent;">Input</a></h3>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">You need to type the data for your contingency table or feed it to your tabledata variable. Additionally, you need to specify the rows and columns for your table.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	&nbsp;</p>

<div class="code" style="border: 1px solid rgb(170, 170, 204); font-size: 11px; font-family: monospace; margin: auto; padding: 6px 5px 13px; overflow: auto; white-space: nowrap; line-height: 23.0399990081787px; background: rgb(243, 243, 255);">
	#---------------INPUT DATA------------------<br style="clear: none; line-height: 0.9em;" />
	tabledata = c(6,9,40,34) # Enter data first row by row and then column by column<br style="clear: none; line-height: 0.9em;" />
	tablerows = 3 # rows in the contigency table<br style="clear: none; line-height: 0.9em;" />
	tablecolumns = 4 # columns in the contigency table<br style="clear: none; line-height: 0.9em;" />
	#-------------------------------------------</div>

<form action="http://tsikerdekis.wuwcorp.com/BayesianTestofIndependence/grabcode" id="form_61092f4ded" method="post" style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<input class="grabcode" name="save" style="border-width: 1px; border-style: solid; border-color: rgb(239, 239, 239) rgb(170, 170, 204) rgb(170, 170, 204) rgb(238, 238, 255); color: rgb(51, 51, 102); font-stretch: normal; font-size: 10.0799999237061px; font-family: Verdana, sans-serif; padding-right: 0.2em; padding-left: 0.2em; line-height: 1em; float: right; background-color: rgb(208, 224, 240);" title="Download" type="submit" value="Grab" />&nbsp;</form>

<p>
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">The hypotheses are:</span></p>

<ul style="list-style-image: url(http://tsikerdekis.wuwcorp.com/templates/modified1/images/sqw.gif); margin-top: 0px; margin-bottom: 0px; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<li style="margin: 0.5em;">
		H0: There is no dependency between the two variables</li>
	<li style="margin: 0.5em;">
		H1: There is a dependency between the two variables</li>
	<li style="margin: 0.5em;">
		H~0: There is almost no dependency. This hypothesis tests for a model close to independence.</li>
</ul>

<p>
	&nbsp;</p>

<h3 id="hn_Output" style="font-stretch: normal; font-size: 12pt; line-height: 15pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; font-weight: 300; margin: 0pt; color: rgb(170, 170, 170); padding-top: 5px; padding-bottom: 5px; background-color: transparent;">
	<a class="heading" href="http://tsikerdekis.wuwcorp.com/BayesianTestofIndependence#hn_Output" style="font-size: 12pt; line-height: 15pt; width: auto; color: rgb(170, 170, 170); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 5px; padding-bottom: 5px; background-color: transparent;">Output</a></h3>

<div class="code" style="border: 1px solid rgb(170, 170, 204); font-size: 11px; font-family: monospace; margin: auto; padding: 6px 5px 13px; overflow: auto; white-space: nowrap; line-height: 23.0399990081787px; background: rgb(243, 243, 255);">
	Your table:&nbsp;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;[,1] [,2]<br style="clear: none; line-height: 0.9em;" />
	[1,] &nbsp; &nbsp;6 &nbsp; 40<br style="clear: none; line-height: 0.9em;" />
	[2,] &nbsp; &nbsp;9 &nbsp; 34<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	The uniform table to be compared with your table:&nbsp;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;[,1] [,2]<br style="clear: none; line-height: 0.9em;" />
	[1,] &nbsp; &nbsp;1 &nbsp; &nbsp;1<br style="clear: none; line-height: 0.9em;" />
	[2,] &nbsp; &nbsp;1 &nbsp; &nbsp;1<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	-----------Results--------------<br style="clear: none; line-height: 0.9em;" />
	Bayes Factor(BF10) for H1 Dependence over H0 Independence: &nbsp;0.4660114&nbsp;<br style="clear: none; line-height: 0.9em;" />
	Bayes Factor(BF01) for H0 Independence over H1 Dependence: &nbsp;2.14587&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	-----------Additional Model Results--------------<br style="clear: none; line-height: 0.9em;" />
	Bayes factor in support of the model close to independence versus the model of independence:<br style="clear: none; line-height: 0.9em;" />
	&nbsp; log.K log.BF &nbsp; BF<br style="clear: none; line-height: 0.9em;" />
	1 &nbsp; &nbsp; 2 &nbsp;-1.76 0.17<br style="clear: none; line-height: 0.9em;" />
	2 &nbsp; &nbsp; 3 &nbsp;-0.50 0.61<br style="clear: none; line-height: 0.9em;" />
	3 &nbsp; &nbsp; 4 &nbsp;-0.25 0.78<br style="clear: none; line-height: 0.9em;" />
	4 &nbsp; &nbsp; 5 &nbsp;-0.07 0.93<br style="clear: none; line-height: 0.9em;" />
	5 &nbsp; &nbsp; 6 &nbsp;-0.02 0.98<br style="clear: none; line-height: 0.9em;" />
	6 &nbsp; &nbsp; 7 &nbsp; 0.00 1.00<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	-------Results--------------<br style="clear: none; line-height: 0.9em;" />
	Bayes Factor(BF10) for H~0 Close to Independence over H0 Independence: &nbsp;0.9954232&nbsp;<br style="clear: none; line-height: 0.9em;" />
	Bayes Factor(BF01) for H0 Independence over H~0 Close to Independence: &nbsp;1.004598</div>

<form action="http://tsikerdekis.wuwcorp.com/BayesianTestofIndependence/grabcode" id="form_61092f4ded_1" method="post" style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<input class="grabcode" name="save" style="border-width: 1px; border-style: solid; border-color: rgb(239, 239, 239) rgb(170, 170, 204) rgb(170, 170, 204) rgb(238, 238, 255); color: rgb(51, 51, 102); font-stretch: normal; font-size: 10.0799999237061px; font-family: Verdana, sans-serif; padding-right: 0.2em; padding-left: 0.2em; line-height: 1em; float: right; background-color: rgb(208, 224, 240);" title="Download" type="submit" value="Grab" />&nbsp;</form>

<p>
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">You need to always verify that your table looks the way that it should. The code is still a bit buggy and sometimes rows get changed for columns. In case your table looks the opposite way, just change the numbers between rows and columns.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">The code performs two analyses. The first, tests the independence hypothesis against the dependence hypothesis. The second analysis tests the hypothesis of Independence against the hypothesis close to independence.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span class="underline" style="text-decoration: underline; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Read the&nbsp;<a href="http://tsikerdekis.wuwcorp.com/BayesFactor" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;">Bayes Factor</a>&nbsp;page for how you should interpret these results.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">In this example, H0 is 2.14 times more likely than H1. The evidence is not really strong however. Additionally, the second test failed to provide support for a model close to independence.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	&nbsp;</p>

<h2 id="hn_Code" style="font-weight: 300; font-stretch: normal; font-size: 15pt; line-height: 18pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; margin: 0pt; color: rgb(153, 51, 85); padding-top: 10px; padding-bottom: 10px; text-shadow: rgb(238, 238, 238) 1px 1px 0px; background-color: transparent;">
	<a class="heading" href="http://tsikerdekis.wuwcorp.com/BayesianTestofIndependence#hn_Code" style="font-size: 15pt; line-height: 18pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 10px; padding-bottom: 10px; background-color: transparent;">Code</a></h2>

<div class="code" style="border: 1px solid rgb(170, 170, 204); font-size: 11px; font-family: monospace; margin: auto; padding: 6px 5px 13px; overflow: auto; white-space: nowrap; line-height: 23.0399990081787px; background: rgb(243, 243, 255);">
	# clears workspace: &nbsp;<br style="clear: none; line-height: 0.9em;" />
	rm(list=ls(all=TRUE))<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	#---------------INPUT DATA------------------<br style="clear: none; line-height: 0.9em;" />
	tabledata = c(6,9,40,34) # Enter data first row by row and then column by column<br style="clear: none; line-height: 0.9em;" />
	tablerows = 2 # rows in the contigency table<br style="clear: none; line-height: 0.9em;" />
	tablecolumns = 2 # columns in the contigency table<br style="clear: none; line-height: 0.9em;" />
	#-------------------------------------------<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	library(LearnBayes)<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	tablesize = c(tablecolumns,tablerows)<br style="clear: none; line-height: 0.9em;" />
	data=matrix(tabledata,tablesize)<br style="clear: none; line-height: 0.9em;" />
	cat("\r\nYour table: \r\n")<br style="clear: none; line-height: 0.9em;" />
	print(data)<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	#chisq.test(data)<br style="clear: none; line-height: 0.9em;" />
	#fisher.test(data)<br style="clear: none; line-height: 0.9em;" />
	totalrowscolumns = tablerows * tablecolumns<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	a=matrix(rep(1,totalrowscolumns),tablesize)<br style="clear: none; line-height: 0.9em;" />
	cat("\r\nThe uniform table to be compared with your table: \r\n")<br style="clear: none; line-height: 0.9em;" />
	print(a)<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	BF10 = ctable(data,a) #BF in support of the dependence hypothesis<br style="clear: none; line-height: 0.9em;" />
	BF01 = &nbsp;1 /BF10<br style="clear: none; line-height: 0.9em;" />
	cat("\r\n-----------Results--------------\r\n")<br style="clear: none; line-height: 0.9em;" />
	cat("Bayes Factor(BF10) for H1 Dependence over H0 Independence: ",BF10,"\r\n")<br style="clear: none; line-height: 0.9em;" />
	cat("Bayes Factor(BF01) for H0 Independence over H1 Dependence: ",BF01,"\r\n")<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	log.K=seq(2,7)<br style="clear: none; line-height: 0.9em;" />
	compute.log.BF=function(log.K)<br style="clear: none; line-height: 0.9em;" />
	&nbsp; log(bfindep(data,exp(log.K),100000)$bf)<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	log.BF=sapply(log.K,compute.log.BF)<br style="clear: none; line-height: 0.9em;" />
	BF=exp(log.BF)<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	#BF in support of the alternative model close to independence<br style="clear: none; line-height: 0.9em;" />
	#Bayes factor against independence assuming alternatives close to independence<br style="clear: none; line-height: 0.9em;" />
	cat("\r\n-----------Additional Model Results--------------\r\n")<br style="clear: none; line-height: 0.9em;" />
	cat("Bayes factor in support of the model close to independence versus the model of independence:\r\n")<br style="clear: none; line-height: 0.9em;" />
	print(round(data.frame(log.K,log.BF,BF),2))<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	#Plotting<br style="clear: none; line-height: 0.9em;" />
	plot(log.K,log.BF)<br style="clear: none; line-height: 0.9em;" />
	lines(log.K,log.BF)<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	cat("\r\n-----------Results--------------\r\n")<br style="clear: none; line-height: 0.9em;" />
	cat("Bayes Factor(BF~00) for H~0 Close to Independence over H0 Independence: ",max(BF),"\r\n")<br style="clear: none; line-height: 0.9em;" />
	cat("Bayes Factor(BF0~0) for H0 Independence over H~0 Close to Independence: ",1/max(BF),"\r\n")</div>

<form action="http://tsikerdekis.wuwcorp.com/BayesianTestofIndependence/grabcode" id="form_61092f4ded_2" method="post" style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<input class="grabcode" name="save" style="border-width: 1px; border-style: solid; border-color: rgb(239, 239, 239) rgb(170, 170, 204) rgb(170, 170, 204) rgb(238, 238, 255); color: rgb(51, 51, 102); font-stretch: normal; font-size: 10.0799999237061px; font-family: Verdana, sans-serif; padding-right: 0.2em; padding-left: 0.2em; line-height: 1em; float: right; background-color: rgb(208, 224, 240);" title="Download" type="submit" value="Grab" />&nbsp;</form>

<p>
	&nbsp;</p>
