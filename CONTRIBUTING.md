# Contributing

Thanks for taking the time to help improve this cheatsheet! This project isn't a code library — it's a single reference document for the [Dataview Query Language (DQL)](https://github.com/blacksmithgu/obsidian-dataview) in [Obsidian.md](https://obsidian.md). Almost every contribution will be a change to [README.md](README.md).

## Ways to contribute

- Add a new DQL query example that isn't covered yet
- Clarify or expand an existing explanation
- Fix a typo, a broken anchor link, or a formatting glitch
- Suggest a new section — please open an issue first so we can agree on scope before you write a PR

## Style rules

So that new additions feel at home next to the existing ones, please match the conventions already used in the README:

- Use `#` for a top-level section and `##` / `###` for subsections
- For each query, show the **syntax snippet first**, then an `Example` subheader, then a fenced code block using ` ```js ` or ` ```sql `, then a short prose explanation
- Keep code blocks copy-paste runnable inside an Obsidian vault with the Dataview plugin enabled
- Update the **Table of Contents** in the README whenever you add, remove, or rename a section (and double-check that anchor links resolve)
- Match the existing friendly, first-person tone

## Test your example before submitting

Because this is a cheatsheet, every snippet should actually work. Before opening a PR:

1. Paste the query into a note inside an Obsidian vault that has the Dataview plugin installed
2. Confirm it renders the expected result
3. If the example relies on specific frontmatter or a folder structure, mention that in the surrounding prose so readers can reproduce it

## Opening an issue

When reporting a problem or requesting a new example, please include:

- A short description of the DQL feature or scenario
- A minimal example that reproduces the issue (if it's a bug in an existing snippet)
- Your Obsidian and Dataview plugin versions, if relevant

## Pull request checklist

- [ ] The PR covers **one logical change** (easier to review and revert)
- [ ] The **Table of Contents** has been updated if you added or renamed a section
- [ ] Every query in the diff has been **tested in Obsidian**
- [ ] The commit message follows the existing style in `git log` — `docs:` for documentation changes, `feat:` for new examples or sections
- [ ] The PR links to the related issue, if there is one

## Code of Conduct

Be kind, constructive, and patient with other contributors. This project follows the spirit of the [Contributor Covenant](https://www.contributor-covenant.org/) — disrespectful or harassing behaviour is not welcome here.
