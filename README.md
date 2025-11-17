# clsString

[![Repo Size](https://img.shields.io/badge/repo-ready-blue)](https://github.com/)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)]

A small, focused C++ utility class that wraps `std::string` and provides a convenient collection of string manipulation helpers (counting, casing, trimming, splitting/joining, replacing, and more).

This README is written for use in Visual Studio Community 2022 projects but the class itself is standard C++ and should compile with most modern compilers.

---

## Table of Contents

* [Features](#features)
* [Prerequisites](#prerequisites)
* [Installation](#installation)
* [Quick Usage](#quick-usage)
* [API Reference](#api-reference)
* [Examples](#examples)
* [Design Notes](#design-notes)
* [Contributing](#contributing)
* [License](#license)

---

## Features

* Wraps `std::string` with an internal `Value` property.
* Static and instance methods for:

  * Length, word count, vowel/letter counts
  * Upper/lower/Invert casing (per-letter and per-word)
  * Trim (left, right, both)
  * Split / Join
  * Replace words, remove punctuations, reverse words
  * Count specific letters and vowels
* Simple, dependency-free implementation (only uses `<string>`, `<vector>`, `<cctype>`, and `<algorithm>` internally).

---

## Prerequisites

* Visual Studio Community 2022 (recommended)
* C++11 or newer (the code uses simple STL features; adjust your project standard if necessary)

> Note: If you use Visual Studio 2022, set the project’s C++ language standard to at least C++11: Project → Properties → C/C++ → Language → C++ Language Standard → ISO C++11 (or later).

---

## Installation

1. Copy `clsString.h` (and accompanying `clsString.cpp` if you separate implementation) to your project folder — for a single-header approach put the entire class in `clsString.h`.
2. In Visual Studio, add the file(s) to your project: right-click the project → Add → Existing Item... → select the header/source files.
3. Make sure `Global.h` (if used) and any project-wide headers are available. If you don’t have `Global.h`, either remove that dependency or create a lightweight placeholder that includes common headers and namespace usages.

---

## Quick Usage

```cpp
#include <iostream>
#include "clsString.h"

using namespace std;

int main()
{
    clsString s("  hello world! this is clsString.  ");

    cout << "Original: '" << s.Value << "'\n";

    s.Trim();
    cout << "Trimmed: '" << s.Value << "'\n";

    cout << "Length: " << s.Length() << "\n";
    cout << "Words: " << s.CountWords() << "\n";

    s.UpperFirstLetterOfEachWord();
    cout << "Title Case: '" << s.Value << "'\n";

    s.RemovePunctuations();
    cout << "No punctuation: '" << s.Value << "'\n";

    vector<string> parts = s.Split(" ");
    cout << "First token: " << (parts.size() ? parts[0] : string("<empty>")) << "\n";

    return 0;
}
```

---

## API Reference

> The following lists the public API provided by `clsString`. Both static and instance variants exist for many helpers.

### Constructors

* `clsString()` — default constructs with empty value.
* `clsString(const string& Value)` — constructs wrapping the provided value.

### Property

* `__declspec(property(get = GetValue, put = SetValue)) string Value;`

  * `SetValue(const string& Value)`
  * `string GetValue()`

### Length

* `static short Length(const string& S1)`
* `short Length()`

### Word counting

* `static short CountWords(string S1)`
* `short CountWords()`

### Casing

* `static string UpperFirstLetterOfEachWord(string S1)`
* `void UpperFirstLetterOfEachWord()`
* `static string LowerFirstLetterOfEachWord(string S1)`
* `void LowerFirstLetterOfEachWord()`
* `static string UpperAllString(string S1)`
* `void UpperAllString()`
* `static string LowerAllString(string S1)`
* `void LowerAllString()`
* `static char InvertLetterCase(char char1)`
* `static string InvertAllLettersCase(string S1)`
* `void InvertAllLettersCase()`

### Letter counting & queries

* `enum enWhatToCount { SmallLetters = 0, CapitalLetters = 1, All = 3 }`
* `static short CountLetters(const string& S1, const enWhatToCount& WhatToCount = enWhatToCount::All)`
* `static short CountCapitalLetters(const string& S1)`
* `short CountCapitalLetters()`
* `static short CountSmallLetters(const string& S1)`
* `short CountSmallLetters()`
* `static short CountSpecificLetter(const string& S1, char Letter, bool MatchCase = true)`
* `short CountSpecificLetter(char Letter, bool MatchCase = true)`
* `static bool IsVowel(char Ch1)`
* `static short CountVowels(const string& S1)`
* `short CountVowels()`

### Splitting & joining

* `static vector<string> Split(string S1, const string& Delim)`
* `vector<string> Split(const string& Delim)`
* `static string JoinString(vector<string>& vString, const string& Delim)`
* `static string JoinString(string arrString[], short Length, const string& Delim)`

### Trimming

* `static string TrimLeft(string S1)`
* `void TrimLeft()`
* `static string TrimRight(string S1)`
* `void TrimRight()`
* `static string Trim(string S1)`
* `void Trim()`

### Other utilities

* `static string ReverseWordsInString(const string& S1)`
* `void ReverseWordsInString()`
* `static string ReplaceWord(const string& MainString, const string& StringToReplace, const string& sRepalceTo, bool MatchCase = true)`
* `string ReplaceWord(const string& MainStirng, const string& sRepalceTo)`
* `static string RemovePunctuations(const string& S1)`
* `void RemovePunctuations()`

---

## Examples

### Example: Count letters and vowels

```cpp
clsString s("Hello, C++ World!");
int capitals = s.CountCapitalLetters();
int vowels = s.CountVowels();
```

### Example: Split & Join

```cpp
string sentence = "apple,banana,orange";
vector<string> fruits = clsString::Split(sentence, ",");
string joined = clsString::JoinString(fruits, "|"); // apple|banana|orange
```

### Example: Replace words (case-insensitive)

```cpp
string t = clsString::ReplaceWord("Hello world hello", "hello", "hi", false);
// t == "hi world hi"
```

---

## Design Notes & Tips

* Many methods come in both `static` and instance form. Use static methods when you only have a `std::string` and do not need to create a `clsString` object.
* Methods operate on ASCII letters using `std::toupper`/`std::tolower` and `std::isupper`/`std::islower`. If you need Unicode/UTF-8-aware behaviour, consider using a dedicated Unicode library (e.g. ICU) or adapt the class accordingly.
* The class currently uses naive tokenization by delimiter; for advanced parsing (preserving quoted tokens, trimming each token, or multi-character delimiters) adjust `Split` implementation as needed.

---

## Contributing

Contributions, bug reports, and improvements are welcome. Please open an issue or submit a pull request with a clear description and test cases.

---

## License

This project is licensed under the MIT License - see the `LICENSE` file for details.

---

If you want, I can also:

* convert this README to Arabic (or bilingual),
* generate a `clsString.cpp` implementation file separated from the header,
* create a minimal Visual Studio 2022 sample project that includes the class.
