# Clojure-Examples
Clojure code examples

## Read console input

```clojure
(ns basics.core)

(defn -main []

  (print "Enter your name: ")
  (flush)
  (let [name (read-line)]
    (println (format "Hello %s!" name))))
```

## Command line arguments

```clojure
(ns basics.core)

(def args2 *command-line-args*)

(defn -main [& args]

  (println args2)
  (println args)

  (println (first args))
  (println (second args))
  (println (last args))
  (println (butlast args))

  (doseq [e args] (println e)))
```

## Format string

```clojure
(ns basics.core)

(defn -main
  []
  (let [name "John Doe" occupation "gardener"]

    (println (format "%s is a %s" name occupation))))
```

## Ranges

```clojure
(ns basics.core)

(defn -main []
  
  (println (range 10))
  (println (range 10 20))
  (println (range 1 20 3))
  (println (range 10 0 -1))

  (println "------------------------")
  
  (println (partition 5 (range 1 20)))
  (println (reduce + (range 5 15)))

  (println (map char (range (int \a) (int \z)))))
 ```

## Get webpage

```clojure
(ns format.core)

(defn -main
  []

  (let [url "http://webcode.me"]

    (println (slurp url))))
```

## Get title of webpage

```clojure
(ns get-title.core)

(defn -main []

  (let [url "http://webcode.me"
        content (slurp url)]

    (println (re-find #"<title>.*</title>" content))
    (println (second (re-find #"<title>(.*)</title>" content)))))
```

## Download image

```clojure
(ns basics.core
  (:require [clojure.java.io :as io]))

(defn -main
  []
  (let [url "http://webcode.me/favicon.ico" file "favicon.ico"]
  (with-open [in (io/input-stream url)
              out (io/output-stream file)]
    (io/copy in out))))
```


## Getting elements of a vector

```clojure
(ns basics.core)

(def nums [1 2 3 4 5 6 7 8])

(defn -main []

  (println (first nums))
  (println (second nums))
  (println (nth nums 5))
  (println (last nums))
  (println (butlast nums))
  (println (reverse nums))
  (println (first (reverse nums)))
  (println (last (butlast nums)))
  (println (take-last 4 nums))
  (println (take-last 1 nums))
  (println (take-last 0 nums))
  (println (drop-last 3 nums)))
```

## dotimes

iteration with side effects  
Evaluate expression `n` times; returns `nil`  

```clojure
(ns basics.core)

(defn -main []

  (dotimes [_ 5] (println "falcon"))
  (dotimes [x 5] (println (* x x))))
```

## doseq

- iteration with side effects
- iterates over a sequence
- if a lazy sequence, forces evaluation
- returns nil

```clojure
(ns basics.core)

(def nums [1 2 3 4 5])

(defn -main []

  (doseq [n nums]
    (println n))

  (doseq [w '("falcon" "falcon" "falcon" "falcon")]
    (println w))

  (doseq [n (range 10)]
    (println n)))
```

---

```clojure
(ns basics.core)

(def ws ["a" "b" "c"])
(def vs [1 2 3])

(defn -main []

  (doseq [e1 ws e2 vs]
    (println [e1 e2])))
```

## get random element

```clojure
(ns basics.core (:gen-class))

(def nums '(1, 2, 3, 4, 5, 6))

(defn -main []

  (println (rand-nth nums))
  )
```

## repeat/repeatedly

```clojure
(ns basics.core)

(defn -main []
   ; repeats 5 times a random int
  (println (repeat 5 (rand-int 100)))
   ; produces 5 random ints
  (println (repeatedly 5 #(rand-int 100))))
```

## do statement

`do` combines single expressions. Functions and let have implicit  
do statements.

```clojure
(ns basics.core)

(def n 5)

(defn -main []

  (let [r (if (even? n)
            (do (println "even")
                true)
            (do (println "odd")
                false))]
    (println r)))
```
## Iterate over runes

```clojure
(ns basics.core)

(defn -main
  []

  (doseq [e (re-seq #"."  "🐜🐬🐄🐘🦂🐫🐑🦍🐯🐞")]
    (println e)))
```

---

```clojure
(ns basics.core)

(import java.text.BreakIterator)

(defn tokenize [text]
  (let [bi (BreakIterator/getCharacterInstance)]
    (.setText bi text)
    (loop [b (.first bi) toks []]
      (let [e (.next bi)]
        (if (= e (BreakIterator/DONE))
          toks
          (recur e (conj toks (subs text b e))))))))

(defn -main
  []
  (doseq [e (tokenize "🐜🐬🐄🐘🦂🐫🐑🦍🐯🐞")]
    (println e)))
```

## inc/dec

```clojure
(ns basics.core)

(def nums '(1 2 3 -4 9 6 7))

(defn -main []

  (prn (inc 3))
  (prn (dec 3))
  (prn (map inc nums))

  (println "-------------------")

  (doseq [x nums]
    (prn (inc x))))
```

## filter

```clojure
(ns basics.core)

(def nums '(-1 2 -3 4 9 7 0 8 11 22 10 -9))

(defn -main []

  (let [x (filter even? nums)]
    (println x))

  (let [x (filter #(< % 0) nums)]
    (println x))

  (let [x (filter (fn [x] (> x 0)) nums)]
    (println x)))
``` 

## Concatenate strings


```clojure
(ns basics.core
  (:require [clojure.string :as string]))

(defn -main []

  (let [w1 "an" w2 "old" w3 "falcon"]
    (println (string/join " " [w1 w2 w3]))
    (printf "%s %s %s\n" w1 w2 w3)
    (println (str w1 " " w2 " " w3))))
```

## Shuffle cards

```clojure
(ns shufflecards.core
  (:require [clojure.string :as str]))

(def cards ["🂡" "🂱" "🃁" "🃑"
            "🂢" "🂲" "🃂" "🃒"
            "🂣" "🂳" "🃃" "🃓"
            "🂤" "🂴" "🃄" "🃔"
            "🂥" "🂵" "🃅" "🃕"
            "🂦" "🂶" "🃆" "🃖"
            "🂧" "🂷" "🃇" "🃗"
            "🂨" "🂸" "🂸" "🂸"
            "🂩" "🂩" "🃉" "🃙"
            "🂪" "🂺" "🃊" "🃚"
            "🂫" "🂻" "🃋" "🃛"
            "🂭" "🂽" "🃍" "🃝"
            "🂮" "🂾" "🃎" "🃞"])

(defn -main []

  (doseq [line (partition 10 (shuffle cards))]

    (println (str/join " " line))))
```

## str vs pr-str

`str` is lazy

```clojure
(ns basics.core)

(defn -main []

  (prn (reduce + (range 10)))
  (prn (str (take 5 (range 10))))
  (prn (pr-str (take 5 (range 10)))))
```  

## and

```clojure
(ns basics.core)

(defn -main []

  (let [x 3 y 4]

    (if (and (> x 0) (> y 0))
      (prn "x & y are positive")
      (prn "it is not true that x & y are positive"))))
``` 

## min/max/count/...

```clojure
(ns basics.core)

(def nums '(1 2 3 -4 9 6 7))

(defn -main []

  (prn (apply min nums))
  (prn (first nums))
  (prn (last nums))
  (prn (apply max nums))
  (prn (reduce + nums))
  (prn (count nums)))
```

## Thread-first/thread-last macros

The thread-first macro takes its argument and inserts it as the first  
argument of the next form, then continues inserting the current form  
as the first argument to the next form, until the end.  

The thread-last macro does the same except it inserts the form as the  
last argument.  

```clojure
(ns basics.core)


(defn -main []

  (let [w1 "old" w2 "falcon"]

    (println (-> w1 (str " " w2)))
    (println (->> w1 (str w2 " ")))))
```

---

```clojure
(ns basics.core)

(defn -main []

  (println (reduce + (map (fn [x] (* x x)) (filter odd? (range 10)))))

  (println (->> (range 10)
                (filter odd?)
                (map (fn [x] (* x x)))
                (reduce +))))
```

Thread macros make the code cleaner & easier to read.  

---

With `macroexpand`, we can see how the macro will be executed.  

```clojure
(ns format.core)

(defn -main
  []

  (println (macroexpand '(-> 4
                             (/ 12)
                             (- 5)
                             (* 3)
                             (+ 5)
                             (- 25))))

  (println (macroexpand '(->> 4
                              (/ 12)
                              (- 5)
                              (* 3)
                              (+ 5)
                              (- 25))))

```


## Lazy seq & doall

```clojure
(ns doallfun.core)

;; Nothing is printed because map returns a lazy-seq
(def x (map println [11 22 33]))

;; doall forces the seq to be realized
(def y (doall (map println [1 2 3])))

(defn -main[]
  (println (type x))
  (println (type y))

  (doall x))
```

## Run programs 

```clojure
(ns format.core)

(require '[clojure.java.shell :as shell])

(defn -main
  []

  (println (:out (shell/sh "ls")))
  (println (:out (shell/sh "cowsay" "Hello there!")))
  (println (:out (shell/sh "pwd")))

  (System/exit 0))
  ```


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

## H2 database example

```clojure
(ns h2ex.core
  (:require [clojure.java.jdbc :as sql]))

(def db
  {:dbtype "h2:mem"
   :dbname "mydb"})

(sql/execute! db ["CREATE TABLE cars(id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(255), price INT)"])
(sql/execute! db ["INSERT INTO cars(name, price) VALUES('Audi', 52642)"])
(sql/execute! db ["INSERT INTO cars(name, price) VALUES('Mercedes', 57127)"])
(sql/execute! db ["INSERT INTO cars(name, price) VALUES('Skoda', 9000)"])
(sql/execute! db ["INSERT INTO cars(name, price) VALUES('Volvo', 29000)"])
(sql/execute! db ["INSERT INTO cars(name, price) VALUES('Bentley', 350000)"])
(sql/execute! db ["INSERT INTO cars(name, price) VALUES('Citroen', 21000)"])
(sql/execute! db ["INSERT INTO cars(name, price) VALUES('Hummer', 41400)"])
(sql/execute! db ["INSERT INTO cars(name, price) VALUES('Volkswagen', 21600)"])


(defn -main []

  (sql/query db ["SELECT * FROM cars"]
             {:row-fn println})

  (prn)

  (println (sql/query db ["SELECT * FROM cars"]
                      {:row-fn :name}))

  (println (sql/query db ["SELECT * FROM cars"]
                      {:row-fn :price
                       :result-set-fn (partial reduce +)}))
  (prn)

  (println (sql/query db ["SELECT SUM(price) as total FROM cars"]
                      {:row-fn :total
                       :result-set-fn first})))
 ```            
 
 ```clojure
 (defproject h2ex "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :url "http://example.com/FIXME"
  :license {:name "EPL-2.0 OR GPL-2.0-or-later WITH Classpath-exception-2.0"
            :url "https://www.eclipse.org/legal/epl-2.0/"}
  :dependencies [[org.clojure/clojure "1.10.0"], 
                 [com.h2database/h2 "1.4.200"],
                 [org.clojure/java.jdbc "0.7.12"]]
  :main "h2ex.core"
  :repl-options {:init-ns h2ex.core})
 ```
 The project file

## JasperReports example

The example generates a simple PDF report with JasperReports library.

```xml
<?xml version = "1.0" encoding = "UTF-8"?>
<!DOCTYPE jasperReport PUBLIC "//JasperReports//DTD Report Design//EN"
        "http://jasperreports.sourceforge.net/dtds/jasperreport.dtd">

<jasperReport xmlns="http://jasperreports.sourceforge.net/jasperreports"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://jasperreports.sourceforge.net/jasperreports
   http://jasperreports.sourceforge.net/xsd/jasperreport.xsd"
              whenNoDataType="NoDataSection"
              name="report" topMargin="20" bottomMargin="20">

    <field name="id" class="java.lang.Long"/>
    <field name="name"/>
    <field name="price" class="java.lang.Integer"/>

    <detail>
        <band height="15">

            <textField>
                <reportElement x="0" y="0" width="50" height="15"/>

                <textElement textAlignment="Right" verticalAlignment="Middle"/>

                <textFieldExpression class="java.lang.Long">
                    <![CDATA[$F{id}]]>
                </textFieldExpression>
            </textField>

            <textField>
                <reportElement x="150" y="0" width="100" height="15" />

                <textElement textAlignment="Left" verticalAlignment="Middle"/>

                <textFieldExpression class="java.lang.String">
                    <![CDATA[$F{name}]]>
                </textFieldExpression>
            </textField>

            <textField>
                <reportElement x="200" y="0" width="100" height="15" />
                <textElement textAlignment="Right" verticalAlignment="Middle"/>

                <textFieldExpression class="java.lang.Integer">
                    <![CDATA[$F{price}]]>
                </textFieldExpression>
            </textField>

        </band>
    </detail>

    <noData>
        <band height="15">
            <staticText>
                <reportElement x="0" y="0" width="200" height="15"/>
                <box>
                    <bottomPen lineWidth="1.0" lineColor="#CCCCCC"/>
                </box>
                <textElement />
                <text><![CDATA[The report has no data]]></text>
            </staticText>
        </band>
    </noData>

</jasperReport>
```
The `report.xml` file

```clojure
(ns jasper.core
  (:require [clj-bean.core :refer [defbean]])
  (:import
   (net.sf.jasperreports.engine JasperCompileManager
                                JasperFillManager
                                JasperExportManager)
   (net.sf.jasperreports.engine.data JRBeanCollectionDataSource)))
(import java.util.HashMap)

(defbean Car
 [[Long id]
  [String name]
  [Integer price]])

(def data [(Car. 1 "Audi" 52642),
           (Car. 2 "Mercedes" 57127),
           (Car. 3 "Skoda" 9000),
           (Car. 4 "Volvo" 29000),
           (Car. 5 "Bentley" 350000),
           (Car. 6 "Citroen" 21000),
           (Car. 7 "Hummer" 41400),
           (Car. 8 "Volkswagen" 21600)])

(def xmlFile "resources/report.xml")
(def jrReport (JasperCompileManager/compileReport xmlFile))

(def params (HashMap.))

(def ds (JRBeanCollectionDataSource. data))
(println (.toString ds))

(def jrPrint (JasperFillManager/fillReport jrReport params ds))

(defn -main
  []
  (JasperExportManager/exportReportToPdfFile jrPrint "report.pdf"))
```
The `jasper.core.clj` file

```clojure
(defproject jasper "0.1.0-SNAPSHOT"
  :description "Jasper simple example"
  :url "http://example.com/FIXME"
  :license {:name "EPL-2.0 OR GPL-2.0-or-later WITH Classpath-exception-2.0"
            :url "https://www.eclipse.org/legal/epl-2.0/"}
  :dependencies [[org.clojure/clojure "1.10.3"], [com.wjoel/clj-bean "0.2.1"],
                 [net.sf.jasperreports/jasperreports "6.17.0"]]
  :repl-options {:init-ns jasper.core}
  :main jasper.core/-main)
```
The `com.wjoel/clj-bean` library is needed for creating JavaBean objects that are required by  
JasperReports.  

