## Proposal: Intl.DisplayNames

### Motivation
Main motivation for Intl.DisplayNames project was to enable developers to get translation of language, region or script display names on the client. Translation of languages, regions or script display names requires large amount of data to transmit on the network, which is already available in most browsers. These display name translations also carry steep data size penalty for developers. This API will allow web developers to shrink the size of their HTML and/ or ECMA script code without the need to include the human readble form of display names and therefore reduce the download size to decrease latency. Also, this API will reduce the localization cost for the web developers. Our goal is to expose this data through Intl API for use in e.g. language, region and script pickers, etc.

### Benefit
* Reduce download size of apps and therefore improve latency.
* Easy for users to build internationalized language, region or script selection UI components (drop down menu or other kinds).
* Reduce translation cost for developers.
* Consistent translation of language, region and script display name on the web.

### Stage
Stage 0

### Syntax
To get localized names of language, script or region, create a Intl.DisplayName object and call the method by passing in appropriate standard code.

The parameter for Intl.DisplayNames constructor follow other Intl Objects in ECMA402 Standard.
The first parameter is Locales, the second parameter is an option Object.

```js
Intl.DisplayNames([ locales [ , options ]])
Intl.DisplayNames.prototype.region(regionCode)
Intl.DisplayNames.prototype.language(languageCode)
Intl.DisplayNames.prototype.languageWithRegion(languageCode, regionCode)
Intl.DisplayNames.prototype.script(scriptCode)
get Intl.DisplayNames.supportedRegionCodesOf([type])
get Intl.DisplayNames.supportedLanguageCodesOf([type])
get Intl.DisplayNames.supportedScriptCodes()
```

* _regionCode_ is either a [ISO-3166 two letters region code](https://www.iso.org/iso-3166-country-codes.html),
or a [three digits UN M49 Geographic Regions](https://unstats.un.org/unsd/methodology/m49/).
* _languageCode_ is either a two letters ISO 639-1 language code or a three letters ISO 639-2 language code.
* _scriptCode_ is a [ISO-15924 four letters script code](http://unicode.org/iso15924/iso15924-codes.html).
* _type_ for Intl.DisplayNames.regionCodes() could be either "all" (default), "two-letters", or "three-digits"
* _type_ for Intl.DisplayNames.languageCodes() could be either "all" (default), "two-letters", or "three-letters"

### Authors
* Frank Tang (@FrankYFTang)
* Zibi Braniecki (@zbraniecki)
* Sascha Brawer (@brawer)
* Nebojša Ćirić (@nciric)

### Prior art
Mozilla already has [vendor specific implementation](https://firefox-source-docs.mozilla.org/intl/dataintl.html#mozintl-getlanguagedisplaynames-locales-langcodes).

### Usage
To create an Intl.DisplayNames for a locale and get the display name for region, language, language in region, or script.
```js
// Get display names in English 
let dn = new Intl.DisplayNames(['en']);
// Get region names
console.log(dn.region('015')); // "Northern Africa"
console.log(dn.region('BZ')); // "Belize"
console.log(dn.region('US')); // "United State"
console.log(dn.region('BA')); // "Bosnia & Herzegovina"
console.log(dn.region('MM')); // "Myanmar (Burma)"

// Get language names
console.log(dn.language('fr')); // "French"
console.log(dn.language('zh')); // "Chinese"
console.log(dn.language('de')); // "German"
console.log(dn.language('yue')); // "Cantonese"

// Get language with region names
console.log(dn.languageWithRegion('fr', 'CA')); // "French (Canada)"
console.log(dn.languageWithRegion('zh', 'TW')); // "Chinese (Taiwan)"

// Get script names
console.log(dn.script('Latn')); // "Latin"
console.log(dn.script('Arab')); // "Arabic"
console.log(dn.script('Kana')); // "Katakana"


// Get display names in Traditional Chinese
dn = new Intl.DisplayNames(['zh-Hant']);
// Get region names
console.log(dn.region('015')); // "北非"
console.log(dn.region('BZ')); // "貝里斯"
console.log(dn.region('US')); // "美國"
console.log(dn.region('BA')); // "波士尼亞與赫塞哥維納"
console.log(dn.region('MM')); // "緬甸"

// Get language names
console.log(dn.language('fr')); // "法文"
console.log(dn.language('zh')); // "中文"
console.log(dn.language('de')); // "德文"
console.log(dn.language('yue')); // "粵語"

// Get script names
console.log(dn.script('Latn')); // "拉丁文"
console.log(dn.script('Arab')); // "阿拉伯文"
console.log(dn.script('Kana')); // "片假名"
```
To get the list of region codes known by the API:
```js
// Get the list of all region codes known by the API
console.log(Intl.DisplayNames.supportedRegionCodesOf()); // ["001", "002", ... , "AD", "AE", "AF"...]
// Same as above
console.log(Intl.DisplayNames.supportedRegionCodesOf("all")); // ["001", "002", ... , "AD", "AE", "AF"...]
// Get the list of three digits (UN M.49) region codes known by the API
console.log(Intl.DisplayNames.supportedRegionCodesOf("three-digits")); // ["001", "002", ... ]
// Get the list of two letters (ISO 3166-1 alpha-2 ) region codes known by the API
console.log(Intl.DisplayNames.supportedRegionCodesOf("two-letters")); // ["AD", "AE", "AF"... ]
```
To get the list of language codes known by the API:
```js
// Get the list of all language codes known by the API
console.log(Intl.DisplayNames.supportedLanguageCodesOf()); // ["aa", "ab", ... , "fil", ...]
// Same as above
console.log(Intl.DisplayNames.supportedLanguageCodesOf("all")); // ["aa", "ab", ... , "fil", ...]
// Get the list of two letters (ISO 639-1) language codes known by the API
console.log(Intl.DisplayNames.supportedLanguageCodesOf("two-letters")); // ["aa", "ab", ...]
// Get the list of three letters (ISO 639-2) language codes known by the API
console.log(Intl.DisplayNames.supportedLanguageCodesOf("three-letters")); // ["aar", "abk", ... , "fil", ...]
```
To get the list of script codes known by the API:
```js
// Get the list of all script codes known by the API
console.log(Intl.DisplayNames.supportedScriptCodes()); // ["Adlm", "Afak", ... ]

```
