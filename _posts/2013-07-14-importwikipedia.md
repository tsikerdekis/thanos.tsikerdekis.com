---
layout: post
author: Michael Tsikerdekis
permalink: /:categories/:year/:month/:day/:title/
title: Importing Wikipedia Dumps to Mysql
---

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">It can be quite frustrating adding Wikipedia Dumps in a local database. For some Wikipedias, such as the English Wikipedia, it takes a long time. This is a collection of scripts I've used to import Wikipedia dumps in Mysql.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<em style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; color: rgb(68, 68, 68); font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Note: This guide is based on an Ubuntu server setup</em><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<em style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; color: rgb(68, 68, 68); font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Warning: The restoring process is likely to take weeks for large Wikipedias such as the English Wikipedia</em></p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<h2 id="hn_Step_1:_Install_MediaWiki" style="font-weight: 300; font-stretch: normal; font-size: 15pt; line-height: 18pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; margin: 0pt; color: rgb(153, 51, 85); padding-top: 10px; padding-bottom: 10px; text-shadow: rgb(238, 238, 238) 1px 1px 0px; background-color: transparent;">
	<a class="heading" href="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql#hn_Step_1:_Install_MediaWiki" style="font-size: 15pt; line-height: 18pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 10px; padding-bottom: 10px; background-color: transparent;">Step 1: Install&nbsp;</a><a href="http://tsikerdekis.wuwcorp.com/MediaWiki" style="font-size: 15pt; line-height: 18pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 10px; padding-bottom: 10px; background-color: transparent;">MediaWiki</a></h2>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">This is the quickest way to develop an almost blank mediawiki db used by Wikipedia. You will need a typical&nbsp;</span><a class="ext" href="https://help.ubuntu.com/community/ApacheMySQLPHP" style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; text-align: justify;" target="\&quot;_new\&quot;">LAMP server</a><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">.&nbsp;</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Mediawiki uses innoDB tables. By default, all innodb tables are saved under one file on the disk. The file cannot shrink and can cause problems. It's best to use an option of&nbsp;</span><a href="http://tsikerdekis.wuwcorp.com/MySQL" style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; text-align: justify;">MySQL</a><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">&nbsp;to create a seperate file on the disc per innodb table. To do this you need to do the following:</span></p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<div class="code" style="border: 1px solid rgb(170, 170, 204); font-size: 11px; font-family: monospace; margin: auto; padding: 6px 5px 13px; overflow: auto; white-space: nowrap; line-height: 23.0399990081787px; background: rgb(243, 243, 255);">
	sudo /etc/my.cnf</div>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<form action="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql/grabcode" id="form_61092f4ded" method="post" style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<input class="grabcode" name="save" style="border-width: 1px; border-style: solid; border-color: rgb(239, 239, 239) rgb(170, 170, 204) rgb(170, 170, 204) rgb(238, 238, 255); color: rgb(51, 51, 102); font-stretch: normal; font-size: 10.0799999237061px; font-family: Verdana, sans-serif; padding-right: 0.2em; padding-left: 0.2em; line-height: 1em; float: right; background-color: rgb(208, 224, 240);" title="Download" type="submit" value="Grab" />&nbsp;</form>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Find the [mysqld] part in the config file and add:</span></p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<div class="code" style="border: 1px solid rgb(170, 170, 204); font-size: 11px; font-family: monospace; margin: auto; padding: 6px 5px 13px; overflow: auto; white-space: nowrap; line-height: 23.0399990081787px; background: rgb(243, 243, 255);">
	innodb_file_per_table=1</div>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<form action="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql/grabcode" id="form_61092f4ded_1" method="post" style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<input class="grabcode" name="save" style="border-width: 1px; border-style: solid; border-color: rgb(239, 239, 239) rgb(170, 170, 204) rgb(170, 170, 204) rgb(238, 238, 255); color: rgb(51, 51, 102); font-stretch: normal; font-size: 10.0799999237061px; font-family: Verdana, sans-serif; padding-right: 0.2em; padding-left: 0.2em; line-height: 1em; float: right; background-color: rgb(208, 224, 240);" title="Download" type="submit" value="Grab" />&nbsp;</form>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Save the file and then on the terminal restart mysql:</span></p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<div class="code" style="border: 1px solid rgb(170, 170, 204); font-size: 11px; font-family: monospace; margin: auto; padding: 6px 5px 13px; overflow: auto; white-space: nowrap; line-height: 23.0399990081787px; background: rgb(243, 243, 255);">
	service mysql restart</div>

<p>
	&nbsp;</p>

<form action="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql/grabcode" id="form_61092f4ded_2" method="post" style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<input class="grabcode" name="save" style="border-width: 1px; border-style: solid; border-color: rgb(239, 239, 239) rgb(170, 170, 204) rgb(170, 170, 204) rgb(238, 238, 255); color: rgb(51, 51, 102); font-stretch: normal; font-size: 10.0799999237061px; font-family: Verdana, sans-serif; padding-right: 0.2em; padding-left: 0.2em; line-height: 1em; float: right; background-color: rgb(208, 224, 240);" title="Download" type="submit" value="Grab" />&nbsp;</form>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Note that any existing innodb tables will remain in the large ibfdata file but any newly created tables will be assigned a different file on the disk.&nbsp;</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">You will need to do some fine tuning on mysql for variables such as: innodb_buffer_pool_size, innodb_log_buffer_size, innodb_additional_mem_pool_size. You will have to investigate a bit to see what's best.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Now you need to install mediawiki. You need to follow the&nbsp;</span><a class="ext" href="http://www.mediawiki.org/wiki/Manual:Installation_guide#Quick_installation_guide" style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; text-align: justify;" target="\&quot;_new\&quot;">Quick Installation Guide for Mediawiki</a><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">. For the most part if you know your root password for mysql mediawiki can setup automatically the database, tables, and the user (if you don't want to have root as your user accessing the database).</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">At this point, if you know exactly which columns on a table you are going to need, you may want to turn some fields in smaller versions, so that they can still exist and avoid errors, however they won't occupy as much space. A good example is the text table that contains two blob fields that track changes. If you are not interested in these changes, you could always turn these blob fields in varchar(2) or something else and save space.</span></p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<h2 id="hn_Step_2:_Find_dumps_and_retrieve_the_list" style="font-weight: 300; font-stretch: normal; font-size: 15pt; line-height: 18pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; margin: 0pt; color: rgb(153, 51, 85); padding-top: 10px; padding-bottom: 10px; text-shadow: rgb(238, 238, 238) 1px 1px 0px; background-color: transparent;">
	<a class="heading" href="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql#hn_Step_2:_Find_dumps_and_retrieve_the_list" style="font-size: 15pt; line-height: 18pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 10px; padding-bottom: 10px; background-color: transparent;">Step 2: Find dumps and retrieve the list</a></h2>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">After the installation, you will need to figure out which dump contains the data that you want. There are many and they contain dumps for different tables. You can look some of the dumps&nbsp;</span><a class="ext" href="http://dumps.wikimedia.org/enwiki/" style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; text-align: justify;" target="\&quot;_new\&quot;">here</a><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">. If you want another language Wikipedia, you have to change "enwiki" to reflect the prefix of the language that you are interested in (e.g., elwiki for Greek Wikipedia, cswiki for Czech Wikipedia, eswiki for Spanish Wikipedia).&nbsp;</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Use this script to create a filelist.txt file containing all files to be downloaded. You will need a proper regular expression to capture the names of the files automatically. As an alternative, you could type all file names manually in a file names filelist.txt. Also you will need to setup the url variable to the directory containing the dumps that interest you.</span></p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<div class="code" style="border: 1px solid rgb(170, 170, 204); font-size: 11px; font-family: monospace; margin: auto; padding: 6px 5px 13px; overflow: auto; white-space: nowrap; line-height: 23.0399990081787px; background: rgb(243, 243, 255);">
	<span class="kw1" style="color: rgb(177, 177, 0);">import</span>&nbsp;<span class="kw3" style="color: rgb(0, 0, 102);">urllib2</span>,&nbsp;<span class="kw3" style="color: rgb(0, 0, 102);">re</span><br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	<span class="co1" style="color: rgb(128, 128, 128); font-style: italic;">#main url to retrieve files</span><br style="clear: none; line-height: 0.9em;" />
	url =&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"http://dumps.wikimedia.org/enwiki/20130102/"</span><br style="clear: none; line-height: 0.9em;" />
	f =&nbsp;<span class="kw3" style="color: rgb(0, 0, 102);">urllib2</span>.<span class="me1">urlopen</span><span class="br0" style="color: rgb(102, 204, 102);">(</span>url<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	data = f.<span class="me1">read</span><span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	a =&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><br style="clear: none; line-height: 0.9em;" />
	m =&nbsp;<span class="kw3" style="color: rgb(0, 0, 102);">re</span>.<span class="me1">findall</span><span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">"(enwiki-<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\d</span>*?-pages-meta-history<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\d</span>+?.xml-p.+?<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\.</span>7z)"</span>,data,<span class="kw3" style="color: rgb(0, 0, 102);">re</span>.<span class="me1">DOTALL</span><span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">for</span>&nbsp;item&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">in</span>&nbsp;m:<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;item&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">not</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">in</span>&nbsp;a:<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; a.<span class="me1">append</span><span class="br0" style="color: rgb(102, 204, 102);">(</span>item<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">print</span>&nbsp;a<br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	f =&nbsp;<span class="kw2" style="font-weight: bold;">open</span><span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">"filelist.txt"</span>,<span class="st0" style="color: rgb(255, 0, 0);">"w"</span><span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">for</span>&nbsp;<span class="kw2" style="font-weight: bold;">file</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">in</span>&nbsp;a:<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; f.<span class="me1">write</span><span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="kw2" style="font-weight: bold;">file</span>+<span class="st0" style="color: rgb(255, 0, 0);">"<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span><span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	f.<span class="me1">close</span><span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">)</span></div>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<form action="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql/grabcode" id="form_61092f4ded_3" method="post" style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<input class="grabcode" name="save" style="border-width: 1px; border-style: solid; border-color: rgb(239, 239, 239) rgb(170, 170, 204) rgb(170, 170, 204) rgb(238, 238, 255); color: rgb(51, 51, 102); font-stretch: normal; font-size: 10.0799999237061px; font-family: Verdana, sans-serif; padding-right: 0.2em; padding-left: 0.2em; line-height: 1em; float: right; background-color: rgb(208, 224, 240);" title="Download" type="submit" value="Grab" />&nbsp;</form>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<h2 id="hn_Step_3:_Start_retrieving_and_importing" style="font-weight: 300; font-stretch: normal; font-size: 15pt; line-height: 18pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; margin: 0pt; color: rgb(153, 51, 85); padding-top: 10px; padding-bottom: 10px; text-shadow: rgb(238, 238, 238) 1px 1px 0px; background-color: transparent;">
	<a class="heading" href="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql#hn_Step_3:_Start_retrieving_and_importing" style="font-size: 15pt; line-height: 18pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 10px; padding-bottom: 10px; background-color: transparent;">Step 3: Start retrieving and importing</a></h2>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">For this step you will need to save a couple of scripts. You will also need the filelist.txt file from the previous step. I have instruction that you need to follow for some files. Also, you may need to install p7zip ubuntu package.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Save all of the following on your disk (same directory).</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">preimport.sql (source&nbsp;</span><a class="ext" href="http://www.brianstempin.com/2012/06/29/loading-the-english-wikipedia-dump-is-a-huge-pain/" style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; text-align: justify;" target="\&quot;_new\&quot;">Brian Stempin</a><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">)</span></p>

<p>
	&nbsp;</p>

<div class="code" style="border: 1px solid rgb(170, 170, 204); font-size: 11px; font-family: monospace; margin: auto; padding: 6px 5px 13px; overflow: auto; white-space: nowrap; line-height: 23.0399990081787px; background: rgb(243, 243, 255);">
	<span class="kw1" style="color: rgb(177, 177, 0);">SET</span>&nbsp;autocommit=<span class="nu0" style="color: rgb(204, 102, 204);">0</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">SET</span>&nbsp;unique_checks=<span class="nu0" style="color: rgb(204, 102, 204);">0</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">SET</span>&nbsp;foreign_key_checks=<span class="nu0" style="color: rgb(204, 102, 204);">0</span>;<br style="clear: none; line-height: 0.9em;" />
	BEGIN;</div>

<p>
	&nbsp;</p>

<form action="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql/grabcode" id="form_61092f4ded_4" method="post" style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<input class="grabcode" name="save" style="border-width: 1px; border-style: solid; border-color: rgb(239, 239, 239) rgb(170, 170, 204) rgb(170, 170, 204) rgb(238, 238, 255); color: rgb(51, 51, 102); font-stretch: normal; font-size: 10.0799999237061px; font-family: Verdana, sans-serif; padding-right: 0.2em; padding-left: 0.2em; line-height: 1em; float: right; background-color: rgb(208, 224, 240);" title="Download" type="submit" value="Grab" />&nbsp;</form>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">postimport.sql (source&nbsp;</span><a class="ext" href="http://www.brianstempin.com/2012/06/29/loading-the-english-wikipedia-dump-is-a-huge-pain/" style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; text-align: justify;" target="\&quot;_new\&quot;">Brian Stempin</a><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">)</span></p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<div class="code" style="border: 1px solid rgb(170, 170, 204); font-size: 11px; font-family: monospace; margin: auto; padding: 6px 5px 13px; overflow: auto; white-space: nowrap; line-height: 23.0399990081787px; background: rgb(243, 243, 255);">
	COMMIT;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">SET</span>&nbsp;autocommit=<span class="nu0" style="color: rgb(204, 102, 204);">1</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">SET</span>&nbsp;unique_checks=<span class="nu0" style="color: rgb(204, 102, 204);">1</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">SET</span>&nbsp;foreign_key_checks=<span class="nu0" style="color: rgb(204, 102, 204);">1</span>;</div>

<p>
	&nbsp;</p>

<form action="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql/grabcode" id="form_61092f4ded_5" method="post" style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<input class="grabcode" name="save" style="border-width: 1px; border-style: solid; border-color: rgb(239, 239, 239) rgb(170, 170, 204) rgb(170, 170, 204) rgb(238, 238, 255); color: rgb(51, 51, 102); font-stretch: normal; font-size: 10.0799999237061px; font-family: Verdana, sans-serif; padding-right: 0.2em; padding-left: 0.2em; line-height: 1em; float: right; background-color: rgb(208, 224, 240);" title="Download" type="submit" value="Grab" />&nbsp;</form>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">mwimport.pl (original source&nbsp;</span><a class="ext" href="http://meta.wikimedia.org/wiki/Data_dumps/mwimport" style="font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; text-align: justify;" target="\&quot;_new\&quot;">here</a><span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">)</span></p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<div class="code" style="border: 1px solid rgb(170, 170, 204); font-size: 11px; font-family: monospace; margin: auto; padding: 6px 5px 13px; overflow: auto; white-space: nowrap; line-height: 23.0399990081787px; background: rgb(243, 243, 255);">
	- You can drastically speed up the&nbsp;<a href="http://perldoc.perl.org/functions/import.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">import</span></a>&nbsp;process by commenting the insert line that adds information&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">for</span>&nbsp;the text table. Look&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">for</span>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"Comment this to save time"</span>&nbsp;in the code below.<br style="clear: none; line-height: 0.9em;" />
	<span class="co1" style="color: rgb(128, 128, 128); font-style: italic;">#!/usr/bin/perl -w</span><br style="clear: none; line-height: 0.9em;" />
	=head1 NAME<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	mwimport -- quick&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">and</span>&nbsp;dirty mediawiki importer<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	=head1 SYNOPSIS<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	cat pages.xml | mwimport&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">[</span>-<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>&nbsp;N|--skip=N<span class="br0" style="color: rgb(102, 204, 102);">]</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	=cut<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">use</span>&nbsp;strict;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">use</span>&nbsp;Getopt::<span class="me2">Long</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">use</span>&nbsp;Pod::<span class="me2">Usage</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="re0" style="color: rgb(0, 0, 255);">$cnt_page</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$cnt_rev</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">%namespace</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$ns_pattern</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$committed</span>&nbsp;=&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">0</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$skip</span>&nbsp;=&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">0</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="co1" style="color: rgb(128, 128, 128); font-style: italic;">## set this to 1 to match "mwdumper --format=sql:1.5" as close as possible</span><br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;Compat<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span>&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">0</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="co1" style="color: rgb(128, 128, 128); font-style: italic;"># 512kB is what mwdumper uses, but 4MB gives much better performance here</span><br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$Buffer_Size</span>&nbsp;= Compat ?&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">512</span>*<span class="nu0" style="color: rgb(204, 102, 204);">1024</span>&nbsp;:&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">4</span>*<span class="nu0" style="color: rgb(204, 102, 204);">1024</span>*<span class="nu0" style="color: rgb(204, 102, 204);">1024</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;textify<span class="br0" style="color: rgb(102, 204, 102);">(</span>$<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$l</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">for</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><a href="http://perldoc.perl.org/functions/defined.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">defined</span></a>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>/&amp;quot;/<span class="st0" style="color: rgb(255, 0, 0);">"/ig;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; s/&amp;lt;/&lt;/ig;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; s/&amp;gt;/&gt;/ig;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; /&amp;(?!amp;)(.*?;)/ and die "</span>textify: does&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">not</span>&nbsp;know &amp;$<span class="nu0" style="color: rgb(204, 102, 204);">1</span><span class="st0" style="color: rgb(255, 0, 0);">";<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; s/&amp;amp;/&amp;/ig;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; $l = length $_;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; s/<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\\</span>/<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\\</span><span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\\</span>/g;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; s/<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>/<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\\</span>n/g;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; s/'/<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\\</span>'/ig;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; Compat and s/"</span>/\\<span class="st0" style="color: rgb(255, 0, 0);">"/ig;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; $_ = "</span><span class="st0" style="color: rgb(255, 0, 0);">'$_'</span><span class="st0" style="color: rgb(255, 0, 0);">";<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; } else {<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; $l = 0;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; $_ = "</span><span class="st0" style="color: rgb(255, 0, 0);">''</span><span class="st0" style="color: rgb(255, 0, 0);">";<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; }<br style="clear: none; line-height: 0.9em;" />
	&nbsp; }<br style="clear: none; line-height: 0.9em;" />
	&nbsp; return $l;<br style="clear: none; line-height: 0.9em;" />
	}<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	sub getline()<br style="clear: none; line-height: 0.9em;" />
	{<br style="clear: none; line-height: 0.9em;" />
	&nbsp; $_ = &lt;&gt;;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; defined $_ or die "</span><a href="http://perldoc.perl.org/functions/eof.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">eof</span></a>&nbsp;at line $.\n<span class="st0" style="color: rgb(255, 0, 0);">";<br style="clear: none; line-height: 0.9em;" />
	}<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	sub ignore_elt($)<br style="clear: none; line-height: 0.9em;" />
	{<br style="clear: none; line-height: 0.9em;" />
	&nbsp; m|^<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\s</span>*&lt;$_[0]&gt;.*?&lt;/$_[0]&gt;<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>$| or die "</span>expected&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&nbsp;element in line $.\n<span class="st0" style="color: rgb(255, 0, 0);">";<br style="clear: none; line-height: 0.9em;" />
	&nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	}<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	sub simple_elt($$)<br style="clear: none; line-height: 0.9em;" />
	{<br style="clear: none; line-height: 0.9em;" />
	&nbsp; if (m|^<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\s</span>*&lt;$_[0]<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\s</span>*/&gt;<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>$|) {<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; $_[1]{$_[0]} = '';<br style="clear: none; line-height: 0.9em;" />
	&nbsp; } elsif (m|^<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\s</span>*&lt;$_[0]&gt;(.*?)&lt;/$_[0]&gt;<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>$|) {<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; $_[1]{$_[0]} = $1;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; } else {<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; die "</span>expected&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&nbsp;element in line $.\n<span class="st0" style="color: rgb(255, 0, 0);">";<br style="clear: none; line-height: 0.9em;" />
	&nbsp; }<br style="clear: none; line-height: 0.9em;" />
	&nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	}<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	sub simple_opt_elt($$)<br style="clear: none; line-height: 0.9em;" />
	{<br style="clear: none; line-height: 0.9em;" />
	&nbsp; if (m|^<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\s</span>*&lt;$_[0]<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\s</span>*/&gt;<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>$|) {<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; $_[1]{$_[0]} = '';<br style="clear: none; line-height: 0.9em;" />
	&nbsp; } elsif (m|^<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\s</span>*&lt;$_[0]&gt;(.*?)&lt;/$_[0]&gt;<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>$|) {<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; $_[1]{$_[0]} = $1;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; } else {<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; return;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; }<br style="clear: none; line-height: 0.9em;" />
	&nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	}<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	sub redirect_elt($)<br style="clear: none; line-height: 0.9em;" />
	{<br style="clear: none; line-height: 0.9em;" />
	&nbsp; if (m|^<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\s</span>*&lt;redirect<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\s</span>*title="</span><span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">[</span>^<span class="st0" style="color: rgb(255, 0, 0);">"]*)"</span>\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*/&gt;\n$|<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span>&nbsp;<span class="co1" style="color: rgb(128, 128, 128); font-style: italic;"># " -- GeSHI syntax highlighting breaks on this line</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>redirect<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;= $<span class="nu0" style="color: rgb(204, 102, 204);">1</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">else</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_opt_elt redirect =&gt;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/return.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">return</span></a>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;opening_tag<span class="br0" style="color: rgb(102, 204, 102);">(</span>$<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; m|^\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*&lt;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&gt;\n$|&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"expected $_[0] element in line $.<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;closing_tag<span class="br0" style="color: rgb(102, 204, 102);">(</span>$<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; m|^\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*&lt;/<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&gt;\n$|&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"$_[0]: expected closing tag in line $.<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;si_nss_namespace<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; m|^\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*&lt;namespace key=<span class="st0" style="color: rgb(255, 0, 0);">"(-?<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\d</span>+)"</span><span class="br0" style="color: rgb(102, 204, 102);">[</span>^/<span class="br0" style="color: rgb(102, 204, 102);">]</span>*?/&gt;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>\n|<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;m|^\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*&lt;namespace key=<span class="st0" style="color: rgb(255, 0, 0);">"(-?<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\d</span>+)"</span><span class="br0" style="color: rgb(102, 204, 102);">[</span>^&gt;<span class="br0" style="color: rgb(102, 204, 102);">]</span>*?&gt;<span class="br0" style="color: rgb(102, 204, 102);">(</span>.*?<span class="br0" style="color: rgb(102, 204, 102);">)</span>&lt;/namespace&gt;\n|<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"expected namespace element in line $.<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$namespace</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>$<span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;= $<span class="nu0" style="color: rgb(204, 102, 204);">1</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;si_namespaces<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; opening_tag<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">"namespaces"</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/eval.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">eval</span></a>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">while</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="nu0" style="color: rgb(204, 102, 204);">1</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; si_nss_namespace;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="co1" style="color: rgb(128, 128, 128); font-style: italic;"># note: $@ is always defined</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; $@ =~ /^expected namespace element /&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"namespaces: $@"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$ns_pattern</span>&nbsp;=&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">'^('</span>.<a href="http://perldoc.perl.org/functions/join.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">join</span></a><span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">'|'</span>,<a href="http://perldoc.perl.org/functions/map.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">map</span></a>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span>&nbsp;<a href="http://perldoc.perl.org/functions/quotemeta.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">quotemeta</span></a>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<a href="http://perldoc.perl.org/functions/keys.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">keys</span></a>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">%namespace</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>.<span class="st0" style="color: rgb(255, 0, 0);">'):'</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; closing_tag<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">"namespaces"</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;siteinfo<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; opening_tag<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">"siteinfo"</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/eval.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">eval</span></a>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">%site</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_elt sitename =&gt; \<span class="re0" style="color: rgb(0, 0, 255);">%site</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_elt base =&gt; \<span class="re0" style="color: rgb(0, 0, 255);">%site</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_elt generator =&gt; \<span class="re0" style="color: rgb(0, 0, 255);">%site</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$site</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>generator<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=~ /^MediaWiki&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">1</span>.20wmf1$/<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/warn.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">warn</span></a><span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">"siteinfo: untested generator '$site{generator}',"</span>,<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">" expect trouble ahead<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_elt&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">case</span>&nbsp;=&gt; \<span class="re0" style="color: rgb(0, 0, 255);">%site</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; si_namespaces;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/print.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">print</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"-- MediaWiki XML dump converted to SQL by mwimport<br style="clear: none; line-height: 0.9em;" />
	BEGIN;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	-- Site: $site{sitename}<br style="clear: none; line-height: 0.9em;" />
	-- URL: $site{base}<br style="clear: none; line-height: 0.9em;" />
	-- Generator: $site{generator}<br style="clear: none; line-height: 0.9em;" />
	-- Case: $site{case}<br style="clear: none; line-height: 0.9em;" />
	--<br style="clear: none; line-height: 0.9em;" />
	-- Namespaces:<br style="clear: none; line-height: 0.9em;" />
	"</span>,<a href="http://perldoc.perl.org/functions/map.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">map</span></a>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"-- $namespace{$_}: $_<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/sort.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">sort</span></a>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$namespace</span><span class="br0" style="color: rgb(102, 204, 102);">{</span><span class="re0" style="color: rgb(0, 0, 255);">$a</span><span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;&lt;=&gt;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$namespace</span><span class="br0" style="color: rgb(102, 204, 102);">{</span><span class="re0" style="color: rgb(0, 0, 255);">$b</span><span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<a href="http://perldoc.perl.org/functions/keys.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">keys</span></a>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">%namespace</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; $@&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">and</span>&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"siteinfo: $@"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; closing_tag<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">"siteinfo"</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;pg_rv_contributor<span class="br0" style="color: rgb(102, 204, 102);">(</span>$<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span>m|^\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*&lt;contributor deleted=<span class="st0" style="color: rgb(255, 0, 0);">"deleted"</span>\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*/&gt;\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*\n|<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">else</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; opening_tag&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"contributor"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">%c</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/eval.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">eval</span></a>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; simple_elt username =&gt; \<span class="re0" style="color: rgb(0, 0, 255);">%c</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; simple_elt id =&gt; \<span class="re0" style="color: rgb(0, 0, 255);">%c</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>contrib_user<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$c</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>username<span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>contrib_id<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;&nbsp; =&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$c</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>id<span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span>$@<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; $@ =~ /^expected username element /&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"contributor: $@"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/eval.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">eval</span></a>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; simple_elt ip =&gt; \<span class="re0" style="color: rgb(0, 0, 255);">%c</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>contrib_user<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$c</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>ip<span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; $@&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">and</span>&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"contributor: $@"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; closing_tag&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"contributor"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;pg_rv_comment<span class="br0" style="color: rgb(102, 204, 102);">(</span>$<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span>m|^\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*&lt;comment\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*/&gt;\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*\n|<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">elsif</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span>m|^\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*&lt;comment deleted=<span class="st0" style="color: rgb(255, 0, 0);">"deleted"</span>\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*/&gt;\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*\n|<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">elsif</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span>s|^\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*<span class="re4" style="color: rgb(0, 153, 153);">&lt;comment&gt;</span><span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">[</span>^&lt;<span class="br0" style="color: rgb(102, 204, 102);">]</span>*<span class="br0" style="color: rgb(102, 204, 102);">)</span>||g<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">while</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="nu0" style="color: rgb(204, 102, 204);">1</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>comment<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;.= $<span class="nu0" style="color: rgb(204, 102, 204);">1</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">last</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; s|^<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">[</span>^&lt;<span class="br0" style="color: rgb(102, 204, 102);">]</span>*<span class="br0" style="color: rgb(102, 204, 102);">)</span>||;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; closing_tag&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"comment"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">else</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/return.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">return</span></a>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;pg_rv_text<span class="br0" style="color: rgb(102, 204, 102);">(</span>$<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span>m|^\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*&lt;text xml:space=<span class="st0" style="color: rgb(255, 0, 0);">"preserve"</span>\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*/&gt;\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*\n|<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>text<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">''</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">elsif</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span>m|^\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*&lt;text deleted=<span class="st0" style="color: rgb(255, 0, 0);">"deleted"</span>\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*/&gt;\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*\n|<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>text<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">''</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">elsif</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span>s|^\<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>*&lt;text xml:space=<span class="st0" style="color: rgb(255, 0, 0);">"preserve"</span>&gt;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">[</span>^&lt;<span class="br0" style="color: rgb(102, 204, 102);">]</span>*<span class="br0" style="color: rgb(102, 204, 102);">)</span>||g<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">while</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="nu0" style="color: rgb(204, 102, 204);">1</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>text<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;.= $<span class="nu0" style="color: rgb(204, 102, 204);">1</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">last</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; getline;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; s|^<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">[</span>^&lt;<span class="br0" style="color: rgb(102, 204, 102);">]</span>*<span class="br0" style="color: rgb(102, 204, 102);">)</span>||;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; closing_tag&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"text"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">else</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"expected text element in line $.<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$start</span>&nbsp;=&nbsp;<a href="http://perldoc.perl.org/functions/time.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">time</span></a>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;stats<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$s</span>&nbsp;=&nbsp;<a href="http://perldoc.perl.org/functions/time.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">time</span></a>&nbsp;-&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$start</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$s</span>&nbsp;||=&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">1</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/printf.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">printf</span></a>&nbsp;<span class="kw2" style="font-weight: bold;">STDERR</span>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"%9d pages (%7.3f/s), %9d revisions (%7.3f/s) in %d seconds<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>,<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$cnt_page</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$cnt_page</span>/<span class="re0" style="color: rgb(0, 0, 255);">$s</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$cnt_rev</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$cnt_rev</span>/<span class="re0" style="color: rgb(0, 0, 255);">$s</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$s</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="co1" style="color: rgb(128, 128, 128); font-style: italic;">### flush_rev($text, $rev, $page)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;flush_rev<span class="br0" style="color: rgb(102, 204, 102);">(</span>$$$<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/return.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">return</span></a>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">for</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$i</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span>,<span class="nu0" style="color: rgb(204, 102, 204);">1</span>,<span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="re0" style="color: rgb(0, 0, 255);">$i</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&nbsp;=~&nbsp;<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>/,\n?$//;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/print.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">print</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"INSERT INTO text(old_id,old_text,old_flags) VALUES $_[0];<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;&nbsp;<span class="co1" style="color: rgb(128, 128, 128); font-style: italic;">#Comment this to save time</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">and</span>&nbsp;<a href="http://perldoc.perl.org/functions/print.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">print</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"INSERT INTO page(page_id,page_namespace,page_title,page_restrictions,page_counter,page_is_redirect,page_is_new,page_random,page_touched,page_latest,page_len) VALUES $_[2];<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/print.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">print</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"INSERT INTO revision(rev_id,rev_page,rev_text_id,rev_comment,rev_user,rev_user_text,rev_timestamp,rev_minor_edit,rev_deleted,rev_len,rev_parent_id) VALUES $_[1];<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">for</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$i</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span>,<span class="nu0" style="color: rgb(204, 102, 204);">1</span>,<span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="re0" style="color: rgb(0, 0, 255);">$i</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&nbsp;=&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">''</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="co1" style="color: rgb(128, 128, 128); font-style: italic;">### flush($text, $rev, $page)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;flush<span class="br0" style="color: rgb(102, 204, 102);">(</span>$$$<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; flush_rev&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">1</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/print.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">print</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"COMMIT;<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$committed</span>&nbsp;=&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$cnt_page</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="co1" style="color: rgb(128, 128, 128); font-style: italic;">### pg_revision(\%page, $skip, $text, $rev, $page)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;pg_revision<span class="br0" style="color: rgb(102, 204, 102);">(</span>$$$$$<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>&nbsp;=&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; opening_tag&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"revision"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/eval.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">eval</span></a>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">%revision</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_elt id =&gt;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_opt_elt parentid =&gt;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_elt timestamp =&gt;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; pg_rv_contributor&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_opt_elt minor =&gt;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; pg_rv_comment&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; pg_rv_text&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_opt_elt sha1 =&gt;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_opt_elt model =&gt;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_opt_elt&nbsp;<a href="http://perldoc.perl.org/functions/format.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">format</span></a>&nbsp;=&gt;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; $@&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">and</span>&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"revision: $@"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; closing_tag&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"revision"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">1</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">and</span>&nbsp;<a href="http://perldoc.perl.org/functions/return.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">return</span></a>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$$rev</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>id<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=~ /^\d+$/&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/return.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">return</span></a><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/warn.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">warn</span></a><span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">"page '$_[0]{title}': ignoring bogus revision id '$$rev{id}'<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>latest_len<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;= textify&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$$rev</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>text<span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">for</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$f</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><a href="http://perldoc.perl.org/functions/qw.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">qw</span></a><span class="br0" style="color: rgb(102, 204, 102);">(</span>comment contrib_user<span class="br0" style="color: rgb(102, 204, 102);">)</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; textify&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$$rev</span><span class="br0" style="color: rgb(102, 204, 102);">{</span><span class="re0" style="color: rgb(0, 0, 255);">$f</span><span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$$rev</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>timestamp<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=~<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>/^<span class="br0" style="color: rgb(102, 204, 102);">(</span>\d\d\d\d<span class="br0" style="color: rgb(102, 204, 102);">)</span>-<span class="br0" style="color: rgb(102, 204, 102);">(</span>\d\d<span class="br0" style="color: rgb(102, 204, 102);">)</span>-<span class="br0" style="color: rgb(102, 204, 102);">(</span>\d\d<span class="br0" style="color: rgb(102, 204, 102);">)</span>T<span class="br0" style="color: rgb(102, 204, 102);">(</span>\d\d<span class="br0" style="color: rgb(102, 204, 102);">)</span>:<span class="br0" style="color: rgb(102, 204, 102);">(</span>\d\d<span class="br0" style="color: rgb(102, 204, 102);">)</span>:<span class="br0" style="color: rgb(102, 204, 102);">(</span>\d\d<span class="br0" style="color: rgb(102, 204, 102);">)</span>Z$/<span class="st0" style="color: rgb(255, 0, 0);">'$1$2$3$4$5$6'</span>/<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/return.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">return</span></a>&nbsp;<a href="http://perldoc.perl.org/functions/warn.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">warn</span></a><span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">"page '$_[0]{title}' rev $$rev{id}: "</span>,<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"bogus timestamp '$$rev{timestamp}'<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&nbsp;.=&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"($$rev{id},$$rev{text},'utf-8'),<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$$rev</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>minor<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=&nbsp;<a href="http://perldoc.perl.org/functions/defined.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">defined</span></a>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$$rev</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>minor<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;?&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">1</span>&nbsp;:&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">0</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">3</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&nbsp;.=&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"($$rev{id},$_[0]{id},$$rev{id},$$rev{comment},"</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; .<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="re0" style="color: rgb(0, 0, 255);">$$rev</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>contrib_id<span class="br0" style="color: rgb(102, 204, 102);">}</span>||<span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; .<span class="st0" style="color: rgb(255, 0, 0);">",$$rev{contrib_user},$$rev{timestamp},$$rev{minor},0,$_[0]{latest_len},$_[0]{latest}),<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>latest<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$$rev</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>id<span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>latest_start<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=&nbsp;<a href="http://perldoc.perl.org/functions/substr.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">substr</span></a>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$$rev</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>text<span class="br0" style="color: rgb(102, 204, 102);">}</span>,&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">0</span>,&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">60</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><a href="http://perldoc.perl.org/functions/length.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">length</span></a>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&nbsp;&gt;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$Buffer_Size</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; flush_rev&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">3</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">4</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>do_commit<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">1</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; ++<span class="re0" style="color: rgb(0, 0, 255);">$cnt_rev</span>&nbsp;%&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">1000</span>&nbsp;==&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">0</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">and</span>&nbsp;stats;<br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="co1" style="color: rgb(128, 128, 128); font-style: italic;">### page($text, $rev, $page)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;page<span class="br0" style="color: rgb(102, 204, 102);">(</span>$$$<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; opening_tag&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"page"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">%page</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; ++<span class="re0" style="color: rgb(0, 0, 255);">$cnt_page</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/eval.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">eval</span></a>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_elt title =&gt; \<span class="re0" style="color: rgb(0, 0, 255);">%page</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_opt_elt ns =&gt; \<span class="re0" style="color: rgb(0, 0, 255);">%page</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_elt id =&gt; \<span class="re0" style="color: rgb(0, 0, 255);">%page</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; redirect_elt \<span class="re0" style="color: rgb(0, 0, 255);">%page</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; simple_opt_elt restrictions =&gt; \<span class="re0" style="color: rgb(0, 0, 255);">%page</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$page</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>latest<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">0</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">while</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="nu0" style="color: rgb(204, 102, 204);">1</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; pg_revision \<span class="re0" style="color: rgb(0, 0, 255);">%page</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$skip</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">1</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="co1" style="color: rgb(128, 128, 128); font-style: italic;"># note: $@ is always defined</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; $@ =~ /^expected revision element /&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"page: $@"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; closing_tag&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"page"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="re0" style="color: rgb(0, 0, 255);">$skip</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; --<span class="re0" style="color: rgb(0, 0, 255);">$skip</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">else</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$page</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>id<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=~ /^\d+$/<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/warn.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">warn</span></a><span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">"page '$page{title}': bogus id '$page{id}'<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$ns</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="re0" style="color: rgb(0, 0, 255);">$page</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>title<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=~&nbsp;<a href="http://perldoc.perl.org/functions/s.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">s</span></a>/<span class="re0" style="color: rgb(0, 0, 255);">$ns_pattern</span>//o<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$ns</span>&nbsp;=&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$namespace</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>$<span class="nu0" style="color: rgb(204, 102, 204);">1</span><span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">else</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$ns</span>&nbsp;=&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">0</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">for</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$f</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><a href="http://perldoc.perl.org/functions/qw.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">qw</span></a><span class="br0" style="color: rgb(102, 204, 102);">(</span>title restrictions<span class="br0" style="color: rgb(102, 204, 102);">)</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; textify&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$page</span><span class="br0" style="color: rgb(102, 204, 102);">{</span><span class="re0" style="color: rgb(0, 0, 255);">$f</span><span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span>Compat<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$page</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>redirect<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$page</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>latest_start<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=~ /^<span class="st0" style="color: rgb(255, 0, 0);">'#(?:REDIRECT|redirect) / ?<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; 1 : 0;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; } else {<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; $page{redirect} = $page{latest_start} =~ /^'</span><span class="co1" style="color: rgb(128, 128, 128); font-style: italic;">#REDIRECT /i ? 1 : 0;</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$page</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>title<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=~&nbsp;<a href="http://perldoc.perl.org/functions/y.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">y</span></a>/ /_/;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span>Compat<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&nbsp;.=&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"($page{id},$ns,$page{title},$page{restrictions},0,"</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; .<span class="st0" style="color: rgb(255, 0, 0);">"$page{redirect},0,RAND(),"</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; .<span class="st0" style="color: rgb(255, 0, 0);">"DATE_ADD('1970-01-01', INTERVAL UNIX_TIMESTAMP() SECOND)+0,"</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; .<span class="st0" style="color: rgb(255, 0, 0);">"$page{latest},$page{latest_len}),<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">else</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>&nbsp;.=&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"($page{id},$ns,$page{title},$page{restrictions},0,"</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; .<span class="st0" style="color: rgb(255, 0, 0);">"$page{redirect},0,RAND(),NOW()+0,$page{latest},$page{latest_len}),<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="re0" style="color: rgb(0, 0, 255);">$page</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>do_commit<span class="br0" style="color: rgb(102, 204, 102);">}</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; flush&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">0</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">1</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$_</span><span class="br0" style="color: rgb(102, 204, 102);">[</span><span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">]</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/print.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">print</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"BEGIN;<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sub</span>&nbsp;terminate<br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"terminated by SIG$_[0]<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$SchemaVer</span>&nbsp;=&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">'0.8'</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$SchemaLoc</span>&nbsp;=&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"http://www.mediawiki.org/xml/export-$SchemaVer/"</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$Schema</span>&nbsp;&nbsp; &nbsp;=&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"http://www.mediawiki.org/xml/export-$SchemaVer.xsd"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$help</span>;<br style="clear: none; line-height: 0.9em;" />
	GetOptions<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">"skip=i"</span>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; =&gt; \<span class="re0" style="color: rgb(0, 0, 255);">$skip</span>,<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"help"</span>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; =&gt; \<span class="re0" style="color: rgb(0, 0, 255);">$help</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;pod2usage<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="nu0" style="color: rgb(204, 102, 204);">2</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>;<br style="clear: none; line-height: 0.9em;" />
	<span class="re0" style="color: rgb(0, 0, 255);">$help</span>&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">and</span>&nbsp;pod2usage<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="nu0" style="color: rgb(204, 102, 204);">1</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	getline;<br style="clear: none; line-height: 0.9em;" />
	m|^&lt;mediawiki \Qxmlns=<span class="st0" style="color: rgb(255, 0, 0);">"$SchemaLoc"</span>&nbsp;xmlns:xsi=<span class="st0" style="color: rgb(255, 0, 0);">"http://www.w3.org/2001/XMLSchema-instance"</span>&nbsp;xsi:schemaLocation=<span class="st0" style="color: rgb(255, 0, 0);">"$SchemaLoc $Schema"</span>&nbsp;version=<span class="st0" style="color: rgb(255, 0, 0);">"$SchemaVer"</span>\E xml:lang=<span class="st0" style="color: rgb(255, 0, 0);">".."</span>&gt;$|<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"unknown schema or invalid first line<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	getline;<br style="clear: none; line-height: 0.9em;" />
	<span class="re0" style="color: rgb(0, 0, 255);">$SIG</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>TERM<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;=&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$SIG</span><span class="br0" style="color: rgb(102, 204, 102);">{</span>INT<span class="br0" style="color: rgb(102, 204, 102);">}</span>&nbsp;= \&amp;terminate;<br style="clear: none; line-height: 0.9em;" />
	siteinfo;<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">my</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="re0" style="color: rgb(0, 0, 255);">$text</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$page</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;=&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="st0" style="color: rgb(255, 0, 0);">''</span>,&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">''</span>,&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">''</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>;<br style="clear: none; line-height: 0.9em;" />
	<a href="http://perldoc.perl.org/functions/eval.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">eval</span></a>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">while</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">(</span><span class="nu0" style="color: rgb(204, 102, 204);">1</span><span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">{</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; page&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$text</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$page</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">}</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">}</span>;<br style="clear: none; line-height: 0.9em;" />
	$@ =~ /^expected page element /&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"$@ (committed $committed pages)<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	flush&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$text</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$rev</span>,&nbsp;<span class="re0" style="color: rgb(0, 0, 255);">$page</span>;<br style="clear: none; line-height: 0.9em;" />
	stats;<br style="clear: none; line-height: 0.9em;" />
	m|&lt;/mediawiki&gt;|&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;<a href="http://perldoc.perl.org/functions/die.html" style="font-family: inherit; font-size: inherit; line-height: 13pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px;"><span class="kw3" style="color: rgb(0, 0, 102);">die</span></a>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"mediawiki: expected closing tag in line $.<span class="es0" style="color: rgb(0, 0, 153); font-weight: bold;">\n</span>"</span>;<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	=head1 COPYRIGHT<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	Copyright&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">2007</span>&nbsp;by Robert Bihlmeyer<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	This program is free software; you can redistribute it&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">and</span>/<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;modify<br style="clear: none; line-height: 0.9em;" />
	it under the terms of the GNU General Public License as published by<br style="clear: none; line-height: 0.9em;" />
	the Free Software Foundation; either version&nbsp;<span class="nu0" style="color: rgb(204, 102, 204);">2</span>&nbsp;of the License,&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span><br style="clear: none; line-height: 0.9em;" />
	<span class="br0" style="color: rgb(102, 204, 102);">(</span>at your option<span class="br0" style="color: rgb(102, 204, 102);">)</span>&nbsp;any later version.<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	You may also redistribute&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">and</span>/<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;modify this software under the terms<br style="clear: none; line-height: 0.9em;" />
	of the GNU Free Documentation License without invariant sections,&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">and</span><br style="clear: none; line-height: 0.9em;" />
	without front-cover&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;back-cover texts.<br style="clear: none; line-height: 0.9em;" />
	&nbsp;<br style="clear: none; line-height: 0.9em;" />
	This program is distributed in the hope that it will be useful,<br style="clear: none; line-height: 0.9em;" />
	but WITHOUT ANY WARRANTY; without even the implied warranty of<br style="clear: none; line-height: 0.9em;" />
	MERCHANTABILITY&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">or</span>&nbsp;FITNESS FOR A PARTICULAR PURPOSE. &nbsp;See the<br style="clear: none; line-height: 0.9em;" />
	GNU General Public License&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">for</span>&nbsp;more details.</div>

<p>
	&nbsp;</p>

<form action="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql/grabcode" id="form_61092f4ded_6" method="post" style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<input class="grabcode" name="save" style="border-width: 1px; border-style: solid; border-color: rgb(239, 239, 239) rgb(170, 170, 204) rgb(170, 170, 204) rgb(238, 238, 255); color: rgb(51, 51, 102); font-stretch: normal; font-size: 10.0799999237061px; font-family: Verdana, sans-serif; padding-right: 0.2em; padding-left: 0.2em; line-height: 1em; float: right; background-color: rgb(208, 224, 240);" title="Download" type="submit" value="Grab" />&nbsp;</form>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">adddb.sh - You will need to change the urll variable to the url that you will be using. Wikipedia releases some files under bz2 compression and other files under 7z. I left a line commented in this script that you need to enable in case your files are bz2 and not 7z (don't forget to comment the line underneath that that deals with 7z). Finally, you will need to add your mysql username password and database.</span></p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<div class="code" style="border: 1px solid rgb(170, 170, 204); font-size: 11px; font-family: monospace; margin: auto; padding: 6px 5px 13px; overflow: auto; white-space: nowrap; line-height: 23.0399990081787px; background: rgb(243, 243, 255);">
	!/bin/bash<br style="clear: none; line-height: 0.9em;" />
	<span class="re2" style="color: rgb(0, 0, 255);">urll=</span><span class="st0" style="color: rgb(255, 0, 0);">'http://dumps.wikimedia.org/enwiki/20130102/'</span><br style="clear: none; line-height: 0.9em;" />
	<br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">for</span>&nbsp;i&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">in</span>&nbsp;$<span class="br0" style="color: rgb(102, 204, 102);">(</span>&nbsp;<span class="kw2" style="font-weight: bold;">cat</span>&nbsp;filelist.txt&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">)</span>;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">do</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw3" style="color: rgb(0, 0, 102);">echo</span>&nbsp;retrieving:&nbsp;<span class="re1" style="color: rgb(0, 0, 255);">$i</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw2" style="font-weight: bold;">wget</span>&nbsp;<span class="re1" style="color: rgb(0, 0, 255);">$urll</span><span class="re1" style="color: rgb(0, 0, 255);">$i</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw3" style="color: rgb(0, 0, 102);">echo</span>&nbsp;mwimport:&nbsp;<span class="re1" style="color: rgb(0, 0, 255);">$i</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="re3">#bzcat&nbsp;<span class="re1" style="color: rgb(0, 0, 255);">$i</span>&nbsp;|&nbsp;<span class="kw2" style="font-weight: bold;">perl</span>&nbsp;mwimport.pl &gt; temp.sql</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; 7za e -so&nbsp;<span class="re1" style="color: rgb(0, 0, 255);">$i</span>&nbsp;|perl mwimport.pl &gt; temp.sql<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="re2" style="color: rgb(0, 0, 255);">result=</span>$<span class="br0" style="color: rgb(102, 204, 102);">(</span>&nbsp;<span class="kw2" style="font-weight: bold;">tail</span>&nbsp;temp.sql -n1&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">)</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="re2" style="color: rgb(0, 0, 255);">result1=</span><span class="st0" style="color: rgb(255, 0, 0);">'COMMIT;'</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw3" style="color: rgb(0, 0, 102);">echo</span>&nbsp;checking...<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw3" style="color: rgb(0, 0, 102);">echo</span>&nbsp;<span class="re1" style="color: rgb(0, 0, 255);">$result</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">if</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">[</span>&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"$result"</span>&nbsp;=&nbsp;<span class="st0" style="color: rgb(255, 0, 0);">"$result1"</span>&nbsp;<span class="br0" style="color: rgb(102, 204, 102);">]</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">then</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw3" style="color: rgb(0, 0, 102);">echo</span>&nbsp;OKAY:&nbsp;<span class="re1" style="color: rgb(0, 0, 255);">$i</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw2" style="font-weight: bold;">rm</span>&nbsp;<span class="re1" style="color: rgb(0, 0, 255);">$i</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw2" style="font-weight: bold;">cat</span>&nbsp;preimport.sql temp.sql postimport.sql | mysql -f -u &lt;USER IN MYSQL&gt; -p&lt;PASSWORD&gt; --<span class="re2" style="color: rgb(0, 0, 255);">database=</span>&lt;MEDIAWIKI DB&gt;<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw2" style="font-weight: bold;">rm</span>&nbsp;temp.sql<br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">else</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw3" style="color: rgb(0, 0, 102);">echo</span>&nbsp;FAIL:&nbsp;<span class="re1" style="color: rgb(0, 0, 255);">$i</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw3" style="color: rgb(0, 0, 102);">exit</span><br style="clear: none; line-height: 0.9em;" />
	&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span class="kw1" style="color: rgb(177, 177, 0);">fi</span><br style="clear: none; line-height: 0.9em;" />
	<span class="kw1" style="color: rgb(177, 177, 0);">done</span></div>

<p>
	&nbsp;</p>

<form action="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql/grabcode" id="form_61092f4ded_7" method="post" style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<input class="grabcode" name="save" style="border-width: 1px; border-style: solid; border-color: rgb(239, 239, 239) rgb(170, 170, 204) rgb(170, 170, 204) rgb(238, 238, 255); color: rgb(51, 51, 102); font-stretch: normal; font-size: 10.0799999237061px; font-family: Verdana, sans-serif; padding-right: 0.2em; padding-left: 0.2em; line-height: 1em; float: right; background-color: rgb(208, 224, 240);" title="Download" type="submit" value="Grab" />&nbsp;</form>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<h2 id="hn_Step_4:_Run_sh_adddb.sh" style="font-weight: 300; font-stretch: normal; font-size: 15pt; line-height: 18pt; font-family: 'Droid Serif', 'Open Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'DejaVu Sans', Arial, 'Trebuchet MS', Verdana, sans-serif; margin: 0pt; color: rgb(153, 51, 85); padding-top: 10px; padding-bottom: 10px; text-shadow: rgb(238, 238, 238) 1px 1px 0px; background-color: transparent;">
	<a class="heading" href="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql#hn_Step_4:_Run_sh_adddb.sh" style="font-size: 15pt; line-height: 18pt; width: auto; color: rgb(153, 51, 85); text-shadow: rgb(238, 238, 238) 1px 1px 0px; font-stretch: normal; margin: 0pt; padding-top: 10px; padding-bottom: 10px; background-color: transparent;">Step 4: Run "sh adddb.sh"</a></h2>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<p>
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">The script will start retrieving the first file in filelist.txt, extract it and process it using mwimport.pl, check on whether the extraction is successful and finally add the information to the database. After that, it will delete the file and carry on with the second file in filelist.txt all the way to the end.</span><br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">You will need to do this preferably under a screen. If you don't have it installed:</span></p>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<div class="code" style="border: 1px solid rgb(170, 170, 204); font-size: 11px; font-family: monospace; margin: auto; padding: 6px 5px 13px; overflow: auto; white-space: nowrap; line-height: 23.0399990081787px; background: rgb(243, 243, 255);">
	<span class="kw2" style="font-weight: bold;">sudo</span>&nbsp;aptitude&nbsp;<span class="kw2" style="font-weight: bold;">install</span>&nbsp;screen<br style="clear: none; line-height: 0.9em;" />
	screen -t Restoringwiki<br style="clear: none; line-height: 0.9em;" />
	<span class="kw2" style="font-weight: bold;">sh</span>&nbsp;adddb.<span class="kw2" style="font-weight: bold;">sh</span></div>

<p>
	&nbsp;</p>

<p>
	&nbsp;</p>

<form action="http://tsikerdekis.wuwcorp.com/WikipediaDumpstoMysql/grabcode" id="form_61092f4ded_8" method="post" style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">
	<input class="grabcode" name="save" style="border-width: 1px; border-style: solid; border-color: rgb(239, 239, 239) rgb(170, 170, 204) rgb(170, 170, 204) rgb(238, 238, 255); color: rgb(51, 51, 102); font-stretch: normal; font-size: 10.0799999237061px; font-family: Verdana, sans-serif; padding-right: 0.2em; padding-left: 0.2em; line-height: 1em; float: right; background-color: rgb(208, 224, 240);" title="Download" type="submit" value="Grab" />&nbsp;</form>

<p>
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<br style="clear: none; line-height: 0.9em; color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; text-align: justify;" />
	<span style="color: rgb(68, 68, 68); font-family: 'Droid Serif', 'Open Sans', Cambria, Georgia, 'DejaVu Serif', serif; font-size: 14.3999996185303px; line-height: 23.0399990081787px; text-align: justify;">Ctrl+A+D will detach the screen. It's still active in the background. To view the progress you can enter that screen again by using "screen -r " and hit tab to get the number of the screen automatically.</span></p>
