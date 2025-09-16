---
title: "Decimal overtime calculator"
date: 2025-09-05T15:48:00
tags: ["Tools", "Work"]
---

My workplace has a quirky rule when it comes to submitting overtime, everything needs to be calculated in decimal hours. While this isn't especially taxing as far as mathmatical problems go I have [built a bash script](https://github.com/breadcat/nix-configs/blob/main/scripts/overtid.nix) to do this, however I'd prefer not to SSH into a server to run a bit of maths, or grab a calculator so here's the same thing in simple JavaScript:

  <label>Start Time: <input type="time" id="startTime" value="07:30"></label>

  <label>End Time: <input type="time" id="endTime" value="19:30"></label>

  <details><summary>Custom cutoffs</summary>
    <label>Morning: <input type="time" id="morningCutoff" value="08:30"></label>
    <br>
    <label>Evening: <input type="time" id="eveningCutoff" value="18:00"></label>
  </details>

  <div class="result" id="result"></div>

  <script>
    function timeToDecimal(timeStr) {
      const [h, m] = timeStr.split(":").map(Number);
      return h + m / 60;
    }

    function calculate() {
      const start = timeToDecimal(document.getElementById("startTime").value);
      const end = timeToDecimal(document.getElementById("endTime").value);
      const morningCut = timeToDecimal(document.getElementById("morningCutoff").value);
      const eveningCut = timeToDecimal(document.getElementById("eveningCutoff").value);

      let before = 0, after = 0;

      if (start < morningCut) {
        before = Math.max(0, Math.min(end, morningCut) - start);
      }

      if (end > eveningCut) {
        after = Math.max(0, end - Math.max(start, eveningCut));
      }

      const total = before + after;

      document.getElementById("result").innerHTML = `
        <br>
        Hours before: ${before.toFixed(2)}. Hours after: ${after.toFixed(2)}<br>
        Total hours: ${total.toFixed(2)}
      `;
    }

    // live update
    document.querySelectorAll("input").forEach(input => {
      input.addEventListener("input", calculate);
    });

    // Run at start
    calculate();
  </script>
