# List comprehensions

List comprehensions are created with for.  

# Simple

```clojure
(ns basics.core)

(defn -main []

  (let [odds (for [x (range 10) :when (odd? x)] x)
        evens (for [x (range 10) :when (even? x)] x)
        all (concat evens odds)]

    (println evens)
    (println odds)
    (println (sort all))))
```

---

```clojure
(ns basics.core)

(defn -main []

  (let [data (for [e1 [\a \b \c]
                   e2 [\x \y \z]]
               (str e1 e2))]
    (println data)))
```

# :let and :when

```clojure
(ns basics.core)

(defn -main []

  (let [nums (for [x [0 1 2 3 4 5]
                   :let [y (+ x 3)]
                   :when (even? y)]
               y)]

    (println nums)))
```


## Nested for

```clojure
(ns basics.core)

(defn -main []

  (let [data (for [i (range 3)]
               (for [j (range 3)]
                 [i j]))]

    (println data)))
```


# :when and :while

```clojure
(ns basics.core)

(defn -main []
  (println (for [x (range 3)
                 y (range 3)
                 :when (not= (+ x y) 2)]
             [x y]))

  (println (for [x (range 3)
                 y (range 3)
                 :while (not= (+ x y) 2)]
             [x y])))
```