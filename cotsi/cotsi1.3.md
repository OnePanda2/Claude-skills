---
name: cotsi
description: >
  Cotsi (Complex-to-Simple) explains any complex topic, book, or LeetCode problem using analogies
  from famous TV sitcoms and movies. Trigger when the user types /cotsi followed by a topic or
  book name. Also trigger for /cotsi change character to [name] from [show]. Soft triggers:
  "explain this like I'm watching TV", "break this down like a sitcom", "teach me X the fun way".
  Always trigger for any /cotsi command — never handle inline without reading this skill.
  Also handles ALL LeetCode problem solving: use when the user pastes a LeetCode problem, asks
  for an optimized algorithm, wants Python LeetCode code, needs a dry run, asks for line-by-line
  explanation, or wants help with data structures and algorithms for coding interviews.
  LeetCode technical sections (code, dry run, complexity, edge cases) stay standard — the Approach
  section uses Cotsi sitcom-dialogue style to make the intuition memorable and sticky.
---

# Cotsi — Complex to Simple

Cotsi turns any complex topic or book into a multi-part sitcom-style explanation, using character
analogies, dialogue, and scene-setting to make ideas stick. It also embeds memory hacks throughout
so the user doesn't just understand — they *remember*.

---

## Trigger Format

```
/cotsi [topic or book name]
/cotsi change character to [character name] from [show/movie name]
```

---

## Step 0 — Check for Saved Character Preferences

Before doing anything else, check Claude's memory for a saved Cotsi character preference.

- If a preference **exists in memory**: skip asking, go straight to Step 2.
- If **no preference exists**: go to Step 1.

---

## Step 1 — Ask for Character Preference (first time only)

Ask the user once:

> "Before I begin — which TV sitcom or movie characters should I use to explain this?
> You can name a show (e.g. *The Office*, *Friends*, *Breaking Bad*) or specific characters.
> If you'd like, I'll default to **The Office US**, **The Big Bang Theory**, and **Friends**."

Wait for their reply before proceeding.

Once they confirm (or accept the default), **save the preference to memory** using this format:

> `Cotsi character preference: [User's chosen shows/characters]`

From this point forward, never ask again — use memory. If the user sends:
`/cotsi change character to [X] from [Y]` — update the memory entry and confirm the change.

---

## Step 2 — Understand the Topic

Identify what the user wants explained:
- A **book** → explain chapter by chapter (or part by part for long books)
- A **concept or subject** → break it into logical subtopics and explain each like a chapter
- A **technical topic** → break into fundamentals → mechanics → implications

If the scope is genuinely unclear (e.g. "/cotsi Python"), ask one focused question:
> "Do you want a broad intro to Python, or a specific part — like data structures, functions, or OOP?"

---

## Step 3 — Set the Scene and Begin Explaining

Open with a **1–2 line scene-setter** to ground the user in the sitcom world. Then begin.

### Explanation Rules

**Use the saved character preference throughout.** Rotate characters to match the concept:
- Use a "confused but curious" character (Penny, Michael Scott, Joey) to ask the naive question
- Use a "smart explainer" character (Sheldon, Leonard, Jim, Chandler) to give the answer
- Use "real world stakes" characters (Jan, Ryan, Dwight) to show why it matters

**Structure each chapter/section like this:**

```
### Chapter [N]: [Title]

[Scene: 1-2 lines setting up the situation in the show's world]

**[Character A]:** [Confused question or naive framing of the concept]

**[Character B]:** [Simple, analogy-rich explanation using show-world objects/situations]

**[Character A]:** [Follow-up that forces a deeper clarification]

**[Character B]:** [The deeper explanation — still in character]

---

🧠 **Memory Hack:** [A mnemonic, acronym, visual anchor, or rhyme to lock this in]
```

**Key rules for analogies:**
- Ground every abstraction in something from the show's world (the Dunder Mifflin budget, Sheldon's roommate agreement, Central Perk rent, Ross's dinosaur fossils)
- Never use jargon without first having a character "discover" what it means through dialogue
- One core analogy per chapter — don't mix metaphors

### Visual Layer (after every chapter's dialogue + memory hack)

After the memory hack in each chapter, generate an **animated HTML visual** using `show_widget`
that reinforces the concept just explained in dialogue. This is mandatory — every chapter gets one.

**What the visual must show:**
- The core concept rendered as a clean, step-by-step animated diagram
- State changes shown explicitly (values moving, boxes highlighting, arrows appearing)
- Labels in plain English — no raw variable names, no code syntax inside the visual
- A title bar showing the chapter name and the concept being visualized
- A "Step X / N" counter so the user always knows where they are
- Prev / Next buttons so the user can step through at their own pace

**Visual style rules:**
- Dark background `#0f1117`, accent `#7c6af7`, success `#22c55e`, highlight `#f59e0b`
- Smooth CSS transitions 300–500ms, no jarring jumps
- Font: monospace for values, sans-serif for labels
- Max-width 680px, stack vertically on mobile
- No external libraries — pure HTML + CSS + JS only

**What to visualize per concept type:**

| Concept Type | What to Animate |
|---|---|
| Data structures (arrays, stacks, queues, trees) | Boxes/nodes with pointer arrows, push/pop animations |
| Algorithms (sorting, searching) | Array cells highlighting and swapping step by step |
| Math / formulas | Variables updating live as the formula runs on an example |
| System concepts (memory, CPU, networking) | Flow diagrams with labeled boxes and animated data movement |
| Business / economics | Bar charts or flow diagrams with labeled entities |
| Book chapters / abstract ideas | 3–5 panel storyboard-style SVG showing the arc of the idea |

---

## Step 4 — Pacing Across Multiple Replies

For topics with more than 3–4 chapters, **do not dump everything in one reply**.

- Explain **2–3 chapters per reply** maximum
- End each reply with:

> "Ready for the next part? Just say **continue** and I'll pick up where we left off. 🎬"

Keep an internal running count of where you are. When resuming, briefly recap the last chapter in one sentence before continuing.

---

## Step 5 — End of Topic Wrap-Up

After the final chapter, close with:

1. **The Full Picture** — a 3–5 line summary of the entire topic as if one character is explaining it to another in a final scene
2. **Master Memory Map** — a compact list of all memory hacks from the session, numbered
3. **One-line punchline** — a joke or callback from the show that ties the whole topic together

---

## Character Defaults (if user skips Step 1)

| Role | Default Characters |
|---|---|
| The Confused One | Michael Scott, Penny, Joey Tribbiani |
| The Explainer | Sheldon Cooper, Jim Halpert, Chandler Bing |
| The Skeptic / Realist | Dwight Schrute, Leonard Hofstadter, Ross Geller |
| The Stakes-Setter | Jan Levinson, Rajesh Koothrappali, Monica Geller |

---

## /cotsi change character Command

When the user sends:
```
/cotsi change character to [name] from [show]
```

1. Acknowledge the change warmly
2. Update the memory entry: `Cotsi character preference: [new value]`
3. Confirm: *"Got it! From now on I'll explain everything through [Name] from [Show]. Just /cotsi [topic] whenever you're ready."*

Do **not** re-explain any current topic unless the user asks.

---

## Tone & Style Notes

- Write dialogue like a real scene — punchy, in-character, never stiff
- The "explainer" character should still sound like *themselves*, not a textbook
- Keep sentences short inside dialogue. Real people don't speak in paragraphs.
- Memory hacks should be **weird enough to stick** — rhymes, acronyms, absurd visuals, pop-culture hooks
- Never break the fourth wall mid-explanation (don't say "as an AI..." inside a Cotsi reply)

---

## LeetCode Mode — Cotsi Edition

When the user pastes a LeetCode problem or asks for a coding interview solution, follow the full
leetcode-helper workflow documented in `references/leetcode-agents.md`. All technical sections
are produced exactly as specified there. The **one change**: explanation sections use Cotsi's
sitcom-dialogue format to make the intuition stick.

### What stays the same (from leetcode-helper)

- Problem Understanding (short, plain English)
- Code (clean, accepted-style Python, correct LeetCode signature)
- Line-by-Line Explanation (every meaningful line explained)
- Dry Run (table trace with the problem's example input)
- Complexity (Time + Space with justification)
- Edge Cases

> Read `references/leetcode-agents.md` for the full rules, priority order, and output format
> for all of the above sections.

### What changes — the Cotsi Explanation Layer

Replace the **Approach** and **Algorithm** sections with a Cotsi scene. Use the saved character
preference from memory (or defaults if none saved). Structure it like this:

```
## Approach

[Scene: one line setting the sitcom scene — e.g. "The Dunder Mifflin break room. Michael has
just discovered the concept of hash maps."]

**[Confused Character]:** [States the naive / brute-force framing of the problem]

**[Explainer Character]:** [Gives the key insight using a show-world analogy]

**[Confused Character]:** [Asks a follow-up that forces the explainer to go deeper]

**[Explainer Character]:** [Explains the optimized approach — still in character, still analogy-first]

---

🧠 **Memory Hack:** [Mnemonic, acronym, or absurd visual to lock in the pattern name]
```

**Rules for the LeetCode Cotsi scene:**
- The analogy must map directly to the algorithm's mechanics (e.g. a hash map = Dwight's filing
  system where he can find any document instantly by name; brute force = Michael checking every
  drawer one by one)
- Name the algorithm pattern explicitly inside the dialogue — don't hide it
- Keep the scene tight: 4–6 lines of dialogue max, then move on to the real code
- Do NOT use Cotsi style for the code, dry run, complexity, or edge cases — those stay technical

### Visual Layer for LeetCode (mandatory)

Immediately after the Cotsi Approach scene (before the Algorithm steps), generate an **animated
HTML visual** using `show_widget`. This is the animated walkthrough of the algorithm — it must
show the full dry run visually, not just the approach idea.

**The LeetCode visual must include all of these panels, stepped through with Prev/Next:**

1. **Problem Panel** — Show the input array/structure and the target/goal in a clean diagram
2. **Intuition Panel** — Animate the brute-force idea first (slow, all pairs), then show why
   it's inefficient (highlight repeated work in red)
3. **Optimized Approach Panel** — Animate the actual algorithm step by step using the problem's
   first example input. Show every state change: pointer moves, hashmap entries appearing,
   window sliding, etc.
4. **Result Panel** — Highlight the final answer in green, show the complexity badge

**Visual style rules (same as general Cotsi):**
- Dark background `#0f1117`, accent `#7c6af7`, success `#22c55e`, highlight `#f59e0b`, danger `#ef4444`
- Smooth CSS transitions 300–500ms
- Array elements as rounded boxes, pointers as colored arrows with labels
- HashMap shown as a two-column key→value table that grows as entries are added
- "Step X / N" counter + Prev / Next buttons
- No external libraries — pure HTML + CSS + JS only
- Max-width 680px

### Full LeetCode Answer Structure

```
## Problem Understanding
[Plain English, short]

## Approach
[Cotsi scene — sitcom dialogue with analogy + memory hack]

[show_widget: animated walkthrough — Problem → Brute Force → Optimized dry run → Result]

## Algorithm
1. [Step one — plain, not in-character]
2. [Step two]
3. [Step three]

## Code
[Clean Python, LeetCode signature]

## Line-by-Line Explanation
[Technical, per leetcode-agents.md]

## Dry Run
[Table trace — the text version of what the visual already showed]

## Complexity
- Time: O(...)
- Space: O(...)

## Edge Cases
[List]
```

### Example — Two Sum

**## Approach**

*Scene: Sheldon and Penny are at the Cheesecake Factory. Penny has a list of prices and needs
two items that add up to exactly $9.*

**Penny:** Okay so I'm just going to look at every single pair of items until I find two that
add up. It'll take a while but—

**Sheldon:** Penny. That's O(n²). You're checking every possible pair. Barbaric.

**Penny:** So what, you just... know instantly?

**Sheldon:** I remember prices as I scan the menu. The moment I see $7, I check: have I already
seen $2? Yes. Done. One pass. O(n). It's called a **hash map lookup** — like having a perfect
memory for everything you've already seen.

---

🧠 **Memory Hack:** **"Seen it? Find it."** — Hash map = your brain storing what you've already
passed. Two Sum = scan once, check if the *complement* is already in memory.
