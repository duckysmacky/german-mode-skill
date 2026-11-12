# German mode skill

> Practice German in any AI conversation, without stopping what you're doing

![version](https://img.shields.io/badge/version-1.0.0-blue)
![Claude Code](https://img.shields.io/badge/Claude%20Code-plugin-D97757)
![Codex](https://img.shields.io/badge/Codex-plugin-000000)
![Agent Skills](https://img.shields.io/badge/Agent%20Skills-npx%20skills%20add-success)

## About

`german-mode` is an agent skill that switches the conversation to German at your level. You keep chatting, learning, working or coding as usual. It is not a simple "let's practice German" prompt, rather it is meant to be used in your chats as-is, without interrupting the flow or context, letting you practice German while you are working on other things.

Language apps teach you vocabulary for ordering coffee. If you spend hours a day talking to an AI about real things, that time can double as reading and writing practice. `german-mode` turns it into German input that is just slightly above your level ("i+1"), with enough support that you never get lost.

It works in the Claude app, Claude Code, Codex, the ChatGPT app and any other agent that supports [Agent Skills](https://github.com/vercel-labs/skills).

## How to use

To activate german mode, simply write the skill command and then write your message, in **any** session:
- `/german-mode` - most apps (Claude app, Claude Code, etc.)
- `$german-mode` - Codex
- `@german-mode` - ChatGPT app

Then the agent answers in German, translates harder words inline into your own language (or languages), and ends each reply with a short summary of new vocabulary and corrections to your German. Your work, code, commits and files are not affected. Only the conversation switches to German.

## What it does

- **Answers in German at your level**, from A1 to C2. At B1-B2, for example, it uses short, clear sentences, mostly the Perfekt, and adds a few B2 structures on purpose (Passiv, Konjunktiv II, Relativsätze).
- **Translates harder words inline into your language**, right where they appear. If you speak more than one language, it uses your first language by default and switches to another one when that fits the meaning better:
  `die Abhängigkeit [рус.: зависимость]`, `etwas auf die lange Bank schieben [eng.: to put sth. on the back burner]`.
  Each word is translated only the first time it appears, so seeing it again later reinforces it.
- **Adds a learning summary** under a `---` line at the end of each reply:
  - **Wörter**: new words, with article, plural form and verb forms.
  - **Grammatik**: at most one grammar point from the reply.
  - **Deine Nachricht**: corrections to your German, or a German version of what you wrote in another language.
- **Corrects you without interrupting.** If you write German with mistakes, the agent answers the content normally and puts the corrections in the summary:
  `~~mit mein Freund~~ -> **mit meinem Freund**` (*mit* + Dativ).
- **Never forces you to write German.** If you write in your own language, it still answers in German and shows you how you could have said your message in German.
- **Stays on** for the rest of the conversation, through topic changes and long tool-heavy tasks, until you turn it off.
- **Leaves the work alone.** Code, comments, identifiers, commit messages, file contents, shell commands and quoted errors stay in their original language. It uses the anglicisms German developers actually say (*der Commit*, *pushen*), and introduces proper German terms (*der Rückgabewert*) where they're worth learning.
- **Supports regional German.** Standard German by default, or Austrian or Swiss everyday words (*Jänner*, *das Velo*) if you live there.

## Example

With level `B1-B2` and translations into Russian, then English:

> **You:** Ich habe gestern mit mein Freund über Rust geredet und er sagt, dass Rust ist zu schwer für lernen.
>
> **Agent:** Da hat dein Freund nicht ganz unrecht [рус.: не совсем неправ]: Rust hat eine steile Lernkurve [рус.: крутая кривая обучения], vor allem wegen des Borrow Checkers. Aber der Aufwand lohnt sich [рус.: окупается]. Was findet er denn am schwierigsten?
>
> ---
> **Wörter**
> - die Lernkurve, -n — кривая обучения
> - sich lohnen — окупаться, стоить того
>
> **Deine Nachricht**
> - ~~mit mein Freund~~ -> **mit meinem Freund**: *mit* + Dativ, maskulin: mein -> meinem
> - ~~dass Rust ist zu schwer~~ -> **dass Rust zu schwer ist**: im *dass*-Satz steht das Verb am Ende
> - ~~für lernen~~ -> **zum Lernen**: „für + Infinitiv“ gibt es im Deutschen nicht

In a coding session the explanation is in German, while the code block and the error code (`E0502`) stay exactly as they are.

## Install

### Claude Code

Add my plugin marketplace and install the plugin:

```sh
claude plugin marketplace add duckysmacky/duckysmacky-plugin-marketplace
claude plugin install german-mode@duckysmacky
```

### Claude app (desktop and web)

1. Open **Customize** in the left sidebar and go to the **Plugins** tab.
2. Click **Add**, then select **Add marketplace**.
3. Choose **Add from a repository** and enter `duckysmacky/duckysmacky-plugin-marketplace`.
4. Back in the **Plugins** tab, open **Discover**, select **german-mode** from the **Duckysmacky** marketplace, and click **Add**.

### Codex

```sh
codex plugin marketplace add duckysmacky/duckysmacky-plugin-marketplace
codex plugin add german-mode@duckysmacky
```

### ChatGPT app

1. Add the marketplace using the terminal command in the Codex section above.
2. Restart the ChatGPT desktop app.
3. Select **ChatGPT**, choose **Work**, and open **Plugins**.
4. Select the **Duckysmacky** marketplace, choose **german-mode**, and install it.
5. Start a new chat to use the plugin.

### Other agents

`npx skills` supports Claude Code, Codex, Cursor, OpenCode, Gemini CLI, GitHub Copilot, and other agents. Let it detect installed agents automatically:

```sh
npx skills add duckysmacky/german-mode-skill
```

The plugin is a single skill, so nothing is lost when you install it this way.

## First run and settings

The first time you start German mode, the agent asks you three things in one message:

1. **Level**: A1, A2, B1, B2, C1, C2, or a range like `B1-B2`. Pick the level you can read comfortably; the mode pushes slightly above it on purpose.
2. **Translation languages**: one or more languages you know well, for example `English` or `Russian, English`. The first one is the default; the others are used when they fit a word better.
3. **Regional variety** (optional): standard, Austrian or Swiss German.

The answers are saved as a small TOML file, `german-mode.toml`:

```toml
level = "B1-B2"
translations = ["Russian", "English"]
variety = "standard"
```

The file lives in the agent's own directory, so every agent keeps its own settings:

| Agent | Settings file |
| --- | --- |
| Claude Code | `~/.claude/german-mode.toml` |
| Codex | `~/.codex/german-mode.toml` |
| Gemini CLI | `~/.gemini/german-mode.toml` |
| OpenCode | `~/.config/opencode/german-mode.toml` |
| Other agents | `~/.agents/german-mode.toml` |

`~/.agents/german-mode.toml` is also the shared fallback. The agent reads its own file first and this one second, so you can put one file there to use the same settings everywhere.

Without file access (the Claude and ChatGPT apps), the settings go into the app's memory, if it is turned on. Otherwise the agent shows you the TOML to paste into your personal preferences (Claude: **Settings -> Profile**) or custom instructions (ChatGPT: **Settings -> Personalization**).

The settings are not stored inside the skill folder, because plugin updates overwrite it and the apps keep it read-only. You can also write the file yourself; the agent picks it up the next time German mode starts.

To change the settings at any time, run `/german-mode setup` or just say "change my German level to B2".

## Usage

| Command | What it does |
|---|---|
| `/german-mode` | Turns German mode on (runs the setup on first use). Typing it again while the mode is on turns it off. |
| `/german-mode on` | Turns it on, and never toggles it off. |
| `/german-mode off` | Turns it off. "Deutschmodus aus" or "stop German" also work. |
| `/german-mode setup` | Asks for level, translation languages and regional variety again, and saves them. |
| `/german-mode B2` | Uses another level (`A1`-`C2`) for this conversation only. |
| `/german-mode english` | Translates into another language for this conversation only. |
| `/german-mode quiet` | Inline translations only, without the summary block. |

How you invoke the skill depends on the agent: `/german-mode` in Claude, `/german-mode:german-mode` when installed as a Claude Code plugin, `$german-mode` in Codex. You can also just say "practice German with me" or "talk to me in German". If you ask "explain that in English" once, the agent does that for that part and keeps German mode on.

## Customizing

The skill is a single `SKILL.md` file. Most things are covered by the settings, but you can edit the file for anything else, such as the target of 3-8 translations per reply or the level descriptions in the "Language level" table.

## Limitations

- Large language models write very good German but are not certified teachers. Corrections are reliable for everyday grammar, but check them against a grammar reference when something seems off.
- In very long sessions the model can slowly drift back into your language. If that happens, run `/german-mode on` to remind it.
- The level is calibrated by instructions, not measured, so how hard the German feels varies a little between topics and models.
- In the apps, settings only persist if memory or custom instructions are available. Without them, the agent asks again in each new conversation.

