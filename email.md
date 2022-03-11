# Emails 

## Simple text mail

```clojure
(ns first.core
  (:import java.util.Properties)
  (:import (jakarta.mail.internet MimeMessage InternetAddress))
  (:import (jakarta.mail Transport
                         Session
                         Authenticator
                         Message$RecipientType
                         PasswordAuthentication)))

(def user "username")
(def password "password")
(def port 2525)
(def host "smtp.mailtrap.io")

(def props (doto (Properties.)
             (.putAll {"mail.smtp.user" user "mail.smtp.host" host
                       "mail.smtp.port" port "mail.smtp.auth", "true"})))

(def auth (proxy [Authenticator] []
            (getPasswordAuthentication []
              (PasswordAuthentication. user password))))

(def session (Session/getInstance props auth))
(def msg (MimeMessage. session))


(defn -main
  []

  (.setFrom msg (InternetAddress. "john.doe@example.com"))
  (.addRecipient msg Message$RecipientType/TO (InternetAddress. "roger.roe@example.com"))

  (.setSubject msg "Mail subject")
  (.setText msg "Mail body")

  (Transport/send msg))
```

## HTML content

```clojure
(ns html.core
  (:import java.util.Properties)
  (:import (jakarta.mail.internet MimeMessage InternetAddress))
  (:import (jakarta.mail Transport
                         Session
                         Authenticator
                         Message$RecipientType
                         PasswordAuthentication)))

(def user "username")
(def password "password")
(def port 2525)
(def host "smtp.mailtrap.io")

(def props (doto (Properties.)
             (.putAll {"mail.smtp.user" user "mail.smtp.host" host
                       "mail.smtp.port" port "mail.smtp.auth", "true"})))

(def auth (proxy [Authenticator] []
            (getPasswordAuthentication []
              (PasswordAuthentication. user password))))

(def session (Session/getInstance props auth))
(def msg (MimeMessage. session))


(defn -main
  []

  (.setFrom msg (InternetAddress. "john.doe@example.com"))
  (.addRecipient msg Message$RecipientType/TO (InternetAddress. "roger.roe@example.com"))


  (.setSubject msg "Mail subject")
  (.setText msg "Mail body")
  (.setContent msg "<p>an <em>old falcon</em> in the sky</p>" "text/html; charset=utf-8")
  (Transport/send msg))
```

## POP3 example

```clojure
(ns pop3.core
  (:import (jakarta.mail Session Folder)))

(def user "username")
(def password "password")
(def host "pop3.mailtrap.io")
(def port 9950)

(def props (System/getProperties))

(def session (Session/getDefaultInstance props))
(def store (.getStore session "pop3"))

(.connect store host port user password)
(def inbox (.getFolder store "Inbox"))
(.open inbox Folder/READ_ONLY)
(def messages (.getMessages inbox))


(defn -main []

  (println (count messages))

  (doseq [m messages] (let  [subject (.getSubject m) body (slurp (.getInputStream m))]
                        (println subject)
                        (println body)
                        (prn "---------------------------")))

  (.close inbox true)
  (.close store))
```




