# Classroom Seating Planner

A lightweight, no-install tool for teachers to design and manage classroom seating plans. Built as a single HTML file — open it in a browser and it works.

**Live demo:** https://vcanela.github.io/seating-planner/

---

## Features

- **3-step workflow** — Layout, Students, Plan. Move through steps at your own pace; everything is saved automatically.
- **Room layout painter** — Click or drag to mark which seats exist in your room. Set any number of rows and columns.
- **Student management** — Paste a comma- or newline-separated list of names. Duplicate names are detected and flagged.
- **Constraint system** — Two constraint types:
  - *Sit at Front* — places one student in row 0, with row-by-row fallback if row 0 is full or already taken by separation constraints.
  - *Keep Separated* — maximises the grid distance between two students.
- **Projection mode** — Constraints can be hidden/folded so students can't see who is constrained when you display the plan on a projector. The UI collapses to a neutral "Constraints are set" label.
- **Plan generation** — Monte Carlo simulation across 200 iterations. Constrained students are placed deterministically first; the rest are distributed randomly, and the best-scoring layout wins.
- **Drag-and-drop adjustments** — Swap any two students on the grid after a plan is generated, without regenerating from scratch.
- **Reshuffle similarity** — After reshuffling, a message shows what percentage of seats changed, so you can judge how much variation occurred.
- **Plan flexibility indicator** — A live bar shows the percentage of students who are constrained (green = mostly free, amber = mixed, red = heavily constrained).
- **Multi-class library** — Store multiple named classes in your browser's localStorage. Switch between them with a compact selector. Every change is auto-saved.
- **Export / Import** — Download a class as a JSON file, or import a JSON file to create a new entry in the library. The schema is simple enough for an AI to generate from a plain-language description (see below).
- **Print support** — A clean print layout hides the sidebar and shows the seating grid with student names.

---

## Usage walkthrough

### Step 1 — Layout

1. Set the number of **rows** and **columns** for your room.
2. Click or drag on the grid to **paint the seats that exist** in your room. Empty cells represent aisles or gaps.
3. If you teach multiple classes, use the **class selector** to switch between saved classes or create a new one.

### Step 2 — Students

1. Paste your student list into the text box — names separated by commas, newlines, or both.
2. The app parses the list and displays each name as a tag. Duplicates are highlighted so you can clean them up.

### Step 3 — Plan

1. Add any **constraints** using the card-style picker:
   - *Sit at Front* — type or select one student's name.
   - *Keep Separated* — type or select two students' names.
2. When you're ready, click **Generate Plan**. The Monte Carlo engine runs 200 candidate layouts and picks the highest-scoring one.
3. **Drag students** on the grid to manually swap seats if you want to fine-tune the result.
4. Use **Reshuffle** to regenerate while keeping constraints. The similarity metric tells you how much changed.
5. Click **Print** for a clean printable layout, or **Export** to save the class as a JSON file.

### Projecting to the class

Click the **hide constraints** toggle before displaying the plan. The constraint list folds to a neutral label — students can see their seats but not who is constrained or why.

---

## Multi-class workflow

The class library lives in your browser's localStorage. A compact selector at the top of the sidebar lets you:

- **Switch** between saved classes instantly.
- **Rename** a class by editing the name field.
- **Create** a new empty class.
- **Delete** a class you no longer need.

Every change — layout, students, constraints, and the current plan — is auto-saved as you work. No save button required.

> **Important — localStorage is per browser, per device.** If you switch computers or browsers, your classes will not follow you. **Export your classes as JSON files before switching devices** and re-import them on the new machine. Treat exported files as your source of truth.

---

## JSON schema reference

Exported files and importable files follow schema version 1.0. The format is intentionally minimal — no numeric IDs, no null values, just names and simple objects.

```json
{
  "version": "1.0",
  "meta": {
    "name": "Year 9A",
    "created": "2026-05-08",
    "modified": "2026-05-08"
  },
  "room": {
    "rows": 5,
    "cols": 6,
    "seats": ["0-0", "0-1", "0-2"]
  },
  "students": ["Alice", "Bob", "Charlie", "Diana"],
  "constraints": [
    { "type": "front", "student": "Alice" },
    { "type": "separate", "students": ["Bob", "Charlie"] }
  ]
}
```

### Field reference

| Field | Type | Description |
|---|---|---|
| `version` | string | Schema version. Currently `"1.0"`. |
| `meta.name` | string | Display name of the class (e.g. `"Year 9A"`). |
| `meta.created` | string | ISO 8601 date the class was first created. |
| `meta.modified` | string | ISO 8601 date of the last modification. |
| `room.rows` | integer | Total number of rows in the grid. |
| `room.cols` | integer | Total number of columns in the grid. |
| `room.seats` | string[] | List of active seat positions as `"row-col"` strings (0-indexed). Any grid cell not listed is treated as a gap or aisle. |
| `students` | string[] | Ordered list of student names. Names must be unique within the array. |
| `constraints` | object[] | List of constraint objects (see below). Order does not matter. |

### Constraint objects

**Sit at Front**
```json
{ "type": "front", "student": "Alice" }
```
Places the named student in row 0. If row 0 has no available seat (due to the room layout or other constraints), placement falls back to row 1, row 2, and so on.

**Keep Separated**
```json
{ "type": "separate", "students": ["Bob", "Charlie"] }
```
The planner maximises the grid distance between the two named students. The `students` array must contain exactly two names.

---

## AI-assisted class setup

Because the schema has no IDs or complex structures, an AI assistant can generate a valid importable file from a plain-language description — given one exported example as a reference.

**Example workflow:**

1. Export any existing class from the app (File → Export).
2. Open your AI assistant of choice (e.g. Claude at claude.ai).
3. Attach the exported file and send a prompt like:

> "Here's an example schema [attach exported file]. Create one for a 5×6 room where all seats are active. Students: Alice, Bob, Charlie, Diana, Emily, Frank. Keep separated: Alice & Bob, Charlie & Diana. Sit at front: Emily. Name the class 'Year 10B'."

4. The AI reads the schema from your attached example and produces a valid JSON file.
5. Import the generated file into the app using the Import button.

This is particularly useful at the start of term when you need to set up many classes quickly, or when a colleague sends you a student list.

---

## Local development

No build step. No dependencies to install.

1. Clone or download the repository:
   ```
   git clone https://github.com/vcanela/seating-planner.git
   ```
2. Open `index.html` in any modern browser.

That's it. React 18, Tailwind CSS, and Babel standalone are all loaded from CDN at runtime. Any text editor works for making changes.

If you want to serve it over a local network (e.g. to test on a tablet), use any static file server:
```
npx serve .
# or
python3 -m http.server 8080
```

---

## Contributing

Bug reports and feature suggestions are welcome via [GitHub Issues](https://github.com/vcanela/seating-planner/issues). If you'd like to submit a pull request, please open an issue first to discuss the change so there are no surprises.

Since the entire app is a single `index.html`, keep changes focused and well-commented — other teachers who aren't developers may want to read and adapt the code too.
