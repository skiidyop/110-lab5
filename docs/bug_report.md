# Lemon Clicker Bug Report

**Name: Carmen Tan**<br>
**Tickets solved: 1** <!-- e.g. 1, 2, 3, 5, 6 (at least five) -->

Copy the template below once for each ticket you solved. 

---

## Ticket <number>: <ticket title>

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:**

**What was wrong:**

### Fix

<!-- What did you change, and why does that change fix the problem? -->

### How did you find it?

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** <!-- what you saw, e.g. text on screen, console error -->
2. **Traced back to:** <!-- where that came from, e.g. a function, a request in the Network tab, a server log -->
3. **Traced back to:**
4. **Buggy code:** <!-- the file, function and line where the trace ended -->

<!-- It is okay to use your judgement here in limiting the number of traceback steps. We want to understand your 
    reasoning for how you discovered where the bug was -->

### Confirming the fix

<!-- How did you confirm the problem is gone? -->


## Ticket 1: Stats always say "Stats unavailable"

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function: MenuScreenController.ts / loadStatsFromServer()**

**What was wrong: The reason the loadStatsFromServer() was fetching "/api/stat", but our server was looking for "/api/stats", so there would always be no response from the server.**

### Fix
I changed the fetch in loadStatsFromServer to match the server's get request for the stats. This fixed the problem because now the server gives the correct response.
<!-- What did you change, and why does that change fix the problem? -->

### How did you find it?

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** On the menu screen, I saw seeing Stats unavailable where a summary of the past scores should've been. In the console, there was an error, which said "Error: unable to load stats".
2. **Traced back to:** I looked for what function in the MenuScreen was throwing the error, and it was loadStatsFromServer()
4. **Buggy code:** MenuScreenController.ts/loadStatsFromServer() Line 47

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**
The hypothesis that ultimately led me to identify the buggy code was that maybe we weren't getting a response from the server because there was a typo in the resource we were trying to fetch.


### Confirming the fix

After reloading the menu screen, the stats appear under the play button instead of Stats unavailable.