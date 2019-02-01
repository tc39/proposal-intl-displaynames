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
Intl.DisplayNames.prototype.ofLanguage( localeBaseName )
Intl.DisplayNames.prototype.ofRegion( regionCode )
Intl.DisplayNames.prototype.ofScript( scriptCode )
```
* _regionCode_ is either a [ISO-3166 two letters region code](https://www.iso.org/iso-3166-country-codes.html),
or a [three digits UN M49 Geographic Regions](https://unstats.un.org/unsd/methodology/m49/).
* _scriptCode_ is a [ISO-15924 four letters script code](http://unicode.org/iso15924/iso15924-codes.html).
* _languageCode_ is either a two letters ISO 639-1 language code or a three letters ISO 639-2 language code.
* _localeBaseName_ is the _languageCode_ ["-" _scriptCode_] ["-" _regionCode_ ] *("-" _variant_ ) subsequence of the unicode_language_id grammar in [UTS 35's Unicode Language and Locale Identifiers grammar](http://unicode.org/reports/tr35/#Unicode_language_identifier).
* _options_ may have "localeMatcher", "style", "type" and "capitalization" properties.
  * The value of _style_ could be either "short" or "long" to indicate the length of the display names. For example, ofLanguage("en-US") will return "English (United States)" under "long" style, but "English (US)" under "short" style. The default is "long".
  * The value of _type_ could be either "standard" or "dialect" to indicate the type of the name. For example, ofLanguage("en-GB") will return "English (United Kingdom)" under "standard" type, but "British English" under "dialect" type. The default is "standard".
  * The value of _capitalization_ could be either "none", "beginning", "middle", "ui", or "standalone" to indicate how the capitization rule should perform for the context of using the return display name. 
    * "none": The capitalization context to be used is unknown. 
    * "beginning": The capitalization context if a display name is to be formatted with capitalization appropriate for the beginning of a sentence.
    * "middle": The capitalization context if a  display name is to be formatted with capitalization appropriate for the middle of a sentence.
    * "ui": The capitalization context if a display name is to be formatted with capitalization appropriate for a user-interface list or menu item.
    * "standalone": The capitalization context if a display name is to be formatted with capitalization appropriate for stand-alone usage such as an isolated name on a page.

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
// Get display names in English 
let displayNames = new Intl.DisplayNames(['en']);
// Get region names
console.log(displayNames.ofRegion('419')); // "Latin America"
console.log(displayNames.ofRegion('BZ')); // "Belize"
console.log(displayNames.ofRegion('US')); // "United State"
console.log(displayNames.ofRegion('BA')); // "Bosnia & Herzegovina"
console.log(displayNames.ofRegion('MM')); // "Myanmar (Burma)"

// Get language names
console.log(displayNames.ofLanguage('fr')); // "French"
console.log(displayNames.ofLanguage('de')); // "German"
console.log(displayNames.ofLanguage('yue')); // "Cantonese"
console.log(displayNames.ofLanguage('fr-CA')); // "French (Canada)"
console.log(displayNames.ofLanguage('zh-Hant')); // "Chinese (Traditional)"
console.log(displayNames.ofLanguage('en-US')); // "English (United States)"
console.log(displayNames.ofLanguage('zh-TW')); // "Chinese (Taiwan)"

// Get script names
console.log(displayNames.ofScript('Latn')); // "Latin"
console.log(displayNames.ofScript('Arab')); // "Arabic"
console.log(displayNames.ofScript('Kana')); // "Katakana"


// Get display names in Traditional Chinese
displayNames = new Intl.DisplayNames(['zh-Hant']);
// Get region names
console.log(displayNames.ofRegion('419')); // "拉丁美洲"
console.log(displayNames.ofRegion('BZ')); // "貝里斯"
console.log(displayNames.ofRegion('US')); // "美國"
console.log(displayNames.ofRegion('BA')); // "波士尼亞與赫塞哥維納"
console.log(displayNames.ofRegion('MM')); // "緬甸"

// Get language names
console.log(displayNames.ofLanguage('fr')); // "法文"
console.log(displayNames.ofLanguage('zh')); // "中文"
console.log(displayNames.ofLanguage('de')); // "德文"
console.log(displayNames.ofLanguage('yue')); // "粵語"

// Get script names
console.log(displayNames.ofScript('Latn')); // "拉丁文"
console.log(displayNames.ofScript('Arab')); // "阿拉伯文"
console.log(displayNames.ofScript('Kana')); // "片假名"
