---
title: "PHx Custom Modifiers"
_old_id: "693"
_old_uri: "evo/phx/phx-custom-modifiers"
---

Installation
============

1. Login to the MODx manager
2. Go to Resources -> Manage Resources -> Snippets
3. Click "new snippet"
4. Prefix the snippet name with "**phx:**"
5. Copy the code to that new snippet and save it

or

1. Create new file with "snippet.phx.php" in assets/plugins/phx/modifiers/ (Example: assets/plugins/phx/modifiers/phpthumb.phx.php)
2. Copy the code to that new file, and here you go! :)
3. Change the permissions to 0644 to your new file

Contributed
===========

phx:7bit
--------

- description: returns the 7bit representation of a string
- usage: **\[<ins>string:7bit</ins>\]**

```
<pre class="brush: php">
<?php
$text = mb_convert_encoding($output,'HTML-ENTITIES',mb_detect_encoding($output));
$text = preg_replace(array('/&szlig;/','/&(..)lig;/','/&([aouAOU])uml;/','/&(.)[^;]*;/'),array('ss',"$1","$1".'e',"$1"),$text);
return $text;
?>

```phx:age
-------

- description: Takes a unixtime date input and calculates the age in years. It kind of fakes leap years but it's probably close enough for most uses.
- example: Set a TV called "date\_birth" with the unixtime widget, then output it as \[<ins>date\_birth:age</ins>\]
- usage: \[<ins>date\_birth:age</ins>\]
- added by: themancan

```
<pre class="brush: php">
<?php
$output = floor((time() - $output) / 31557600);
return $output;
?>

```phx:alternateClass
------------------

- description: Allows rows or other repeating elements to alternate provided the $output is an integer generated by a loop.
- usage: **\[<ins>phx:alternateClass=`class name`</ins>\]**
- example: **<li class="\[<ins>maxigallery.picture.pos:alternativeClass=`alt`</ins>\]">** outputs **<li class="alt">** for every other <li> in the loop.
- notes: Originally designed for use with [MaxiGallery](http://wiki.modxcms.com/index.php/MaxiGallery) to apply an alternating class to the <li> galleryPictureTpl template chunk.
- added by: smashingred

```
<pre class="brush: php">
<?php
if($output % 2)
{
echo $options;
}else{
echo '';
}
?>

```phx:bbcode
----------

- description: parse bb code (also escapes all html and MODx tags characters)
- usage: **\[<ins>variable:bbcode</ins>\]**

```
<pre class="brush: php">
<?php
$string = preg_replace("/&amp;(#[0-9]+|[a-z]+);/i", "&$1;", htmlspecialchars($output));
$string = preg_replace('~\[b\](.+?)\[/b\]~is', '<b>\1</b>', $string);
$string = preg_replace('~\[i\](.+?)\[/i\]~is', '<i>\1</i>', $string);
$string = preg_replace('~\[u\](.+?)\[/u\]~is', '<span style="text-decoration: underline;">\1</span>', $string);
$string = preg_replace('~\[link\]www.(.+?)\[/link\]~is', '<a href="http://www.\1" target="_blank">www.\1</a>', $string);
$string = preg_replace('~\[link\](.+?)\[/link\]~is', '<a href="\1" target="_blank">\1</a>', $string);
$string = preg_replace('~\[link=(.+?)\](.+?)\[/link\]~is', '<a href="\1" target="_blank">\2</a>', $string);
$string = preg_replace('~\[url\]www.(.+?)\[/url\]~is', '<a href="http://www.\1" target="_blank">www.\1</a>', $string);
$string = preg_replace('~\[url\](.+?)\[/url\]~is', '<a href="\1" target="_blank">\1</a>', $string);
$string = preg_replace('~\[url=(.+?)\](.+?)\[/url\]~is', '<a href="\1" target="_blank">\2</a>', $string);
$string = preg_replace('~\[img\](.+?)\[/img\]~is', '<img src="\1" alt="[image]" style="margin: 5px 0px 5px 0px" />', $string);
$string = preg_replace('~\[img-l\](.+?)\[/img\]~is', '<img src="\1" alt="[image]" style="border: thin solid #DFE5F2; FLOAT: left; MARGIN-RIGHT: 20px" />', $string);
$string = preg_replace('~\[img-r\](.+?)\[/img\]~is', '<img src="\1" alt="[image]" style="border: thin solid #DFE5F2; FLOAT: right; MARGIN-LEFT: 20px;" />', $string);
$string = preg_replace('~\[color=(.+?)\](.+?)\[/color\]~is', '<font color="\1">\2</font>', $string);
$string = preg_replace('~\[left\](.+?)\[/left\]~is', '<div style="text-align:left;">\1</u>', $string);
$string = preg_replace('~\[center\](.+?)\[/center\]~is', '<div style="text-align:center;">\1</u>', $string);
$string = preg_replace('~\[right\](.+?)\[/right\]~is', '<div style="text-align:right;">\1</u>', $string);

$string = str_replace(array("[","]","`"),array("&#91;","&#93;","&#96;"),$string);

return $string;
?>

```phx:cdata
---------

- description: Surround content with CDATA without the whitespace
- usage: **\[<ins>content:cdata</ins>\]**
- added by: Hori

```
<pre class="brush: php">
<?php
if (!function_exists('cdata')){
function cdata($inhoud){
$before='<![CDATA[';
$after=']]>';
return $before.$inhoud.$after;
}
}
return cdata($output);
?>

```phx:character\_limit
--------------------

- description: limits content to specified number of characters (good for news summary)
- usage: **\[****+content:character\_limit=`35`+****\]** - limits characters to 35
- added by: themancan

```
<pre class="brush: php">
<?php
$output_truncated = substr($output, 0, $options);
return $output_truncated;
?>

```- Improved version by prouve

```
<pre class="brush: php">
<?php
if (strlen($output) > $options) {
$output_cutted = substr($output, 0, $options);
$last_space = strrpos($output_cutted, " ");
$output = substr($output_cutted, 0, $last_space) . " [...]";
}
return $output;
?>

```phx:checkdomainexists
---------------------

- description: See if a domain is avaliable or not, it acts with Conditional operators.
- usages:
- **\[<ins>phx:checkdomainexists=`www.mydomain.com`:then=`it exists!`:else=`Houston, we've got a problem!`</ins>\]**
- **\[<ins>phx:checkdomainexists=`\[+phx:post=`domain`</ins>\]`:then=`it exists!`:else=`Houston, we've got a problem!`+\]**
- added by: CuSS (José Moreira - [Microdual](http://www.microdual.com/))

```
<pre class="brush: php">
<?php
/*
// checkdomainexists - Phx Script Created by José Moreira (www.microdual.com)
// Based on: AJAX Domain Availablity Check script (Version 0.93) - Created by "Ganesh" @ http://www.bootstrike.com/Webdesign/
// Give him an applause!
*/
 
/****************************************************
Usage: isDomainAvailable(domain,domaintlds)
Examples:
isDomainAvailable("www.microdual.com") //to allow all tlds
isDomainAvailable("www.microdual.com", array('com','net','org')); // to allow only com, net and org domains.
isDomainAvailable("www.microdual.com", array('com.sg')); // to allow only com.sg domain checking
****************************************************/
function isDomainAvailable($domain,$allowedTLDs=null){
global $error;
/* data from dnservers.php */
$ext = array(
// '.EXT' => array('WHOIS SERVER NAME','Text To Match for Available Domain'),
'.com' => array('whois.crsnic.net','No match for'),
'.net' => array('whois.crsnic.net','No match for'),
'.org' => array('whois.publicinterestregistry.net','NOT FOUND'),
'.us' => array('whois.nic.us','Not Found'),
'.biz' => array('whois.biz','Not found'),
'.info' => array('whois.afilias.net','NOT FOUND'),
'.mobi' => array('whois.dotmobiregistry.net', 'NOT FOUND'),
'.tv' => array('whois.nic.tv', 'No match for'),
'.in' => array('whois.inregistry.net', 'NOT FOUND'),
'.co.uk' => array('whois.nic.uk','No match'),
'.co.ug' => array('wawa.eahd.or.ug','No entries found'),
'.or.ug' => array('wawa.eahd.or.ug','No entries found'),
'.sg' => array('whois.nic.net.sg','Domain Not Found'),
'.com.sg' => array('whois.nic.net.sg','Domain Not Found'),
'.per.sg' => array('whois.nic.net.sg','Domain Not Found'),
'.org.sg' => array('whois.nic.net.sg','Domain Not Found'),
'.com.my' => array('whois.mynic.net.my','does not Exist in database'),
'.net.my' => array('whois.mynic.net.my','does not Exist in database'),
'.org.my' => array('whois.mynic.net.my','does not Exist in database'),
'.edu.my' => array('whois.mynic.net.my','does not Exist in database'),
'.my' => array('whois.mynic.net.my','does not Exist in database'),
'.nl' => array('whois.domain-registry.nl','not a registered domain'),
'.ro' => array('whois.rotld.ro','No entries found for the selected'),
'.com.au' => array('whois-check.ausregistry.net.au',"Available\n"),
'.net.au' => array('whois-check.ausregistry.net.au',"Available\n"),
'.ca' => array('whois.cira.ca', 'AVAIL'),
'.org.uk' => array('whois.nic.uk','No match'),
'.name' => array('whois.nic.name','No match'),
'.ac.ug' => array('wawa.eahd.or.ug','No entries found'),
'.ne.ug' => array('wawa.eahd.or.ug','No entries found'),
'.sc.ug' => array('wawa.eahd.or.ug','No entries found'),
'.ws' => array('whois.website.ws','No Match'),
'.be' => array('whois.ripe.net','No entries'),
'.com.cn' => array('whois.cnnic.cn','no matching record'),
'.net.cn' => array('whois.cnnic.cn','no matching record'),
'.org.cn' => array('whois.cnnic.cn','no matching record'),
'.no' => array('whois.norid.no','no matches'),
'.se' => array('whois.nic-se.se','No data found'),
'.nu' => array('whois.nic.nu','NO MATCH for'),
'.com.tw' => array('whois.twnic.net','No such Domain Name'),
'.net.tw' => array('whois.twnic.net','No such Domain Name'),
'.org.tw' => array('whois.twnic.net','No such Domain Name'),
'.cc' => array('whois.nic.cc','No match'),
'.nl' => array('whois.domain-registry.nl','is free'),
'.pl' => array('whois.dns.pl','No information about'),
'.eu' => array('whois.eu','Status: AVAILABLE'),
'.pt' => array('whois.dns.pt','No match'),
'.se' => array('whois.iis.se','not found'), //contributed by Mr. Roger xxx@tving.se
);
 
$R6629C5988EEFCD88EA6F77A2AE672B96 = trim($domain); if (preg_match('/^([a-zA-Z0-9]([a-zA-Z0-9\-]{0,61}[a-zA-Z0-9])?\.)*[a-zA-Z0-9]([a-zA-Z0-9\-]{0,61}[a-zA-Z0-9])?$/i',$R6629C5988EEFCD88EA6F77A2AE672B96) != 1) { $error = 'Invalid domain (Letters, numbers and hypens only) ('.$R6629C5988EEFCD88EA6F77A2AE672B96.')'; return false; } preg_match('@^(http://www\.|http://|www\.)?([^/]+)@i', $R6629C5988EEFCD88EA6F77A2AE672B96, $R2BC3A0F3554F7C295CD3CC4A57492121); $RE035DC327C1676D83C2CA9CA79C74BCB = ''; $R6629C5988EEFCD88EA6F77A2AE672B96 = $R2BC3A0F3554F7C295CD3CC4A57492121[2]; $R37D331C368B44BDD85AF95D9FFFFD202 = explode('.', $R6629C5988EEFCD88EA6F77A2AE672B96); $R33D3EC748433467E20D0947C3032E305 = ''; if (count($R37D331C368B44BDD85AF95D9FFFFD202) == 3) { $R33D3EC748433467E20D0947C3032E305 = strtolower($R37D331C368B44BDD85AF95D9FFFFD202[1].'.'.$R37D331C368B44BDD85AF95D9FFFFD202[2]); } else if (count($R37D331C368B44BDD85AF95D9FFFFD202) == 2) { $R33D3EC748433467E20D0947C3032E305 = strtolower($R37D331C368B44BDD85AF95D9FFFFD202[1]); } else { $error = 'Invalid Domain and/or TLD server entry does not exist'; return false; } if ($allowedTLDs != null) { $R630663E4CF314AFD500B9B8E1AA95DF0 = count($allowedTLDs); $RDBF866E6293BB59E654033E299EC8CFE = false; for ($RA16D2280393CE6A2A5428A4A8D09E354 = 0; $RA16D2280393CE6A2A5428A4A8D09E354 < $R630663E4CF314AFD500B9B8E1AA95DF0; $RA16D2280393CE6A2A5428A4A8D09E354++) { if ($allowedTLDs[$RA16D2280393CE6A2A5428A4A8D09E354] === $R33D3EC748433467E20D0947C3032E305) { $RDBF866E6293BB59E654033E299EC8CFE = true; break; } } if (!$RDBF866E6293BB59E654033E299EC8CFE) { $error = 'TLD '.$R33D3EC748433467E20D0947C3032E305.' not allowed'; return false; } } $R019FB4DA0E10A95A57615147DF79F334 = false; if (!array_key_exists('.'.$R33D3EC748433467E20D0947C3032E305, $ext)) { $R019FB4DA0E10A95A57615147DF79F334 = true; } $RE22CBD8984E1727D0A587413D72A88CF = gethostbyname ($R6629C5988EEFCD88EA6F77A2AE672B96); if (($RE22CBD8984E1727D0A587413D72A88CF != $R6629C5988EEFCD88EA6F77A2AE672B96) && ($RE22CBD8984E1727D0A587413D72A88CF != '208.67.219.132')) { return false; } else { $server = ''; if ($R019FB4DA0E10A95A57615147DF79F334) { $RBD7EDCF7DA1CE9EA93A9B3BBD829FFBB = explode('.',$R33D3EC748433467E20D0947C3032E305); if (count($RBD7EDCF7DA1CE9EA93A9B3BBD829FFBB) > 1) $server = $RBD7EDCF7DA1CE9EA93A9B3BBD829FFBB[1].'.whois-servers.net'; else $server = $R33D3EC748433467E20D0947C3032E305.'.whois-servers.net'; $R7B8A9F2F48B874D40BD75BDD12F02557 = @gethostbyname($R33D3EC748433467E20D0947C3032E305.'.whois-servers.net'); } else { $server = $ext['.' .$R33D3EC748433467E20D0947C3032E305][0]; $R7B8A9F2F48B874D40BD75BDD12F02557 = @gethostbyname($server); } if ($R33D3EC748433467E20D0947C3032E305 == 'es') { $error = 'Error: ES not supported. They don\'t have a public whois server :('; return false; } if ($R33D3EC748433467E20D0947C3032E305 == 'au') { $server = $ext['.com.au'][0]; $R7B8A9F2F48B874D40BD75BDD12F02557 = @gethostbyname($server); } if ($R7B8A9F2F48B874D40BD75BDD12F02557 == $server) { $error = 'Error: Invalid extension - '.$R33D3EC748433467E20D0947C3032E305.'. Or server has outgoing connections blocked to '.$server.'. Domain does not have DNS entry, so chances are high it is available.'; return false; } $RAD10634E7F72CAA071320F21AEE5930D = @fsockopen($server, 43,$R32D00070D4FFBCCE2FC669BBA812D4C2,$RE5840D3E86DCF8489051E4F70C757552,10); if ($R32D00070D4FFBCCE2FC669BBA812D4C2 == '10060') { $error = 'Error: Invalid extension - '.$R33D3EC748433467E20D0947C3032E305.' (or whois server is down). Domain does not have DNS entry, so chances are high it is available.'; return false; } if (!$RAD10634E7F72CAA071320F21AEE5930D || ($RE5840D3E86DCF8489051E4F70C757552 != '')) { $error = 'Error: ('.$server.') '.$RE5840D3E86DCF8489051E4F70C757552.' ('.$R32D00070D4FFBCCE2FC669BBA812D4C2.')'; return false; } fputs($RAD10634E7F72CAA071320F21AEE5930D, "$R6629C5988EEFCD88EA6F77A2AE672B96\r\n"); while( !feof($RAD10634E7F72CAA071320F21AEE5930D) ) { $RE035DC327C1676D83C2CA9CA79C74BCB .= fgets($RAD10634E7F72CAA071320F21AEE5930D,128); } fclose($RAD10634E7F72CAA071320F21AEE5930D); if($R33D3EC748433467E20D0947C3032E305 == 'org') nl2br($RE035DC327C1676D83C2CA9CA79C74BCB); if ($R019FB4DA0E10A95A57615147DF79F334) { if ( (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'No match for') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'NOT Found') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'NOT FOUND') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'Not found: ') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,"No Found\n") !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'NOMATCH') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,"AVAIL\n") !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'No entries found') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'NO MATCH') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'No match') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'No such Domain') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'is free') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'FREE') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'No data Found') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'No Data Found') !== false) || ($RE035DC327C1676D83C2CA9CA79C74BCB == "Available\n") || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'No information about') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'no matching record') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'does not Exist in database') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'Status: AVAILABLE') !== false) || (strpos($RE035DC327C1676D83C2CA9CA79C74BCB,'not a registered domain') !== false) ) { return true; } return false; } else { if ((strpos($R33D3EC748433467E20D0947C3032E305,'.au') > 0) && ($RE035DC327C1676D83C2CA9CA79C74BCB == "Not Available\n")) { return false; } if(preg_match('/'.$ext['.' . $R33D3EC748433467E20D0947C3032E305][1].'/i', $RE035DC327C1676D83C2CA9CA79C74BCB)) { return true; } else { return false; } } } return false;
}
// echo $error;
$condition[] = intval(!isDomainAvailable($options));
?>

```phx:docfield
------------

- description: get specified field from document (id)
- usage: **\[****+docid:docfield=`field`****+\]**
- defaults to pagetitle
- added by: bwente

```
<pre class="brush: php">
<?php
$field = (strlen($options)>0) ? $options : 'pagetitle';
$docfield = $modx->getTemplateVarOutput(array($field), $output, 1);
return $docfield[$field];
?>

```phx:domainhasmail
-----------------

- description: See if a domain has a mail server or not, it acts with Conditional operators.
- **IMPORTANT**: It uses **exec()** function, make sure you have it enabled on your **php.ini**!
- usages:
- **\[+****phx:domainhasmail=`www.mydomain.com`:then=`it has mail!`:else=`Oh no!`****+\]**
- **\[****+phx:domainhasmail=`\[<ins>phx:post=`domain`+</ins>****\]`:then=`it has mail!`:else=`Oh no!`\]**
- added by: CuSS (José Moreira - [Microdual](http://www.microdual.com/))

```
<pre class="brush: php">
<?php
function checkdnsmail($a,$b=''){if(!empty($a)){if($b=='') $b="A";exec("nslookup -type=$b $a",$c);foreach($c as $d){if(eregi("^$a",$d)) return true;}return false;}return false;}
$condition[] = intval(checkdnsmail($options));
?>

```phx:evenodd
-----------

- description: Returns even if even, otherwise odd. Nearly identical to the 'alternateClass' above.
- usage: **\[****+ditto\_iteration:evenodd+\]**. Useful for creating alternate templates in Ditto without needing an additional chunk. For example, **class="news\_item\_\[+ditto\_iteration:evenodd****+\]"** will return **news\_item\_even** or **news\_item\_odd**
- default: Defaults to "odd", as there is no test to ensure the input is an integer.
- added by: themancan

```
<pre class="brush: php">
<?php
if ($output % 2) {
return 'even';
} else {
return 'odd';
}
?>

```phx:fileexists
--------------

- description: See if a file exists or not, it acts with Conditional operators. Useful for autodetect images on chunks, and replace broken images by an blank image. You can use it to show products images automaticly by using \[**id**\] or something on your image name.
- usages:
- **\[****+phx:if=`assets/images/path/to/your/file.jpg`:fileexists:then=`Hey, the file exists!`:else=`Oh no! Where is the file?`+****\]**
- **\[****+myimage:fileexists:then=`Hey, the file exists!`:else=`Oh no! Where is the file?`****+\]**
- **<img src="\[****+myimage:fileexists:then=`\[<ins>myimage:phpthumb=`w=200`+</ins>****\]`:else=`\[****+phx:input=`path/to/blank/image.jpg`:phpthumb=`w=200`****+\]`\]" />**
- added by: CuSS (José Moreira - [Microdual](http://www.microdual.com/))

```
<pre class="brush: php">
<?php
$condition[] = intval(file_exists($output));
?>

```phx:firstlast
-------------

- description: Set the number of elements per row. Returns $class\_first if it's the first element in a row; $class\_last if it's the last.
- usage: **\[****+ditto\_iteration:firstlast=`4`****+\]**. Useful for applying classes to the first or last element per row. Kind of like alternate classes, but suited for multiple rows.

```
<pre class="brush: php">
<?php
if (!$options) {
$options = 2;
}
$class_first = 'alpha';
$class_last = 'omega';
 
if (($output % $options) == '0') {
return $class_first;
} elseif (($output % $options) == ($options - 1)) {
return $class_last;
} else {
return;
}
?>

```phx:genitive
------------

- description: Creates the genitive of a name - appends either ?s or ?
- usage: **\[+****myname:genitive****+\]** or **\[+****myname:genitive=`endchar|normalchar|otherchar`****+\]**
- added by: maFF

```
<pre class="brush: php">
<?php
/**
* Creates the genitive of a name.
*
* Examples:
* Joe => Joe's
* Mary => Mary's
* Lucas => Lucas'
*
* Usage:
*
* [+myname:genitive+]
* [+myname:genitive=`endchar|normalchar|otherchar`+]
*
*/
 
$options = explode('|', $options);
$name = $output;
 
$endchar = (!empty($options[0])) ? $options[0] : 's'; // char a name has to end with to get treated specially
$normalchar = (!empty($options[1])) ? $options[1] : '&prime;s'; // char to append to 'normal' names
$otherchar = (!empty($options[2])) ? $options[2] : '&prime;'; // char to append to names which end with $endchar
 
if(substr($name, -1) == $endchar) $genitive = $name.$otherchar;
else $genitive = $name.$normalchar;
 
return $genitive;
?>

```phx:get
-------

- description: get value by name from superglobal $\_GET array
- usage: **\[****<ins>phx:get=`name`</ins>****\]**

```
<pre class="brush: php">
<?php
return htmlspecialchars($_GET[$options]);
?>

```phx:gettype
-----------

- description: See the type of the result, usefull to make mathematical operations like avaliate a number to see if it is multiple of another or to use as security, to get a var from POST and GET and see if it is a number.
- usages:
- **\[+****counter:math=`?/4`:gettype:is=`integer`:then=`This number is multiple of four!`****+\]**
- **\[****+phx:post=`mobilephone`:gettype:ne=`integer`then=`this var contains non-numeric chars`****+\]**
- added by: CuSS (José Moreira - [Microdual](http://www.microdual.com/))

```
<pre class="brush: php">
<?php
function gettype_fromstring($string){
// (c) José Moreira - Microdual (www.microdual.com)
return gettype(getcorrectvariable($string));
}
function getcorrectvariable($string){
// (c) José Moreira - Microdual (www.microdual.com)
// With the help of Svisstack (http://stackoverflow.com/users/283564/svisstack)
/* FUNCTION FLOW */
// *1. Remove unused spaces
// *2. Check if it is empty, if yes, return blank string
// *3. Check if it is numeric
// *4. If numeric, this may be a integer or double, must compare this values.
// *5. If string, try parse to bool.
// *6. If not, this is string.
$string=trim($string);
if(empty($string)) return "";
if(!preg_match("/[^0-9.]+/",$string)){
if(preg_match("/[.]+/",$string)){
return (double)$string;
}else{
return (int)$string;
}
}
if($string=="true") return true;
if($string=="false") return false;
return (string)$string;
}
return gettype_fromstring($output);
?>

```phx:grabsrcurl
--------------

- description: gets the contents of the `src` attribute of an element. For example, if applied to `<img src="modx.png" />` it will return `modx.png`.
- usage: **\[****<ins>+string:grabsrcurl+</ins>****\]** or, more usefully, \[**\*TV:grabsrcurl**\*\]

```
<pre class="brush: php">
<?php
return preg_replace('/.*src=([\'"])((?:(?!\1).)*)\1.*/si','$2',$output);
?>

```phx:has
-------

- description: See if the output string has the option (like 'is' but not equal - 'has'). This is needed if the return is an array (multiple selects from TV) or a big string.
- usages:
- **\[+****myMultipleTemplateVariable:has=`one-of-the-strings`:then=`it has!`:else=`Oh no, you didn't select it!`****+\]**
- added by: CuSS (José Moreira - [Microdual](http://www.microdual.com/))

```
<pre class="brush: php">
<?php
$condition[] = intval(strpos($output,$options)!==false);
?>

```phx:ifnotempty
--------------

- description: The opposite of the native PHX "ifempty" function. Returns the option value ONLY if the input value is empty (excluding whitespace)
- usage: **\[****+phx:if=`string`:ifnotempty=`String to return if not empty`****+\]**

```
<pre class="brush: php">
<?php
if (trim($output) != '') {
return $options;
}
?>

```phx:isempty
-----------

- description: The opposite of the native PHX "ifempty" function, like ifnotempty, but it returns true or false to then and else operators
- usage:
- **\[+****phx:if=`string`:isempty:then=`String to return if empty`****+\]**
- **\[****+myplaceholder:isempty:then=`String to return if empty`****+\]**
- added: CuSS (José Moreira - [Microdual](http://www.microdual.com/))

```
<pre class="brush: php">
<?php
$condition[] = intval(trim($output) == '');
?>

```phx:isnotempty
--------------

- description: The opposite of "isempty" function, it returns true or false to then and else operators.
- usage:
- **\[+****phx:if=`string`:isnotempty:then=`String to return if not empty`****+\]**
- **\[****+myplaceholder:isnotempty:then=`String to return if not empty`****+\]**
- added: CuSS (José Moreira - [Microdual](http://www.microdual.com/))

```
<pre class="brush: php">
<?php
$condition[] = intval(trim($output) != '');
?>

```phx:loggedinmgr
---------------

- usage: **\[****+phx:loggedinmgr:then=`loggedInString`:else=`notLoggedInString`****+\]**
- modded: CuSS (José Moreira - [Microdual](http://www.microdual.com/))

```
<pre class="brush: php">
<?php
global $modx;
$condition[] = intval(isset ($_SESSION['mgrValidated']));
?>

```phx:loggedinweb
---------------

- usage: **\[+****phx:loggedinweb:then=`loggedInString`:else=`notLoggedInString`****+\]**
- added: CuSS (José Moreira - [Microdual](http://www.microdual.com/))

```
<pre class="brush: php">
<?php
global $modx;
$condition[] = intval(isset ($_SESSION['webValidated']));
?>

```phx:make\_url
-------------

- description: Takes a string and makes it a URL if it's not.
- example: If a user enters 'www.example.com', but you want to link to that, if you did 'href="www.example.com"' it wouldn't work. Yeah, you should be validating that on the server side anyway, but this modifier is useful in case you can't. It will make "www.example.com" into "[http://www.example.com](http://www.example.com/)", but won't touch things that start with an http:// or https://
- usage: **\[<ins>member\_url:make\_url</ins>\]**
- default: Adds "http://" to the beginning of the string.
- added by: themancan

```
<pre class="brush: php">
<?php
if (preg_match('%https?://%i', $output)) {
return $output;
} else {
return 'http://' . $output;
}
?>

```phx:mydate
----------

- description: Date modified for non-Unix timestamp dates.
- example: Useful when the date is stored differently, e.g. Friday, May 12, 2008 etc.
- usage: **\[<ins>date:mydate=`%d-%m-%Y %H:%M`</ins>\]**
- added by Legatissima who found it in the support forum, written by Doze

```
<pre class="brush: php">
<?php
return strftime($options, strtotime($output));
?>

```phx:nohttp
----------

- description: Removes the http:// from a URL, to create a display-friendly web address
- usage: **\[****+string:nohttp****+\]**

```
<pre class="brush: php">
<?php
$url = str_replace('http://', '', $output);
return $url;
?>

```phx:ordinal
-----------

- description: Ordinal numbers.
- usage: \*\[**number:ordinal**\]\*
- notes: Adds 'st' to 21 to make it the 21st or 'nd' to 132 for 132nd.
- added by: bwente

```
<pre class="brush: php">
<?php
if (!function_exists('ordinal'))
{
function ordinal($cardinal){
$test_c = abs($cardinal) % 10;
$ext = ((abs($cardinal) %100 < 21 && abs($cardinal) %100 > 4) ? 'th'
: (($test_c < 4) ? ($test_c < 3) ? ($test_c < 2) ? ($test_c < 1)
? 'th' : 'st' : 'nd' : 'rd' : 'th'));
return $cardinal.$ext;
}
}
return ordinal($output);
?>

```phx:outer
---------

- description: Surround not empty string with text.
- usage: **\[<ins>string:outer=`before|after`</ins>\]**
- example: Ditto has no outer template, but you can use this \[**+phx:input=`\[!Ditto? &noResults=` `!\]`:outer=`<div class="xyz">|</div>`**+\]
- added by: Jako

```
<pre class="brush: php">
<?php
$options = explode("|", $options);
$outer = '';
if (trim($output) != '') $outer = $options[0].$output.$options[1];
return $outer;
?>

```phx:parent
----------

- description: get specified document field from parent document (id)
- usage: **\[+****variable:parent=`field`****+\]**
- defaults to pagetitle

```
<pre class="brush: php">
<?php
$field = (strlen($options)>0) ? $options : 'pagetitle';
$parent = $modx->getParent($output,1,$field);
return $parent[$field];
?>

```phx:parseLinks
--------------

- usage: **\[****+placeholder:parseLinks****+\]**

```
<pre class="brush: php">
<?php
/*
* phx:parseLinks
*/
$t = $output;
$t = ereg_replace("[a-zA-Z]+://([.]?[a-zA-Z0-9_/-])*", "<a href=\"\\0\">\\0</a>", $t);
$t = ereg_replace("(^| |\n)(www([.]?[a-zA-Z0-9_/-])*)", "\\1<a href=\"http://\\2\">\\2</a>", $t);
return $t;
?>

```phx:phpthumb
------------

- description: uses [phpThumb](http://phpthumb.sourceforge.net/) to resize images
- usage: **\[****+myimage+\]** contains the full path to the image
- **\[<ins>myimage:phpthumb=`w=450`</ins>\]**
- **\[<ins>myimage:phpthumb=`w=450&ar=x&fltr{}=gray&fltr{}=rcd|8|1`</ins>\]**
- **\[<ins>myimage</ins>\]** contains only the filename
- **\[<ins>myimage:phpthumb=`w=450#\[(base\_path)\]assets/images/`</ins>\]**
- **\[<ins>myimage</ins>\]** contains no data referring to the image
- **\[<ins>myimage:phpthumb=`w=450#\[(base\_path)\]assets/images/myimage.jpg#1`</ins>\]**

phx:phone
---------

- description: transform human readable number ex. _+1 (234) 567-89-10_ into machine form ex. _+12345678910_ (remove " ", "(", ")", "-")
- usage: **\[<ins>phx:input=``phone number``:phone</ins>\]**
- installation: create snippet named **phx:phone** and paste code by: Aramaki

```
<pre class="brush: php">
<?php
return str_replace( array ("(", ")", "-", " "), "", $output );
?>

```phx:post
--------

- description: get value by name from superglobal $\_POST array
- usage: **\[+****phx:post=`name`****+\]**

```
<pre class="brush: php">
<?php
return htmlspecialchars($_POST[$options]);
?>

```phx:price\_format
-----------------

- description: Formats a number (TV) to a specific format with a specific number of decimals. Useful for allowing users to enter a "price" TV, and making sure you have a consistent XX.YY format, even if they just enter XX.
- usage: **\[****+product\_price:price\_format****+\]**.
- default: Defaults to 2 decimal places.
- added by: themancan

```
<pre class="brush: php">
<?php
if (!function_exists(format_number)) {
function format_number($str,$decimal_places='2',$decimal_padding="0"){
/* taken from http://us2.php.net/manual/en/function.number-format.php#54799 */
/* firstly format number and shorten any extra decimal places */
/* Note this will round off the number pre-format $str if you dont want this fucntionality */
$str = number_format($str,$decimal_places,'.',''); // will return 12345.67
$number = explode('.',$str);
$number[1] = (isset($number[1]))?$number[1]:''; // to fix the PHP Notice error if str does not contain a decimal placing.
$decimal = str_pad($number[1],$decimal_places,$decimal_padding);
return (float) $number[0].'.'.$decimal;
}
}
return format_number($output);
?>

```phx:rating
----------

- description: user rating system based on Jot post count
- usage: **\[+****comment.userpostcount:rating+****\]**

```
<pre class="brush: php">
<?php
$defaultValue = " <img src=assets/images/icons/star.png />";
if (intval($output) <= '1') {
$newvalue = '<img src=assets/images/icons/bronze.png />';
}
elseif (intval($output) <= '2') {
$newvalue = '<img src=assets/images/icons/silver.png />';
}
elseif (intval($output) <= '3') {
$newvalue = '<img src=assets/images/icons/gold.png />';
}
else {
$newvalue = $defaultValue;
}
return $newvalue;
?>

```phx:request
-----------

- description: get value by name from superglobal $\_REQUEST array
- usage: **\[****+phx:request=`name`****+\]**

```
<pre class="brush: php">
<?php
return htmlspecialchars($_REQUEST[$options]);
?>

```phx:return\_domain
------------------

- description: Takes a string and returns the domain.
- example: "http://wiki.modxcms.com/index.php?title=PHx/CustomModifiers&action=edit" would return just 'wiki.modxcms.com'.
- note: This is designed to be used on a MODx TV with an Input Type of 'URL'. It doesn't do any sort of error checking.
- usage: **\[<ins>url:return\_domain</ins>\]**.
- added by: themancan

```
<pre class="brush: php">
<?php
preg_match('%(?:https?://)?(?:www\.)?([^/]*)%i', $output, $matches);
return $matches[1];
?>

```phx:smileys
-----------

- description: Converts text patterns into smiley images.
- usage: \*\[**content:smileys**\]\*
- notes: it works with the silk iconset by famfamfam out of the box, if you put the icons into assets/images/smileys. If you want to use other smileys/replacement tags just edit the smileys array.
- added by: maFF

```
<pre class="brush: php">
<?php
$smiley_basepath = $modx->config['base_url'].'assets/images/smileys/';
$smiley_imagetag = '<img src="'.$smiley_basepath.'##smiley##" alt="##text##" title="##text##" />';
 
$smileys = array(
':grin:' => array( // replace pattern
'emoticon_evilgrin.png', // file name
':D'), // text version for alt and title tags
':smile:' => array(
'emoticon_happy.png',
':)'),
':sad:' => array(
'emoticon_unhappy.png',
':('),
':wink:' => array(
'emoticon_wink.png',
';)'),
':tongue:' => array(
'emoticon_tongue.png',
':P'),
':surprised:' => array(
'emoticon_surprised.png',
':O')
);
 
foreach($smileys as $search => $smiley) {
$thistag = $smiley_imagetag;
$thistag = str_replace('##smiley##', $smiley[0], $thistag);
$thistag = str_replace('##text##', $smiley[1], $thistag);
$output = str_replace($search, $thistag, $output);
}
return $output;
?>

```phx:stripsnippet
----------------

- description: Removes cached and uncached snippet in output (usefull in Ditto summarize)
- usage: **\[+****string:stripsnippet****+\]**

```
<pre class="brush: php">
<?php
$string=$output;
$string = preg_replace('~\[\[[^\]]*\]\]~is', '', $string);
$string = preg_replace('~\[\![^!]*\!\]~is', '', $string);
return $string;
?>

```phx:stripparams
---------------

- description: Strips all parameters from an URL, e.g. example.com/page.html?foo=bar&baz=foo becomes example.com/page.html
- usage: **\[<ins>url:stripparams</ins>\]**
- added by: maff

```
<pre class="brush: php">
<?php
$pos = strpos($output, '?');
if($pos !== false) {
$output = substr($output, 0, $pos);
}
 
return $output;
?>

```phx:striptags
-------------

- description: Strips all HTML and PHP tags. You can use the optional parameter to specify tags which should \_not\_ be stripped
- usage: **\[<ins>comment.content:striptags=`<a><img>`</ins>\]**

```
<pre class="brush: php">
<?php
return($modx->stripTags($output, $options));
?>

```phx:thumb
---------

- description: Similar to maFF's phx:phpthumb but just uses a standard phpThumb installation. Requires [phpThumb](http://phpthumb.sourceforge.net/). I'm adding this in addition to the version above because options are always good, and this method just uses an unmodified version of phpThumb. (It also doesn't use the hashing that maFF's version does, so if you need that, use the above version.)
- usage: (basically the same as maFF's version)   
  **\[<ins>myimage</ins>\]** contains the full path to the image   
   **\[<ins>myimage:phpthumb=`w=450`</ins>\]**  
   **\[<ins>myimage:phpthumb=`w=450&ar=x&fltr{}=gray&fltr{}=rcd|8|1`</ins>\]**
- installation: change the path in the code to wherever you like to put your phpThumb. Make sure you make phpThumb's cache directory writable and set phpThumb's config file to how you want it by: themancan

```
<pre class="brush: php">
<?php
$output = '/assets/snippets/thumb/phpThumb.php?src=' . $output . $options;
return $output;
?>

```phx:timesince
-------------

- description: Converts a unix timestamp into the number of years, months, days, hours, minutes, and optionally seconds from the current time.
- usage: **\[****+date:timesince****+\]**
- added by: apoxx

```
<pre class="brush: php">
<?php
if (!function_exists('timeSince'))
{
function timeSince($timestamp)
{
if ($timestamp >= time())
{
$timeago = '0 seconds ago.';
return $timeago;
}
$seconds = time()-$timestamp;
$units = array(
'year' => 31556926,
'month' => 2629743,
'day' => 86400,
'hour' => 3600,
'minute' => 60,
// 'second' => 1
);
$timeago = '';
foreach ($units as $key => $val)
{
if ($seconds >= $val)
{
$results = floor($seconds/$val);
$seconds = ($seconds-($results*$val));
$timeago .= ($results >= 2) ? $results . ' ' . $key . 's, ' : $results . ' ' . $key . ', ';
}
}
return rtrim($timeago, ', ') . ' ago';
}
}
return timeSince($output);
?>

```phx:textile
-----------

- description: Format content using the textile parser class.
- usage: \*\[**content:textile**\]\*
- notes: You need to download [Textile](http://textile.thresholdstate.com/) and save the class file into assets/snippets/textile (or change the path in the snippet).
- added by: maFF

```
<pre class="brush: php">
<?php
if (!class_exists('Textile')) {
$classTextile = $modx->config["base_path"].'assets/snippets/textile/classTextile.php';
if (file_exists($classTextile)) {
require_once($classTextile);
}
}
 
$textile = new Textile();
$output = $textile->TextileThis($output);
unset($textile);
return $output;
?>

```phx:time
--------

- description: Returns Unix timestamp _time()_.
- usage:
- **\[+****phx:time:date=`%Y`****+\]** // Gives the Now Year
- added: CuSS (José Moreira - [Microdual](http://www.microdual.com/))

```
<pre class="brush: php">
<?php
return time();
?>

```phx:tv
------

- description: get a template variable from any page id
- usage: **\[****<ins>+phx:tv=`<docid>?<templatevar>`+</ins>****\]** , where <docid> is the document id (e.g. 25) and <templatevar> is a template variable associated to the given docid.
- default: if the document id or the template variable don't exist no output is produced
- added by: natalino

```
<pre class="brush: php">
<?php
if(strlen($options )>0) {
$data = explode("?",trim($options),2);
$id= (!empty($data[0]) && is_numeric($data[0])) ? $data[0]: '';
$tv= (!empty($data[1])) ? $data[1]: '';
$result = $modx->getTemplateVar($tv, 'name', $id, 1);
return $result['value'];
}
?>

```phx:uri
-------

- description: takes a string and prepares it to be a part of a valid URI.
- usage: **\[****+string:uri****+\]**

```
<pre class="brush: php">
<?php
return rawurlencode($output);
?>

```phx:url
-------

- description: takes a document id and creates the corresponding modx link (similar to \[~ ~\])
- usage: \*\[**id:url**\]\*

```
<pre class="brush: php">
<?php
return $modx->makeUrl($output);
?>

```phx:word\_limit
---------------

- description: limits content to specified number of words (good for news summary)
- usage: **\[+****content:word\_limit=`10`****+\]** - limits content to 10 words
- added by: yentsun

```
<pre class="brush: php">
<?php
$retval = $output;
$array = explode(" ", $output);
if (count($array)<=$options)
{
$retval = $output;
}
else
{
array_splice($array,$options);
$retval = implode(" ", $array);
}
return $retval;
?>

```phx:zeropad
-----------

- description: Zero-padding a string. Takes the number of total digits as the input.
- example: This can be useful when you want to output some kind of ID numbers with fixed digits, or US zip codes: "00001", "00124", "90210"
- usage: **\[<ins>ditto\_iteration:zeropad=`3`</ins>\] \[<ins>zipcode:zeropad=`5`</ins>\]**
- default: No zero-padding.
- added by: ryanlwh

```
<pre class="brush: php">
<?
return sprintf("%0".$options."s",$output);
?>

```