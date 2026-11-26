<!--

@license Apache-2.0

Copyright (c) 2026 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# incrnanmpe

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Compute the [mean percentage error][mean-percentage-error] (MPE) incrementally, ignoring `NaN` values.

<section class="intro">

The [mean percentage error][mean-percentage-error] is defined as

<!-- <equation class="equation" label="eq:mean_percentage_error" align="center" raw="\operatorname{MPE}  = \frac{100}{n} \sum_{i=0}^{n-1} \frac{a_i - f_i}{a_i}" alt="Equation for the mean percentage error."> -->

```math
\mathop{\mathrm{MPE}}  = \frac{100}{n} \sum_{i=0}^{n-1} \frac{a_i - f_i}{a_i}
```

<!-- <div class="equation" align="center" data-raw-text="\operatorname{MPE}  = \frac{100}{n} \sum_{i=0}^{n-1} \frac{a_i - f_i}{a_i}" data-equation="eq:mean_percentage_error">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@2acedf866c9a4f1353af22f95780535612c5ee06/lib/node_modules/@stdlib/stats/incr/nanmpe/docs/img/equation_mean_percentage_error.svg" alt="Equation for the mean percentage error.">
    <br>
</div> -->

<!-- </equation> -->

where `f_i` is the forecast value and `a_i` is the actual value.

</section>

<!-- /.intro -->



<section class="usage">

## Usage

To use in Observable,

```javascript
incrnanmpe = require( 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-incr-nanmpe@umd/browser.js' )
```

To vendor stdlib functionality and avoid installing dependency trees for Node.js, you can use the UMD server build:

```javascript
var incrnanmpe = require( 'path/to/vendor/umd/stats-incr-nanmpe/index.js' )
```

To include the bundle in a webpage,

```html
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/stats-incr-nanmpe@umd/browser.js"></script>
```

If no recognized module system is present, access bundle contents via the global scope:

```html
<script type="text/javascript">
(function () {
    window.incrnanmpe;
})();
</script>
```

#### incrnanmpe()

Returns an accumulator `function` which incrementally computes the [mean percentage error][mean-percentage-error], ignoring `NaN` values.

```javascript
var accumulator = incrnanmpe();
```

#### accumulator( \[f, a] )

If provided input values `f` and `a`, the accumulator function returns an updated [mean percentage error][mean-percentage-error]. If not provided input values `f` and `a`, the accumulator function returns the current [mean percentage error][mean-percentage-error].

```javascript
var accumulator = incrnanmpe();

var m = accumulator( 2.0, 3.0 );
// returns ~33.33

m = accumulator( NaN, 4.0 );
// returns ~33.33

m = accumulator( 1.0, 4.0 );
// returns ~54.17

m = accumulator( 3.0, NaN );
// returns ~54.17

m = accumulator( 3.0, 5.0 );
// returns ~49.44

m = accumulator();
// returns ~49.44
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   Input values are **not** type checked. If provided `NaN`, the value is ignored and the accumulator function returns the current [mean percentage error][mean-percentage-error]. If non-numeric inputs are possible, you are advised to type check and handle accordingly **before** passing the value to the accumulator function.
-   If either `f` or `a` is `NaN`, the input value pair is **skipped** (i.e., the pair does not contribute to the accumulated value).
-   Be careful when interpreting the [mean percentage error][mean-percentage-error] as errors can cancel. This stated, that errors can cancel makes the [mean percentage error][mean-percentage-error] suitable for measuring the bias in forecasts.
-   **Warning**: the [mean percentage error][mean-percentage-error] is **not** suitable for intermittent demand patterns (i.e., when `a_i` is `0`). Interpretation is most straightforward when actual and forecast values are positive valued (e.g., number of widgets sold).

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```html
<!DOCTYPE html>
<html lang="en">
<body>
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/random-base-bernoulli@umd/browser.js"></script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/random-base-uniform@umd/browser.js"></script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/stats-incr-nanmpe@umd/browser.js"></script>
<script type="text/javascript">
(function () {

// Initialize an accumulator:
var accumulator = incrnanmpe();

// For each simulated datum, update the mean percentage error...
var v1;
var v2;
var i;
for ( i = 0; i < 100; i++ ) {
    v1 = ( bernoulli( 0.8 ) < 1 ) ? NaN : uniform( -50.0, 50.0 );
    v2 = ( bernoulli( 0.8 ) < 1 ) ? NaN : uniform( -50.0, 50.0 );
    accumulator( v1, v2 );
}
console.log( accumulator() );

})();
</script>
</body>
</html>
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2026. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/stats-incr-nanmpe.svg
[npm-url]: https://npmjs.org/package/@stdlib/stats-incr-nanmpe

[test-image]: https://github.com/stdlib-js/stats-incr-nanmpe/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/stats-incr-nanmpe/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/stats-incr-nanmpe/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/stats-incr-nanmpe?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/stats-incr-nanmpe.svg
[dependencies-url]: https://david-dm.org/stdlib-js/stats-incr-nanmpe/main

-->

[chat-image]: https://img.shields.io/badge/zulip-join_chat-brightgreen.svg
[chat-url]: https://stdlib.zulipchat.com

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/stats-incr-nanmpe/tree/deno
[deno-readme]: https://github.com/stdlib-js/stats-incr-nanmpe/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/stats-incr-nanmpe/tree/umd
[umd-readme]: https://github.com/stdlib-js/stats-incr-nanmpe/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/stats-incr-nanmpe/tree/esm
[esm-readme]: https://github.com/stdlib-js/stats-incr-nanmpe/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/stats-incr-nanmpe/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/stats-incr-nanmpe/main/LICENSE

[mean-percentage-error]: https://en.wikipedia.org/wiki/Mean_percentage_error

</section>

<!-- /.links -->
