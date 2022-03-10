# Emails 


# HTML content

```clojure
(ns html.core
  (:import java.util.Properties)
  (:import (jakarta.mail.internet MimeMessage InternetAddress))
  (:import (jakarta.mail Transport
                         Session
                         Authenticator
                         Message$RecipientType
                         PasswordAuthentication)))

(def user "9c1d45eaf7af5b")
(def password "ad62926fa75d0f")

(def props (doto (Properties.)
             (.putAll {"mail.smtp.user" user "mail.smtp.host" "smtp.mailtrap.io"
                       "mail.smtp.port" 2525 "mail.smtp.auth", "true"})))

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
