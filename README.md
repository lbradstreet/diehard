# diehard

[![Build
Status](https://travis-ci.org/sunng87/diehard.svg?branch=master)](https://travis-ci.org/sunng87/diehard)
[![Clojars](https://img.shields.io/clojars/v/diehard.svg?maxAge=2592000)](https://clojars.org/diehard)
[![license](https://img.shields.io/github/license/sunng87/diehard.svg?maxAge=2592000)]()

Clojure library to provide safety guard to your application.
Some of the functionality is wrapper over
[Failsafe](https://github.com/jhalterman/failsafe).

## Usage

A quick example for diehard usage.

### Retry block

A retry block will re-execute inner forms when retry criteria matches.

```clojure
(require '[diehard.core :as dh])
(dh/with-retry {:retry-on TimeoutException
                :max-retries 3}
  (fetch-data-from-the-moon))
```

### Circuit breaker

A circuit breaker will track the execution of inner block and skip
execution if the open condition triggered.

```clojure
(require '[diehard.core :as dh])

(defcircuitbreaker my-cb {:failure-threshold-ratio [8 10]
                          :delay-ms 1000})

(dh/with-circuit-breaker my-cb
  (fetch-data-from-the-moon))
```

### Rate limiter

A rate limiter protects your code block to run limited times per
second. It will block or throw exception depends on your
configuration.

```clojure
(require '[diehard.core :as dh])

(defratelimiter my-rl {:rate 100})

(dh/with-rate-limiter my-rl
  (send-people-to-the-moon))
```

## Docs

More options can be found in the documentation
[here](https://sunng87.github.io/diehard).

## License

Copyright © 2016-2017 Ning Sun

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
