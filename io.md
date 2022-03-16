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
