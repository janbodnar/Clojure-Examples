 # Loops
 
 # Basic loops

 ```clojure
 (ns basics.core)

(defn -main []
  ; calculates sum 1..10
  (println (loop [sum 0 i 0]
             (if (> i 10) sum
                 (recur (+ i sum) (inc i)))))
  ; creates vector 0..9
  (println (loop [acc [] i 0]
             (if (= i 10)
               acc
               (recur (conj acc i) (inc i))))))
 ```

 ## dotimes

```clojure
(ns basics.core)

(defn -main []

  (dotimes [_ 5] (println "falcon")))
```

 ## Loops with steps
 
 ```clojure
 (ns basics.core)

(defn -main []
  ; print numbers 0..10 with step 2
  (loop [i 0]
    (println i)
    (when (< i 10)
      (recur (+ 2 i))))
  
  (println "----------------------")

  ; print numbers 0..20 with step 3
  (doseq [i (range 0 20 3)]
    (println i)))
```
 
 ## Nested loops
 
 ```clojure
 (ns basics.core)

(defn -main []

  (doseq [i (range 5), j (range (inc i))]
    (print "*")
    (when (= i j) (println))))
```