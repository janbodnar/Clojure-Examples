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
