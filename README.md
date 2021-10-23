# Clojure-Examples
Clojure code examples

## Regex 

```clojure
(ns regex.core
  (:require [clojure.string :as str]))

(def words
  (->
   (slurp "resources/words.txt")
   (str/split #",\s")
   set))


(defn -main []

  (doseq [e words]
    (println e))
  (println words))
```

Reads and prints comma-separated words

```clojure
(ns regex.core)

(def data (slurp "resources/thermopylae.txt"))

(defn -main []

  (let [wfreq  (->> data
                    (re-seq #"\w+")
                    frequencies
                    (sort-by val)
                    reverse)]
    (doseq [e wfreq] (println e))))
```
Calculate word frequency
