# IO

## Read file line by line

```clojure
(ns readfile.core
  (:require [clojure.java.io :as io]))

(defn readfile []
  (with-open [rdr (io/reader "resources/words.txt")]
    (doseq [line (line-seq rdr)]
      (println line))))

(defn -main []
  (readfile))
```

## Read whole file

Reads the whole file in one go with `slurp`.  

```clojure
(ns slurp.main)

(defn -main
  []
  (println (slurp "http://webcode.me")))
```

# Read nth line

```clojure
(ns basics.core
  (:require [clojure.java.io :as io]))

(defn nth-line
  [fname n]
  (with-open [rdr (io/reader fname)]
    (nth (line-seq rdr) (dec n))))

(defn -main []

  (println (nth-line "resources/words.txt" 4))
  (println (nth-line "resources/words.txt" 1))
  (println (nth-line "resources/words.txt" 2)))
```  
