# Richie

Read new mails from an IMAP mailbox and forward them to a SMTP server. Uses IMAP IDLE instead of polling. Forwarded mails are marked as read and optionally deleted.

# Requirements

* Ruby 2.1+
* IMAP server must support SSL (Port 993)
* SMTP server must support STARTTLS (Port 25)

# Example config file (richie.json)

    {
      "imap": {
        "gmx-inbox": {"server": "imap.gmx.net", "user": "me@gmx.net", "password": "my_secret_password"}, /* Fetches mails from INBOX, marks delivered mails as read */
        "gmx-spam": {"server": "imap.gmx.net", "user": "me@gmx.net", "password": "my_secret_password", "folder": "Spam", "keep": false} /* Fetches mails from Spam folder, deletes them after delivery */
      },
      "smtp": {
        "myself": {"server": "localhost", "to": "mylocaluser"},
        "spam-collector-via-example-org": {"server": "mail.example.org", "user": "myuser", "password": "secret", "to": "incoming@spam-collector.com"}
      },
      "deliveries": {
        "gmx-inbox": ["myself"],
        "gmx-spam": ["myself", "spam-collector-via-example-org"]
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
