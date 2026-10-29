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

**File / function:** MenuScreenController.ts / loadStatsFromServer()

**What was wrong:** The reason the loadStatsFromServer() was fetching "/api/stat", but our server was looking for "/api/stats", so there would always be no response from the server.

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


## Ticket 2: Best score always shows 0

### Actual cause


<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:** server/index.ts, in the app.get() for "app/stats"

**What was wrong:** When finding the best score, the function was checking if each score was less than the current best score. This caused it to never update best score because it was initialized to zero, and since all of the scores are positive, the check never passed and best was never updated.

### Fix

 I corrected this by changing it to check if each score was greater than the running best score. This fixed the behavior because we want the running best to update whenever we find a value bigger than it, so once we search through all the scores, we will have the biggest value saved into best.
<!-- What did you change, and why does that change fix the problem? -->

### How did you find it?

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?** The hypothesis that ultimately led me to find the code was that whatever was calculating the best score was either never running, or calculating it wrong.

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** On the menu screen, the stats that showed a summary of the scores was always showing "Best: 0", even when the average score was above that. There were no errors in the console.
2. **Traced back to:** I looked in MenuScreenController.ts for any function related to the stats. I started from refreshStats() and printed the results from loadStatsFromServer() to see if the object that the server was responding with had a 0 for Best. It did, which confirmed that the bug was coming from the fetch request.
3. **Traced back to:** MenuScreenController.ts/loadStatsFromServer() Line 47
4. **Traced back to:** server/index.ts I looked at the code handling the get request for the stats. The best score was initialized to 0, so I looked for any other lines trying to update it. I saw that line 54 was trying to update the best score, but was clearly never running, so I looked at the if statement encapsulating it.
4. **Buggy code:** server/index.ts Line 53

### Confirming the fix

After reloading the page, the best score was no longer displaying 0. To confirm it was actually showing the best score, I modified one of the objects in leaderboard.json to have a score greater than all the others. Reloading the page showed this updated best score, which confirmed the fix.