# Richie

Read new mails from an IMAP mailbox and forward them to a SMTP server. Uses IMAP IDLE instead of polling. Forwarded mails are marked as read and optionally deleted.

# Requirements

* Ruby 2.1+
* IMAP server must support SSL (Port 993)
* SMTP server must support STARTTLS (Port 25)

# Example config file (richie.json)

    {
      "imap": {
        "me@gmx.net": {"server": "imap.gmx.net", "user": "me@gmx.net", "password": "my_secret_password"}, /* Reads mails from INBOX, marks delivered mails as read */
        "me@example.com": {"server": "mail.example.com", "user": "me@example.com", "password": "password", "folders": ["INBOX", "Spam"] }, /* Reads mails from INBOX and Spam folder */
        "me-again": {"server": "imap.gmx.net", "user": "also-me@gmx.com", "password": "another_password", "keep": false} /* Delete mails after delivery */
      },
      "smtp": {
        "myself": {"server": "localhost", "to": "mylocaluser"},
        "a-friend": {"server": "mail.example.org", "to": "friend@example.com", "user": "myuser", "password": "secret"}
      },
      "deliveries": {
        "me@gmx.net": ["myself"],
        "me@example.com": ["myself"],
        "me-again": ["a-friend", "myself"]
      }
    }

# Usage

    $> ./richie -v -c richie.json


# Installation as a service

    #> adduser --system --no-create-home richie

    #> mkdir /opt/richie
    #> cp richie richie.json /opt/richie
    #> chown richie /opt/richie/richie.json
    #> chmod 0600 /opt/richie/richie.json

    #> cp richie.service /etc/systemd/system
    #> systemctl daemon-reload
    #> service richie start
