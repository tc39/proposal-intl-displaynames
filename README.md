## Proposal: Intl.get[Language|Region|Locale]DisplayNames

### Motivation
Main motivation for Intl project was to enable collation on the client. Collation algorithm requires large amount of data, which is already available in most browsers. Language and region name translations also carry steep data size penalty for developers. Our goal is to expose this data through Intl API for use in e.g. language and region pickers, labeling maps, keyboard selection, etc.

### Syntax
```js
Intl.getLanguageDisplayNames(localePriorityList, languagesToDisplayList)
Intl.getRegionDisplayNames(localePriorityList, regionsToDisplayList)
Intl.getLocaleDisplayNames(localePriorityList, localesToDisplayList)
```
First parameter, localePriorityList, represents the usual locale resolution, where we try to fulfill request by going from first locale, to last.

* For getLanguageDisplayNames, second parameter represents a list of BCP47 language tags.
* For getRegionDisplayNames, second parameter represents a list of two letter country codes.
* For getLocaleDisplayNames, second parameter represents a list of Locale objects or string representations of BCP47 tags.

### Authors
* Zibi Braniecki (@zbraniecki)
* Sascha Brawer (@brawer)
* Nebojša Ćirić (@nciric)

### Prior art
Mozilla already has [vendor specific implementation](https://firefox-source-docs.mozilla.org/intl/dataintl.html#mozintl-getlanguagedisplaynames-locales-langcodes).

### Usage

```js
let langs = Intl.getLanguageDisplayNames(["pl"], ["fr", "de", "en", "sr-Latn-XK"]);
langs === ["Francuski", "Niemiecki", "Angielski", "Serbski"];

let regs = Intl.getRegionDisplayNames(["pl"], ["US", "CA", "MX"]);
regs === ["Stany Zjednoczone", "Kanada", "Meksyk"];

let locs = Intl.getLocaleDisplayNames(["pl"], ["sr-RU", "es-MX", "fr-CA", "sr-Latn-XK"]);
locs === ["Serbski (Rosja)", "Hiszpański (Meksyk)", "Francuski (Kanada)", "Serbski (Łacińskie, Kosowo)"];
```
