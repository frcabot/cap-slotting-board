# CAP Slotting Board

A one-page tool for deciding **who gets the slots** at a Civil Air Patrol activity that has more applicants than room: an encampment, an academy, a flight academy, or anything else that registers through **Registration Zone (Reg Zone)**.

You load the Reg Zone registration export. The board ranks every registrant by points, draws the slot line, and shows **why** each cadet is in, an alternate, or not eligible. Change a setting and the list re-ranks at once.

- **Runs in your browser.** The page reads the exports on your own computer. Nothing is uploaded anywhere.
- **No install, no account.** Open `index.html` in a browser, or use the GitHub Pages link if this repository has one.
- **Works on a phone**, though a computer screen is easier for a long list.

---

## What you load

| File | Needed? | Where it comes from |
|---|---|---|
| **Registration export** for the activity you are slotting | Yes | Reg Zone, the activity's registration export (`.xlsx`) |
| **Staff export** | Optional | Reg Zone, the staff activity's export. Used to find cadets with a **parent on staff**. |
| **Time-in-CAP file** | Optional | Any spreadsheet you make (see below). Needed only for the time-in-CAP, promotion pace and stuck-in-grade factors. |

Load several files at once, or drop them on the page. The board tells each file apart by itself:

- An export whose activity name or file name says **staff** or **cadre** is used for parents on staff.
- Any other Reg Zone export can be slotted. If you load more than one (for example the encampment and an advanced training squadron), the largest is slotted and the rest are set to **Ignore**. Switch which one is slotted in the file list at the top.
- A sheet with a `CAPID` column and time-in-CAP columns is read as the time-in-CAP file.

The board keeps the data you load **in this browser only**, so it is still there next time. **Forget saved data** removes it.

### Columns the board reads

From the registration export: `RegistrantsCAPID`, `Rank`, `NameFirst`, `NameLast`, `Gender`, `AgeAtEventStart` (or `Age`), `MbrType`, `State`, `Wing`, `Unit`, `RegistrationID`, `RegistrationStatus`, `AppEditDate`, `UnitApproved`, `ParentApproved`, and for parent matching: `CadetParentEmail…`, `CadetParentPhone…`, `EmergencyContactNumber`, `EmergencyContactName`, `Addr1`, `Zip`.

From the staff export: `MbrType`, `Rank`, `NameFirst`, `NameLast`, `Email`, `CellPhonePrimary`, `HomePhonePrimary`, `Addr1`, `Zip`, `Unit`.

### The time-in-CAP file

Reg Zone exports don't include how long a cadet has been a member or in their current grade. To use those factors, make a spreadsheet with one row per cadet. **Download a blank template** on the page gives you the right headings.

| CAPID | Join date | Date of current grade |
|---|---|---|
| 123456 | 2024-03-15 | 2026-01-20 |

`Months in CAP` and `Months in grade` columns work too, instead of the dates. Cadets missing from the file get the middle (median) points, or 0 points if you choose.

If the file says a cadet has been in their grade longer than in CAP (usually someone who rejoined), the board uses the time in grade as their time in CAP.

---

## How cadets are slotted

1. **Not eligible** cadets are set aside:
   - a Reg Zone status you turned off, such as Cancelled;
   - an age outside your range;
   - senior members, when **Cadets only** is on.
2. **Kept** cadets take slots first:
   - everyone already **Slotted** in Reg Zone (if that switch is on);
   - cadets with a **parent on staff** (if that switch is on);
   - anyone you mark **Slot** by hand.
3. **Everyone else** is ranked by points. The open slots go down the list, highest points first. A cadet who **can't be slotted yet** (missing a required approval, or below the lowest grade you allow) is skipped and stays on the alternate list in points order, so you can see where they would land.
4. **Ties** go to whoever registered first (lower RegistrationID).

### Slots

| Setting | What it does |
|---|---|
| One pool | One total, with an optional most-males and most-females cap |
| Separate male / female | Fixed male and female slots that add up to the total. Use this for all-male and all-female flights. **Split slots by registration share** fills them in proportion to who registered. |
| Out-of-state cadets | No limit, **at most** N, or **exactly** N (the out-of-state seats are filled first, by points) |

### Points (each 0 to 100)

| Factor | How it scores |
|---|---|
| Grade | C/AB gets 0, C/CMSgt gets full points, evenly in between. Cadet officers count as C/CMSgt. |
| Age | 0 at the "from" age, full points at the oldest cadet registered (or an age you set) |
| Home state | Flat points for cadets from your in-state list. Leave the box blank to use the most common state among the registrations. Choose home address state or CAP wing. |
| Sign-up order | Full points for the first to register, sliding to 0 for the last. Optionally tie a group at the top (see below). |
| Priority list | Flat points for anyone on a list you paste, or more points for a better list number. Name the list yourself, for example "Summer waitlist". |
| Unit approval / Parent approval | Each can be **required** to be slotted, worth points, or both. Cadets 18 and older count as having parent approval. |
| Parent on staff | Flat points, when these cadets are not simply always slotted |
| Time in CAP | More months = more points, or the reverse (to favor newer cadets) |
| Promotion pace | Grades earned compared with the most possible. C/Amn can come right away; each grade after that needs 8 weeks. |
| Stuck in grade | A **penalty**: nothing for the first 8 weeks in a grade, growing to the full penalty at a number of months you set. Never applied to C/CMSgt. |

**Sign-up ties.** Reg Zone exports have **no registration date**, only a `RegistrationID` that counts up. Choose one:

- **None.** Strict order.
- **First N registrations.**
- **RegistrationID up to** a number. This is the exact way to say "everyone who registered on opening day": save an export that evening and note the highest RegistrationID.
- **Registered on or before** a date. The board can only prove a registration date from the last-edit dates of later registrations, so this stops working once records are edited in bulk (slotting, approvals). The page shows how many cadets it ties, so you can see.

### Seeing why

Click any **Result** to see why that cadet landed there: the deciding reason (kept, slotted by points, below the cut, cap reached, missing approval), their score against the lowest score slotted, and which factors gave or cost them the most points.

**Hide names** replaces names and CAPIDs with a fixed code (for example "Cadet K-204"), for a blind review on the numbers only.

---

## Getting the results out

| Button | Gives you |
|---|---|
| Copy slot list | Everyone slotted, with every score, ready to paste into a spreadsheet |
| Copy CAPIDs to slot in Reg Zone | Just the CAPIDs slotted here but not yet in Reg Zone, one per line |
| Copy full results | Everyone, with every score |
| Download results (.xlsx) | A workbook: Slot list, To slot in Reg Zone, Slotted in Reg Zone but out here, All results, and your settings |

The board **never changes Reg Zone.** You slot cadets there yourself, using the list.

**Sharing settings.** **Copy settings** gives a block of text another administrator can paste into **Apply pasted settings**. It holds the CAPIDs of any manual Slot / Leave out choices, but no names or other registration data.

---

## Privacy

- The exports hold personal information (addresses, phone numbers, parent emails). The page reads them **only in your browser**. It stores what it needs in the browser's local storage: names, CAPIDs, grades, ages, states, statuses, and, for parent matching, parent emails, phone numbers, the emergency contact name and a shortened home address, so use your own computer, and click **Forget saved data** when you are done on a shared one.
- Don't commit exports to this repository.
- The page loads one library from a public CDN (SheetJS, for reading `.xlsx`) and fonts from Google Fonts. No data is sent to either.

---

## Hosting it

- **Your own copy:** download `index.html` and open it in Chrome, Edge, Firefox or Safari.
- **GitHub Pages:** in the repository, **Settings > Pages**, source **Deploy from a branch**, branch `main`, folder `/ (root)`. The board is then at `https://<owner>.github.io/cap-slotting-board/`. Each person's data still stays in their own browser.

---

## Notes and limits

- The board ranks; people decide. Check the list, especially cadets marked **can't be slotted yet**, before you slot anyone in Reg Zone.
- Reg Zone's `Gender` decides male and female lines. A cadet with none can't be placed in separate pools; the board warns you.
- Parents on staff are matched by parent email, phone, home address, emergency contact name, or same last name and unit. Check the matches; a shared last name in one unit can be a coincidence.
- Grades above C/CMSgt all count as C/CMSgt for points.

## Companion tools

- [cap-regzone-reminders](https://github.com/frcabot/cap-regzone-reminders): automatic to-do, waitlist and unit commander emails from the same Reg Zone exports.
- [cap-base-access-form](https://github.com/frcabot/cap-base-access-form): base access forms and graduation guest lists for activities on a military installation.
