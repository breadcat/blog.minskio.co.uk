---
title: "Javascript URI decoder"
date: 2026-02-05T10:46:00
tags: ["Snippets", "Work"]
---

<br><textarea id="input" rows="5" cols="60" placeholder="Input text"></textarea>
<br><textarea id="output" rows="5" cols="60" placeholder="Output text" readonly></textarea>

<script>
  const input = document.getElementById("input");
  const output = document.getElementById("output");

  input.addEventListener("input", () => {
    try {
      output.value = decodeURIComponent(input.value);
    } catch {
      output.value = input.value;
    }
  });
</script>

I couldn't find a easily usable one online that didn't need to submit a form so just ended up writing my own.
