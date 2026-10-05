Disclaimer

This tool is provided for general information only and is not legal advice. The authors accept no responsibility for any loss arising from its use. Always verify sections in the official text or with a qualified lawyer.





# IPC to BNS Finder

A free, single-page website that helps you find the **Bharatiya Nyaya Sanhita, 2023 (BNS)** section that replaced an **Indian Penal Code, 1860 (IPC)** section, and the other way round. You can type a crime in plain words, a Hindi word, or a section number.

> **Important:** this is a quick-reference tool, not an official or legal source. See [Accuracy and limits](#accuracy-and-limits) before relying on any result.

---

## What it does

- **Plain-language search.** Type "neighbour stole my phone", "husband demands dowry" or "dhamki" and the page ranks the closest offences.
- **Section number search.** Type `302`, `420`, `103` or `304B` to look up a section in either code.
- **Side-by-side comparison.** Every result shows the IPC section and the BNS section next to each other.
- **Punishment and bail details.** Where the source gives them, each result shows the punishment and whether the offence is cognizable or non-cognizable and bailable or non-bailable.
- **Filters.** Narrow results by category (Body, Women, Property, Document, Public, State, General), or show only offences that are **new in BNS** or **removed from the IPC**.
- **Works offline once loaded.** There is no server and no database. Everything runs in the browser.

## Coverage

| Item | Count |
|---|---|
| Entries in the finder | 448 |
| BNS sections covered | all 358 |
| Source tables | IPC to BNS, and BNS to IPC |

Examples of what is flagged: sedition (IPC 124A) and adultery (IPC 497) appear under "Removed"; mob lynching, terrorism, organised crime and snatching appear under "New in BNS".

---

## How to use it

1. Open the site.
2. Type in the search box. Any of these work:
   - A description: `man followed me home every day`
   - A keyword: `fraud`, `rape`, `kidnap`
   - A Hindi word: `chori`, `dhokha`, `balatkar`, `dahej`
   - A section number: `379`, `303`, `498A`
3. Read the top results. The best match is tagged **Best match**.
4. Use the category buttons to narrow the list, or leave the search box empty to browse everything.
5. **Always confirm the section in the official text** before you act on it.

### Tips
- Short, specific words work better than long stories.
- If nothing matches, try a simpler word ("theft" instead of "someone took my bag").
- Some IPC sections were split into several BNS sub-sections, so you may see more than one row for one offence.

---

## Accuracy and limits

Please read this before using the tool for anything that matters.

- **Not an official source.** It is built from the *Delhi Police Academy BNS Handbook, Edition II (May 2024)*, a police training document, not from the Gazette of India.
- **Automatic extraction.** The tables were read from the PDF by a program, then cleaned. Some headings are still rough, and some rows with several sub-sections have their punishments joined together.
- **Cross-checked, not fully verified.** 423 of 428 IPC-to-BNS pairings agree with the handbook's second table. The remaining ones are marked **CHECK** on the page because the handbook disagrees with itself (for example IPC 254, 376E and 309). The punishment and bail columns were not checked row by row.
- **Newer rows have fewer details.** BNS-only rows from the second table have titles but not always punishments or keywords.
- **Search is approximate.** It is a keyword-and-synonym ranking, not a lawyer. It can return related sections instead of the exact one.
- **Dates matter.** Offences committed before 1 July 2024 are still tried under the IPC, not the BNS.

**Before you file a complaint, draft a charge or give legal advice**, confirm the section in the official BNS text on India Code (indiacode.nic.in) or the Gazette of India, or ask a qualified lawyer.

---

## Testing

The search was tested in Node.js against the page's own code.

| Test | Result |
|---|---|
| 50 queries (plain words, Hindi words, typos, section numbers, removed offences) | correct section in top 3 for all 50, in first place for 40 |
| 20 fresh queries written after tuning | correct section in top 3 for all 20, in first place for 18 |
| Data integrity (empty headings, leaked page text, missing BNS) | 0 issues |
| Empty input, symbols, gibberish, filler-only input | no errors |

These tests check the search ranking and the data format. They are not a legal accuracy audit.

---

## Project files

```
index.html     the whole site (HTML, CSS, JavaScript and data in one file)
README.md      this manual
```

There is nothing to install or build.

---

## How it works (short version)

1. **Data.** A list called `D` holds one row per entry: title, IPC section, BNS section, status (changed, new, removed), category, keywords, notes and cognizable/bailable details.
2. **Cleaning the query.** Your text is lower-cased, filler words are dropped, and each word is reduced to its root ("stolen", "stole", "thief" are treated alike).
3. **Synonyms.** A list of word groups links related words, including common Hindi words (`chori` goes with theft).
4. **Typo tolerance.** Close spellings still match, so "cheted" finds cheating.
5. **Ranking.** Each entry is scored by how well its title, keywords and notes match, and the best results are shown first.

### Updating the data
Open `index.html` in a text editor, find the `var D=[` list, and edit or add rows. Each row is:

```
[title, IPC section, BNS section, status, category, keywords, note, cognizable/bailable]
```

`status` is `chg` (renumbered or changed), `new` (new in BNS) or `gone` (removed). Add everyday words to the keywords field to make an offence easier to find.

---

## Putting it online (free, 24/7)

Because the site is one static file, any static host works. Name the file `index.html`.

**Cloudflare Pages (recommended)**
1. Create a free account at dash.cloudflare.com.
2. Go to Workers & Pages, then Create, then Pages, then Upload assets.
3. Name the project, drag in a folder containing `index.html`, and click Deploy.
4. Your site is live at `your-project.pages.dev`.

**Netlify Drop**
1. Go to app.netlify.com/drop and sign up free.
2. Drag the folder containing `index.html` onto the page.
3. Rename the site in Site configuration.

**GitHub Pages**
1. Create a public repository and upload `index.html`.
2. Go to Settings, then Pages, and choose the main branch.
3. The site appears at `yourname.github.io/repo-name`.

**Firebase Hosting (Google)**
```
npm install -g firebase-tools
firebase login
firebase init hosting     # public folder, not a single-page app
firebase deploy
```

Free-tier limits change, so check each host's pricing page before relying on them. A custom domain (for example `ipcbns.in`) can be bought from any registrar and connected in your host's domain settings.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Page shows plain text with no styling | The file was renamed or opened as text. Keep it as `index.html` and open it in a browser. |
| Search finds nothing | Use a simpler word or a section number. |
| A result looks wrong | Check it against the official BNS text and the **CHECK** note, then edit the row in `index.html`. |
| Site not updating after a re-upload | Hard-refresh the browser (Ctrl+Shift+R) or clear the cache. |

---

## Credits and data source

- Data: *Delhi Police Academy, BNS Handbook, Edition II (May 2024)*, sections on IPC to BNS and BNS to IPC. Confirm that you are allowed to reuse it before publishing widely.
- Statute: *Bharatiya Nyaya Sanhita, 2023*, in force from 1 July 2024.

## Disclaimer

This tool is provided for general information only and is not legal advice. The authors accept no responsibility for any loss arising from its use. Always verify sections in the official text or with a qualified lawyer.
