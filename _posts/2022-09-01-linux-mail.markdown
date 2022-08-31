---
title: "[Linux] Mail Service"
date: 2022-09-01 01:57 +0900
categories: [Linux, Network]
tags: [Linux, guide]
---

# Protocols

Protocol | Port | Description
---- | ---- | ----
SMTP(Simple Mail Transfer Protocol) | 25 | Used to send mails
POP3(Post Office Protocol version 3) | 110 | Let mail clients read received mails. Read mails are automatically erased.
IMAP(Internet Mail Access Protocol) | 143 | Read received mails. Does not erase read mails.

# Mail Programs

Category | Description | Programs
--- | --- | ---
MTA(Mail Transfer Agent) | A server program that sends mails using SMTP | `sendmail`, `qmail`, `postfix`, `MS Exchange Server`
MUA(Mail User Agent) | A user program used to read and send mails | `kmail`, `evolution`, `mutt`, `thunderbird`, `MS Outlook`
MDA(Mail Delivery Agent) |  | `procmail`

# Mail Server

This post assumes `sendmail` is used in the mail server.

## Configuration

1. `/etc/mail/sendmail.cf`

2. `/etc/mail/local-host-names`

    Sets the domain names used in the mail server. Specify one domain per line.

    * Examples
    ```
    $ vi /etc/mail/local-host-names
    mail.org
    mail.co.kr
    linxm.com
    ```

3. `/etc/mail/sendmail.mc`

    A configuration macro file for `/etc/mail/sendmail.cf`. Restore configuration with the following command:

    ```
    $ m4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf
    ```

4. `/etc/mail/access`

    Accepts/rejects specified hosts

    ```
    $ vi /etc/mail/access
    From:spam@gmail.com    REJECT   // Reject and send a message
    Connect:192.158.5      DISCARD  // Reject without any messages
    Connect:127.0.0.5      OK       // Allow
    Connect:heejoon.org    RELAY    // Allow access
    ```

    Run the following command to apply changes:
    ```
    $ makemap hash /etc/mail/access < /etc/mail/access
    ```

5. `/etc/aliases`

    Forward mails sent for an accout to other accounts. Used to create a mailing list where a mail sent to a specific person will automatically be sent to other related personnel.

    ```
    $ vi /etc/aliases
    heejoon: hjwin, hjmac, hjlinux
    ```

    Run one of the following commands to apply changes:
    ```
    $ newaliases
    $ sendmail -bi
    $ sendmail -I
    ```

6. `/etc/mail/virtusertable`

    Create a virtual user only used when specifying the mail recepient. Forward such mails to the actual intended user.

    ```
    $ vi /etc/mail/virtusertable
    ceo@linux.com    heejoon
    ceo@windows.com  bill
    ```

    Apply changes:
    ```
    $ makemap hash /etc/mail/virtusertable < /etc/mail/virtusertable
    ```

7. `~/.forward`

    Forward mails sent to a user to other users. The write permission of `.forward` file must be enabled only for the owner.

    ```
    $ vi .forward
    heejoon@naver.com
    heejoon@gmail.com

    $ chmod 0600 .forward
    ``` 


    