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