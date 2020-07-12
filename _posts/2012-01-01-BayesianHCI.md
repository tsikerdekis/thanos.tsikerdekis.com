---
layout: post
author: Michael Tsikerdekis
permalink: /:categories/:year/:month/:day/:title/
title: Bayesian Statistical Hypothesis Testing for HCI
---


<h6 style="font-size: 15pt; margin: 0pt; font-weight: 300; font-stretch: normal; line-height: 18pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; color: rgb(153, 51, 85); padding-top: 10px; padding-bottom: 10px; text-shadow: rgb(238, 238, 238) 1px 1px 0px; background-color: transparent;">
	<a class="heading" href="http://tsikerdekis.wuwcorp.com/BayesianHCIstats#hn_Disclaimer_Please_read_this_first" style="font-size: 15pt; line-height: 18pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 10px; padding-bottom: 10px; background-color: transparent;">Disclaimer (Please read this first!)</a></h6>

<h5 id="hn_Evidence_for_and_against_the_null_hypothesis_is_possible_:-" style="margin: 0pt; padding-top: 10px; padding-bottom: 10px; font-weight: 300; font-size: 15pt; font-stretch: normal; line-height: 18pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; background-color: transparent;">
	<em style="margin: 0pt; padding-top: 10px; padding-bottom: 10px; color: rgb(153, 51, 85); outline: 0px; font-size: 15pt; line-height: 18pt; width: auto; text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; background-color: transparent;">Evidence for and against the null hypothesis is possible :-)</em></h5>

<div style="float: right;">
	<blockquote class="oval-thought" style="width: 135px; margin-right: auto; margin-bottom: 40px; margin-left: 10px; position: relative; padding: 25px 20px; text-align: center; color: rgb(255, 255, 255); border-radius: 110px 60px; background: linear-gradient(rgb(46, 136, 196), rgb(7, 86, 152));">
		<p style="font-size: 1.25em;">
			Found anything interesting? Any comments or errors?<a href="http://tsikerdekis.wuwcorp.com/Contact" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;">Contact me :-)</a></p>
	</blockquote>
</div>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">I started this wiki so that I can try and gather as many procedures(and code) as I can that currently exists in Bayesian statistics. The goal is to create an&nbsp;</span><em style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; color: rgb(68, 68, 68); font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">easy to read, easy to apply</em><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">&nbsp;guide for each method depending on your data and your design. Although this is geared towards HCI research, most of these methods can be applied in other scientific disciplines such as social sciences, psychology and others. The philosophy behind this guide is to always keep things simple. Just as I don't ask for my visitors on this website to understand HTTP requests, the same should apply for someone that wants to perform Bayesian statistics. You only need to know what is your input, and how to interpret the output. Therefore, the emphasis here is taken away from the math aspects of bayesian statistics.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">My inspiration for developing such content was the site&nbsp;</span><a class="ext" href="http://yatani.jp/HCIstats" style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; text-align: justify;" target="\&quot;_new\&quot;">Statistics for HCI Research</a><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">&nbsp;by Koji Yatani. It is an excellent guide for NHST analysis for HCI.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Keep in mind that&nbsp;</span><em style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; color: rgb(68, 68, 68); font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">I am not an expert of statistics</em><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">. The contents provided here is basically what I learned from my experience of HCI research and by reading different online/offline materials. I always double-check the content before posting, but it still may be not 100% accurate or even wrong. So,&nbsp;</span><em style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; color: rgb(68, 68, 68); font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">use the contents on this website at your discretion</em><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">. I own no responsibility on any kind of consequences, such as you have done a wrong analysis after reading my wiki or your papers do not get into a conference or a journal, or your adviser doesn't like your analysis.&nbsp;</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">I also strongly recommend you get a second opinion on your analysis from other kinds of resources before you really perform a test. If you have found any factual errors, please email me(tsikerdekis@gmail.com). Your comments would be greatly appreciated. Also, I am always looking for R(matlab,stata) code that can perform hypothesis testing so don't hesitate to let me know about it.</span></p>

<hr style="border-top-style: solid; border-width: 1px 0px 0px; border-top-color: rgb(238, 238, 238); margin: 8px auto; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;" />
<h2 id="hn_Basics_of_statistics_A_quick_introduction_to_things_you_need_to_know" style="font-weight: 300; font-stretch: normal; font-size: 15pt; line-height: 18pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; margin: 0pt; color: rgb(153, 51, 85); padding-top: 10px; padding-bottom: 10px; text-shadow: rgb(238, 238, 238) 1px 1px 0px; background-color: transparent;">
	<span style="font-size: 15pt; line-height: 18pt; margin: 0pt; padding-top: 10px; padding-bottom: 10px;">Basics of statistics (A qui</span><font color="#993355"><span style="font-size: 15pt; line-height: 18pt; margin: 0pt; padding-top: 10px; padding-bottom: 10px;">ck introduction to things you need to know)</span></font></h2>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">There are 4 types of variables that you need to know and identify.&nbsp;</span></p>

<ul style="list-style-image: url(http://tsikerdekis.wuwcorp.com/templates/modified1/images/sqw.gif); margin-top: 0px; margin-bottom: 0px; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<li style="margin: 0.5em;">
		<em>Interval/Numerical/Ratio</em>&nbsp;are ordered sets of data (usually numbers) that maintain equal distance between their space (e.g., the distance between 2 and 3 is equal to the distance between 3 and 4).</li>
	<li style="margin: 0.5em;">
		<em>Ordinal</em>&nbsp;are ordered sets of data that do not show an equal distance between their elements. (e.g., "very strong" is definitely higher than "strong" and the same applies for "extremely strong" but the distance between this elements is not necessary equal.)</li>
	<li style="margin: 0.5em;">
		<em>Nominal/Categorical</em>&nbsp;are sets of data with no order (e.g., countries is a good example).</li>
	<li style="margin: 0.5em;">
		<em>Dichotomous</em>&nbsp;are categorical variables that have only two levels (e.g., sex can have values only male and female.)</li>
</ul>

<p>
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">You will also need a general understanding of the&nbsp;</span><a href="http://tsikerdekis.wuwcorp.com/BayesFactor" style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; text-align: justify;">Bayes Factor</a><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">. However, I have connected the link to every procedure's interpretation section as well.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Finally, Bayesian procedures have their pros and cons just as NHST analysis(guide development in progress) BUT the single most appealing thing for me is the power to provide evidence&nbsp;</span><em style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; color: rgb(68, 68, 68); font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">for the null hypothesis</em><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">. Yes, with Bayesian methods you can do it!</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	&nbsp;</p>

<hr style="border-top-style: solid; border-width: 1px 0px 0px; border-top-color: rgb(238, 238, 238); margin: 8px auto; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;" />
<h2 id="hn_What_statistical_test_should_I_use" style="font-weight: 300; font-stretch: normal; font-size: 15pt; line-height: 18pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; margin: 0pt; color: rgb(153, 51, 85); padding-top: 10px; padding-bottom: 10px; text-shadow: rgb(238, 238, 238) 1px 1px 0px; background-color: transparent;">
	<span style="font-size: 15pt; line-height: 18pt; margin: 0pt; padding-top: 10px; padding-bottom: 10px;">What statistical test should I use?</span></h2>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">While with NHST analysis answers are straight forward, Bayesian statistics is still a field under development. This is especially true when it comes to hypothesis testing. The following is a set of techniques that I managed to gather.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	&nbsp;</p>

<table class="data" style="border-width: 2px; border-color: rgb(204, 204, 204); color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<tbody>
		<tr>
			<th style="border-color: rgb(204, 204, 204); padding: 0.1em 0.25em; background-color: rgb(238, 238, 238);">
				&nbsp;</th>
			<th colspan="4" style="border-color: rgb(204, 204, 204); padding: 0.1em 0.25em; background-color: rgb(238, 238, 238);">
				<div class="center" style="text-align: center;">
					Types of your dependent/independent variables</div>
			</th>
		</tr>
		<tr>
			<th style="border-color: rgb(204, 204, 204); padding: 0.1em 0.25em; background-color: rgb(238, 238, 238);">
				&nbsp;</th>
			<th style="border-color: rgb(204, 204, 204); padding: 0.1em 0.25em; background-color: rgb(238, 238, 238);">
				Interval/Ratio</th>
			<th style="border-color: rgb(204, 204, 204); padding: 0.1em 0.25em; background-color: rgb(238, 238, 238);">
				Interval/Ratio, Ordinal</th>
			<th style="border-color: rgb(204, 204, 204); padding: 0.1em 0.25em; background-color: rgb(238, 238, 238);">
				Ordinal,Categorical</th>
			<th style="border-color: rgb(204, 204, 204); padding: 0.1em 0.25em; background-color: rgb(238, 238, 238);">
				Dichotomous</th>
		</tr>
		<tr>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				Compare two unpaired groups</td>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				<a href="http://tsikerdekis.wuwcorp.com/BayesianttestTwoIndependentGroups" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;">Bayesian t-test</a></td>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				Bayesianmannwhitney Bayesian Mann-Whitney test</td>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				<a href="http://tsikerdekis.wuwcorp.com/BayesianTestofIndependence" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;">Bayesian test of independence</a></td>
			<td style="border-top-style: solid; border-bottom-style: solid; border-left-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				Bayesianbinomialtesting Bayesian Binomial</td>
		</tr>
		<tr>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				Compare two paired groups</td>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				--</td>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				--</td>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				--</td>
			<td style="border-top-style: solid; border-bottom-style: solid; border-left-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				--</td>
		</tr>
		<tr>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				Find relationship between two variables</td>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				--</td>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				--</td>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				--</td>
			<td style="border-top-style: solid; border-bottom-style: solid; border-left-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				--</td>
		</tr>
	</tbody>
</table>

<p>
	&nbsp;</p>
