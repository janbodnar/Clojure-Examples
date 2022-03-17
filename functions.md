# Functions


## defn is def & fn

The `defn` form is an abbreviation of `def` & `fn` forms.  

```clojure
(ns basics.core)

(defn greet [name] (format "Hello %s!" name))
(def greet2 (fn [name] (format "Hello %s!" name)))

(defn -main []

  (println (greet "Peter"))
  (println (greet2 "Paul")))
```

## Anonymous functions

Anonymous functions are created with `fn` or `#()`.

This latter is a short syntax which omits the parameter list and  
names parameters based on their position.  

- % is used for a single parameter
- %1, %2, %3, etc are used for multiple parameters
- %& is used for any remaining (variadic) parameters



## Variable # of arguments

Variable number of arguments are created with &.

```clojure
(ns basics.core)

(defn f [x y z & nums]
  (printf "x: %d, y: %d; z: %d; nums: %s\n" x y z nums))

(defn -main []
  (f 1 2 3 4 5 6))
```

## Multi-variadic functions

Functions can take various numbers of arguments.

```clojure
(ns basics.core)

(defn hello
  ([] "Hello there!")
  ([name] (format "Hello %s!" name))
  ([firstname lastname] (format "Hello %s %s!" firstname lastname)))

(defn -main []

  (println (hello "John"))
  (println (hello "John" "Doe"))
  (println (hello)))
```
