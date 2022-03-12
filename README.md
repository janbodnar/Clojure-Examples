# Clojure-Examples
Clojure code examples

## if/else 

```clojure
(ns basics.core)

(defn -main []

  (let [x 2]
    (if (> x 0) (prn "x is positive") (prn "x is negative"))))
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

