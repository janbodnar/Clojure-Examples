# Sortign


# Integers & words

```clojure
(ns basics.core)

(def nums [7 9 3 -2 8 1 0])
(def words ["sky" "cloud" "atom" "brown" "den" "kite" "town"])

(defn -main []

  (println (sort nums))
  (println (sort < nums))
  (println (sort > nums))

  (println "---------------------------------")

  (println (sort words))
  (println (reverse (sort words))))
```

## Sort ints lex. & words by length

```clojure 
(ns basics.core)

(def nums [2 22 12 7 32 9 3 13 31 -12 8 10 11])
(def words ["sky" "cloud" "by" "atom" "brown" "a" "den" "kite" "town"])

(defn -main []

  (println (sort-by str nums))
  (println (sort-by count words)))
```

## Sorting with sort-by


```clojure
(ns basics.core)

(def nums [[1 3] [4 3] [3 0] [4 0] [0 1] [0 3] [2 2] [0 0] [1 1] [3 3]])
(def names [{:fname "John" :lname "Doe"} {:fname "Roger" :lname "Roe"}
            {:fname "Bill" :lname "Newton"} {:fname "Arnold" :lname "Cruger"}
            {:fname "Roman" :lname "Smith"} {:fname "Thomas" :lname "Manner"}
            {:fname "Jo" :lname "Rogan"}])

(defn -main []

  (println (sort-by first nums))
  (println (sort-by second nums))

  (println "----------------------------------")

  (println (sort-by :fname names))
  (println (sort-by (comp count :fname) names))

  (println "----------------------------------")

  (println (sort-by :lname names))
  (println (sort-by (comp count :lname) names)))
```

## Custom comparators

```clojure
(ns basics.core)

(def nums [2 1 7 0 9 3 13 31 -1 8 -4 10 11])
(def words ["sky" "cloud" "by" "atom" "brown" "a" "den" "kite" "town"])

(defn desc [a b]
  (compare b a))

(defn bylendesc [a b]
  (compare (count b) (count a)))

(defn -main []

  (println (sort desc nums))
  (println (sort desc words))
  (println (sort bylendesc words)))
```