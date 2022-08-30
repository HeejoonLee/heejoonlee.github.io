---
title: "[Linux] Communication Commands"
date: 2022-08-31 01:48 +0900
categories: [etc, Linux]
tags: [Linux, guide]
---

Command | Description | Usage
----|----|----
write | Send a message to a user | <code>$ write heejoon</code>
wall | Send a message to all users | <code>$ wall "Shutdown imminent"</code>
mesg | Check messages received via `write` | <code>$ mesg</code> <br> <code>$ mesg n</code> <br> → Reject messages
mail | Send a mail to a user | <code>$ mail heejoon </code> <br> <code>$ mail -s "hello" heejoon </code> <br> → Send a mail to heejoon and set the title to "hello"

