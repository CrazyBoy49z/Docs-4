---
title: "spieFeed"
_old_id: "719"
_old_uri: "revo/spiefeed"
---

## Description

The RSS/Atom feeder based on [SimplePie](http://simplepie.org/) 1.2 which is already included in the package: <http://github.com/rmccue/simplepie/downloads>

## Requirements

- MODx Revolution 2.0.0-RC-2 or later
- PHP5 or later

## History

spieFeed integration was written by [goldsky](/display/~goldsky) as the RSS/Atom feeder, and first released on Oct 18, 2010.

## Download

It can be downloaded from within the MODx Revolution manager via \[Package Management\], or from the MODx Extras Repository, here: <http://modxcms.com/extras/package/738> or here: <http://modx.com/extras/package/spiefeed>

## Development and Bug Reporting

spieFeed is stored and developed in GitHub, and can be found here:<https://github.com/goldsky/spiefeed>

Bugs can be filed here: <https://github.com/goldsky/spiefeed/issues>

## Usage

spieFeed is used by placing the Snippet call into your content and passing a 'url' parameter:

``` php
[[!spieFeed? &setFeedUrl=`http://path.com/to/my/rss.feed.rss`]]
```

For multiple sources can call this

``` php
http://feeds.feedburner.com/modx-announce | http://www.voanews.com/templates/Articles.rss?sectionPath=/russian/news
```

separated by pipe ( | ) symbol(s).

### Available Properties

| Name                                                                                                                           | Description                                                          | Options | Default                                     |
| ------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------- | ------- | ------------------------------------------- |
| setFeedUrl                                                                                                                     | URL of the feed to retrieve.                                         | Any URL | <http://feeds.feedburner.com/modx-announce> | <http://www.voanews.com/templates/Articles.rss?sectionPath=/russian/news> |
| enableCache                                                                                                                    | This option allows you to disable caching all-together in SimplePie. |
| However, disabling the cache can lead to longer load times. [info](http://simplepie.org/wiki/reference/simplepie/enable_cache) | 0                                                                    | 1       | 1                                           |
| enableOrderByDate                                                                                                              | Sometimes feeds don't have their items in chronological order.       |
By default, SimplePie will re-order them to be in such an order.
With this option, you can enable/disable the reordering of items into reverse chronological order if you don't want it. [info](http://simplepie.org/wiki/reference/simplepie/enable_order_by_date) | 0 | 1 | 1 |
| setCacheDuration | Set the minimum time (in seconds) for which a feed will be cached. [info](http://simplepie.org/wiki/reference/simplepie/set_cache_duration) | integers in seconds | 3600 |
| setCacheLocation | Set the file system location (not WWW location) where the cache files should be written. The cache folder should be make or error will be returned. [info](http://simplepie.org/wiki/reference/simplepie/set_cache_location) |  | core/components/spiefeed/cache |
| setFaviconHandler | Set the handler to enable the display of cached favicons. [info](http://simplepie.org/wiki/reference/simplepie/set_favicon_handler) | \[str image handler file\], \[query\] | false, "i" |
| setItemLimit | Set the maximum number of items to return per feed with Multifeeds.
This is NOT for limiting the number of items to loop through in a single feed.
For that, you want to pass $start and $length parameters to get\_items(). [info](http://simplepie.org/wiki/reference/simplepie/set_item_limit) | integer | null |
| setJavascript | Set the query string that triggers SimplePie to generate the JavaScript code for embedding media files. [info](http://simplepie.org/wiki/reference/simplepie/set_javascript) | javascript query string parameter | js |
| stripAttributes | Set which attributes get stripped from an entry's content.
The default set of attributes is stored in the property SimplePie?strip\_attributes, not to be confused with the method SimplePie?strip\_attributes().
This way, you can modify the existing list without having to create a whole new one. [info](http://simplepie.org/wiki/reference/simplepie/strip_attributes) | attributes get stripped from an entry"s content | array("bgsound", "class", "expr", "id", "style", "onclick", "onerror", "onfinish", "onmouseover", "onmouseout", "onfocus", "onblur", "lowsrc", "dynsrc") |
| stripComments | Set whether to strip out HTML comments from an entry's content. [info](http://simplepie.org/wiki/reference/simplepie/strip_comments) | 0 | 1 | 0 |
| stripHtmlTags | Set which HTML tags get stripped from an entry's content.
The default set of tags is stored in the property SimplePie?strip\_htmltags, not to be confused with the method SimplePie?strip\_htmltags().
This way, you can modify the existing list without having to create a whole new one. [info](http://simplepie.org/wiki/reference/simplepie/strip_htmltags) | HTML tags get stripped from an entry"s content | array("base", "blink", "body", "doctype", "embed", "font", "form", "frame", "frameset", "html", "iframe", "input", "marquee", "meta", "noscript", "object", "param", "script", "style") |
| dateFormat | Date format supports anything that works with PHP's date() function. Only supports the English language. [info](http://simplepie.org/wiki/reference/simplepie_item/get_date) | PHP"s date() | "j F Y, g:i a" |
| localDateFormat | Returns the date/timestamp of the posting in the localized language. Date format supports anything that works with PHP's strftime() function. To display in other languages, you need to change the locale with PHP's setlocale() function. The available localizations depend on which ones are installed on your web server. [info](http://simplepie.org/wiki/reference/simplepie_item/get_local_date) | PHP"s strftime() of the posting in the localized language. setlocale() matters. | "%c" |
| getItemStart | Returns an array of SimplePie\_Item references for each item in the feed, which can be looped through. [info](http://simplepie.org/wiki/reference/simplepie/get_items) | integers | 0 |
| getItemLength | Returns an array of SimplePie\_Item references for each item in the feed, which can be looped through. [info](http://simplepie.org/wiki/reference/simplepie/get_items) | integers | 0 |
| forceFSockopen | If cURL is available, SimplePie will use it instead of the built-in fsockopen functions for fetching remote feeds. This config option will force SimplePie to use fsockopen even if cURL is installed. [info](http://simplepie.org/wiki/reference/simplepie/force_fsockopen) | 0 | 1 | 1 |
| setInputEncoding | Allows you to override the character encoding of the feed.
This is only useful for times when the feed is reporting an incorrect character encoding (as per RFC 3023 and Determining the character encoding of a feed).
This setting is similar to set\_output\_encoding().
The number of supported character encodings depends on whether your web host supports mbstring, iconv, or both. See Supported Character Encodings for more information. [info](http://simplepie.org/wiki/reference/simplepie/set_input_encoding) | [http://simplepie.org/wiki/faq/supported\_character\_encodings](http://simplepie.org/wiki/faq/supported_character_encodings) | false |
| setOutputEncoding | Allows you to override SimplePie's output to match that of your webpage.
This is useful for times when your webpages are not being served as UTF-8.
This setting will be obeyed by handle\_content\_type(), and is similar to set\_input\_encoding().
It should be noted, however, that not all character encodings can support all characters.
If your page is being served as ISO-8859-1 and you try to display a Japanese feed, you'll likely see garbled characters. Because of this, it is highly recommended to ensure that your webpages are served as UTF-8.
The number of supported character encodings depends on whether your web host supports mbstring, iconv, or both. See Supported Character Encodings for more information. [info](http://simplepie.org/wiki/reference/simplepie/set_output_encoding) | [http://simplepie.org/wiki/faq/supported\_character\_encodings](http://simplepie.org/wiki/faq/supported_character_encodings) | "UTF-8" |
| sortBy | Allow you to sort the result by one of the available placeholders | placeholders | date |
| sortOrder | The sorting order | ASC | DESC | DESC |
| tpl | Template | chunk name | defaultSpieFeedTpl |
| rowCls | Class name for each row | class name | spie-row |
| firstRowCls | Class name for the first row | class name | spie-first-row |
| lastRowCls | Class name for the last row | class name | spie-last-row |
| oddRowCls | Class name for the odd row | class name | spie-odd-row |
| &debug (1.5-pl) | Debug mode, to return Simple Pie's error | 0 | 1 | 0 |
| &toArray (1.5-pl) | Return the result as an array | 0 | 1 | 0 |
| &emptyMessage (1.5-pl) | Custom message on the empty return | anything |  |

### Available Placeholders

- favicon
- link
- title
- description
- content
- permalink
- imageLink
- imageTitle
- imageUrl
- imageWidth
- imageHeight
- date
- localDate
- copyright
- latitude
- longitude
- language
- encoding
- authorName
- authorLink
- authorEmail
- category
- contributor
- getType
- itemImageThumbnailUrl
- itemImageWidth
- itemImageHeight