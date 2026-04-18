
# 🛠️ Scenario: Saskatoon – Counting IPs

## 📌 Problem

Given a web server access log, identify the IP address with the highest number of requests.

Scenario from: SadServers

---

## 🧠 Objective

Efficiently process a log file to:

* Extract IP addresses
* Count occurrences
* Identify the most frequent IP

---

## 🔍 Initial Approach

```bash
awk '{print $1}' access.log | uniq -c | sort
```

### ❌ Why This Failed

The `uniq` command only counts **consecutive identical lines**.

Example:

```
192.168.1.1
10.0.0.1
192.168.1.1
```

Here, `uniq`:

* Counts first `192.168.1.1`
* Stops when it sees `10.0.0.1`
* Treats the next `192.168.1.1` as a new entry

➡️ Result: Incorrect counts

---

## 🧪 Investigation & Insight

* `uniq` does **not group all identical values globally**
* It only works on **adjacent (sorted) data**
* Therefore, sorting is required before using `uniq`

---

## ✅ Correct Solution

```bash
awk '{print $1}' /home/admin/access.log | sort | uniq -c | sort -nr | head -n 1
```

---

## 🔎 Command Breakdown

* `awk '{print $1}'` → Extract IP addresses
* `sort` → Group identical IPs together
* `uniq -c` → Count occurrences
* `sort -nr` → Sort numerically (descending)
* `head -n 1` → Get the most frequent IP

---

## 💡 Key Learning

* `uniq` requires **sorted input** to work correctly
* Order of commands in pipelines is critical in Linux
* Small misunderstandings in tools can lead to incorrect results

---

## ⚡ Performance

* ⏱️ Solved in: **12 minutes 27 seconds**

---

## 🎥 Recording

Terminal session recorded using asciinema:
👉 Add your recording file here: `session.cast`

---

## 🧩 Real-World Relevance

This pattern is commonly used for:

* Detecting abusive IPs
* Analyzing traffic patterns
* Debugging high-load systems

---

## 🚀 Improvements / Alternatives

* Use `sort | uniq -c | sort -nr` as a standard pattern for frequency analysis
* For very large files, consider:

  * `awk` with associative arrays
  * Tools like `sort --parallel` for performance

---

## 📚 Takeaway

Understanding how Unix tools behave (like `uniq`) is just as important as knowing the commands themselves.
