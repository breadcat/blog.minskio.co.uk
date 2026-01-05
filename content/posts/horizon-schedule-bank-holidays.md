---
title: "Gamma Horizon Bank Holiday Schedules"
date: 2025-11-07T16:31:00
tags: ["Snippets", "Work"]
---

It's not too often that I need to bulk import bank holidays into a Horizon account, however when I do I like to do it as simply as possible. What I've compiled below are the bank holiday dates for the UK next couple of years, compiled from [this goverment resource](https://www.gov.uk/bank-holidays).

What I like to do is copy the first column, alt-tab to enter it as the name, then alt-tab back, copy the second column, alt-tab back to Horizon then tab x3, paste, tab x2, paste before finally clicking Create. You can get quite good at this with a bit of practice.

```
BH 2026-01-01	01/01/2026
BH 2026-04-03	03/04/2026
BH 2026-04-06	06/04/2026
BH 2026-05-04	04/05/2026
BH 2026-05-25	25/05/2026
BH 2026-08-31	31/08/2026
BH 2026-12-25	25/12/2026
BH 2026-12-28	28/12/2026

BH 2027-01-01	01/01/2027
BH 2027-03-26	26/03/2027
BH 2027-03-29	29/03/2027
BH 2027-05-03	03/05/2027
BH 2027-05-31	31/05/2027
BH 2027-08-30	30/08/2027
BH 2027-12-27	27/12/2027
BH 2027-12-28	28/12/2027
```

One easily solved issue you may come across is the date selector window remaining open as you've pasted in a date versus clicking one. This can be fixed by removing the element entirely by blocking the following rule:
```
##.ui-corner-all.ui-helper-clearfix.ui-widget-content.ui-widget.ui-datepicker
```

Another solution entirely is to use Contact call routing, but you've gotta pay for that improvement.
