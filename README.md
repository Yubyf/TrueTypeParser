# TrueTypeParser-Light

[![Maven Central](https://img.shields.io/maven-central/v/io.github.yubyf/truetypeparser-light?color=brightgreen&label=Maven%20Central)](https://search.maven.org/artifact/io.github.yubyf/truetypeparser-light)
[![License](https://img.shields.io/github/license/Yubyf/TruetypeParser-Light)](https://github.com/Yubyf/truetypeparser/blob/master/LICENSE)
[![API](https://img.shields.io/badge/API-21%2B-blue.svg?style=flat)](https://android-arsenal.com/api?level=21)
![GitHub top language](https://img.shields.io/github/languages/top/yubyf/TruetypeParser-Light)

TrueType Font Parser Light for Android based on [Apache FOP](http://xmlgraphics.apache.org/fop/).

Since the original repo has not been updated, this fork has made several optimizations to the **TrueTypeParser-Light** module:

- Refactored with Kotlin.
- Optimized the memory usage of file reading([issue](https://github.com/jaredrummler/TrueTypeParser/issues/4)).
- Added several extra font properties in `TTFFile`, such as `manufacturer`, `designer`, `vendorURL`, `designerURL`, `preferSubfamily`, etc.
- Modified the `locale` related fields to `map` type, users can get the corresponding field values by `Locale` as key. Please see the [Usage](#Usage) for details of usage.

## Installation

1\. Download [the latest AAR](https://repo1.maven.org/maven2/io/github/yubyf/truetypeparser-light/2.0.0/truetypeparser-light-2.0.0.aar).

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
val copyrightMap : MutableMap<String, String> = ttfFile.copyright
val fullNameMap : MutableMap<String, String> = ttfFile.fullName
val postscriptNameMap : MutableMap<String, String> = ttfFile.postscriptName
val familyMap : MutableMap<String, String> = ttfFile.family
val subfamilyMap : MutableMap<String, String> = ttfFile.subfamily
val manufacturerMap : MutableMap<String, String> = ttfFile.manufacturer
val designerMap : MutableMap<String, String> = ttfFile.designer
```

And for ease of use, lib also provides a `getter` function for each field with `java.util.Locale` as the parameter  and the `String` as the return value:

```kotlin
val copyright : String = ttfFile.getCopyright(Locale.ENGLISH)
val fullName : String = ttfFile.getFullName(Locale.SIMPLIFIED_CHINESE)
val family : String = ttfFile.getCopyright(Locale.JAPANESE)
...
```

If no value is found for the specified `Locale`, the `getter` function will fallback in order of [Locale.ENGLISH], [Locale.ROOT], the same language, and the first entry to return the first value that matches.

### Standard fields(`String`)


```kotlin
val vendorURL : String = ttfFile.vendorURL
val designerURL : String = ttfFile.designerURL
val weight : Int = ttfFile.weightClass
```

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
