# Panic Attack App — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a single `panic.html` file that shows one random calm-down technique per click, zero dependencies, works offline.

**Architecture:** All content, styles, and logic in one HTML file. A JS array holds ~20 techniques. Fisher-Yates shuffle cycles through all before repeating. Two UI states: landing (big button) and technique display (title + steps + next button).

**Tech Stack:** Vanilla HTML5, CSS3, JavaScript (ES6). No frameworks, no build step, no internet required.

---

### Task 1: Scaffold the HTML shell

**Files:**
- Create: `panic.html`

**Step 1: Create the file with the base HTML structure**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>You're okay</title>
  <style>
    /* styles go here */
  </style>
</head>
<body>
  <!-- content goes here -->
  <script>
    // logic goes here
  </script>
</body>
</html>
```

**Step 2: Verify**

Open `panic.html` in a browser. Should show a blank white page with title "You're okay".

**Step 3: Commit**

```bash
git add panic.html
git commit -m "feat: scaffold panic.html shell"
```

---

### Task 2: Add CSS — ultra-minimal monochrome

**Files:**
- Modify: `panic.html` (style block)

**Step 1: Write the styles**

Replace the `/* styles go here */` comment with:

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html, body {
  height: 100%;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  background: #fff;
  color: #000;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 2rem;
}

#landing {
  text-align: center;
}

#technique {
  display: none;
  max-width: 600px;
  width: 100%;
}

h2 {
  font-size: clamp(1.5rem, 4vw, 2.5rem);
  font-weight: 700;
  margin-bottom: 2rem;
  line-height: 1.2;
}

.steps {
  font-size: clamp(1rem, 2.5vw, 1.25rem);
  line-height: 1.8;
  margin-bottom: 3rem;
  white-space: pre-line;
}

button {
  font-family: inherit;
  font-size: clamp(1rem, 2.5vw, 1.25rem);
  font-weight: 600;
  background: #000;
  color: #fff;
  border: none;
  padding: 1rem 2.5rem;
  cursor: pointer;
  letter-spacing: 0.02em;
}

button:hover {
  background: #333;
}

#next-btn {
  display: block;
  margin-top: 2rem;
}
```

**Step 2: Verify**

Reload the page. Should still be blank (no HTML content yet) but no errors in console.

**Step 3: Commit**

```bash
git add panic.html
git commit -m "feat: add monochrome CSS styles"
```

---

### Task 3: Add HTML structure — two states

**Files:**
- Modify: `panic.html` (body content)

**Step 1: Replace `<!-- content goes here -->` with**

```html
<div id="landing">
  <button id="start-btn">I'm having a panic attack</button>
</div>

<div id="technique">
  <h2 id="technique-title"></h2>
  <div class="steps" id="technique-steps"></div>
  <button id="next-btn">Next technique</button>
</div>
```

**Step 2: Verify**

Reload. Should show one large black button centered on a white page. The technique div is hidden.

**Step 3: Commit**

```bash
git add panic.html
git commit -m "feat: add landing and technique HTML structure"
```

---

### Task 4: Add technique content (JS array)

**Files:**
- Modify: `panic.html` (script block)

**Step 1: Replace `// logic goes here` with the techniques array**

```js
const techniques = [
  // Breathing
  {
    title: "Box Breathing",
    steps: "Breathe in for 4 counts.\nHold for 4 counts.\nBreathe out for 4 counts.\nHold for 4 counts.\n\nRepeat 4 times.\nYou're doing great."
  },
  {
    title: "4-7-8 Breath",
    steps: "Breathe in quietly through your nose for 4 counts.\nHold your breath for 7 counts.\nExhale completely through your mouth for 8 counts.\n\nRepeat 3 times."
  },
  {
    title: "Slow Exhale",
    steps: "Breathe in normally.\nNow breathe out slowly — make it twice as long as the inhale.\n\nDo this 6 times.\nSlow exhales activate your body's rest response."
  },

  // Grounding
  {
    title: "5-4-3-2-1 Grounding",
    steps: "Name 5 things you can see.\nName 4 things you can physically feel.\nName 3 things you can hear.\nName 2 things you can smell.\nName 1 thing you can taste.\n\nTake your time with each one."
  },
  {
    title: "Feet on the Floor",
    steps: "Press both feet flat on the floor.\nFeel the ground under you.\nNotice the texture, the temperature, the pressure.\n\nYou are here. You are supported.\nStay with this feeling for 30 seconds."
  },
  {
    title: "Hold Something Cold",
    steps: "Find something cold — ice, a cold drink, cold water.\nHold it in your hands.\nFocus entirely on the sensation.\n\nThe cold brings you back to the present moment.\nStay with it until your breathing slows."
  },
  {
    title: "Body Scan",
    steps: "Start at the top of your head.\nSlowly move your attention down through your body.\n\nNotice: forehead, jaw, shoulders, chest, stomach, hands, legs, feet.\n\nAt each part: notice the sensation. Don't try to change it."
  },
  {
    title: "Name What You See",
    steps: "Look around and silently name 10 objects you can see.\n\nBe specific: not 'chair' but 'wooden chair with blue cushion'.\n\nThis pulls your brain out of panic and into the present."
  },

  // Physical
  {
    title: "Clench and Release",
    steps: "Make tight fists with both hands.\nHold for 5 seconds.\nRelease completely.\n\nNow tense your shoulders up to your ears.\nHold for 5 seconds.\nRelease.\n\nRepeat 3 times. Feel the difference."
  },
  {
    title: "Splash Cold Water",
    steps: "Go to a sink.\nSplash cold water on your face — especially your forehead and cheeks.\n\nThis triggers the dive reflex and slows your heart rate.\nPat your face dry slowly."
  },
  {
    title: "Run in Place",
    steps: "Run in place for 30 seconds.\nLet your body burn off the adrenaline.\n\nPanic primes your body to fight or flee.\nGive it somewhere to go.\n\nStop. Breathe. Notice how you feel."
  },
  {
    title: "Shake It Out",
    steps: "Stand up if you can.\nShake your hands like you're flicking water off them.\nThen shake your arms. Your legs. Your whole body.\n\n30 seconds of shaking.\nAnimals do this after stress. It works."
  },

  // Cognitive
  {
    title: "This Will Pass",
    steps: "A panic attack cannot last forever.\nYour body cannot sustain this level of arousal.\n\nThe average panic attack peaks in 10 minutes.\nYou have survived every single one before this.\n\nThis one will pass too."
  },
  {
    title: "Panic Is Not Danger",
    steps: "Panic feels like danger. It is not danger.\n\nYour heart is beating fast: that's adrenaline, not a heart attack.\nYou feel lightheaded: that's shallow breathing, not dying.\n\nThe alarm is loud. The alarm is wrong.\nYou are safe."
  },
  {
    title: "Name 3 True Facts",
    steps: "Say three things that are true right now.\n\nFor example:\n'I am sitting in my living room.'\n'It is Tuesday afternoon.'\n'I can hear cars outside.'\n\nFacts are an anchor. Use them."
  },
  {
    title: "What Would You Tell a Friend?",
    steps: "If your best friend was feeling exactly what you're feeling right now, what would you say to them?\n\nSay it to yourself.\nOut loud if you can.\n\nYou deserve the same compassion you'd give them."
  },

  // Affirmations
  {
    title: "You Have Done This Before",
    steps: "Every panic attack you've ever had has ended.\n\nEvery single one.\n\nYou are still here.\nYou are stronger than this feeling.\n\nThis one ends too."
  },
  {
    title: "You Are Allowed to Slow Down",
    steps: "You don't have to keep going right now.\n\nYou are allowed to stop.\nYou are allowed to rest.\nYou are allowed to take up space.\n\nSlow down.\nYou have permission."
  },
  {
    title: "Soften Something",
    steps: "Notice where you're holding tension in your body.\nYour jaw? Your shoulders? Your hands?\n\nJust soften it slightly. You don't have to let go completely.\nJust a little softer.\n\nAnd breathe."
  },
  {
    title: "One Thing at a Time",
    steps: "You don't have to solve everything right now.\n\nYou only have to do one thing:\nGet through the next minute.\n\nThat's it.\nJust the next minute.\nYou can do that."
  }
];
```

**Step 2: Verify**

Open browser console, type `techniques.length`. Should return `20`.

**Step 3: Commit**

```bash
git add panic.html
git commit -m "feat: add 20 calm-down techniques"
```

---

### Task 5: Add shuffle logic and UI interactions

**Files:**
- Modify: `panic.html` (script block, after the array)

**Step 1: Add the shuffle + interaction code after the `techniques` array**

```js
// Fisher-Yates shuffle
function shuffle(arr) {
  const a = [...arr];
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

let deck = [];
let index = 0;

function nextTechnique() {
  if (index >= deck.length) {
    deck = shuffle(techniques);
    index = 0;
  }
  return deck[index++];
}

function showTechnique(t) {
  document.getElementById('technique-title').textContent = t.title;
  document.getElementById('technique-steps').textContent = t.steps;
  document.getElementById('landing').style.display = 'none';
  document.getElementById('technique').style.display = 'block';
}

document.getElementById('start-btn').addEventListener('click', () => {
  showTechnique(nextTechnique());
});

document.getElementById('next-btn').addEventListener('click', () => {
  showTechnique(nextTechnique());
});
```

**Step 2: Verify manually**

- Open `panic.html` in browser
- The landing button is visible, centered
- Click "I'm having a panic attack" — technique appears
- Click "Next technique" — new technique appears
- Keep clicking — all 20 eventually appear, then cycle repeats
- No console errors

**Step 3: Commit**

```bash
git add panic.html
git commit -m "feat: add shuffle logic and button interactions"
```

---

### Task 6: Final polish — add a subtle back option

**Files:**
- Modify: `panic.html` (CSS + HTML + JS)

**Step 1: Add a small "Start over" link below the next button**

In the HTML, after the `#next-btn` button:
```html
<button id="next-btn">Next technique</button>
<a id="back-link" href="#">start over</a>
```

**Step 2: Add CSS for the link**

```css
#back-link {
  display: block;
  margin-top: 1.5rem;
  font-size: 0.85rem;
  color: #999;
  text-decoration: none;
  text-align: center;
}
#back-link:hover { color: #000; }
```

**Step 3: Add JS handler**

```js
document.getElementById('back-link').addEventListener('click', (e) => {
  e.preventDefault();
  document.getElementById('technique').style.display = 'none';
  document.getElementById('landing').style.display = 'block';
});
```

**Step 4: Verify**

- Click the main button, see a technique
- Click "start over" link — returns to landing screen
- Click main button again — shows a technique (not the same cycle position)

**Step 5: Commit**

```bash
git add panic.html
git commit -m "feat: add start over link for navigation back to landing"
```

---

### Task 7: Done — open and bookmark

**Step 1: Open the file in your default browser**

```bash
open /Users/david.scebat/Documents/panic-app/panic.html
```

**Step 2: Bookmark it** — drag the tab to your bookmarks bar or save as bookmark named "panic" for one-click access.

**Step 3: Optional — add to Dock as app** (macOS)
- In Safari: File → Add to Dock
- This gives you a near-native icon you can click without opening the browser first
