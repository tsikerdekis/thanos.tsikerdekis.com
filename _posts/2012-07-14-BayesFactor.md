---
layout: post
author: Michael Tsikerdekis
permalink: /:categories/:year/:month/:day/:title/
title: Bayes Factor
---

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Contrary to NHST where you have a p value along with the effect size to determine whether there is an effect and how big is it, Bayes factor answers both of these questions. In other words, one simple result determines which hypothesis is asserted and by how much according to your data.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">You determine which hypothesis is more likely given the data based on the Bayes Factor. The way to interpret a general Bayes Factor is the following. If a Bayes factor is denoted as BFxy then you say that the data are n times more likely under Hx than Hy. An example based on the BF10 would be that the data would be 0.53 times more likely under H1 than H0. If we use the BF01 then we would say that the data are 1.87 times more likely under H0 than H1(which is more meaningful). Basically, one version of Bayes factor(e.g. BF01) is the inverted version of the other(e.g BF10). Pick the version of Bayes factor that is above 1.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	&nbsp;</p>

<h2 id="hn_Interpreting_a_Bayes_Factor" style="font-weight: 300; font-stretch: normal; font-size: 15pt; line-height: 18pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; margin: 0pt; color: rgb(153, 51, 85); padding-top: 10px; padding-bottom: 10px; text-shadow: rgb(238, 238, 238) 1px 1px 0px; background-color: transparent;">
	<a class="heading" href="http://tsikerdekis.wuwcorp.com/BayesFactor#hn_Interpreting_a_Bayes_Factor" style="font-size: 15pt; line-height: 18pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 10px; padding-bottom: 10px; background-color: transparent;">Interpreting a Bayes Factor</a></h2>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">There is a scale used to determine how strong is the evidence presented by the Bayes factor. The scale was developed by Harold Jeffreys in his book "Theory of probability" (H. Jeffreys (1961). The Theory of Probability (3 ed.). Oxford. p. 432).</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	&nbsp;</p>

<table class="data" style="border-width: 2px; border-color: rgb(204, 204, 204); color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<tbody>
		<tr>
			<th style="border-color: rgb(204, 204, 204); padding: 0.1em 0.25em; background-color: rgb(238, 238, 238);">
				Bayes Factor</th>
			<th style="border-color: rgb(204, 204, 204); padding: 0.1em 0.25em; background-color: rgb(238, 238, 238);">
				Strength of Evidence</th>
		</tr>
		<tr>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				&lt; 1:1</td>
			<td style="border-top-style: solid; border-bottom-style: solid; border-left-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				Negative (Supports the opposite model)</td>
		</tr>
		<tr>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				1:1 to 3:1</td>
			<td style="border-top-style: solid; border-bottom-style: solid; border-left-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				Barely worth mentioning</td>
		</tr>
		<tr>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				3:1 to 10:1</td>
			<td style="border-top-style: solid; border-bottom-style: solid; border-left-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				Substantial</td>
		</tr>
		<tr>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				10:1 to 30:1</td>
			<td style="border-top-style: solid; border-bottom-style: solid; border-left-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				Strong</td>
		</tr>
		<tr>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				30:1 to 100:1</td>
			<td style="border-top-style: solid; border-bottom-style: solid; border-left-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				Very strong</td>
		</tr>
		<tr>
			<td style="border-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				&gt; 100:1</td>
			<td style="border-top-style: solid; border-bottom-style: solid; border-left-style: solid; border-color: rgb(204, 204, 204); padding: 0.1em 0.25em;">
				Decisive<br />
				&nbsp;</td>
		</tr>
	</tbody>
</table>

<p>
	&nbsp;</p>
