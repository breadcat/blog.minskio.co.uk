---
title: "Automating Grabbing Payslips For Use With Paperless"
date: 2019-02-04T11:47:00
tags: ["formats", "guides", "linux", "servers", "software"]
---

My workplace has recently started sending out Payslips as email attachments instead of the usual physical sheet which I'm a big fan of, [Paperless](https://github.com/the-paperless-project/paperless) is always on hand to sort and process any paperwork I have which keeps things organised and under control.

To tie all these processes together, we're going to use `getmail`, `mpack` and `qpdf`.

Please note that this will download your **entire inbox** every time so it helps if you don't run the script too often, and keep your inbox size to manageable levels.

Firstly, we'll need to specify a number of variables for use later in the script:
```
email_sender="payslipsender@address"
email_username="youremailaddress"
email_password="youremailpassword"
payslip_password="yourpdfpassword"
payslip_pattern=Payslip
payslip_filetype=pdf
import_directory="$HOME/import"
temp_directory="$(mktemp -d)"
```

We'll change to the temporary directory and make the directories that `getmail` expects to be there:
```
cd "$temp_directory" || exit
mkdir {cur,new,tmp}
```

As I don't really want to keep an copy of my whole inbox around for no good reason, I dump my email to a temporary directory and write my `getmail` config file into this directory with a heredoc.
Here I'm using IMAP with SSL but getmail supports [a number of different methods of grabbing mail](http://pyropus.ca/software/getmail/configuration.html#conf-retriever):

```
cat << EOF > getmailrc
[retriever]
type = SimpleIMAPSSLRetriever
server = your.imap.server
username = $email_username
port = 993
password = $email_password

[destination]
type = Maildir
path = $temp_directory/
EOF
```

Then run `getmail` using the temporary directory as your working directory:
```
getmail --getmaildir "$temp_directory"
```

Change directory to our newly saved items, then extract all attachments that match our search pattern in the variable above. Lastly, move these attachments to the Paperless import directory.
```
cd new || exit
grep "$email_sender" ./* | cut -f1 -d: | uniq | xargs munpack -f
mv "$payslip_pattern"*"$payslip_filetype" "$import_directory"
```

Now Paperless won't work on these files unless they're decrypted, which we can do as follows:
```
cd "$import_directory" || exit
for i in $payslip_pattern*$payslip_filetype; do
	fileProtected=0
	qpdf "$i" --check || fileProtected=1
	if [ $fileProtected == 1 ]; then
		qpdf --password="$payslip_password" --decrypt "$i" "decrypt-$i" && rm "$i"
	fi
done
```

Now we have a directory full of unencrypted files to let Paperless work with. Last but not least, we'll need to delete the old temporary directory we used:
```
rm -r "$temp_directory"
```

Lastly all you need to do is set up the above script as a cron job to run after pay day! The cron line I'm using is as follows:
```
0 0 2 * * $HOME/path/to/script/payslip.sh &
```

* **Edit 2019-02-27:** `pdftk` replaced with `qpdf` as it required java which pulls down ~200MB dependencies.
* **Edit 2019-08-09:** Added cron section.
