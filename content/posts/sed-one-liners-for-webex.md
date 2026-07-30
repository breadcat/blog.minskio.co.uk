---
title: "Sed one liners for Webex"
date: 2026-06-30T13:59:00
lastmod: 2026-07-02T11:31:00
tag: ["Formats", "Guides", "Linux", "PBX", "Snippets", "Work"]
---

I recently came across a customer with nearly a thousand speed dials in a CSV document. In a similar (but significantly better) vein to [my earlier post](/migrating-lg-phone-link-contacts-to-horizon-integrator/) regarding this, here are the one liners I used to get this working:

<li>Append +44 country to the start of every line:<br><code>sed -i 's/^/+44/' numbers.txt</code></li><br>

<li>Replace leading 0 with +44 on every line:<br><code>sed -i 's/^0/+44/' numbers.txt</code></li><br>

<li>Paste together names, numbers and number type into a new CSV document:<br><code>paste names.txt numbers.txt | awk -F'\t' 'BEGIN{OFS=","} {print $1,"","",$2,"work"}' > contacts.csv</code></li><br>

<li>Add webex contact header to newly created CSV document:<br><code>sed -i '1iDisplay Name,First Name,Last Name,Phone Number 1,Phone Number 1 Type,Phone Number 2,Phone Number 2 Type,Phone Number 3,Phone Number 3 Type,Phone Number 4,Phone Number 4 Type,Phone Number 5,Phone Number 5 Type,Contact Email,SIP URI,Title,Company Name,Address Street,Address City,Address State,Address Country,Address Zip,Group ID 1,Group ID 2,Group ID 3,Group ID 4,Group ID 5' document.csv</code></li><br>

<li>Remove all whitespace before comma delimiters:<br><code>sed -i 's/ *,/,/g' contacts.csv</code></li><br>

<li>Remove identical duplicates:<br><code>awk 'NR==1 || !seen[$0]++' contacts.csv > contacts_unique.csv</code></li><br>

<li>Move remaining name duplicates names to a second file to deal with later:<br>
<code>awk -F, 'BEGIN{OFS=","}
NR==1 {print > "contacts_dedup.csv"; print > "duplicates.csv"; next}
!seen[$1]++ {print > "contacts_dedup.csv"; next}
{print > "duplicates.csv"}' contacts.csv</code></li><br>

<li>Print duplicated numbers:<br><code>awk -F',' '++count[$5] == 2 { print $5 }'</code></li>
