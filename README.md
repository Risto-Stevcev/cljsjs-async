# cljsjs/async

[![Clojars](https://img.shields.io/clojars/v/cljsjs/async.svg?maxAge=2592000)](https://clojars.org/cljsjs/async)

Higher-order functions and common patterns for asynchronous code.

After adding the dependency to your project you can require the packaged library like so:

```clojure
(ns application.core
  (:require cljsjs.async))
```

## Examples

#### async.parallel

```clojure
(def myatom (atom []))

(js/async.parallel
  (clj->js [(fn [callback]
              (js/setTimeout 
                (fn [] 
                  (swap! myatom conj "one")
                  (callback nil "one")) 200))
            (fn [callback]
              (js/setTimeout 
                (fn [] 
                  (swap! myatom conj "two")
                  (callback nil "two")) 100))])
  (fn [error response]
    (println response) ; #js [one two]
    (println @myatom)  ; [two one]
    ))
```


#### async.series

```clojure
(def myatom (atom []))

(js/async.series
  (clj->js [(fn [callback]
              (js/setTimeout 
                (fn [] 
                  (swap! myatom conj "one")
                  (callback nil "one")) 200))
            (fn [callback]
              (js/setTimeout 
                (fn [] 
                  (swap! myatom conj "two")
                  (callback nil "two")) 100))])
  (fn [error response]
    (println response) ; #js [one two]
    (println @myatom)  ; [one two]
    ))
```
