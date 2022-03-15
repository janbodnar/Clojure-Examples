# Functions

## Variable # of arguments

```clojure
(ns basics.core)

(defn f [x y z & nums]
  (printf "x: %d, y: %d; z: %d; nums: %s\n" x y z nums))

(defn -main []
  (f 1 2 3 4 5 6))
```
