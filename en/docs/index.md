**CodeListHub** is an [Open Data project](https://opendatahandbook.org/guide/en/what-is-open-data/) that provides a central repository for code lists or key directories in the [OpenCodeList](https://openpotato.github.io/opencodelist/en/) format. These code lists can be queried and downloaded via a [free API](api.md). Additionally, the [CodeListHub Explorer](https:///explorer.codelisthub.org/en) allows for convenient browsing of CodeListHub using a web browser.

## What are Code Lists?

Code lists are structured collections of codes or keys used for the identification and classification of data. These directories are essential in various fields such as databases, management systems, scientific research, and industrial applications. They help organize, store, and retrieve data consistently and efficiently.

Code lists play a central role in data standardisation and harmonisation. By using standardised codes, different systems and organisations can uniformly interpret and exchange data.

Examples of code lists include:

+ International codes for countries, languages, and currencies from the International Organization for Standardization (ISO).
+ National territorial codes (e.g. municipality codes of the Federal Republic of Germany).
+ National and subnational keys in the public sector (e.g. statistical codes from national statistical agencies).

## What is the OpenCodeList Standard?

[OpenCodeList](https://openpotato.github.io/opencodelist/en/) defines a generic standard data format for representing code lists in JSON format. This format can be easily generated and read by almost any programming language. With the help of the [OpenCodeList Document Schema](https://github.com/openpotato/opencodelist/tree/main/schemas/v0.3/schema.json), documents in the OpenCodeList format can be validated for syntactical correctness.

OpenCodeList can be used for exchanging code lists between services or applications, as a representation format for official code lists, or as a response format for API requests (e.g. for RESTful web services).

An OpenCodeList document exists in two forms:

+ **Code List:** A code list is a classic relational table with columns and data rows, with at least one column serving as the key (code).

    OpenCodeList allows the definition of generic code lists. A code list without data is a meta code list.

+ **Code List Sets:** A code list set is a list of references to external code lists or external code list sets.

    A code list set can represent:

    + The aggregation of different versions of a code list in a set.
    + The creation of a hierarchy of code list sets.
    + The creation of an index of all available code lists.

    A code list set without data is a meta code list set.

## Why CodeListHub?

In our view, standardising the representation of code lists is only half the battle. Code lists also need to be stored somewhere and made available. Ideally, this should be in an open, central location. CodeListHub is this location (or aims to become one).

CodeListHub provides code lists both via an [API](api.md) and a simple [online explorer](https://explorer.codelisthub.org/en), in both the OpenCodeList format and CSV format. Code list management is handled through a central [GitHub repository](https://github.com/openpotato/codelisthub.data). This has the advantage that changes can be traced via Git and, if necessary, reverted.
