# HTTP Client


## Dependencies 

```
:dependencies [[org.clojure/clojure "1.10.3"]
               [clj-http "3.12.3"]]
```

## HEAD request

```clojure
(ns headreq.main
  (:require [clj-http.client :as client]))

(def resp (client/head "http://webcode.me"))

(defn -main
  []
  (println resp)
  (prn "----------------------------")
  (doseq [[k v] resp] (prn k v))
  (prn "----------------------------")
  (prn (get resp :headers))
  )
 ```


## GET request

```clojure
(ns getreq.main
  (:require [clj-http.client :as client]))

(def resp (client/get "http://webcode.me"))

(defn -main
  []
  (println (get resp :body)))  
```

## POST request

```clojure
(ns postreq.main
  (:require [clj-http.client :as client]))

(def url "https://httpbin.org/post")
(def data {:name "John Doe" :occupation "gardener"})
(def resp (client/post url {:form-params data}))

(defn -main
  []
  (println (get resp :body)))
```

## Query string

```cloure
(ns query_str.main
  (:require [clj-http.client :as client]))

(def url "http://webcode.me/qs.php")
(def data {"name" "John Doe" "occupation" "gardener"})
(def resp (client/get url {:accept :json :query-params data}))

(defn -main
  []
  (println (get resp :body)))
```

## User agent 

```clojure
(ns user_agent.main
  (:require [clj-http.client :as client]))

(def url "http://webcode.me/ua.php")
(def resp (client/get url {:headers {"User-Agent" "Clojure program"}}))

(defn -main
  []
  (println (get resp :body)))
 ```

## Download image

```clojure
(ns downimg.main
  (:require [clj-http.client :as client])
  (:require [clojure.java.io :as io]))

(def url "http://webcode.me/favicon.ico")

(defn -main
  []

  (let [data (client/get url {:as :byte-array :throw-exceptions false})]
    (io/copy (get data :body) (io/file "favicon.ico"))))
```

