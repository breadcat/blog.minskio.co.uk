---
title: "Notes on Sorting Paperwork"
date: 2020-06-23T15:29:00
tags: ["Formats", "Guides", "Minimalism", "Snippets", "Software", "Windows"]
---

# Intention

The purpose of this whole project ties in with digitally having copies of things. This makes searching and sorting much, much easier.

The only physical paperwork I keep are certain important documents (like birth certificates, physical driving licence, etc).

# Hardware

I'm using a Fujitsu ScanSnap iX500 as my scanner of choice, it was recommended by the Paperless project and it folds away neatly when not in use.

I initially intended on this hardware being used wirelessly, which I'm sure it is capable of; but for the once a month or so I go through my paperwork I doubt it's worth the hassle setting it up.

# Software (or the lack of)

*Preface:* I'm doing all of this on Windows. I've attempted to get this working nicely on Linux, but my results scanning with `sane` were terrible, even when tweaking input it was slower, with worse quality, wouldn't feed multiple sheets and had to be manually initiated from the command line for every scan.

So, on with the software. The only real software that's required is the [ScanSnap Manager from Fujitsu](http://scansnap.fujitsu.com/global/dl/). It's a little bulky but for it's girth it will (attempt to) auto-rotate and OCR the document with pretty high rate of success. Only around 5-10% of documents require any manual intervention in my experience.

One point worth noting is the ScanSnap software will add two autorun entries to your system startup. They can be removed with the following commands:
```
reg delete "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Run" /v "ScanSnap WIA Service Checker" /f
reg delete "HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Run" /v "ScanSnap OnlineUpdate Watcher" /f
```

In the past, I used [Paperless](https://github.com/the-paperless-project/paperless) to automatically OCR and pseudo-sort my documents, but this had a high failure rate (~75% of documents couldn't be parsed), was quite _heavy-weight_ software and was complete overkill for what I needed.

Nowadays, I just scan the files to a single PDF document and organise away in a folder structure. Files are generally sorted well enough that I don't need to ever manually search for file contents as I always have an idea of what I need. Worst case scenario I believe you can grep strings from OCR'd pdf files anyway.

# Naming

When it comes to naming my files, I follow a few rules to help keep things organised:

- Include a brief summary in the filename (e.g. insurance-renewal-invitation)
- Always use lower case characters
- Replace spaces with hyphens
- Use ISO dates where possible, or if you receive a biannual statement, name it 2019a and 2019b
- For cars, reference the registration number

# Duplicates

If you've read my [notes on sorting pictures]() post you'll know that I try to remove duplicates, however with paperwork I don't bother de-duplicate the files for a few reasons:

- Time/bandwidth taken to download files during duplicate search
- Scans vs identical scans vs digital copies will never be identical
- OCR isn't perfect and will produce different results base on the above
- My files are generally sorted well enough that I'd be getting duplicate filenames.

# File manipulation

Once scanned, I rename all my files to their titles are applicable (see above), then I manually check each one for blank, rotated and out of order pages. I upload these to my server and then manually sort any files that require attention using `qpdf`. Below are some basic commands to work with documents:

## Deleting
```
qpdf input.pdf --pages input.pdf 1-9,26 -- outputfile.pdf
```

## Splitting
```
qpdf input.pdf --pages input.pdf 1-2,4 -- outputfile1.pdf
qpdf input.pdf --pages input.pdf 3,5-6 -- outputfile2.pdf
```

## Rotating
```
qpdf in out.pdf --rotate=180:1,4
```

## Decrypting
```
qpdf --password=yourpassword --decrypt input.pdf outputfile.pdf
```

## OCR
For files that I didn't scan myself, sometimes they will require manual OCR'ing. For this I use [OCRmyPDF](https://github.com/jbarlow83/OCRmyPDF):
```
ocrmypdf -l eng input.pdf output_ocr.pdf
```

## Lower case filenames
```
mmv '*' '#l1'
```

# Folder Structure

Lastly, and the key to keeping things easy to work with, my folder structure looks something like this:

```
.
|-- computing
|   `-- provider
|-- finances
|   `-- bank-name
|       |-- correspondance
|       `-- statements
|-- household
|   |-- appliances
|   |   `-- appliance
|   |-- council
|   |-- insurance
|   |   `-- year provider
|   |-- purchase
|   |-- recycling
|   |-- renovation
|   |-- utilities
|   |   `-- sorted by provider
|   `-- voting
|-- personal
|   |-- business-cards
|   |-- certifications
|   |-- driving-licence
|   |-- gym
|   |-- travel
|   |   `-- year location
|   `-- workplace
|       |-- contracts
|       |-- interviews
|       |-- p60
|       |-- pension
|       |-- tax
|       `-- wages
`-- transport
    |-- insurance
    |   `-- year registration
    |-- mot
    |   `-- year registration
    |-- purchases
    |-- repairs
    |   `-- registration
    |-- roadside-assistance
    |   `-- year provider
    `-- road-tax
```

This is all kept backed up on cloud storage, as you'd expect. So finally, at the end of all this, we have sorted, digital-only paperwork, backed up online.