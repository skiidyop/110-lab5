# Lemon Clicker Bug Report

**Name: Carmen Tan**<br>
**Tickets solved: 1, 2, 3** <!-- e.g. 1, 2, 3, 5, 6 (at least five) -->

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

---

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

---

## Ticket 3: The lemon only lands in four spots

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:** GameScreenView.ts / randomizeLemonPosition()

**What was wrong:** When calculating the random X and Y positions of the lemon, the function was immediately rounding the result of Math.random(). Since Math.random() generates a decimal between 0 and 1, rounding it would always result in either 0 or 1. This value was then multiplied by the canvas dimensions and added to the minimum position values. As a result, the lemon could only appear at the minimum or maximum positions instead of anywhere within the canvas bounds.

### Fix

I moved Math.round() to encapsulate the entire calculation instead of just Math.random(). This fixed the problem because the random decimal was now scaled to fit the canvas bounds before being rounded. As a result, the lemon can now appear at any integer position within the specified range instead of only at the minimum or maximum positions.
<!-- What did you change, and why does that change fix the problem? -->

### How did you find it?

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**
The hypothesis that ultimately led me to identify the buggy code was that the math used to calculate the random positions of the lemon was incorrect. On the game screen, I noticed that the lemon was only spawning in the four corners of the canvas. This made me think that the random position calculations were only producing the minimum or maximum values for the X and Y coordinates. I looked for the code that was generating the lemon's random spawn locations and found the randomizeLemonPosition() function in GameScreenView.ts. After looking at the calculations, and I noticed that Math.random() was being rounded before being scaled.

### Confirming the fix

Replaying the game, the lemon spawns randomly within the correct bounds.

---

## Ticket 4: Stuck on the Game Over screen

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:** GameScreenController.ts / endGame()

**What was wrong:** The function was trying to call stopTimer() on the game timer interval, but the parantheses were omitted. This caused the timer to keep running. Since the timer would check every second that timeRemaining was less than or equal to 0, once the timeRemaining dropped below 0, it would keep running every second and call endGame() over and over. endGame() switches the screen to the results screen, so everytime it is called it switches to the results screen which is why you get stuck.

### Fix

I added the parantheses to the stopTimer() call. This fixed the problem because now endGame is called only once, and stops the timer before it's called again. 

### How did you find it?

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** It looked as if the results screen was constantly refreshing. The leaderboard was updating and being filled with the score as well.
2. **Traced back to:** ResultsScreenController showResultsFromServer() I knew this function was being called repeatedly because it is the function responsible for both updating the leadboard and showing the results view.
3. **Traced back to:** main.ts switchToScreen() calls ResultsScreenController
4. **Traced back to:** types.ts interface ScreenSwitcher calls switchToScreen()
5. **Traced back to:** I knew that GameScreenController.ts was responsible for switching to the Results Screen after the game was over, which took me to endGame()
4. **Buggy code:** GameScreenController.ts endGame() Line 87

### Confirming the fix

The results screens only shows once and the ending game bell no longer replays forever.