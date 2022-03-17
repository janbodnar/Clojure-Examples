# Conditions

## if/else 

```clojure
(ns basics.core)

(defn -main []

  (let [x 2]
    (if (> x 0) (println "x is positive") (println "x is negative"))))
 ```

## Single conditions with when

```clojure
(ns basics.core)

(def r (+ -5 (rand-int 10)))

(defn -main []

  (println r)

  (when (< r 0)
    (println "the value is negative"))

  (when (> r 0)
    (println "the value is positive"))

  (when (== r 0)
    (println "the value is zero")))
```

## Multiple conditions with cond

```clojure
(ns basics.core)

(def r (+ -5 (rand-int 10)))

(defn -main []

  (println (do (println r)
               (cond
                 (< r 0) "the value is negative"
                 (> r 0) "the value is positive"
                 (== r 0) "the value is zero"))))

```