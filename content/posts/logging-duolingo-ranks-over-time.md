---
title: Logging Duolingo ranks over time
layout: table
tags: [ "Languages", "Lists" ]
date: 2022-05-03T14:58:00
lastmod: 2025-07-30T12:45:00
---

I was curious as to how my rank would change over time and what the rate of attrition would look like over time.

This page was originally an attempt to mix markdown and bash scripting into a single executable page that updates itself when run that was itself documentation of how it worked, however the header that Hugo (which is what this site is built with) requires entirely breaks the markdown/bash quirk that allowed it to work with plain markdown.

In the background, the data source I'm scraping is [duome.eu](https://duome.eu/) and dumps values directly to this page using `blog_duolingo_rank` as part of `server.sh` in [my Dockerfiles project](https://github.com/breadcat/Dockerfiles).

Currently this script is manually run whenever I'm curious. Perhaps as time goes on, I'll automate this and eventually graph out these values (similarly to my [weight tracking](/weight/) page), but that's a project for later.

<table id="sortable-table">
  <thead><tr>
    <th>Date</th><th>Time</th><th>Streak</th><th>Lingots</th>
  </tr></thead><tbody>
  <tr><td>2022-04-10</td><td>N/A</td><td>3078</td><td>3337</td></tr>
  <tr><td>2022-04-11</td><td>N/A</td><td>3068</td><td>3343</td></tr>
  <tr><td>2022-04-12</td><td>11:39</td><td>3056</td><td>3347</td></tr>
  <tr><td>2022-04-14</td><td>23:35</td><td>3047</td><td>3355</td></tr>
  <tr><td>2022-04-19</td><td>23:57</td><td>3314</td><td>3314</td></tr>
  <tr><td>2022-04-20</td><td>11:37</td><td>2986</td><td>3321</td></tr>
  <tr><td>2022-04-24</td><td>23:06</td><td>2962</td><td>3336</td></tr>
  <tr><td>2022-05-03</td><td>15:17</td><td>2909</td><td>3316</td></tr>
  <tr><td>2022-05-04</td><td>14:43</td><td>2910</td><td>3316</td></tr>
  <tr><td>2022-05-07</td><td>23:12</td><td>2909</td><td>3252</td></tr>
  <tr><td>2022-05-10</td><td>10:05</td><td>3271</td><td>3271</td></tr>
  <tr><td>2022-05-16</td><td>09:57</td><td>3299</td><td>3299</td></tr>
  <tr><td>2022-05-19</td><td>17:15</td><td>2818</td><td>3244</td></tr>
  <tr><td>2022-05-28</td><td>23:33</td><td>2793</td><td>3213</td></tr>
  <tr><td>2022-05-30</td><td>19:58</td><td>3222</td><td>3222</td></tr>
  <tr><td>2022-06-13</td><td>12:26</td><td>2742</td><td>3201</td></tr>
  <tr><td>2022-06-28</td><td>12:40</td><td>3106</td><td>3318</td></tr>
  <tr><td>2022-07-12</td><td>18:25</td><td>3320</td><td>3320</td></tr>
  <tr><td>2022-08-03</td><td>13:19</td><td>3155</td><td>3310</td></tr>
  <tr><td>2022-08-04</td><td>09:07</td><td>3152</td><td>3311</td></tr>
  <tr><td>2022-09-28</td><td>12:13</td><td>3059</td><td>3207</td></tr>
  <tr><td>2022-10-17</td><td>12:12</td><td>3152</td><td>3152</td></tr>
  <tr><td>2022-11-29</td><td>22:37</td><td>2966</td><td>3085</td></tr>
  <tr><td>2023-02-28</td><td>18:23</td><td>2922</td><td>3001</td></tr>
  <tr><td>2023-05-23</td><td>23:38</td><td>3030</td><td>2958</td></tr>
  <tr><td>2023-09-12</td><td>00:00</td><td>2985</td><td>2985</td></tr>
  <tr><td>2023-10-11</td><td>13:47</td><td>2842</td><td>2842</td></tr>
  <tr><td>2023-11-13</td><td>20:32</td><td>2819</td><td>2819</td></tr>
  <tr><td>2023-11-29</td><td>23:23</td><td>2843</td><td>2843</td></tr>
  <tr><td>2023-12-28</td><td>11:33</td><td>2863</td><td>2863</td></tr>
  <tr><td>2023-12-31</td><td>23:59</td><td>2866</td><td>2866</td></tr>
  <tr><td>2024-01-07</td><td>23:59</td><td>2869</td><td>2869</td></tr>
  <tr><td>2024-01-14</td><td>23:59</td><td>2873</td><td>2873</td></tr>
  <tr><td>2024-01-21</td><td>23:59</td><td>2875</td><td>2875</td></tr>
  <tr><td>2024-01-28</td><td>23:59</td><td>2881</td><td>2881</td></tr>
  <tr><td>2024-02-04</td><td>23:59</td><td>2884</td><td>2884</td></tr>
  <tr><td>2024-02-08</td><td>09:49</td><td>2885</td><td>2885</td></tr>
  <tr><td>2024-02-11</td><td>23:59</td><td>2887</td><td>2887</td></tr>
  <tr><td>2024-02-18</td><td>23:59</td><td>2892</td><td>2892</td></tr>
  <tr><td>2024-02-25</td><td>23:59</td><td>2895</td><td>2895</td></tr>
  <tr><td>2024-03-03</td><td>23:59</td><td>2897</td><td>2897</td></tr>
  <tr><td>2024-03-04</td><td>01:54</td><td>2897</td><td>2897</td></tr>
  <tr><td>2024-03-10</td><td>23:59</td><td>2900</td><td>2900</td></tr>
  <tr><td>2024-03-17</td><td>23:59</td><td>2903</td><td>2903</td></tr>
  <tr><td>2024-03-24</td><td>23:59</td><td>2903</td><td>2903</td></tr>
  <tr><td>2024-03-31</td><td>23:59</td><td>2906</td><td>2906</td></tr>
  <tr><td>2024-04-07</td><td>12:10</td><td>2913</td><td>2913</td></tr>
  <tr><td>2024-04-07</td><td>23:59</td><td>2913</td><td>2913</td></tr>
  <tr><td>2024-04-14</td><td>23:59</td><td>2914</td><td>2914</td></tr>
  <tr><td>2024-04-21</td><td>23:59</td><td>2919</td><td>2919</td></tr>
  <tr><td>2024-04-28</td><td>23:59</td><td>2921</td><td>2921</td></tr>
  <tr><td>2024-05-05</td><td>23:59</td><td>2925</td><td>2925</td></tr>
  <tr><td>2024-05-12</td><td>23:59</td><td>2927</td><td>2927</td></tr>
  <tr><td>2024-05-19</td><td>23:59</td><td>2931</td><td>2931</td></tr>
  <tr><td>2024-05-26</td><td>23:59</td><td>2941</td><td>2941</td></tr>
  <tr><td>2024-06-02</td><td>23:59</td><td>2942</td><td>2942</td></tr>
  <tr><td>2024-06-09</td><td>23:59</td><td>2945</td><td>2945</td></tr>
  <tr><td>2024-06-16</td><td>23:59</td><td>2958</td><td>2958</td></tr>
  <tr><td>2024-06-23</td><td>23:59</td><td>2960</td><td>2960</td></tr>
  <tr><td>2024-06-30</td><td>23:59</td><td>2967</td><td>2967</td></tr>
  <tr><td>2024-07-07</td><td>23:59</td><td>3014</td><td>3014</td></tr>
  <tr><td>2024-07-14</td><td>23:59</td><td>3037</td><td>3037</td></tr>
  <tr><td>2024-07-21</td><td>23:59</td><td>3076</td><td>3076</td></tr>
  <tr><td>2024-07-28</td><td>23:59</td><td>2496</td><td>2496</td></tr>
  <tr><td>2024-08-04</td><td>23:59</td><td>2506</td><td>2506</td></tr>
  <tr><td>2024-08-11</td><td>23:59</td><td>2528</td><td>2528</td></tr>
  <tr><td>2024-08-18</td><td>23:59</td><td>2538</td><td>2538</td></tr>
  <tr><td>2024-08-25</td><td>23:59</td><td>2555</td><td>2555</td></tr>
  <tr><td>2024-09-01</td><td>23:59</td><td>2563</td><td>2563</td></tr>
  <tr><td>2024-09-08</td><td>23:59</td><td>2566</td><td>2566</td></tr>
  <tr><td>2024-09-15</td><td>23:59</td><td>2574</td><td>2574</td></tr>
  <tr><td>2024-09-22</td><td>23:59</td><td>2495</td><td>2495</td></tr>
  <tr><td>2024-09-29</td><td>23:59</td><td>2512</td><td>2512</td></tr>
  <tr><td>2024-10-06</td><td>23:59</td><td>2506</td><td>2506</td></tr>
  <tr><td>2024-10-13</td><td>23:59</td><td>2529</td><td>2529</td></tr>
  <tr><td>2024-10-20</td><td>23:59</td><td>2512</td><td>2512</td></tr>
  <tr><td>2024-10-27</td><td>23:59</td><td>2518</td><td>2518</td></tr>
  <tr><td>2024-11-03</td><td>23:59</td><td>2539</td><td>2539</td></tr>
  <tr><td>2024-11-10</td><td>23:59</td><td>2545</td><td>2545</td></tr>
  <tr><td>2024-11-12</td><td>11:57</td><td>2546</td><td>2546</td></tr>
  <tr><td>2024-11-17</td><td>23:59</td><td>2632</td><td>2632</td></tr>
  <tr><td>2024-11-24</td><td>23:59</td><td>2666</td><td>2666</td></tr>
  <tr><td>2024-12-01</td><td>23:59</td><td>2654</td><td>2654</td></tr>
  <tr><td>2024-12-08</td><td>23:59</td><td>2639</td><td>2639</td></tr>
  <tr><td>2025-03-23</td><td>23:59</td><td>2543</td><td>2543</td></tr>
  <tr><td>2025-04-06</td><td>23:59</td><td>2534</td><td>2534</td></tr>
  <tr><td>2025-04-13</td><td>23:59</td><td>2517</td><td>2517</td></tr>
  <tr><td>2025-05-04</td><td>23:59</td><td>2495</td><td>2495</td></tr>
  <tr><td>2025-05-11</td><td>23:59</td><td>2473</td><td>2473</td></tr>
  <tr><td>2025-05-18</td><td>23:59</td><td>2493</td><td>2493</td></tr>
  <tr><td>2025-05-25</td><td>23:59</td><td>2467</td><td>2467</td></tr>
  <tr><td>2025-06-01</td><td>23:59</td><td>2441</td><td>2441</td></tr>
  <tr><td>2025-06-08</td><td>23:59</td><td>2420</td><td>2420</td></tr>
  <tr><td>2025-06-15</td><td>23:59</td><td>2441</td><td>2441</td></tr>
  <tr><td>2025-06-22</td><td>23:59</td><td>2428</td><td>2428</td></tr>
  <tr><td>2025-07-20</td><td>23:59</td><td>2375</td><td>2375</td></tr>
  <tr><td>2025-07-27</td><td>23:59</td><td>2398</td><td>2398</td></tr>
</tbody></table>