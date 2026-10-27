# Python Activity 1: Case Studies (21–30)

This repository contains Python solutions, algorithms, pseudocode, and test specifications for **Python Activity 1 (Case Studies 21–30)** for Semester 1 (2026).

---

## 📋 Standard Case Study Structure

Each case study in this repository is structured into the following standardized sections:
1. **Problem Statement**
2. **Objectives**
3. **Assumptions**
4. **Algorithm**
5. **Flowchart**
6. **Pseudocode**
7. **Python Source Code**
8. **Test Cases**
9. **Code and Output Screenshots**
10. **Conclusion**

---

## 📚 Case Studies Index

| # | Case Study Title | Focus Area / Key Concepts |
|---|------------------|---------------------------|
| **21** | Traffic Violation Penalty Calculator | Loops, Conditionals, Dictionaries, Penalty Logic |
| **22** | Laboratory Chemical Inventory Validation | Input Validation, Dictionaries, Safety Rules |
| **23** | Student Timetable Conflict Detection | Nested Loops, Interval Overlap, List Analysis |
| **24** | Courier Charge Optimization | Volume Calculation, Limit Validation, Cost Comparison |
| **25** | Progressive Water Tariff Analysis | Slab Calculations, Functions, List Processing |
| **26** | Nested Loop Examination Seating Generator | 2D Seating Layout, Grid Printing, Set Logic |
| **27** | Advanced Numeric Pattern Generator | Loop Symmetry, Conditional Character Formatting |
| **28** | Repeated Transaction Detection | Duplication Flagging, Set Tracking, Threshold Filtering |
| **29** | Password Policy Validation Engine | String Processing, Rule Engine, Pattern Checking |
| **30** | Student Email and Identifier Validation | String Parsing, Formatting Rules, Error Flagging |

---

## ⚙️ Summary of Solutions

### Question 21: Traffic Violation Penalty Calculator
* **Description:** Processes vehicle violations, calculates total fines including repeat-offence surcharges, and checks for license disqualification conditions.
* **Key Rules:**
  * Base fines: `speeding` = ₹1000, `signal` = ₹1500, `no_helmet` = ₹500.
  * Repeat offence adds ₹500.
  * Disqualification triggered if a signal violation occurs or total fine reaches/exceeds ₹3000.
```python
rules = {"speeding": 1000, "signal": 1500, "no_helmet": 500}
total = 0
disqualified = False
vehicle = input("Vehicle (car/bike): ").lower()
n = int(input("Number of offences: "))

for i in range(n):
    offence = input("Offence: ").lower()
    repeat = input("Repeat? (yes/no): ").lower()
    if offence not in rules:
        print("Invalid offence; skipped")
        continue
    fine = rules[offence] + (500 if repeat == "yes" else 0)
    total += fine
    if offence == "signal" or total >= 3000:
        disqualified = True

print("Vehicle:", vehicle)
print("Total penalty: Rs.", total)
print("Disqualification:", "YES" if disqualified else "NO")
```

---

### Question 22: Laboratory Chemical Inventory Validation
* **Description:** Validates chemical records against max quantity limits and flags prohibited incompatible chemical combinations.
* **Key Rules:**
  * Limits: Acid (Corrosive, max 10L), Bleach (Oxidizer, max 5L), Ethanol (Flammable, max 8L).
  * Incompatibility warning triggered if both `acid` and `bleach` are present in the inventory.
```python
rules = {"acid": ("corrosive", 10), "bleach": ("oxidizer", 5), "ethanol": ("flammable", 8)}
records = []
n = int(input("Number of records: "))

for i in range(n):
    name = input("Chemical: ").lower()
    cat = input("Category: ").lower()
    qty = float(input("Quantity (L): "))
    records.append((name, cat, qty))

for name, cat, qty in records:
    if name not in rules:
        print(name, "UNKNOWN")
    elif cat != rules[name][0] or qty > rules[name][1]:
        print(name, "INVALID")
    else:
        print(name, "VALID")

names = [r[0] for r in records]
if "acid" in names and "bleach" in names:
    print("WARNING: acid and bleach are incompatible")
```

---

### Question 23: Student Timetable Conflict Detection
* **Description:** Checks for schedule overlap between course time intervals on the same day and flags assigned rooms that are marked unavailable.
```python
classes = [
    ("Python", "Mon", 540, 600, "R101"),
    ("Maths", "Mon", 590, 650, "R102"),
    ("English", "Tue", 600, 660, "R103")
]
unavailable = ["R102"]
bad = False

for i in range(len(classes)):
    for j in range(i + 1, len(classes)):
        a, b = classes[i], classes[j]
        if a[1] == b[1] and a[2] < b[3] and b[2] < a[3]:
            print("Time conflict:", a[0], "and", b[0])
            bad = True

for course, day, start, end, room in classes:
    if room in unavailable:
        print("Unavailable room:", room, "for", course)
        bad = True

print("Timetable is", "NOT valid" if bad else "valid")
```

---

### Question 24: Courier Charge Optimization
* **Description:** Calculates volumetric dimensions and total costs across shipping tiers, rejecting restricted parcels and selecting the cheapest eligible service option.
```python
weight = float(input("Weight kg: "))
l = float(input("Length cm: "))
w = float(input("Width cm: "))
h = float(input("Height cm: "))
zone = int(input("Zone (1-3): "))
urgent = input("Urgent? (yes/no): ").lower()
restricted = input("Restricted? (yes/no): ").lower()

fees = {1: 50, 2: 100, 3: 150}
services = {"Standard": (10, 50000, 40), "Express": (5, 30000, 80)}
options = []
volume = l * w * h

if zone in fees and restricted == "no":
    for name, (mw, mv, base) in services.items():
        if weight <= mw and volume <= mv:
            cost = base + weight * 20 + fees[zone] + (100 if urgent == "yes" else 0)
            options.append((cost, name))

if options:
    options.sort()
    for cost, name in options:
        print(name, "charge: Rs.", cost)
    print("Cheapest service:", options[0][1])
else:
    print("No eligible service")
```

---

### Question 25: Progressive Water Tariff Analysis
* **Description:** Calculates household water bills using a tiered slab system, fixed base costs, high-consumption penalties, and identifies the maximum consumer.
```python
def bill(u):
    total = 50 + min(u, 10) * 2
    if u > 10:
        total += min(u - 10, 10) * 3
    if u > 20:
        total += (u - 20) * 5
    if u > 30:
        total += 100
    return total

homes = []
n = int(input("Households: "))
for i in range(n):
    name = input("Name: ")
    units = float(input("Units: "))
    homes.append((name, units, bill(units)))

for name, units, amount in homes:
    print(name, "Units:", units, "Bill: Rs.", amount)
    if units > 30:
        print("Penalty applied")

if homes:
    top = max(homes, key=lambda x: x[1])
    print("Highest consumption:", top[0], top[1], "units")
```

---

### Question 26: Nested Loop Examination Seating Generator
* **Description:** Generates a seating arrangement grid while handling reserved seating coordinates and alternate empty column placement.
```python
rows = int(input("Rows: "))
cols = int(input("Seats per row: "))
alternate = input("Alternate seats empty? (yes/no): ").lower()
reserved = {(1, 2)}

if rows <= 0 or cols <= 0:
    print("Rows and columns must be positive")
else:
    for r in range(1, rows + 1):
        for c in range(1, cols + 1):
            if (r, c) in reserved:
                seat = "RES"
            elif alternate == "yes" and c % 2 == 0:
                seat = "EMPTY"
            else:
                seat = f"R{r}C{c}"
            print(f"{seat:8}", end="")
        print()
```

---

### Question 27: Advanced Numeric Pattern Generator
* **Description:** Validates requested size input and constructs a hollow symmetric diamond shape using string multiplication and conditional formatting.
```python
n = int(input("Diamond size: "))
if n <= 0:
    print("Enter a positive size")
else:
    for i in range(1, n + 1):
        s = " " * (n - i)
        print(s + "*" if i == 1 else s + "*" + " " * (2 * i - 3) + "*")
    for i in range(n - 1, 0, -1):
        s = " " * (n - i)
        print(s + "*" if i == 1 else s + "*" + " " * (2 * i - 3) + "*")
```

---

### Question 28: Repeated Transaction Detection
* **Description:** Scans transaction records to flag repeated transaction IDs (using set tracking) and identify high-value amounts exceeding a designated threshold.
```python
transactions = [("T101", 500), ("T102", 2500), ("T101", 500), ("T103", 1200)]
threshold = 2000
seen = set()

for tid, amount in transactions:
    reasons = []
    if tid in seen:
        reasons.append("duplicate ID")
    else:
        seen.add(tid)
    if amount > threshold:
        reasons.append("amount above threshold")
    if reasons:
        print(tid, "Rs.", amount, "-", ", ".join(reasons))
```

---

### Question 29: Password Policy Validation Engine
* **Description:** Checks candidate passwords against complexity criteria (length, character types, consecutive repetition rules, and blacklisted substrings).
```python
p = input("Sample password: ")
failed = []

if len(p) < 8:
    failed.append("Minimum length is 8")
if not any(c.isupper() for c in p):
    failed.append("Add uppercase")
if not any(c.islower() for c in p):
    failed.append("Add lowercase")
if not any(c.isdigit() for c in p):
    failed.append("Add digit")
if not any(not c.isalnum() for c in p):
    failed.append("Add special character")
if any(p[i] == p[i+1] == p[i+2] for i in range(len(p) - 2)):
    failed.append("Avoid 3 repeated characters")
if "password" in p.lower():
    failed.append("Avoid word password")

if failed:
    print("Password does not meet policy:")
    for item in failed:
        print("-", item)
else:
    print("Password meets sample policy")
```

---

### Question 30: Student Email and Identifier Validation
* **Description:** Validates structural formatting for email addresses (presence of single `@`, valid domain dot, no spaces) and registration IDs (alphanumeric and hyphen characters only).
```python
email = input("Student email: ").strip()
sid = input("Student ID: ").strip()
errors = []

if email.count("@") != 1:
    errors.append("Email must contain exactly one @")
else:
    user, domain = email.split("@")
    if not user:
        errors.append("Username missing")
    if not domain or "." not in domain:
        errors.append("Invalid domain")
    if " " in email:
        errors.append("Email must not contain spaces")

if not sid:
    errors.append("Student ID missing")
elif not all(c.isalnum() or c == "-" for c in sid):
    errors.append("Invalid student ID characters")

if errors:
    print("Invalid details:")
    for e in errors:
        print("-", e)
else:
    print("Email and student ID are valid")
```

---

## 🚀 Execution Instructions

1. Ensure Python 3.x is installed on your system.
2. Run any case study file directly via terminal:
   ```bash
   python code1.py
   ```