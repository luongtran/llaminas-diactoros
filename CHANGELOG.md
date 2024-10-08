# Changelog

All notable changes to this project will be documented in this file, in reverse chronological order by release.

## 1.3.3 - 2016-01-04

### Added

- Nothing.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#135](https://github.com/zendframework/zend-diactoros/pull/135) fixes the
  behavior of `ServerRequestFactory::marshalHeaders()` to no longer omit
  `Cookie` headers from the aggregated headers. While the values are parsed and
  injected into the cookie params, it's useful to have access to the raw headers
  as well.

## 1.3.2 - 2015-12-22

### Added

- [zendframework/zend-diactoros#124](https://github.com/zendframework/zend-diactoros/pull/124) adds four
  more optional arguments to the `ServerRequest` constructor:
  - `array $cookies`
  - `array $queryParams`
  - `null|array|object $parsedBody`
  - `string $protocolVersion`
  `ServerRequestFactory` was updated to pass values for each of these parameters
  when creating an instance, instead of using the related `with*()` methods on
  an instance.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#122](https://github.com/zendframework/zend-diactoros/pull/122) updates the
  `ServerRequestFactory` to retrieve the HTTP protocol version and inject it in
  the generated `ServerRequest`, which previously was not performed.

## 1.3.1 - 2015-12-16

### Added

- Nothing.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#113](https://github.com/zendframework/zend-diactoros/pull/113) fixes an
  issue in the response serializer, ensuring that the status code in the
  deserialized response is an integer.
- [zendframework/zend-diactoros#115](https://github.com/zendframework/zend-diactoros/pull/115) fixes an
  issue in the various text-basd response types (`TextResponse`, `HtmlResponse`,
  and `JsonResponse`); due to the fact that the constructor was not
  rewinding the message body stream, `getContents()` was thus returning `null`,
  as the pointer was at the end of the stream. The constructor now rewinds the
  stream after populating it in the constructor.

## 1.3.0 - 2015-12-15

### Added

- [zendframework/zend-diactoros#110](https://github.com/zendframework/zend-diactoros/pull/110) adds
  `Laminas\Diactoros\Response\SapiEmitterTrait`, which provides the following
  private method definitions:
  - `injectContentLength()`
  - `emitStatusLine()`
  - `emitHeaders()`
  - `flush()`
  - `filterHeader()`
  The `SapiEmitter` implementation has been updated to remove those methods and
  instead compose the trait.
- [zendframework/zend-diactoros#111](https://github.com/zendframework/zend-diactoros/pull/111) adds
  a new emitter implementation, `SapiStreamEmitter`; this emitter type will
  loop through the stream instead of emitting it in one go, and supports content
  ranges.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- Nothing.

## 1.2.1 - 2015-12-15

### Added

- Nothing.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#101](https://github.com/zendframework/zend-diactoros/pull/101) fixes the
  `withHeader()` implementation to ensure that if the header existed previously
  but using a different casing strategy, the previous version will be removed
  in the cloned instance.
- [zendframework/zend-diactoros#103](https://github.com/zendframework/zend-diactoros/pull/103) fixes the
  constructor of `Response` to ensure that null status codes are not possible.
- [zendframework/zend-diactoros#99](https://github.com/zendframework/zend-diactoros/pull/99) fixes
  validation of header values submitted via request and response constructors as
  follows:
  - numeric (integer and float) values are now properly allowed (this solves
    some reported issues with setting Content-Length headers)
  - invalid header names (non-string values or empty strings) now raise an
    exception.
  - invalid individual header values (non-string, non-numeric) now raise an
    exception.

## 1.2.0 - 2015-11-24

### Added

- [zendframework/zend-diactoros#88](https://github.com/zendframework/zend-diactoros/pull/88) updates the
  `SapiEmitter` to emit a `Content-Length` header with the content length as
  reported by the response body stream, assuming that
  `StreamInterface::getSize()` returns an integer.
- [zendframework/zend-diactoros#77](https://github.com/zendframework/zend-diactoros/pull/77) adds a new
  response type, `Laminas\Diactoros\Response\TextResponse`, for returning plain
  text responses. By default, it sets the content type to `text/plain;
  charset=utf-8`; per the other response types, the signature is `new
  TextResponse($text, $status = 200, array $headers = [])`.
- [zendframework/zend-diactoros#90](https://github.com/zendframework/zend-diactoros/pull/90) adds a new
  `Laminas\Diactoros\CallbackStream`, allowing you to back a stream with a PHP
  callable (such as a generator) to generate the message content. Its
  constructor accepts the callable: `$stream = new CallbackStream($callable);`

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#77](https://github.com/zendframework/zend-diactoros/pull/77) updates the
  `HtmlResponse` to set the charset to utf-8 by default (if no content type
  header is provided at instantiation).

## 1.1.4 - 2015-10-16

### Added

- [zendframework/zend-diactoros#98](https://github.com/zendframework/zend-diactoros/pull/98) adds
  `JSON_UNESCAPED_SLASHES` to the default `json_encode` flags used by
  `Laminas\Diactoros\Response\JsonResponse`.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#96](https://github.com/zendframework/zend-diactoros/pull/96) updates
  `withPort()` to allow `null` port values (indicating usage of default for
  the given scheme).
- [zendframework/zend-diactoros#91](https://github.com/zendframework/zend-diactoros/pull/91) fixes the
  logic of `withUri()` to do a case-insensitive check for an existing `Host`
  header, replacing it with the new one.

## 1.1.3 - 2015-08-10

### Added

- [zendframework/zend-diactoros#73](https://github.com/zendframework/zend-diactoros/pull/73) adds caching of
  the vendor directory to the Travis-CI configuration, to speed up builds.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#71](https://github.com/zendframework/zend-diactoros/pull/71) fixes the
  docblock of the `JsonResponse` constructor to typehint the `$data` argument
  as `mixed`.
- [zendframework/zend-diactoros#73](https://github.com/zendframework/zend-diactoros/pull/73) changes the
  behavior in `Request` such that if it marshals a stream during instantiation,
  the stream is marked as writeable (specifically, mode `wb+`).
- [zendframework/zend-diactoros#85](https://github.com/zendframework/zend-diactoros/pull/85) updates the
  behavior of `Laminas\Diactoros\Uri`'s various `with*()` methods that are
  documented as accepting strings to raise exceptions on non-string input.
  Previously, several simply passed non-string input on verbatim, others
  normalized the input, and a few correctly raised the exceptions. Behavior is
  now consistent across each.
- [zendframework/zend-diactoros#87](https://github.com/zendframework/zend-diactoros/pull/87) fixes
  `UploadedFile` to ensure that `moveTo()` works correctly in non-SAPI
  environments when the file provided to the constructor is a path.

## 1.1.2 - 2015-07-12

### Added

- Nothing.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#67](https://github.com/zendframework/zend-diactoros/pull/67) ensures that
  the `Stream` class only accepts `stream` resources, not any resource.

## 1.1.1 - 2015-06-25

### Added

- Nothing.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#64](https://github.com/zendframework/zend-diactoros/pull/64) fixes the
  behavior of `JsonResponse` with regards to serialization of `null` and scalar
  values; the new behavior is to serialize them verbatim, without any casting.

## 1.1.0 - 2015-06-24

### Added

- [zendframework/zend-diactoros#52](https://github.com/zendframework/zend-diactoros/pull/52),
  [zendframework/zend-diactoros#58](https://github.com/zendframework/zend-diactoros/pull/58),
  [zendframework/zend-diactoros#59](https://github.com/zendframework/zend-diactoros/pull/59), and
  [zendframework/zend-diactoros#61](https://github.com/zendframework/zend-diactoros/pull/61) create several
  custom response types for simplifying response creation:

  - `Laminas\Diactoros\Response\HtmlResponse` accepts HTML content via its
    constructor, and sets the `Content-Type` to `text/html`.
  - `Laminas\Diactoros\Response\JsonResponse` accepts data to serialize to JSON via
    its constructor, and sets the `Content-Type` to `application/json`.
  - `Laminas\Diactoros\Response\EmptyResponse` allows creating empty, read-only
    responses, with a default status code of 204.
  - `Laminas\Diactoros\Response\RedirectResponse` allows specifying a URI for the
    `Location` header in the constructor, with a default status code of 302.

  Each also accepts an optional status code, and optional headers (which can
  also be used to provide an alternate `Content-Type` in the case of the HTML
  and JSON responses).

### Deprecated

- Nothing.

### Removed

- [zendframework/zend-diactoros#43](https://github.com/zendframework/zend-diactoros/pull/43) removed both
  `ServerRequestFactory::marshalUri()` and `ServerRequestFactory::marshalHostAndPort()`,
  which were deprecated prior to the 1.0 release.

### Fixed

- [zendframework/zend-diactoros#29](https://github.com/zendframework/zend-diactoros/pull/29) fixes request
  method validation to allow any valid token as defined by [RFC
  7230](http://tools.ietf.org/html/rfc7230#appendix-B). This allows usage of
  custom request methods, vs a static, hard-coded list.

## 1.0.5 - 2015-06-24

### Added

- Nothing.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#60](https://github.com/zendframework/zend-diactoros/pull/60) fixes
  the behavior of `UploadedFile` when the `$errorStatus` provided at
  instantiation is not `UPLOAD_ERR_OK`. Prior to the fix, an
  `InvalidArgumentException` would occur at instantiation due to the fact that
  the upload file was missing or invalid. With the fix, no exception is raised
  until a call to `moveTo()` or `getStream()` is made.

## 1.0.4 - 2015-06-23

This is a security release.

A patch has been applied to `Laminas\Diactoros\Uri::filterPath()` that ensures that
paths can only begin with a single leading slash. This prevents the following
potential security issues:

- XSS vectors. If the URI path is used for links or form targets, this prevents
  cases where the first segment of the path resembles a domain name, thus
  creating scheme-relative links such as `//example.com/foo`. With the patch,
  the leading double slash is reduced to a single slash, preventing the XSS
  vector.
- Open redirects. If the URI path is used for `Location` or `Link` headers,
  without a scheme and authority, potential for open redirects exist if clients
  do not prepend the scheme and authority. Again, preventing a double slash
  corrects the vector.

If you are using `Laminas\Diactoros\Uri` for creating links, form targets, or
redirect paths, and only using the path segment, we recommend upgrading
immediately.

### Added

- [zendframework/zend-diactoros#25](https://github.com/zendframework/zend-diactoros/pull/25) adds
  documentation. Documentation is written in markdown, and can be converted to
  HTML using [bookdown](http://bookdown.io). New features now MUST include
  documentation for acceptance.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#51](https://github.com/zendframework/zend-diactoros/pull/51) fixes
  `MessageTrait::getHeaderLine()` to return an empty string instead of `null` if
  the header is undefined (which is the behavior specified in PSR-7).
- [zendframework/zend-diactoros#57](https://github.com/zendframework/zend-diactoros/pull/57) fixes the
  behavior of how the `ServerRequestFactory` marshals upload files when they are
  represented as a nested associative array.
- [zendframework/zend-diactoros#49](https://github.com/zendframework/zend-diactoros/pull/49) provides several
  fixes that ensure that Diactoros complies with the PSR-7 specification:
  - `MessageInterface::getHeaderLine()` MUST return a string (that string CAN be
    empty). Previously, Diactoros would return `null`.
  - If no `Host` header is set, the `$preserveHost` flag MUST be ignored when
    calling `withUri()` (previously, Diactoros would not set the `Host` header
    if `$preserveHost` was `true`, but no `Host` header was present).
  - The request method MUST be a string; it CAN be empty. Previously, Diactoros
    would return `null`.
  - The request MUST return a `UriInterface` instance from `getUri()`; that
    instance CAN be empty. Previously, Diactoros would return `null`; now it
    lazy-instantiates an empty `Uri` instance on initialization.
- [ZF2015-05](https://getlaminas.org/security/advisory/ZF2015-05) was
  addressed by altering `Uri::filterPath()` to prevent emitting a path prepended
  with multiple slashes.

## 1.0.3 - 2015-06-04

### Added

- [zendframework/zend-diactoros#48](https://github.com/zendframework/zend-diactoros/pull/48) drops the
  minimum supported PHP version to 5.4, to allow an easier upgrade path for
  Symfony 2.7 users, and potential Drupal 8 usage.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- Nothing.

## 1.0.2 - 2015-06-04

### Added

- [zendframework/zend-diactoros#27](https://github.com/zendframework/zend-diactoros/pull/27) adds phonetic
  pronunciation of "Diactoros" to the README file.
- [zendframework/zend-diactoros#36](https://github.com/zendframework/zend-diactoros/pull/36) adds property
  annotations to the class-level docblock of `Laminas\Diactoros\RequestTrait` to
  ensure properties inherited from the `MessageTrait` are inherited by
  implementations.

### Deprecated

- Nothing.

### Removed

- Nothing.
-
### Fixed

- [zendframework/zend-diactoros#41](https://github.com/zendframework/zend-diactoros/pull/41) fixes the
  namespace for test files to begin with `LaminasTest` instead of `Laminas`.
- [zendframework/zend-diactoros#46](https://github.com/zendframework/zend-diactoros/pull/46) ensures that
  the cookie and query params for the `ServerRequest` implementation are
  initialized as arrays.
- [zendframework/zend-diactoros#47](https://github.com/zendframework/zend-diactoros/pull/47) modifies the
  internal logic in `HeaderSecurity::isValid()` to use a regular expression
  instead of character-by-character comparisons, improving performance.

## 1.0.1 - 2015-05-26

### Added

- [zendframework/zend-diactoros#10](https://github.com/zendframework/zend-diactoros/pull/10) adds
  `Laminas\Diactoros\RelativeStream`, which will return stream contents relative to
  a given offset (i.e., a subset of the stream).  `AbstractSerializer` was
  updated to create a `RelativeStream` when creating the body of a message,
  which will prevent duplication of the stream in-memory.
- [zendframework/zend-diactoros#21](https://github.com/zendframework/zend-diactoros/pull/21) adds a
  `.gitattributes` file that excludes directories and files not needed for
  production; this will further minify the package for production use cases.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#9](https://github.com/zendframework/zend-diactoros/pull/9) ensures that
  attributes are initialized to an empty array, ensuring that attempts to
  retrieve single attributes when none are defined will not produce errors.
- [zendframework/zend-diactoros#14](https://github.com/zendframework/zend-diactoros/pull/14) updates
  `Laminas\Diactoros\Request` to use a `php://temp` stream by default instead of
  `php://memory`, to ensure requests do not create an out-of-memory condition.
- [zendframework/zend-diactoros#15](https://github.com/zendframework/zend-diactoros/pull/15) updates
  `Laminas\Diactoros\Stream` to ensure that write operations trigger an exception
  if the stream is not writeable. Additionally, it adds more robust logic for
  determining if a stream is writeable.

## 1.0.0 - 2015-05-21

First stable release, and first release as `laminas-diactoros`.

### Added

- Nothing.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- Nothing.
