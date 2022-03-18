# Sorting


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

## Sort words ci

```clojure
(ns basics.core
  (:require [clojure.string :as str]))

(def words ["sky" "Sun" "Albert" "cloud" "by" "Earth" "else"
            "atom" "brown" "a" "den" "kite" "town"])

(defn -main []

  (println (sort words))
  (println (sort-by (comp str/lower-case) words)))
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

---

Compare first by lenght, if elements have same length then    
alphabetically/ci.  

```clojure
(ns basics.core)

(defn cmp [s1 s2]
  (let [len1 (count s1), len2 (count s2)]
    (if (= len1 len2)
      (compare (.toLowerCase s1) (.toLowerCase s2))
      (- len2 len1))))

(def words ["sky" "Sun" "Albert" "cloud" "by" "Earth" "else"
            "atom" "brown" "a" "den" "kite" "town"])

(defn -main []

  (println (sort cmp words)))
```
## Sort by multiple fields

```clojure
(ns basics.core)

(def users [{:fname "Robin" :lname  "Brown" :salary 2300}
            {:fname "Janet" :lname  "Doe" :salary 980}
            {:fname "John" :lname  "Doe" :salary 1230}
            {:fname "Vivien" :lname  "Doe" :salary 1010}
            {:fname "Amy" :lname  "Doe" :salary 1250}
            {:fname "Joe" :lname  "Draker" :salary 1190}
            {:fname "Lucy" :lname  "Novak" :salary 670}
            {:fname "Albert" :lname  "Novak" :salary 1930}
            {:fname "Ken" :lname  "Novak" :salary 2990}
            {:fname "Ben" :lname  "Walter" :salary 2050}])

(defn -main []
  ; sort by last name asc & salary asc
  (doseq [u (sort-by (juxt :lname :salary) users)] (println u))
  (println "-------------------------------")
  ; sort by last name asc & salary desc
  (doseq [u (sort-by (juxt :lname (comp - :salary)) users)] (println u)))
```

