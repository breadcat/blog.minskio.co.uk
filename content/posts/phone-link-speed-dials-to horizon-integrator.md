---
title: "Migrating LG Phone-Link contacts to Horizon Integrator"
date: 2019-07-23T12:48:00
tags: ["Databases", "Formats", "Guides", "Linux", "PBX", "Servers", "Windows", "Work"]
---

If you've used LG's Phone-Link software for any reasonable period of time, you should have a large XML file in your `%appdata%\PHONE-LiNK` directory named `Recent.xml`.

Make a copy of this and get it on a computer with a Linux environment. For this process, we're going to need `xmllint` from the `libxml2-utils` package and `xmlstarlet` from the `xmlstarlet` package.

Once these are installed, verify your `Recent.xml` file is the expected unholy mess we've come to know and love via `less Recent.xml`. Once you've been sufficiently disheartened, close `less` by pressing `q`.

From this, make a backup as that's always a good idea:
```
cp "Recent.xml" "backup-Recent.xml"
```

Now we're going to lint the file, merge the incoming and outgoing blocks, and build a CSV file:
```
xmllint "Recent.xml" --format --output "Recent.xml"
sed -i 's/CalledContact/CallerContact/g' "Recent.xml"
xmlstarlet sel -T -t -m /Recent/CallerContact -v "concat(Name,';',Contact,';',Tel,';;',Email,';',Company)" -n "Recent.xml" > "Recent.csv"
```

Now we have a messy CSV file with the information in it we wanted.

We're going to sort this, remove unecessary entries (from places with no names) and fix up the delimiters (as Phone-Link will save Names as Contact, Company which breaks CSV files):
```
sort -b -u -o "Recent.csv" "Recent.csv"
sed -i '/^(/d' "Recent.csv"
sed -i 's/, / - /g' "Recent.csv"
sed -i 's/;/,/g' "Recent.csv"
```

Last but not least, we're going to add our header row, again using `sed`:
```
sed -i '1 i\First Name,Last Name,Number,Extension,Email,Company' "Recent.csv"
```

Now you can start working your way through the inane and sometimes arcane Horizon Company Directory requirements, as detailed below:
* No brackets
* No @ symbols (except for email addresses)
* No apostrophes
* No ampersands
* No slashes
* No hashes
* No periods
* No hyphens
* No spaces
* 15 characters max
* No empty fields (except for extension, and email)

The way I go about this is to split the columns into separate files, apply the filters and then rebuild the file afterwards.

So, let's get splitting:
```
for i in {1..6}; do cut -f"$i" -d\, "Recent.csv" > "column-$i.txt"; done
```

Here is where I manually split the *Name* field into *First Name* and *Last Name*. I did this manually as some names didn't nicely fit into the *A B* format.

Now we've got our columns split, lets get processing those invalid characters. You'll need to process each file individually, as certain files need certain filters.
```
sed -i -e "s/#//g" file.txt # hash symbols
sed -i -e "s/'//g" file.txt # apostrophes
sed -i -e "s/@//g" file.txt # at symbols
sed -i -e "s/\///g" file.txt # slashes
sed -i -e "s/&//g" file.txt # ampersands
sed -i -e "s/(//g" file.txt # open brackets
sed -i -e "s/)//g" file.txt # close brackets
sed -i -e "s/-//g" file.txt # hyphens
sed -i -e "s/ //g" file.txt # spaces
sed -i -e 's/\.//g' file.txt # periods
sed -i -e 's/^$/x/' file.txt # replace blank lines
sed -i -e "s/^\(.\{15\}\).*/\1/g" file.txt # snip columns to length
```

Once done, merge the files back together using paste, removing duplicates with uniq:
```
paste -d, column-{1..6}.txt | uniq > Recent-processed.csv
```

Now try to upload the resulting file, and manually fix any inevitable errors that will have cropped up like duplicate entries and trailing invalid characters. Above is the process I used for my dataset.