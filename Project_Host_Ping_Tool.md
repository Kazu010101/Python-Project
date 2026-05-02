---
title: "Host Pinger – Interactive Network Reachability Checker"
tags:
  - python
  - networking
  - ping
  - automation
  - soc-analyst
  - cybersecurity
  - portfolio
  - capstone
created: 2024
status: complete
language: Python
library: ping3
---

# Capstone Project: Host Pinger – Interactive Network Reachability Checker

> **Type:** Capstone Project — Python Network Automation
> **Library:** `ping3`
> **Skills:** Lists, loops, input handling, indexing, exception handling
> **SOC Relevance:** Host availability checking, network triage, asset reachability verification

---

## Table of Contents

1. [[#Scenario]]
2. [[#How It Works]]
3. [[#The Script]]
4. [[#Python Skills Demonstrated]]
5. [[#Possible Improvements]]
6. [[#Related Concepts]]

---

## Scenario

> **Capstone Scenario — SOC Analyst: Rapid Host Triage During an Incident**
>
> It's 2:00 AM. A security alert fires in the SIEM; a group of internal servers have been flagged for unusual outbound traffic. You need to immediately verify which of these hosts are **still reachable on the network** before escalating to the IR team.
>
> You don't want to open five separate terminal tabs or remember the exact hostname for each server. Instead, you launch this script, enter the list of suspicious hostnames one by one, and then rapidly ping each one by number, just like a numbered checklist.
>
> Within 60 seconds, you have a clear picture:
> - Which hosts are **up and responding** (with exact response times logged)
> - Which hosts have gone **dark** (ping timeout — potential sign of shutdown, isolation, or compromise)
> - Which hostnames **can't be resolved** (DNS failure — possibly a decommissioned or fake asset)
>
> This information feeds directly into your incident ticket and determines your next action: isolate, investigate, or escalate.

---

## How It Works

The script runs in three phases:

```
Phase 1 — Collect
  User enters hostnames one by one → stored in a list
  Press ENTER with no input → collection ends

Phase 2 — Display & Select
  Numbered list of all stored hostnames is printed
  User picks a number → script pings that host
  Loop repeats until user presses ENTER to quit

Phase 3 — Ping & Report
  ping3 sends an ICMP ping to the chosen host
  Displays response time in ms, or a specific error message
```

**Program flow diagram:**

```
START
  │
  ▼
Enter hostnames (loop)
  │
  ├─ No hostnames entered? ──► Print warning ──► END
  │
  ▼
Display numbered list (loop)
  │
  ├─ User presses ENTER ──────────────────────► Print "Goodbye" ──► END
  │
  ├─ Invalid number / not an integer ─────────► Print error, loop back
  │
  └─ Valid number selected
        │
        ├─ Ping succeeds ──► Print response time in ms
        ├─ Timeout ────────► Print "Ping Timed-out"
        └─ Host unknown ───► Print "Host unknown"
              │
              └──────────────────────────────────► Loop back to list
```

---

## The Script

```python
import ping3
ping3.EXCEPTIONS = True

# Set an empty list, prompt user to repeatedly enter and store hostnames until the user presses ENTER to quit
hostnameList = []
hostName = input("Please enter a host name, or press ENTER to quit: ")
while hostName != '':
    hostnameList.append(hostName)
    hostName = input("Please enter a host name, or press ENTER to quit: ")

# If user presses ENTER at the beginning, the program ends.
if not hostnameList:
    print("You haven't entered any host names.")
    print("Goodbye")

# Loop to display hostnameList & index number of each hostname
else:
    while True:
        count = 0
        for n in hostnameList:
            print(str(count) + ". " + n)
            count = count + 1
# prompt user to input an index number to ping & exit the program if presses enter.
        selection = input("Please enter the number of the desired host to ping (or press ENTER to quit): ")
        if selection == '':
            break
# Specify that the valid user's input to choose is only from the hostnameList
        try:
            selection = int(selection)
            if 0 <= selection < len(hostnameList):
                PingHost = hostnameList[selection]
# Make a ping attempt on the hostname, handle & show successful ping.
                try:
                    pingTime = ping3.ping(PingHost)
                    if pingTime is not None:
                        print("Ping to '" + PingHost + "': " + str(pingTime) + " ms")
# Handle errors of ping-timeout, host-unknown, invalid index, and value-error
                except ping3.errors.Timeout:
                    print("Ping Timed-out")
                except ping3.errors.HostUnknown:
                    print("Host '" + PingHost + "' unknown")
            else:
                print("Invalid index: index must be 0.." + str(len(hostnameList) - 1))
        except ValueError:
            print("Invalid index: index must be an integer.")
# message printed when the user exiting the script
    print("Goodbye")
```

**Installation:**
```bash
pip install ping3
```

> **Permissions Note**
> On Linux/macOS, `ping3` may require elevated privileges to send ICMP packets.
> Run with `sudo python3 script.py` if you get a permission error.
> On Windows, run the terminal as Administrator.

---

## Python Skills Demonstrated

### 1. Third-Party Library Import & Configuration
**Lines 1–2**
```python
import ping3
ping3.EXCEPTIONS = True
```
`ping3` is an external library installed via `pip`. Setting `ping3.EXCEPTIONS = True` switches the library from returning `None` on errors to raising proper Python exceptions — which is required for the `try/except` blocks later to work correctly. 

---

### 2. List Initialisation & `.append()`
**Lines 10–14**
```python
hostnameList = []
hostName = input("...")
while hostName != '':
    hostnameList.append(hostName)
    hostName = input("...")
```
An empty list is created first, then items are added to it one at a time using `.append()`. This is the standard pattern for building a list dynamically from user input. The list grows with every valid entry the user provides.

---

### 3. `while` Loop with a Sentinel Value
**Lines 12–14 and Lines 22–23**
```python
while hostName != '':
    ...

while True:
    ...
    if selection == '':
        break
```
Two different `while` loop patterns are used:
- A **condition-based loop** (`while hostName != ''`) that exits when the sentinel value (empty string) is detected
- An **infinite loop** (`while True`) that exits via a `break` statement when the user presses ENTER

---

### 4. Empty List Check with `if not`
**Lines 17–19**
```python
if not hostnameList:
    print("You haven't entered any host names.")
```
`if not hostnameList` evaluates to `True` when the list is empty. This is more Pythonic than writing `if len(hostnameList) == 0`. It acts as a **guard clause** — catching the edge case before the main logic runs.

---

### 5. `for` Loop with a Manual Counter
**Lines 24–27**
```python
count = 0
for n in hostnameList:
    print(str(count) + ". " + n)
    count = count + 1
```
A `for` loop iterates over each item in the list, while a manual `count` variable tracks the index number for display. This produces the numbered menu. An alternative Python approach would be `enumerate()`, but the manual counter demonstrates clear understanding of loop mechanics.

---

### 6. List Indexing & Bounds Checking
**Lines 34–36**
```python
selection = int(selection)
if 0 <= selection < len(hostnameList):
    PingHost = hostnameList[selection]
```
The user's input is converted to an integer and used as a **list index**. The bounds check `0 <= selection < len(hostnameList)` ensures the number actually exists in the list before attempting to access it — preventing an `IndexError`. This is a chained comparison, a clean Python feature.

---

### 7. Nested `try/except` Exception Handling
**Lines 33–50**

The script uses **two levels** of exception handling, one inside the other:

```
Outer try/except
  └─ Catches ValueError (non-integer input like "abc")
       │
       └─ Inner try/except
            ├─ Catches ping3.errors.Timeout
            └─ Catches ping3.errors.HostUnknown
```

| Exception | Trigger | Response |
|---|---|---|
| `ValueError` | User types "abc" instead of a number | `"Invalid index: index must be an integer."` |
| `IndexError` (via bounds check) | Number outside list range | `"Invalid index: index must be 0..N"` |
| `ping3.errors.Timeout` | Host unreachable / not responding | `"Ping Timed-out"` |
| `ping3.errors.HostUnknown` | DNS can't resolve the hostname | `"Host '...' unknown"` |

This layered approach means the program **never crashes from user error** — every failure path is caught and communicated clearly.

---

### 8. String Conversion with `str()`
**Lines 26, 40, 48**
```python
print(str(count) + ". " + n)
print("Ping to '" + PingHost + "': " + str(pingTime) + " ms")
print("Invalid index: index must be 0.." + str(len(hostnameList) - 1))
```
Python cannot concatenate strings and integers directly. `str()` converts numbers to strings so they can be joined with `+`. This appears three times in the script for display purposes.

---

### Skills Summary Table

| Skill | Lines | Concept |
|---|---|---|
| Library import & config | 1–2 | `import`, modifying library globals |
| List creation & append | 10–14 | `[]`, `.append()` |
| `while` loop (sentinel) | 12–14 | Input-controlled loop exit |
| `while True` + `break` | 22–23, 30–31 | Infinite loop with manual exit |
| Empty list guard | 17–19 | `if not list` truthiness check |
| `for` loop + counter | 24–27 | Iteration with manual index tracking |
| List indexing | 35–36 | `list[index]`, bounds validation |
| Nested exception handling | 33–50 | `try/except`, multiple exception types |
| Type conversion | 26, 34, 40, 48 | `int()`, `str()` |

---



