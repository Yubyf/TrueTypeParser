# TrueTypeParser-Light

[![Maven Central](https://img.shields.io/maven-central/v/io.github.yubyf/truetypeparser-light?color=informational&label=Maven%20Central)](https://search.maven.org/artifact/io.github.yubyf/truetypeparser-light)
![GitHub Workflow Status (branch)](https://img.shields.io/github/workflow/status/yubyf/TrueTypeParser-Light/CI/master?label=Test&logo=github)
[![License](https://img.shields.io/github/license/Yubyf/TruetypeParser-Light)](https://github.com/Yubyf/truetypeparser/blob/master/LICENSE)
[![API](https://img.shields.io/badge/API-21%2B-blue.svg?style=flat)](https://android-arsenal.com/api?level=21)
![GitHub top language](https://img.shields.io/github/languages/top/yubyf/TruetypeParser-Light)

TrueType Font Parser Light for Android based on [Apache FOP](http://xmlgraphics.apache.org/fop/).

## Installation

1\. Download [the latest AAR](https://repo1.maven.org/maven2/io/github/yubyf/truetypeparser-light/2.0.1/truetypeparser-light-2.0.1.aar).

2\. Added the dependency from `mavenCentral`:

Kotlin DSL

 ```Kotlin
 implementation("io.github.yubyf:truetypeparser-light:$latest_version")
 ```

Groovy

 ```groovy
 implementation 'io.github.yubyf:truetypeparser-light:${latest_version}'
 ```

Maven:

```xml
<dependency>
  <groupId>io.github.yubyf</groupId>
  <artifactId>truetypeparser-light</artifactId>
  <version>$latest_version</version>
  <type>aar</type>
</dependency>
```

## Usage

### Parse

```kotlin
val fontPath = "Your font path"
val ttfFile: TTFFile = TTFFile.open(File(fontPath))
```

or

```kotlin
val inputStream = "Your font InputStream(File|Assets|...)"
val ttfFile: TTFFile = TTFFile.open(inputStream)
```

### Locale related fields(`Map`)

All keys of `locale` related fields are standardized from `languageId` of `Macintosh` or `Microsoft` platform as the tags for identifying languages based on the IETF BCP 47 specification - [BCP47](https://tools.ietf.org/html/bcp47), like `en-US`, `zh-CN`, etc.

```kotlin
val copyrightMap : Map<String, String> = ttfFile.copyrights
val fullNameMap : Map<String, String> = ttfFile.fullNames
val postscriptNameMap : Map<String, String> = ttfFile.postscriptNames
val familyMap : Map<String, String> = ttfFile.families
val subfamilyMap : Map<String, String> = ttfFile.subfamilies
val manufacturerMap : Map<String, String> = ttfFile.manufacturers
val designerMap : Map<String, String> = ttfFile.designers
val sampleTextMap : Map<String, String> = ttfFile.sampleTexts
```

And for ease of use, lib also provides a `getter` function for each field with `java.util.Locale` as the parameter and the `String` as the return value:

```kotlin
// Kotlin style getter operator function
val copyright : String = copyrightMap[Locale.ENGLISH]
val fullName : String = fullNameMap[Locale.SIMPLIFIED_CHINESE]
val family : String = familyMap[Locale.JAPANESE]

// General getter function
val copyright : String = getValueOrFallbackByLocale(copyrightMap, Locale.ENGLISH)
val fullName : String = getValueOrFallbackByLocale(fullNameMap, Locale.SIMPLIFIED_CHINESE)
val family : String = getValueOrFallbackByLocale(familyMap, Locale.JAPANESE)
...
```

If no value is found for the specified `java.util.Locale`, fallback and try to find the first matching locale in the following order and returns the value corresponding to the found locale:

- Locale in field `Map` that has the same language as the given `java.util.Locale`
- Tag of `java.util.Locale.US` generated by `java.util.Locale#toLanguageTag()`
- Locale with English language
- Tag of `java.util.Locale.ROOT` generated by `java.util.Locale#toLanguageTag()`
- Locale of first entry in field `Map`

### Standard fields(`String/Primitives`)

```kotlin
val vendorURL : String = ttfFile.vendorURL
val designerURL : String = ttfFile.designerURL
val weight : Int = ttfFile.weightClass
```

### Variation fields

```kotlin
// Whether the font is variable font.
val variable : Boolean = ttfFile.variable

// Get the variation axes.
val axes : List<VariationAxis> = ttfFile.variationAxes
// Get the variation instances.
val instances : List<VariationInstance> = ttfFile.variationInstances
```

## ChangeLog

### 2.1.3

- Added properties `variationAxes` and `variationInstances` for variable fonts.

### 2.1.2

- Added property `variable` to indicate if the font is a variable font.

### 2.1.1

- Replaced `getXXX()` functions of locale related fields with operator function `get()` and general function `getValueOrFallbackByLocale()`.

### 2.0.1

Since the [original repo](https://github.com/jaredrummler/TrueTypeParser) has not been updated, this fork has made several optimizations to the **TrueTypeParser-Light** module:

- Refactored with Kotlin.
- Optimized the memory usage of file reading([issue](https://github.com/jaredrummler/TrueTypeParser/issues/4)).
- Added several extra font properties in `TTFFile`, such as `manufacturer`, `designer`, `vendorURL`, `designerURL`, `preferSubfamily`, etc.
- Modified the `locale` related fields to `map` type, users can get the corresponding field values by `Locale` as key. Please see the [Usage](#Usage) for details of usage.

## License

    Copyright (c) 2022 Alex Liu

    Copyright (C) 2016 Jared Rummler

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
