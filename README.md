League Uri Parser
=======

[![Build Status](https://img.shields.io/travis/thephpleague/uri-parser/master.svg?style=flat-square)](https://travis-ci.org/thephpleague/uri-parser)
[![Latest Version](https://img.shields.io/github/release/thephpleague/uri-parser.svg?style=flat-square)](https://github.com/thephpleague/uri-parser/releases)

This package contains a userland PHP uri parser compliant with [RFC 3986](http://tools.ietf.org/html/rfc3986).

System Requirements
-------

You need:

- **PHP >= 5.6.0** but the latest stable version of PHP is recommended
- the `mbstring` extension
- the `intl` extension

Installation
--------

```bash
$ composer require league/uri-parser
```

Documentation
---------

This is a drop-in replacement to PHP's `parse_url` function, with the following differences:

- The parser is RFC3986 compliant

```php
<?php

use League\Uri\Parser;

$parser = new Parser();
var_export($parser('http://foo.com?@bar.com/'));
//returns the following array
//array(
//  'scheme' => 'http',
//  'user' => null,
//  'pass' => null,
//  'host' => 'foo.com',
//  'port' => null,
//  'path' => '',
//  'query' => '@bar.com/',
//  'fragment' => null,
//);

var_export(parse_url('http://foo.com?@bar.com/'));
//returns the following array
//array(
//  'scheme' => 'http',
//  'host' => 'bar.com',
//  'user' => 'foo.com?',
//  'path' => '/',
//);
```

- The `Parser::__invoke` method always returns all URI components.

```php
<?php

use League\Uri\Parser;

$parser = new Parser();
var_export($parser('http://www.example.com/'));
//returns the following array
//array(
//  'scheme' => 'http',
//  'user' => null,
//  'pass' => null,
//  'host' => 'www.example.com',
//  'port' => null,
//  'path' => '/',
//  'query' => null,
//  'fragment' => null,
//);

var_export(parse_url('http://www.example.com/'));
//returns the following array
//array(
//  'scheme' => 'http',
//  'host' => 'www.example.com',
//  'path' => '/',
//);
```

- Accessing individual component is simple without needing extra parameters:

```php
<?php

use League\Uri\Parser;

$uri = 'http://www.example.com/';
$parser = new Parser();
$parser($uri)['query']; //returns null
parse_url($uri, PHP_URL_QUERY); //returns null
```

- Empty component and undefined component are treated differently

A distinction is made between an unspecified component, which will be set to `null` and an empty component which will be equal to the empty string.

```php
<?php

use League\Uri\Parser;

$uri = 'http://www.example.com/?';
$parser = new Parser();
$parser($uri)['query'];         //returns ''
parse_url($uri, PHP_URL_QUERY); //returns `null`
```

- The path component is never equal to `null`

Since a URI is made of at least a path component, this component is never equal to `null`

```php
<?php

use League\Uri\Parser;

$uri = 'http://www.example.com?';
$parser = new Parser();
$parser($uri)['path'];         //returns ''
parse_url($uri, PHP_URL_PATH); //returns `null`
```

Just like `parse_url`, the `League\Uri\Parser` only parses and extracts from the URI string its components. **You still need to validate them against its scheme specific rules.**

```php
<?php

use League\Uri\Parser;

$uri = 'http:www.example.com';
$parser = new Parser();
var_export($parser($uri));
//returns the following array
//array(
//  'scheme' => 'http',
//  'user' => null,
//  'pass' => null,
//  'host' => null,
//  'port' => null,
//  'path' => 'www.example.com',
//  'query' => null,
//  'fragment' => null,
//);
```

**This invalid HTTP URI is successfully parsed.**

Testing
-------

`URI Parser` has:

- a [PHPUnit](https://phpunit.de) test suite.
- a coding style compliance test suite using [PHP CS Fixer](http://cs.sensiolabs.org/).

To run the tests, run the following command from the project folder.

```bash
$ composer test
```

Benchmark
-------

Additionally, a benchmark test suite using [PHP Bench](https://github.com/phpbench/phpbench) is provided.

```bash
$ composer benchmarks
```

Contributing
-------

Contributions are welcome and will be fully credited. Please see [CONTRIBUTING](.github/CONTRIBUTING.md) and [CONDUCT](CONDUCT.md) for details.

Security
-------

If you discover any security related issues, please email nyamsprod@gmail.com instead of using the issue tracker.

Credits
-------

- [ignace nyamagana butera](https://github.com/nyamsprod)
- [All Contributors](https://github.com/thephpleague/uri-parser/contributors)

License
-------

The MIT License (MIT). Please see [License File](LICENSE) for more information.