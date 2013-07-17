import imaplib
import string

sms = []

#########################################################################################
#credentials file must contain uname pword in a single line, separated by a space
f = open('credentials','r');
cred = f.readline().split();
f.close();

#########################################################################################
#imap processing
imap_server = imaplib.IMAP4_SSL("imap.gmail.com",993)
imap_server.login(cred[0], cred[1])

imap_server.select('INBOX')

status, email_ids = imap_server.search(None, '(UNSEEN)')
unread = email_ids[0].split()

#looking for the text and time within the emails
if len(email_ids[0])!=0:
  for mail in unread:
    _, response = imap_server.fetch(mail, '(RFC822 BODY[TEXT])')

    head   = response[0][1]
    date_b = head.index("Date")
    date_e = head.index("\r",date_b)

    text   = response[1][1]
    span   = text.index("<span>")
    blkqt  = text.index("<blockquote>")
    blkqt2 = text.index("</blockquote>")

    sms.append(head[date_b:date_e] + "||" + text[span+6:blkqt] + text[blkqt+12:blkqt2])

#########################################################################################
f = open('data.txt','w')
for msg in sms:
  f.write(msg+"\n=========\n");

f.close();
#########################################################################################
