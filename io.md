# IO

## Create new file

```clojure
(ns strings.core 
  (:require [clojure.java.io :as io]))

(import '(java.io File))

(defn -main []

  (.createNewFile (io/file "resources/words.txt"))
  (.createNewFile (File. "resources/words2.txt")))
```

## Rename file 

```clojure
(ns basics.core
  (:require [clojure.java.io :as io]))

(defn -main []
  (.renameTo (io/file "resources/words.txt") (io/file "resources/words2.txt")))
```

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

## Read nth line

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

## Copy file

```clojure
(ns strings.core
  (:require [clojure.java.io :as io]))

(defn -main []

  (io/copy (io/file "resources/words.txt") (io/file "resources/words2.txt")))
```

## Write to file 

```clojure
(ns basics.core)

(defn -main []

  (spit "resources/words2.txt" "sky\nblue\nrock\ncloud"))
```

## Append to file

```clojure
(ns basics.core
  (:require [clojure.java.io :as io]))

(defn -main []

    (with-open [wr (io/writer "resources/words.txt" :append true)]
      (.write wr "book")))
```
