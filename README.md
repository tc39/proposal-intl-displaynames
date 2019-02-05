## Proposal: Intl.DisplayNames

### Stage
Stage 1 - Advanced into Stage 1 in TC39 2019-1-31 / Discussed within ECMA402 members since July 2017. 

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
Intl.DisplayNames.prototype.of( codes )
```
* _options_ may have "localeMatcher", "style", and "type" properties.
  * The value of _style_ could be either "narrow", "short" or "long" to indicate the length of the display names. For example, ofLanguage("en-US") will return "English (United States)" under "long" style, but "English (US)" under "short" style. The default is "long".
  * The value of _type_ could be either "region", "script", "language", "currency", "dateField", or "dateSymbol".
* _codes_ is either a String or an array of string.
* Intl.DisplayNames.prototype.of( codes ) function take either a String or an array of String as input and always return an Array of String, which corresponding to the display name of the _codes_ array.
  * If the type is "region", the String in _codes_ should either [ISO-3166 two letters region code](https://www.iso.org/iso-3166-country-codes.html),
or a [three digits UN M49 Geographic Regions](https://unstats.un.org/unsd/methodology/m49/).
  * If the type is "script", the String in _codes_ should be [ISO-15924 four letters script code](http://unicode.org/iso15924/iso15924-codes.html).
  * If the type is "language", the String in _codes_ should be  _languageCode_ ["-" _scriptCode_] ["-" _regionCode_ ] *("-" _variant_ ) subsequence of the unicode_language_id grammar in [UTS 35's Unicode Language and Locale Identifiers grammar](http://unicode.org/reports/tr35/#Unicode_language_identifier). _languageCode_ is either a two letters ISO 639-1 language code or a three letters ISO 639-2 language code.
  * If the type is "currency", the String in _codes_ should be [3-letter ISO 4217 currency code](https://www.iso.org/iso-4217-currency-codes.html).
  * If the type is "dateField", the String in _codes_ should be one of the following: 
    * "era", "year", "quarter", "month", "weekOfYear", "weekday", "day", "dayperiod", "hour", "minute", "second", "zone".
  * If the type is "dateSymbol", the String in _codes_ should be one of the following: 
    * "sunday",   "monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "january", "february", "march", "april", "may", "june", "july", "august", "september", "october", "november", "december", "q1", "q2", "q3", "q4", "am", "pm".

### Authors
* Frank Tang (@FrankYFTang)
* Zibi Braniecki (@zbraniecki)
* Sascha Brawer (@brawer)
* Nebojša Ćirić (@nciric)

### Prior art
Mozilla already has [vendor specific implementation](https://firefox-source-docs.mozilla.org/intl/dataintl.html#mozintl-getlanguagedisplaynames-locales-langcodes).

### Usage
To create an Intl.DisplayNames for a locale and get the display name for region, language, or script.
```js
// Get display names of region in English 
let regionDisplayNames = new Intl.DisplayNames(['en'], {type: 'region'});
// Get region names
console.log(regionDisplayNames.of('419')); // ["Latin America"]
console.log(regionDisplayNames.of(['BZ', 'US', 'BA', 'MM'])); 
// ["Belize", "United State", "Bosnia & Herzegovina", "Myanmar (Burma)"]

// Get display names of region in Traditional Chinese
regionDisplayNames = new Intl.DisplayNames(['zh-Hant'], {type: 'region'});
// Get region names
console.log(regionDisplayNames.of('419')); // ["拉丁美洲"]
console.log(regionDisplayNames.of(['BZ', 'US', 'BA', 'MM'])); 
// ["貝里斯", "美國", "波士尼亞與赫塞哥維納", "緬甸"]

// Get display names of language in English 
let languageDisplayNames = new Intl.DisplayNames(['en'], {type: 'language'});
// Get language names
console.log(languageDisplayNames.of(['fr', 'de', 'yue', 'fr-CA'])); 
// ["French", "German", "Cantonese", "French (Canada)"]
console.log(languageDisplayNames.of(['zh-Hant', 'en-US', 'zh-TW']));
// ["Chinese (Traditional)", "English (United States)", "Chinese (Taiwan)"]

// Get display names of language in Traditional Chinese 
languageDisplayNames = new Intl.DisplayNames(['zh-Hant'], {type: 'language'});
// Get language names
console.log(languageDisplayNames.of(['fr', 'zh', 'de', 'yue'])); 
// ["法文", "中文", "德文", "粵語"]

// Get display names of script in English 
let scriptDisplayNames = new Intl.DisplayNames(['en'], {type: 'script'});
// Get script names
console.log(scriptDisplayNames.of(['Latn', 'Arab', 'Kana'])); 
// ["Latin", "Arabic", "Katakana"]

// Get display names of script in Traditional Chinese 
scriptDisplayNames = new Intl.DisplayNames(['zh-Hant'], {type: 'script'});
console.log(scriptDisplayNames.of(['Latn', 'Arab', 'Kana'])); 
// ["拉丁文", "阿拉伯文", "片假名"]

```

### Supporting Materials:
* [Slide for promoting from Stage 0 to Stage 1 for TC39 2019 Janurary meeting.](https://goo.gl/qzQK8A)
* Slide for promoting from Stage 1 to Stage 2 for TC39 2019 March meeting. ([Work in Progress](https://goo.gl/ZAaVds
))

