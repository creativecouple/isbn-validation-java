Parse/format/validate ISBNs with the official ISBN ranges definition. <br/>
The parser is optimized for speed, the ISBN class for simplicity.

[![Download](https://maven-badges.herokuapp.com/maven-central/de.creativecouple.validation/isbn-core/badge.svg) ](https://maven-badges.herokuapp.com/maven-central/de.creativecouple.validation/isbn-core)
[![Javadocs](https://www.javadoc.io/badge/de.creativecouple.validation/isbn-core.svg)](https://creativecouple.github.io/isbn-validation-java/)

# ISBN Validation
A common issue in digital commerce systems is to validate article identifiers.
In the publishing business there is the so-called ISBN standard, maintained by the
[International ISBN Agency](https://www.isbn-international.org/).
By using its official ISBN range definitions this library offers up-to-date ISBN validation
beyond simply comparing the EAN-13 checkdigit.

All number ranges that are not yet assigned to a local ISBN agency or
which have no group range defined will be treated as invalid.
The dashes will be inserted automatically at the correct positions for grouping the
agency prefix, group code, publisher code, and title code.

Example:
- :x: wrong grouping: 978-05-5750-469-5 or 978-0557504695 (on amazon.com)
- :heavy_check_mark: correct grouping: 978-0-557-50469-5

The [ISBN domain object](https://creativecouple.github.io/isbn-validation-java/de/creativecouple/validation/isbn/ISBN.html)
wraps the ISBN string representation efficiently to be used
as identifier in your domain-driven code base.
It normalizes and corrects the given string input with respect to hyphens/dashes,
parsing both ISBN-10 and ISBN-13 numbers into the same object, the
[ISBN](https://creativecouple.github.io/isbn-validation-java/de/creativecouple/validation/isbn/ISBN.html). 

## Usage

Once you [add this library as dependency](https://search.maven.org/artifact/de.creativecouple.validation/isbn-core)
to your project definition, you can simply convert your strings into
[ISBN objects](https://creativecouple.github.io/isbn-validation-java/de/creativecouple/validation/isbn/ISBN.html) via:

```java
  ISBN isbn = ISBN.valueOf(someString)
```

This will potentially throw a `NumberFormatException` if the input string is not a valid ISBN.

You can get different string representations to be used e.g. in your databases:

```java
  ISBN isbn = ISBN.valueOf("978-0557504695"); // wrong hyphenation on amazon.com

  isbn.toString();        // standard representation = "978-0-557-50469-5"
  isbn.toCompactString(); // compact representation = "9780557504695"
  
  isbn.getPrefix();          // EAN/UCC prefix = "978" 
  isbn.getAgencyName();      // EAN/UCC prefix's agency name = "International ISBN Agency" 
  isbn.getGroup();           // group code = "0" 
  isbn.getGroupPrefix();     // group's ISBN prefix = "978-0" 
  isbn.getGroupName();       // group's agency name = "English language" 
  isbn.getPublisher();       // publisher code = "557" 
  isbn.getPublisherPrefix(); // publisher's ISBN prefix = "978-0-557" 
  isbn.getTitle();           // title code = "50469" 
  isbn.getCheckdigit();      // checkdigit character = '5' 
```

----

If you only want to work with Strings instead of the ISBN object you can simply use the
[ISBNValidator](https://creativecouple.github.io/isbn-validation-java/de/creativecouple/validation/isbn/ISBNValidator.html)
to check them against the chosen rules. This is one of the 12 combinations of ISBN length (ISBN-10 / ISBN-13 / both)
and hyphenation style (any / correct / none / correct-or-none). 

```java
  ISBNValidator validator = ISBNValidator.isbn10().hyphenation(NONE);
        
  validator.isValid("978-0-557-50469-5"); // false
  validator.isValid("9780557504695") // false
  validator.isValid("0-557-50469-4"); // false
  validator.isValid("0557504694") // true
```

### Javadoc

Read the full Java docs online at
https://creativecouple.github.io/isbn-validation-java/

## Versioning

Our build pipeline checks regularly for new versions of the ISBN range definition provided by the
International ISBN Agency. When there are changes then a new version of this library will be deployed.

## Contribute

You can clone this repository locally and build as a standard maven project.
Feel free to open issues to this repository or create pull requests.

### Generate a new ISBN range definition

1. Download the ISBN ranges as XML file from: https://www.isbn-international.org/range_file_generation.
1. Name the downloaded file `RangeMessage.xml` and put it into the local directory `generate`.
1. From within `generate` directory, run the shell command:
`java GenerateIsbnRanges.java < RangeMessage.xml > ../src/main/java/de/creativecouple/validation/isbn/ISBNRanges.java`
1. From the project directory run `mvn clean test`

The formatter plugin will auto-format all source files during the maven run.
So you are able to easily spot the differences since the previous version.

## Disclaimer

The MIT License (MIT)
Copyright (c) 2022 CreativeCouple

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:
 
The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
