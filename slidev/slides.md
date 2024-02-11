---
theme: default
background: ./images/earth.jpg
class: text-center
highlighter: shikiji
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
author: Francis Deh
drawings:
  persist: false
transition: slide-left
title: Introduction
selectable: true
mdc: true
---
# Date and Time for Programmers 
## 🧑🏾‍💻


<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
---

# Objectives

Understand the way date and time are represented and the key notes for programmers

- 🌎 **The Globe** - world timezones
- 📒 **Definitions** - UTC, GMT, Zulu, DST and UTC offsets
- 🧑‍💻 **Server Side talks** - azure storage, mongodb and sqlalchemy use case
- ⏳ **More of UTC** - UTC date and time formats
- 🐍 **Python Playground** - explore some date time utils in python
- 📤 **API UX** - returning date and time: ISO8601, Unix Epoch, RFC and Custom formats
- 🛠 **Other Usecases** - different apps, different needs
- 🙋🏽‍♂️ **Recap and Questions** - summary and questions
- 🔗 **Resources** - links to resources

<br>
<br>

Let's begin 🔥

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---
layout: default
hideInToc: true
---

# Table of contents

<Toc maxDepth="1"></Toc>

---
transition: slide-up
---

# The Globe
<img src="https://miro.medium.com/v2/resize:fit:1400/format:webp/0*AuW8APtlc27iI-5D" class="h-full w-full" />

---
---
# Definitions
|     |     |
| --- | --- |
| GMT | Greenwich Mean Time used to be the official standard, but it was calculated based on the average solar time, that means, it was calculated based on the rotation motion of the earth. [Greenwich (London) Royal Observatory](https://en.wikipedia.org/wiki/File:Greenwich_clock.jpg) |
| UTC | Coordinated Universal Time (successor of GMT) is the primary time standard by which the world regulates clocks and time. UTC is calculated based on atomic time (more precise).|
| Z   | More related to how Aviation/Military refers to 0 UTC offset |
| DST | Local time changes by political decisions. Increases/decreases UTC offsets by 1. BST, CEST |
| UTC offsets | UTC, UTC+1, UTC+2, UTC-5 |

<br>
* NB: UTC is represented as +00:00 or UTC+0 or Z

---
layout: image-right
image: ./images/serve.jpg
---
# Storing time on the server
Most sources recommend storing dates in UTC. And manipulate based on timezone.

- [Mongo DB](https://www.mongodb.com/docs/v4.4/tutorial/model-time-data/#:~:text=MongoDB%20stores%20times%20in%20UTC,time%20in%20their%20application%20logic.)
- [SqlAlchemy](https://docs.sqlalchemy.org/en/20/core/custom_types.html#store-timezone-aware-timestamps-as-timezone-naive-utc)
- [Azure Storage](https://learn.microsoft.com/en-us/rest/api/storageservices/formatting-datetime-values)

What is the [Current UTC time?](https://www.utctime.net/)

---
---
# UTC date structure
- 2020–01–27T06:00:00Z (Greenwich)
- 2020–01–27T00:00:00–06:00 (México City)
- 2020–01–27T14:00:00+08:00 (Beijing)
<br>
The Date format is YYY-MM-DD
<br>
*T* begins the time section
<br>
The Time format is HH-MM-SS
<br>
The ending is the timezone utc offset:
<br>
*Z* for UTC+0 or GMT

---
---
# Date Manipulation in Python

Python has aware date time and naive datetime. This [article](https://blog.miguelgrinberg.com/post/it-s-time-for-a-change-datetime-utcnow-is-now-deprecated) mentions an upcoming change in the datetime package.

```py {all|4|6-7|10|all}
def aware_utcnow() -> datetime:
    """Generates a utc time with UTC timezone.
    NB: only the naive is stored in the DB."""
    return datetime.now(timezone.utc)

def naive_utcnow() -> datetime:
    return aware_utcnow().replace(tzinfo=None)

def convert_date_time_to_iso(value: datetime) -> None:
    # first convert date time to utc timezone or the user's timezone, then to iso format
    # and return to user.
    return value.toisoformat()
```
<br>
Do a demo with Python Shell and JS

<style>
.footnotes-sep {
  @apply mt-20 opacity-10;
}
.footnotes {
  @apply text-sm opacity-75;
}
.footnote-backref {
  display: none;
}
</style>

---

# Returning Date from API: 1
1. Use ISO8601
<div grid="~ cols-2 gap-4">
<div>

There are several formats to returning dates. Depends on the usecase. But a popular and accepted format is the ISO8601 format.
- [What is ISO8601 format?](https://en.wikipedia.org/wiki/ISO_8601)
- [UTC date in various format](https://www.utctime.net/)
- [UTC date in various format](https://greenwichmeantime.com/articles/clocks/iso/)


</div>
<div>
A sample API response:
<br>
```json
{
  "created_at": "2024-01-28T15:41:26Z"
}
```
<br>
In JS
```js
new Date().toISOString()
```
<br>
In Python
```py
from datetime import datetime, timezone
datetime.now(timezone.utc).isoformat()
```
</div>
</div>

---
---
# Returning Date from API: 2
2. Use ISO8601 but timezone as separate field 
<div grid="~ cols-2 gap-4">
<div>
<br>
For example, Australia/Sydney
You can find timezones here

- [IANA timezone database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
<br>
This is suitable for flight and virtual meetings

</div>
<div>
<br>
A sample API response:
<br>
```json
{
  "departure_time": "2023-09-24T11:00:00",
  "departure_timezone": "Australia/Sydney"
}
```
</div>
</div>

---
---
# Returning Date from API: 3
3. Encode timezone offset with the datefield
<div grid="~ cols-2 gap-4">
<div>
<br>
If you need to put into account the specific date and timezone with DST inclusive.
<br>
This is suitable for gallery apps that could use reminders etc

</div>
<div>
<br>
A sample API response:
<br>
```json
{
  "taken_at": "2023-01-020T18:30:01-05:00"
}
```
</div>
</div>

---
---
# Returning Date from API: 4
4. Use Unix timestamp - sparingly
<div grid="~ cols-2 gap-4">
<div>
<br>
System dates imply a scientific, linear view of time. Most programming languages internally represent dates as the number of seconds that have elapsed since that have elapsed since 00:00:00 UTC on 1 January 1970. This is known as Unix time and is a sensible choice for representing a date as a number.

- [Unix Time Stamp](https://www.unixtimestamp.com/)
<br>
This is suitable for flight and virtual meetings

</div>
<div>
<br>
A sample API response:
<br>
```json
{
  "created_at": 1682648103
}
```
</div>
</div>


---
layout: center
class: text-center
---

# Thank you.

[Resources](https://www.notion.so/Date-and-Time-for-Programmers-3c02a3057f1d4e47aed04018d0433589?pvs=4)
