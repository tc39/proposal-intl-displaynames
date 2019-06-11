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
  * The value of _type_ could be either "region", "script", "language", "currency", "dateField", or "dateSymbol".
* _code_ is a String.
* Intl.DisplayNames.prototype.of( code ) function take a String as input and return a String, the display name of the _code_.
  * If the type is "region", the _code_ should be either an [ISO-3166 two letters region code](https://www.iso.org/iso-3166-country-codes.html),
or a [three digits UN M49 Geographic Regions](https://unstats.un.org/unsd/methodology/m49/).
  * If the type is "script", the _code_ should be an [ISO-15924 four letters script code](http://unicode.org/iso15924/iso15924-codes.html).
  * If the type is "language", the _code_ should be a _languageCode_ ["-" _scriptCode_] ["-" _regionCode_ ] *("-" _variant_ ) subsequence of the unicode_language_id grammar in [UTS 35's Unicode Language and Locale Identifiers grammar](http://unicode.org/reports/tr35/#Unicode_language_identifier). _languageCode_ is either a two letters ISO 639-1 language code or a three letters ISO 639-2 language code.
  * If the type is "currency", the _code_ should be a [3-letter ISO 4217 currency code](https://www.iso.org/iso-4217-currency-codes.html).
  * If the type is "dateField", the _code_ should be one of the following: 
    * "era", "year", "quarter", "month", "weekOfYear", "weekday", "day", "dayPeriod", "hour", "minute", "second", "zone".
  * If the type is "dateSymbol", the _code_ should be one of the following: 
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
var regionDisplayNames = new Intl.DisplayNames(['en'], {type: 'region'});
// Get region names
console.log(regionDisplayNames.of('419')); // "Latin America"
console.log(regionDisplayNames.of('BZ')); // "Belize"
console.log(regionDisplayNames.of('US')); // "United States"
console.log(regionDisplayNames.of('BA')); // "Bosnia & Herzegovina"
console.log(regionDisplayNames.of('MM')); // "Myanmar (Burma)"

// Get display names of region in Traditional Chinese
regionDisplayNames = new Intl.DisplayNames(['zh-Hant'], {type: 'region'});
// Get region names
console.log(regionDisplayNames.of('419')); // "拉丁美洲"
console.log(regionDisplayNames.of('BZ')); // "貝里斯"
console.log(regionDisplayNames.of('US')); // "美國"
console.log(regionDisplayNames.of('BA')); // "波士尼亞與赫塞哥維納"
console.log(regionDisplayNames.of('MM')); // "緬甸"
```

#### Language Display Names
To create an Intl.DisplayNames for a locale and get the display name for a
language-script-region sequence.
```js
// Get display names of language in English
var languageDisplayNames = new Intl.DisplayNames(['en'], {type: 'language'});
// Get language names
console.log(languageDisplayNames.of('fr')); // "French"
console.log(languageDisplayNames.of('de')); // "German"
console.log(languageDisplayNames.of('fr-CA')); // "Canadian French"
console.log(languageDisplayNames.of('zh-Hant')); // "Traditional Chinese"
console.log(languageDisplayNames.of('en-US')); // "American English"
console.log(languageDisplayNames.of('zh-TW')); // "Chinese (Taiwan)"]

// Get display names of language in Traditional Chinese
languageDisplayNames = new Intl.DisplayNames(['zh-Hant'], {type: 'language'});
// Get language names
console.log(languageDisplayNames.of('fr')); // "法文"
console.log(languageDisplayNames.of('zh')); // "中文"
console.log(languageDisplayNames.of('de')); // "德文"
```

#### Script Code Display Names
To create an Intl.DisplayNames for a locale and get the display name for
a script code.
```js
// Get display names of script in English
var scriptDisplayNames = new Intl.DisplayNames(['en'], {type: 'script'});
// Get script names
console.log(scriptDisplayNames.of('Latn')); // "Latin"
console.log(scriptDisplayNames.of('Arab')); // "Arabic"
console.log(scriptDisplayNames.of('Kana')); // "Katakana"

// Get display names of script in Traditional Chinese
scriptDisplayNames = new Intl.DisplayNames(['zh-Hant'], {type: 'script'});
console.log(scriptDisplayNames.of('Latn')); // "拉丁文"
console.log(scriptDisplayNames.of('Arab')); // "阿拉伯文"
console.log(scriptDisplayNames.of('Kana')); // "片假名"
```
#### Currency Code Display Names
To create an Intl.DisplayNames for a locale and get the display name for
currency code.
```js
// Get display names of currency code in English
var currencyDisplayNames = new Intl.DisplayNames(['en'], {type: 'currency'});
// Get script names
console.log(currencyDisplayNames.of('USD')); // "US Dollar"
console.log(currencyDisplayNames.of('EUR')); // "Euro"
console.log(currencyDisplayNames.of('TWD')); // "New Taiwan Dollar"
console.log(currencyDisplayNames.of('CNY')); // "Chinese Yuan"

// Get display names of currency code in Traditional Chinese
currencyDisplayNames = new Intl.DisplayNames(['zh-Hant'], {type: 'currency'});
console.log(currencyDisplayNames.of('USD')); // "美元"
console.log(currencyDisplayNames.of('EUR')); // "歐元"
console.log(currencyDisplayNames.of('TWD')); // "新台幣"
console.log(currencyDisplayNames.of('CNY')); // "人民幣"
```
#### Date Field Display Names
To create an Intl.DisplayNames for a locale and get the display name for
date field.
```js
// Get display names of date field in English
var dateFieldDisplayNames = new Intl.DisplayNames(['en'], {type: 'dateField'});
// Get script names
console.log(dateFieldDisplayNames.of('year')); // "year"
console.log(dateFieldDisplayNames.of('quarter')); // "quarter"
console.log(dateFieldDisplayNames.of('weekOfYear')); // "weekOfYear"
console.log(dateFieldDisplayNames.of('hour')); // "hour"

// Get display names of date field in Traditional Chinese
dateFieldDisplayNames = new Intl.DisplayNames(['zh-Hant'], {type: 'dateField'});
console.log(dateFieldDisplayNames.of('year')); // "年"
console.log(dateFieldDisplayNames.of('quarter')); // "季"
console.log(dateFieldDisplayNames.of('weekOfYear')); // "週"
console.log(dateFieldDisplayNames.of('hour')); // "小時"
```
#### Date Symbol Display Names
To create an Intl.DisplayNames for a locale and get the display name for
date symbol.
```js
// Get display names of date symbol in English
var dateFieldDisplayNames = new Intl.DisplayNames(['en'], {type: 'dateSymbol'});
// Get script names
console.log(dateFieldDisplayNames.of('monday')); // "Monday"
console.log(dateFieldDisplayNames.of('april')); // "April"
console.log(dateFieldDisplayNames.of('q3')); // "3rd quarter"
console.log(dateFieldDisplayNames.of('pm')); // "PM"

// Get display names of date symbol in Traditional Chinese
dateFieldDisplayNames = new Intl.DisplayNames(['zh-Hant'], {type: 'dateSymbol'});
console.log(dateFieldDisplayNames.of('monday')); // "星期一"
console.log(dateFieldDisplayNames.of('april')); // "4月"
console.log(dateFieldDisplayNames.of('q3')); // "第3季"
console.log(dateFieldDisplayNames.of('pm')); // "下午"
```

### Supporting Materials:
* [Slide for promoting from Stage 0 to Stage 1 for TC39 2019 Janurary meeting.](https://goo.gl/qzQK8A)
* [Slide for promoting from Stage 1 to Stage 2 for TC39 2019 June meeting.](https://goo.gl/ZAaVds)
* Slide for promoting from Stage 2 to Stage 3 for TC39 2019 July meeting. TBW

