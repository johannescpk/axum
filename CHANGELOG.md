# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

# Unreleased

- Overall compile time improvements. If you're having issues with compile time
  please file an issue!
- Remove `prelude`. Explicit imports are now required ([#195](https://github.com/tokio-rs/axum/pull/195))
- Add dedicated `Router` to replace the `RoutingDsl` trait ([#214](https://github.com/tokio-rs/axum/pull/214))
- Replace `axum::route(...)` with `axum::Router::new().route(...)`. This means
  there is now only one way to create a new router. Same goes for
  `axum::routing::nest`. ([#215](https://github.com/tokio-rs/axum/pull/215))
- Make `FromRequest` default to being generic over `body::Body` ([#146](https://github.com/tokio-rs/axum/pull/146))
- Implement `std::error::Error` for all rejections ([#153](https://github.com/tokio-rs/axum/pull/153))
- Add `Router::or` for combining routes ([#108](https://github.com/tokio-rs/axum/pull/108))
- Add `handle_error` to `service::OnMethod` ([#160](https://github.com/tokio-rs/axum/pull/160))
- Add `OriginalUri` for extracting original request URI in nested services ([#197](https://github.com/tokio-rs/axum/pull/197))
- Implement `FromRequest` for `http::Extensions`
- Implement SSE as an `IntoResponse` instead of a service ([#98](https://github.com/tokio-rs/axum/pull/98))
- Add `Headers` for easily customizing headers on a response ([#193](https://github.com/tokio-rs/axum/pull/193))
- Add `Redirect` response ([#192](https://github.com/tokio-rs/axum/pull/192))
- Make `RequestParts::{new, try_into_request}` public ([#194](https://github.com/tokio-rs/axum/pull/194))

## Breaking changes

- Add associated `Body` and `BodyError` types to `IntoResponse`. This is
  required for returning responses with bodies other than `hyper::Body` from
  handlers. See the docs for advice on how to implement `IntoResponse` ([#86](https://github.com/tokio-rs/axum/pull/86))
- Replace `body::BoxStdError` with `Error`, which supports downcasting ([#150](https://github.com/tokio-rs/axum/pull/150))
- `get` routes will now also be called for `HEAD` requests but will always have
  the response body removed ([#129](https://github.com/tokio-rs/axum/pull/129))
- Change WebSocket API to use an extractor ([#121](https://github.com/tokio-rs/axum/pull/121))
- Make WebSocket `Message` an enum ([#116](https://github.com/tokio-rs/axum/pull/116))
- `WebSocket` now uses `Error` as its error type ([#150](https://github.com/tokio-rs/axum/pull/150))
  behavior ([#120](https://github.com/tokio-rs/axum/pull/120))
- Implement `routing::MethodFilter` via [`bitflags`](https://crates.io/crates/bitflags)
- Removed `extract::UrlParams` and `extract::UrlParamsMap`. Use `extract::Path` instead
- `EmptyRouter` now requires the response body to implement `Send + Sync + 'static'` ([#108](https://github.com/tokio-rs/axum/pull/108))
- `extractor_middleware` now requires `RequestBody: Default` ([#167](https://github.com/tokio-rs/axum/pull/167))
- Convert `RequestAlreadyExtracted` to an enum with each possible error variant ([#167](https://github.com/tokio-rs/axum/pull/167))
- `Router::check_infallible` now returns a `CheckInfallible` service. This
  is to improve compile times.
- These future types have been moved
    - `extract::extractor_middleware::ExtractorMiddlewareResponseFuture` moved
      to `extract::extractor_middleware::future::ResponseFuture` ([#133](https://github.com/tokio-rs/axum/pull/133))
    - `routing::BoxRouteFuture` moved to `routing::future::BoxRouteFuture` ([#133](https://github.com/tokio-rs/axum/pull/133))
    - `routing::EmptyRouterFuture` moved to `routing::future::EmptyRouterFuture` ([#133](https://github.com/tokio-rs/axum/pull/133))
    - `routing::RouteFuture` moved to `routing::future::RouteFuture` ([#133](https://github.com/tokio-rs/axum/pull/133))
    - `service::BoxResponseBodyFuture` moved to `service::future::BoxResponseBodyFuture` ([#133](https://github.com/tokio-rs/axum/pull/133))
- The following types no longer implement `Copy` ([#132](https://github.com/tokio-rs/axum/pull/132))
    - `EmptyRouter`
    - `ExtractorMiddleware`
    - `ExtractorMiddlewareLayer`
    - `QueryStringMissing`
- `RequestParts` changes ([#153](https://github.com/tokio-rs/axum/pull/153))
    - `method` new returns an `&http::Method`
    - `method_mut` new returns an `&mut http::Method`
    - `take_method` has been removed
    - `uri` new returns an `&http::Uri`
    - `uri_mut` new returns an `&mut http::Uri`
    - `take_uri` has been removed
- These rejections have been removed as they're no longer used
    - `MethodAlreadyExtracted` ([#153](https://github.com/tokio-rs/axum/pull/153))
    - `UriAlreadyExtracted` ([#153](https://github.com/tokio-rs/axum/pull/153))
    - `VersionAlreadyExtracted` ([#153](https://github.com/tokio-rs/axum/pull/153))
    - `UrlParamsRejection`
    - `InvalidUrlParam`
- The following services have new response future types:
    - `service::OnMethod`
    - `handler::OnMethod`
    - `routing::Nested`
- Remove `axum::sse` ([#98](https://github.com/tokio-rs/axum/pull/98))

# 0.1.3 (06. August, 2021)

- Fix stripping prefix when nesting services at `/` ([#91](https://github.com/tokio-rs/axum/pull/91))
- Add support for WebSocket protocol negotiation ([#83](https://github.com/tokio-rs/axum/pull/83))
- Use `pin-project-lite` instead of `pin-project` ([#95](https://github.com/tokio-rs/axum/pull/95))
- Re-export `http` crate and `hyper::Server` ([#110](https://github.com/tokio-rs/axum/pull/110))
- Fix `Query` and `Form` extractors giving bad request error when query string is empty. ([#117](https://github.com/tokio-rs/axum/pull/117))
- Add `Path` extractor. ([#124](https://github.com/tokio-rs/axum/pull/124))
- Fixed the implementation of `IntoResponse` of `(HeaderMap, T)` and `(StatusCode, HeaderMap, T)` would ignore headers from `T` ([#137](https://github.com/tokio-rs/axum/pull/137))
- Deprecate `extract::UrlParams` and `extract::UrlParamsMap`. Use `extract::Path` instead ([#138](https://github.com/tokio-rs/axum/pull/138))

# 0.1.2 (01. August, 2021)

- Implement `Stream` for `WebSocket` ([#52](https://github.com/tokio-rs/axum/pull/52))
- Implement `Sink` for `WebSocket` ([#52](https://github.com/tokio-rs/axum/pull/52))
- Implement `Deref` most extractors ([#56](https://github.com/tokio-rs/axum/pull/56))
- Return `405 Method Not Allowed` for unsupported method for route ([#63](https://github.com/tokio-rs/axum/pull/63))
- Add extractor for remote connection info ([#55](https://github.com/tokio-rs/axum/pull/55))
- Improve error message of `MissingExtension` rejections ([#72](https://github.com/tokio-rs/axum/pull/72))
- Improve documentation for routing ([#71](https://github.com/tokio-rs/axum/pull/71))
- Clarify required response body type when routing to `tower::Service`s ([#69](https://github.com/tokio-rs/axum/pull/69))
- Add `axum::body::box_body` to converting an `http_body::Body` to `axum::body::BoxBody` ([#69](https://github.com/tokio-rs/axum/pull/69))
- Add `axum::sse` for Server-Sent Events ([#75](https://github.com/tokio-rs/axum/pull/75))
- Mention required dependencies in docs ([#77](https://github.com/tokio-rs/axum/pull/77))
- Fix WebSockets failing on Firefox ([#76](https://github.com/tokio-rs/axum/pull/76))

# 0.1.1 (30. July, 2021)

- Misc readme fixes.

# 0.1.0 (30. July, 2021)

- Initial release.
