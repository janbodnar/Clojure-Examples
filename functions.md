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

## Variable # of arguments

Variable number of arguments are created with &.

```clojure
(ns basics.core)

(defn f [x y z & nums]
  (printf "x: %d, y: %d; z: %d; nums: %s\n" x y z nums))

(defn -main []
  (f 1 2 3 4 5 6))
```
