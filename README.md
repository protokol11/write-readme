# write-readme

Pi skill that writes or audits a project README: section set fixed by project type, proof command above the fold, then link, heading and snippet checks.

## Why

A reader deciding on a project runs a funnel: they have to care, then trust, then use. Most READMEs leak readers at every step. The one-line description fails the care test, so the reader never reaches the install command that would build trust, and never runs the command that turns into use. The failures are structural, not stylistic: a documentation repo carrying an Install section, a CLI with no CLI section, a proof command buried below the fold, claims no one can check.

The skill treats the README as a checklist problem, not a writing talent problem. Each rule traces to a source in `EVIDENCE.md`, and the skill names what the evidence does not establish — the voice numbers come from a 1997 web-usability study of websites, not READMEs, and the skill keeps that date attached when quoting them.

## Quick start

```sh
git clone https://github.com/protokol11/write-readme.git
cp -r write-readme/write-readme ~/.agents/skills/write-readme
```

In pi: `/skill:write-readme` audits or writes the README. Details below.

## Install

Prerequisites: None.

Copy the `write-readme/` directory into your skills folder:

```sh
cp -r write-readme ~/.agents/skills/write-readme
```

Any agent that loads a `SKILL.md` can then run the skill.

For pi specifically, register it as a slash skill:

```sh
pi /skill:write-readme
```

The skill sets `disable-model-invocation: true`, so pi loads it only on explicit invocation — force-load, not background selection.

## Usage

One path to one result: audit an existing README.

1. Point the skill at a repo with a README:

```sh
ls path/to/repo/README.md
```

2. Invoke the skill in pi:

```
/skill:write-readme
```

3. The skill reads the manifest, entry points, `bin` field and workspace config, writes down five answers (project type, audience, one-sentence purpose, package description, proof command), then runs the proof command in a clean checkout.

Expected output: a README whose section set matches the project type, a one-line description under 120 characters matching the package and repository descriptions word for word, and a recorded pass or fail for every runnable check in `REFERENCE.md`.

## How it works

Ten steps in `SKILL.md`, from intake through visuals to the check run. The load-bearing rules live in `REFERENCE.md`:

- **Section table.** Fifteen rows; four hard-required (Title, one-line description, Contributing, License); rows drop or appear by condition — a documentation repo skips Install and Usage, a CLI adds a CLI section, a monorepo adds a root package table plus one scoped README per package.
- **Proof command above the fold.** One command that proves the project works, runnable from the first screenful, with its real output.
- **Checks.** Link resolution (`lychee`), heading structure (`markdownlint` with MD041/MD043/MD025/MD040), section order (`standard-readme` remark preset, marked work in progress), prose style (`vale`), snippet execution (`pytest --doctest-glob`, `mdbook test`, a shell runner), alt text against WCAG 2.2 SC 1.1.1, live badge URLs, the 500 KiB GitHub truncation limit, and the three-way description match.

## Evidence base

`EVIDENCE.md` maps every rule to a source tagged **declared**, **measured** or **correlational**, with year and scope kept attached: 79% of readers scan rather than read word for word (1997, websites); 28.9% of popular projects hold at least one outdated code reference (EMSE 2023); badge effects are correlational 2018 npm data. It also lists what the evidence does not establish — no study measures README quality against task success, and three section-table rows (configuration, API placement, troubleshooting order) rest on judgement, overridable by project convention.

## Contributing

File issues in the `protokol11` repository. Run the checks in `REFERENCE.md` against any README you change: `lychee --no-progress README.md`, `markdownlint README.md`, `vale README.md`.

## License

[MIT](LICENSE)
