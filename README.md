# Asset Tracker (Google Sheets)

Asset tracker built completely in Google Sheets, programmed with Google Apps Script (JavaScript). Used to track team progress with formulas, dropdown validation and a general status dashboard.

Originally made for RoboRapture (Videogame, 2025).

> **Note:** This is an early, formulas-only version, shared with permission. All game content, names and IDs have been replaced with dummy data.
> 
> **The later version, handling more team members and much more assets, used Google Apps Script coding, yet is under NDA.**
<br>

## The problem

A small team of developers was producing `n` audio assets across five categories, each with different implementation / handling needs.

**We needed to be able to track in real time:**

- What was designed
- Who is responsible for designing `x` asset
- What was implemented in the engine
- Who is responsible for implementing `x` asset
- Which code/object inside the engine handled `x` asset
- What asset/process had bugs

**All of this without paying for a project-management tool.**

<br>

## What it does

- **One tab per asset category:** SFX, UI, MX (music), AMB (ambience) and VO. -> Easier for the implementation team.
- **A status workflow per asset:** Design, Technical, Wwise Implement and GameSyncs, each a dropdown with fixed values (Not Started, In Progress, Designed, Done, Not Needed).
- **Bug and placeholder flags**, plus a column linking each asset to its object in the game engine.
- **Per-person tracking:** designer and technical owner on every row. (Essential to track individual progress and issues)
- **A dashboard tab** with totals, `Done / To do` counts and percentages per person and per category.
- **Color thresholds:** red under 30%, yellow under 60%, green under 90%, blue above 90%. (Great for presentations and pitching to investors; showcasing total team progress.)
<br>

## How it works

| Piece               | Implementation                                              |
| ------------------- | ----------------------------------------------------------- |
| Status dropdowns    | Data validation with a fixed list, values stay consistent   |
| Dashboard counts    | `COUNTIFS` across tabs by category, owner and status        |
| Percentages         | Done divided by total, per person and overall               |
| Color thresholds    | Conditional formatting rules on the percentage cells        |
| Protected dashboard | Locked tab, so only the formulas' source data can be edited |

### Example formulas

```
=COUNTIFS(MX!K:K, "Brandon", MX!L:L, "Done") # Counts rows marked as "done" to obtain total designed assets

=DIVIDE(SUM(N11:R11), SUM(N10:R10)) # Divides amount of assets needed and amount of assets created to obtain progress percentage
```
<br>

## Design decisions

### Why separate tabs instead of one big sheet?

Every asset type `SFX`, `UI`, `MX`, `AMB`, `VO`; is handled completely different implementation-wise (pun intended).

- `SFX` are connected to specific events, scene changes, active objects, etc. These are usually one-shot intended, connected to the game objects (e.g. an enemy) and activates depending on how the game is progressing.
<br>

- `UI` are connected to the player's interaction with menus and icons. These are easier to implement depending on action (hover-in, click, hover-off, etc.) yet there is a whole team in charge of the player's controls, thus UI has to be implemented by them.
<br>

- `MX` is all music played throughout the gameplay. This behaves depending on states (menu music vs. gameplay music); and sometimes switches depending on the boss fight. Behavior is connected directly to Unity's scenes. Also, needs specialized 5.1 mixing.
<br>

- `AMB` behaves closely to music, yet works with background beds, emitters and specs. Usually worked in Quad mixing, being the lesser priority (volume-wise) in the mix.
<br>

- `VO` is completely handled by a Dialogue System (C# code); it has specific lines that activate depending on gameplay context.
<br>

**TLDR:** Every one of these categories are handled by a different team given that they have different needs/uses. Different developers handle these assets and the Google Sheets let them have clarity over how "this specific category" is doing regarding progress.

Merge conflicts are avoided by having different people handling different asset categories, everyone knows what assets they can and can't touch.

Also, due to the large amount of assets, having different tabs + dashboard let's us have more clarity and order.

<br>


### Why fixed dropdowns instead of free text?

We established from a beginning which are the possible scenarios to for an asset progress and implementation process progress. Having a meeting with the developers and designers let's us all be in tune for handling assets.

Team lead and instructors needed a priority tracking, responsible person, implementation and GameSync object need. All per asset.

Designers needed a channel width, naming convention, loop (LP), subwoofer channel (LFE), and variation need. All per asset.

Developers needed a design progress, placeholder, GameSync description, Bug check, Unity Object / Unity Script connection. All per asset.

Dropdowns let us have a rigid tracking state per need; if something needed a disambiguation, we could always use Discord for clearer communication.


<br>

### Why a locked dashboard?

Only team leads and instructors were able to change the programming connections in this window. And we checked this window rather than asset per asset; knowing if all designers/developers were progressing as intended and which areas needed help.

> We did have an issue with a designer who eventually left the project, falsely claiming they were working and developing more assets than we believed. Exactly this locked dashboard let us double-check these claims. It was an unintended use but helped solve a bigger problem in just a couple of minutes.


<br>

## Limitations and what I'd improve

- Everything is manual: statuses are updated by hand, with no change history beyond Sheets' own.
- Formulas recalculate over whole columns, which gets slower as the sheet grows.
- Next step: automate row stamps, dashboard generation and validation with Google Apps Script.

