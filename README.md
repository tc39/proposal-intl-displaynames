## Proposal: Intl.DisplayNames

### Stage
Stage 2
* Advanced into Stage 2 in TC39 2019-6-5.
* Advanced into Stage 1 in TC39 2019-1-31.
* Discussed within ECMA402 members since July 2017. 


### Motivation
Main motivation for Intl.DisplayNames project was to enable developers to get translation of language, region or script display names on the client. Translation of languages, regions or script display names requires large amount of data to transmit on the network, which is already available in most browsers. These display name translations also carry steep data size penalty for developers. This API will allow web developers to shrink the size of their HTML and/ or ECMA script code without the need to include the human readble form of display names and therefore reduce the download size to decrease latency. Also, this API will reduce the localization cost for the web developers. Our goal is to expose this data through Intl API for use in e.g. language, region and script pickers, etc.

### Benefit
* Reduce download size of apps and therefore improve latency.
* Easy for users to build internationalized language, region or script selection UI components (drop down menu or other kinds).
* Reduce translation cost for developers.
* Consistent translation of language, region and script display name on the web.

### Scope
This proposal is intended to provide translation for strings of particular items which are application-independent, rather than translation for all kinds of strings. There are two classes of strings we're considering here:
 
- Strings that are already available because they're needed for other APIs. For example, names for the days of the week are necessary to provide Intl.DateTimeFormat capabilities. In the absence of a direct API, we see programs parsing the output of Intl.DateTimeFormat to find these names, which is an unreliable/unstable technique.
- Strings that are universally standardized and are likely necessary for any multilingual JavaScript application. For example language/region names.

For example, for language/region names, W3C recommends users to use a locale selector when handling multilingual content, and currently that means shipping a long list of translated language/region names with every such website.
Such data may potentially be time and politically sensitive and is unlikely to be specific to any particular website (i.e., it's unlikely that website A will want a different translation of any language name from website B).
For that reason, providing this data in the engine lowers the cost of shipping multilingual websites with locale selectors and move the responsibility for keeping the mapping of BCP47 language/region codes to display names with the engine which is in a better position to keep it up to date.

The set of strings included may grow over time, but we expect to restrict the growth according to pragmatic requirements, including:
- Strings which are included should be generically useful across multiple application types.
- The inclusion of strings should not be too much of a burden on implementations in terms of data size.
- There should be an open data source that implementations can use for the string values, e.g., CLDR.

Additional strings included in Intl.DisplayNames would be added to the specification through a future ECMA-402 proposal, to be presented in the ECMA-402 Task Group and TC39 as part of its standardization through TC39 processes.

### Syntax
To get localized names of language, script or region, create a Intl.DisplayName object and call the method by passing in appropriate standard code.

The parameter for Intl.DisplayNames constructor follow other Intl Objects in ECMA402 Standard.
The first parameter is *locales*, which is either a BCP 47 language tag or an array of such language tags ([more information](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl#Locale_identification_and_negotiation)); the second parameter is an option Object.

```js
Intl.DisplayNames([ locales [ , options ]])
Intl.DisplayNames.prototype.of( code )
```
* _options_ may have "localeMatcher", "style", and "type" properties.
  * The value of _style_ could be either "narrow", "short" or "long" to indicate the length of the display names. For example, ofLanguage("en-US") will return "English (United States)" under "long" style, but "English (US)" under "short" style. The default is "long".
  * The value of _type_ could be either "region", "script", "language", "currency", "dateTime", or "dateTimeField".
* _code_ is a String.
* Intl.DisplayNames.prototype.of( code ) function take a String as input and return a String, the display name of the _code_.
  * If the type is "region", the _code_ should be either an [ISO-3166 two letters region code](https://www.iso.org/iso-3166-country-codes.html),
or a [three digits UN M49 Geographic Regions](https://unstats.un.org/unsd/methodology/m49/).
  * If the type is "script", the _code_ should be an [ISO-15924 four letters script code](http://unicode.org/iso15924/iso15924-codes.html).
  * If the type is "language", the _code_ should be a _languageCode_ ["-" _scriptCode_] ["-" _regionCode_ ] *("-" _variant_ ) subsequence of the unicode_language_id grammar in [UTS 35's Unicode Language and Locale Identifiers grammar](http://unicode.org/reports/tr35/#Unicode_language_identifier). _languageCode_ is either a two letters ISO 639-1 language code or a three letters ISO 639-2 language code.
  * If the type is "currency", the _code_ should be a [3-letter ISO 4217 currency code](https://www.iso.org/iso-4217-currency-codes.html).
  * If the type is "dateTimeField", the _code_ should be one of the following:
    * "era", "year", "quarter", "month", "weekOfYear", "weekday", "day", "dayPeriod", "hour", "minute", "second", "timeZoneName".
  * If the type is "dateTime", the _code_ should be one of the following:
    * "sunday",   "monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "january", "february", "march", "april", "may", "june", "july", "august", "september", "october", "november", "december", "q1", "q2", "q3", "q4", "am", "pm".

### Authors
* Frank Tang (@FrankYFTang)
* Zibi Braniecki (@zbraniecki)
* Sascha Brawer (@brawer)
* Nebojša Ćirić (@nciric)

### Prior art
Mozilla already has [vendor specific implementation](https://firefox-source-docs.mozilla.org/intl/dataintl.html#mozintl-getlanguagedisplaynames-locales-langcodes).

### Usage
#### Region Code Display Names
To create an Intl.DisplayNames for a locale and get the display name for a
region code.
```js
// Get display names of region in English
var regionNames = new Intl.DisplayNames(['en'], {type: 'region'});
console.log(regionNames.of('419')); // "Latin America"
console.log(regionNames.of('BZ')); // "Belize"
console.log(regionNames.of('US')); // "United States"
console.log(regionNames.of('BA')); // "Bosnia & Herzegovina"
console.log(regionNames.of('MM')); // "Myanmar (Burma)"

// Get display names of region in Traditional Chinese
regionNames = new Intl.DisplayNames(['zh-Hant'], {type: 'region'});
console.log(regionNames.of('419')); // "拉丁美洲"
console.log(regionNames.of('BZ')); // "貝里斯"
console.log(regionNames.of('US')); // "美國"
console.log(regionNames.of('BA')); // "波士尼亞與赫塞哥維納"
console.log(regionNames.of('MM')); // "緬甸"
```

#### Language Display Names
To create an Intl.DisplayNames for a locale and get the display name for a
language-script-region sequence.
```js
// Get display names of language in English
var languageNames = new Intl.DisplayNames(['en'], {type: 'language'});
console.log(languageNames.of('fr')); // "French"
console.log(languageNames.of('de')); // "German"
console.log(languageNames.of('fr-CA')); // "Canadian French"
console.log(languageNames.of('zh-Hant')); // "Traditional Chinese"
console.log(languageNames.of('en-US')); // "American English"
console.log(languageNames.of('zh-TW')); // "Chinese (Taiwan)"]

// Get display names of language in Traditional Chinese
languageNames = new Intl.DisplayNames(['zh-Hant'], {type: 'language'});
console.log(languageNames.of('fr')); // "法文"
console.log(languageNames.of('zh')); // "中文"
console.log(languageNames.of('de')); // "德文"
```

#### Script Code Display Names
To create an Intl.DisplayNames for a locale and get the display name for
a script code.
```js
// Get display names of script in English
var scriptNames = new Intl.DisplayNames(['en'], {type: 'script'});
// Get script names
console.log(scriptNames.of('Latn')); // "Latin"
console.log(scriptNames.of('Arab')); // "Arabic"
console.log(scriptNames.of('Kana')); // "Katakana"

// Get display names of script in Traditional Chinese
scriptNames = new Intl.DisplayNames(['zh-Hant'], {type: 'script'});
console.log(scriptNames.of('Latn')); // "拉丁文"
console.log(scriptNames.of('Arab')); // "阿拉伯文"
console.log(scriptNames.of('Kana')); // "片假名"
```
#### Currency Code Display Names
To create an Intl.DisplayNames for a locale and get the display name for
currency code.
```js
// Get display names of currency code in English
var currencyNames = new Intl.DisplayNames(['en'], {type: 'currency'});
// Get currency names
console.log(currencyNames.of('USD')); // "US Dollar"
console.log(currencyNames.of('EUR')); // "Euro"
console.log(currencyNames.of('TWD')); // "New Taiwan Dollar"
console.log(currencyNames.of('CNY')); // "Chinese Yuan"

// Get display names of currency code in Traditional Chinese
currencyNames = new Intl.DisplayNames(['zh-Hant'], {type: 'currency'});
console.log(currencyNames.of('USD')); // "美元"
console.log(currencyNames.of('EUR')); // "歐元"
console.log(currencyNames.of('TWD')); // "新台幣"
console.log(currencyNames.of('CNY')); // "人民幣"
```
#### Date Time Field Display Names
To create an Intl.DisplayNames for a locale and get the display name for
date time field.
```js
// Get display names of date time field in English
var dateTimeFieldNames = new Intl.DisplayNames(['en'], {type: 'dateTimeField'});
console.log(dateTimeFieldNames.of('year')); // "year"
console.log(dateTimeFieldNames.of('quarter')); // "quarter"
console.log(dateTimeFieldNames.of('weekOfYear')); // "weekOfYear"
console.log(dateTimeFieldNames.of('hour')); // "hour"

// Get display names of date time field in Traditional Chinese
dateTimeFieldNames = new Intl.DisplayNames(['zh-Hant'], {type: 'dateTimeField'});
console.log(dateTimeFieldNames.of('year')); // "年"
console.log(dateTimeFieldNames.of('quarter')); // "季"
console.log(dateTimeFieldNames.of('weekOfYear')); // "週"
console.log(dateTimeFieldNames.of('hour')); // "小時"
```
#### Date Time Display Names
To create an Intl.DisplayNames for a locale and get the display name for
date time.
```js
// Get display names of date time in English
var dateTimeNames = new Intl.DisplayNames(['en'], {type: 'dateTime'});
console.log(dateTimeNames.of('monday')); // "Monday"
console.log(dateTimeNames.of('april')); // "April"
console.log(dateTimeNames.of('q3')); // "3rd quarter"
console.log(dateTimeNames.of('pm')); // "PM"

// Get display names of date time in Traditional Chinese
dateTimeNames = new Intl.DisplayNames(['zh-Hant'], {type: 'dateTime'});
console.log(dateTimeNames.of('monday')); // "星期一"
console.log(dateTimeNames.of('april')); // "4月"
console.log(dateTimeNames.of('q3')); // "第3季"
console.log(dateTimeNames.of('pm')); // "下午"
```

### Supporting Materials:
* [Slide for promoting from Stage 0 to Stage 1 for TC39 2019 January meeting.](https://goo.gl/qzQK8A)
* [Slide for promoting from Stage 1 to Stage 2 for TC39 2019 June meeting.](https://goo.gl/ZAaVds)
* [Slide for promoting from Stage 2 to Stage 3 for TC39 2019 July meeting.](http://shorturl.at/yPSZ1)

