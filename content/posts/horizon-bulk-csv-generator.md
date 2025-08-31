---
title: "Horizon bulk CSV generator"
date: 2025-08-16T08:26:00
tags: ["Downloads", "Tools", "Work"]
---

I had a bit of a thought recently that each time I generate a bulk upload CSV file for Horizon I spend more time copy and pasting data to-and-from Excel and notepad versus actually inputting data so I threw together a bulk upload generator.

<label>Domain: <input type="text" id="domain" placeholder="example.com"></label>

<label>Extension Range: <input type="text" id="range" placeholder="200-220"></label>

<label><input type="checkbox" id="device">Include Devices</label>

<button onclick="downloadCSV()">Download CSV</button>

<div id="preview"></div>

<script>
  let lastGenerated = { headers: [], rows: [], domain: "", start: 0, end: 0 };

  function buildData() {
    const domain = document.getElementById('domain').value.trim();
    const rangeInput = document.getElementById('range').value.trim();
    const includeDevice = document.getElementById('device').checked;

    // Parse range safely
    const parts = rangeInput.split('-').map(p => parseInt(p, 10));
    if (parts.length !== 2 || isNaN(parts[0]) || isNaN(parts[1]) || parts[0] > parts[1] || !domain) {
      document.getElementById('preview').innerHTML = "<p style='color:red'>Enter a valid domain and range.</p>";
      lastGenerated = { headers: [], rows: [], domain: "", start: 0, end: 0 };
      return;
    }

    const start = parts[0];
    const end = parts[1];

    // Headers
    let headers = ["User ID","First Name","Last Name","Email","Phone Number","Extension"];
    if (includeDevice) headers.push("Device");
    headers.push("Service Pack");

    let rows = [headers];

    for (let ext = start; ext <= end; ext++) {
      let userId = `ext${ext}@${domain}`;
      let lastName = ext.toString();
      let email = "email@example.com";
      let servicePack = "Premium";

      let row = [userId, "", lastName, email, "", ext.toString()];
      if (includeDevice) row.push(""); // device placeholder
      row.push(servicePack);

      rows.push(row);
    }

    // Save for download later
    lastGenerated = { headers, rows, domain, start, end };

    // Build preview
    let html = "<h3>Preview</h3><table><thead><tr>";
    headers.forEach(h => { html += `<th>${h}</th>`; });
    html += "</tr></thead><tbody>";
    rows.slice(1, 11).forEach(r => {
      html += "<tr>";
      r.forEach(cell => { html += `<td>${cell}</td>`; });
      html += "</tr>";
    });
    if (rows.length > 11) {
      html += `<tr><td colspan="${headers.length}">...and ${rows.length - 11} more rows</td></tr>`;
    }
    html += "</tbody></table>";
    document.getElementById('preview').innerHTML = html;
  }

  function downloadCSV() {
    if (!lastGenerated.rows.length) {
      alert("Please enter valid inputs first.");
      return;
    }

    const { rows, domain, start, end } = lastGenerated;

    const csvContent = rows.map(r => r.join(",")).join("\n");
    const blob = new Blob([csvContent], { type: "text/csv" });
    const url = URL.createObjectURL(blob);

    const safeDomain = domain.replace(/[^a-zA-Z0-9.-]/g, "_");
    const fileName = `${safeDomain}-${start}-${end}.csv`;

    const a = document.createElement("a");
    a.href = url;
    a.download = fileName;
    a.click();
    URL.revokeObjectURL(url);
  }

  // Live update listeners
  document.getElementById('domain').addEventListener('input', buildData);
  document.getElementById('range').addEventListener('input', buildData);
  document.getElementById('device').addEventListener('change', buildData);
</script>