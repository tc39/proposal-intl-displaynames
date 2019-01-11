## Proposal: Intl.DisplayNames

### Motivation
Main motivation for Intl project was to enable collation on the client. Collation algorithm requires large amount of data, which is already available in most browsers. Language, region and script name translations also carry steep data size penalty for developers. Our goal is to expose this data through Intl API for use in e.g. language, region and script pickers, etc.

### Stage
Stage 0

### Syntax
To get localized names of language, script or region, create a Intl.DisplayName object and call the method by passing in appropriate standard code.

The parameter for Intl.DisplayNames constructor follow other Intl Objects in ECMA402 Standard.
The first parameter is Locales, the second parameter is an option Object.

The parameter for Intl.DisplayNames.prototype.language() method is an ISO-639 two or three letters language
code. It returns the localized string of language display name to present that code in the
language specified in the Intl.DisplayNames constructor.

The parameter for Intl.DisplayNames.prototype.region() method is either an ISO-3166 two letters region code,
or a three digits UN M.49 region code.
It returns the localized string of region display name to present that code in the
language specified in the Intl.DisplayNames constructor.

The first parameter for Intl.DisplayNames.prototype.languageWithRegion() method is an ISO-639 language
code. The second parameter for Intl.DisplayNames.prototype.region method is either an
ISO-3166 two letter region code, or a three digits UN M.49 region code.
It returns the localized string of language of region display name to present that two code in the
language specified in the Intl.DisplayNames constructor.

The parameter for Intl.DisplayNames.prototype.script() method is an ISO-15924 four letters script code.
It returns the localized string of script display name to present that code in the
language specified in the Intl.DisplayNames constructor.

```js
Intl.DisplayNames([ locales [ , options ]])
Intl.DisplayNames.prototype.region(regionCode)
Intl.DisplayNames.prototype.language(languageCode)
Intl.DisplayNames.prototype.languageWithRegion(languageCode, regionCode)
Intl.DisplayNames.prototype.script(scriptCode)
get Intl.DisplayNames.regionCodes([type])
get Intl.DisplayNames.languageCodes([type])
get Intl.DisplayNames.scriptCodes()
```

* regionCode is either a two letter ISO 3166-1 alpha-2 region code or a three digits UN M.49 area code.
* languageCode is either a two letters ISO 639-1 language code or a three letters ISO 639-2 language code.
* scriptCode is a four letter ISO 15924 script code.
* type for Intl.DisplayNames.regionCodes() could be either "all" (default), "two-letters", or "three-digits"
* type for Intl.DisplayNames.languageCodes() could be either "all" (default), "two-letters", or "three-letters"

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
console.log(Intl.DisplayNames.regionCodes()); // ["001", "002", ... , "AD", "AE", "AF"...]
// Same as above
console.log(Intl.DisplayNames.regionCodes("all")); // ["001", "002", ... , "AD", "AE", "AF"...]
// Get the list of three digits (UN M.49) region codes known by the API
console.log(Intl.DisplayNames.regionCodes("three-digits")); // ["001", "002", ... ]
// Get the list of two letters (ISO 3166-1 alpha-2 ) region codes known by the API
console.log(Intl.DisplayNames.regionCodes("two-letters")); // ["AD", "AE", "AF"... ]
```
To get the list of language codes known by the API:
```js
// Get the list of all language codes known by the API
console.log(Intl.DisplayNames.languageCodes()); // ["aa", "ab", ... , "fil", ...]
// Same as above
console.log(Intl.DisplayNames.languageCodes("all")); // ["aa", "ab", ... , "fil", ...]
// Get the list of two letters (ISO 639-1) language codes known by the API
console.log(Intl.DisplayNames.languageCodes("two-letters")); // ["aa", "ab", ...]
// Get the list of three letters (ISO 639-2) language codes known by the API
console.log(Intl.DisplayNames.languageCodes("three-letters")); // ["aar", "abk", ... , "fil", ...]
```
To get the list of script codes known by the API:
```js
// Get the list of all script codes known by the API
console.log(Intl.DisplayNames.scriptCodes()); // ["Adlm", "Afak", ... ]

```
