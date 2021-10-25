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
  ;; (:require [clj-bean.core :refer :all])
  (:import
   (net.sf.jasperreports.engine JasperCompileManager
                                JasperFillManager
                                JasperExportManager)
   (net.sf.jasperreports.engine.data JRBeanCollectionDataSource)))
(import java.util.HashMap)
;; (import 'java.util.ArrayList)
;; (import 'clj-bean.core)


;; (defrecord Car [^Long id ^String name ^Integer price])
;; (defrecord Car [id name price])

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
  ;; :aot jasper.core/Car
  :main jasper.core/-main)
```
The `com.wjoel/clj-bean` library is needed for creating JavaBean objects that are required by  
JasperReports.  

