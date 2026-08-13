---
title: "Webex bulk UK bank holidays generator"
date: 2026-08-13T12:59:00
tags: ["Downloads", "Tools", "Work"]
---

For pretty much every Webex site I build I end up manually adding [UK bank holiday dates](https://www.gov.uk/bank-holidays), this page generates that list (up until 2028). Just fill in the Location name, click download, then upload them via `Management > Locations > Location Name > Calling > Calling features settings > Schedules > Bulk Manage > Upload .csv data`.

If you want to build your own, check out the source code and the dates are defined in an ISO-8601 const block.

<label for="location">Location</label>
<input type="text" id="location" placeholder="Enter location" >
<button id="generateButton">Download CSV</button>

<div id="status"></div>

<script>

    /* Bank holiday dates in ISO-8601 standard */
    const bankHolidayDates = [
        "2026-08-31",
        "2026-12-25",
        "2026-12-28",
        "2027-01-01",
        "2027-03-26",
        "2027-03-29",
        "2027-05-03",
        "2027-05-31",
        "2027-08-30",
        "2027-12-27",
        "2027-12-28",
        "2028-01-03",
        "2028-04-14",
        "2028-04-17",
        "2028-05-01",
        "2028-05-29",
        "2028-08-28",
        "2028-12-25",
        "2028-12-26"
    ];


    /* CSV Headers */
    const headers = [
        "Name",
        "Location",
        "Schedule action",
        "Schedule type",
        "Event action"
    ];


    // Add the 10 event groups.
    for (let i = 1; i <= 10; i++) {

        headers.push(
            `Event ${i} name`,
            `Event ${i} start date`,
            `Event ${i} end date`,
            `Event ${i} start HOLIDAY`,
            `Event ${i} end HOLIDAY`,
            `Event ${i} recurrence type`,
            `Event ${i} day of recurrence by week`,
            `Event ${i} day of yearly recurrence by day`,
            `Event ${i} week of yearly recurrence by day`,
            `Event ${i} month of yearly recurrence by day`,
            `Event ${i} date of yearly recurrence by date`,
            `Event ${i} month of yearly recurrence by date`
        );
    }

    function isoToAmericanDate(isoDate) {

        const [year, month, day] = isoDate.split("-");

        return `${month}/${day}/${year}`;
    }

    function csvEscape(value) {

        if (value === null || value === undefined) {
            return "";
        }

        const text = String(value);

        if (
            text.includes(",") ||
            text.includes('"') ||
            text.includes("\n") ||
            text.includes("\r")
        ) {
            return `"${text.replace(/"/g, '""')}"`;
        }

        return text;
    }


    /* Create event 1 CSV */

    function createRow(location, isoDate) {

        const americanDate =
            isoToAmericanDate(isoDate);

        const row = [

            // Event 1
            "Bank Holidays",
            location,
            "ADD",
            "HOLIDAY",
            "ADD",

            isoDate,          // Event 1 name
            americanDate,     // Event 1 start date
            americanDate,     // Event 1 end date
            "12:00",          // Event 1 start HOLIDAY
            "14:00",          // Event 1 end HOLIDAY
            "NONE",           // Event 1 recurrence type
            "",               // Event 1 day of recurrence by week
            "",               // Event 1 day of yearly recurrence by day
            "",               // Event 1 week of yearly recurrence by day
            "",               // Event 1 month of yearly recurrence by day
            "",               // Event 1 date of yearly recurrence by date
            ""                // Event 1 month of yearly recurrence by date
        ];


        // Events 2–10
        for (let i = 2; i <= 10; i++) {

            for (let field = 0; field < 12; field++) {
                row.push("");
            }
        }


        return row;
    }

    function generateCSV(location) {

        const rows = [];

        rows.push(headers);
        for (const isoDate of bankHolidayDates) {

            rows.push(
                createRow(location, isoDate)
            );
        }

        return rows
            .map(row =>
                row
                    .map(csvEscape)
                    .join(",")
            )
            .join("\r\n");
    }

    function downloadCSV(csv, location) {

        const blob = new Blob(
            ["\uFEFF" + csv],
            {
                type: "text/csv;charset=utf-8;"
            }
        );

        const url =
            URL.createObjectURL(blob);

        const link =
            document.createElement("a");

        link.href = url;

        // Clean location for filename.
        let filename =
            location
                .trim()
                .replace(/[^a-z0-9]+/gi, "_")
                .replace(/^_+|_+$/g, "");

        if (!filename) {
            filename = "Location";
        }

        link.download =
            `Bank_Holidays_${filename}.csv`;

        document.body.appendChild(link);

        link.click();

        document.body.removeChild(link);

        URL.revokeObjectURL(url);
    }

    document
        .getElementById("generateButton")
        .addEventListener("click", function () {

            const location =
                document
                    .getElementById("location")
                    .value
                    .trim();

            const status =
                document.getElementById("status");


            if (!location) {

                status.style.display = "block";
                status.style.background = "#fff0f0";
                status.style.color = "#8a1f1f";

                status.textContent =
                    "Please enter a Location.";

                return;
            }


            const csv =
                generateCSV(location);


            downloadCSV(
                csv,
                location
            );

            status.style.display = "block";
            status.style.background = "#eef8ee";
            status.style.color = "#276327";

            status.textContent =
                `CSV generated successfully for "${location}". ` +
                `${bankHolidayDates.length} holiday rows created.`;
        });

</script>
