# HTTP Client


## Dependencies 

```
  :dependencies [[org.clojure/clojure "1.10.3"]
                 [clj-http "3.12.3"]]
```

## HEAD request

```
(ns headreq.main
  (:require [clj-http.client :as client]))

(def headers (client/head "http://webcode.me"))

(defn -main
  []
  (println headers)
  (prn "----------------------------")
  (doseq [[k v] headers] (prn k v))
  (prn "----------------------------")
  (prn (get headers :headers))
  )
 ```


## GET request

```
(ns getreq.main
  (:require [clj-http.client :as client]))

(def resp (client/get "http://webcode.me"))

(defn -main
  []
  (println (get resp :body)))  
```

## Download image

```
(ns downimg.main
  (:require [clj-http.client :as client])
  (:require [clojure.java.io :as io]))

(def url "http://webcode.me/favicon.ico")

(defn -main
  []

  (let [data (client/get url {:as :byte-array :throw-exceptions false})]
    (io/copy (get data :body) (io/file "favicon.ico"))))
```

