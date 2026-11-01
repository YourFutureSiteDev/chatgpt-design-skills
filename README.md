# ChatGPT design skills

Free skills for ChatGPT that make the interfaces it designs look and move like a designer made them, plus a few that sharpen its writing and make it check its own work.

They are the same skills as [claude-design-skills](https://github.com/YourFutureSiteDev/claude-design-skills), set up two ways so they work on any ChatGPT plan.

## Pick your way in

| Your plan | Use | Why |
|---|---|---|
| Business, Enterprise or Edu | The skill zips | These plans have ChatGPT Skills, which read the same SKILL.md format. |
| Free, Plus or Pro | The Custom GPT kits | No Skills on these plans yet, so each skill comes as GPT instructions plus one knowledge file. |

### Way 1: upload a skill (Business, Enterprise, Edu)

1. Download the skill's zip from the [latest release](https://github.com/YourFutureSiteDev/chatgpt-design-skills/releases/latest).
2. In ChatGPT, open Skills and upload the zip from your computer.
3. ChatGPT checks it first. Most skills go straight through.

Then just ask for something the skill covers, for example "design a landing page for a bakery" for `hallmark`.

OpenAI's guide to Skills: https://academy.openai.com/public/resources/skills

### Way 2: make a Custom GPT (any plan)

1. Open the skill's folder in [custom-gpt](custom-gpt).
2. In ChatGPT, create a GPT (Explore GPTs, then Create, then Configure), or make a Project.
3. Paste the text from the box in `instructions.md` into the Instructions field.
4. Upload the `<skill>-knowledge.md` file from the same folder under Knowledge.
5. For `ui-ux-pro-max`, also turn on Code Interpreter.

Making a GPT needs a paid plan. A Project works the same way: paste the instructions into the Project instructions and add the knowledge file as a Project file.

## What is in it

### Design and build

| Skill | What it does | Kit |
|---|---|---|
| `ui-ux-pro-max` | Searchable design database: 84 styles, 192 palettes, 74 font pairings, 98 UX rules. Ask it for a design system before you build. | [GPT kit](custom-gpt/ui-ux-pro-max) |
| `hallmark` | Pages that do not look AI made. Picks a structure and a theme, then runs a slop test before handing back. Also audits and redesigns. | [GPT kit](custom-gpt/hallmark) |
| `impeccable` | Design, critique, audit and polish any frontend, and flag common AI design tells. | [GPT kit](custom-gpt/impeccable) |
| `taste` | Reads the brief and commits to a real design direction for landing pages, portfolios and redesigns. | [GPT kit](custom-gpt/taste) |
| `emil-design-eng` | Emil Kowalski's rules for UI polish and the small details that make software feel good. | [GPT kit](custom-gpt/emil-design-eng) |
| `apple-design` | Apple's fluid, physical motion translated for the web: springs, gestures, momentum, materials. | [GPT kit](custom-gpt/apple-design) |
| `ui-signals` | 23 feedback animations (drag, progress, retry, button press, count up, empty states) and a strict rule for where each one belongs. Comes with the CSS and JS. | [GPT kit](custom-gpt/ui-signals) |
| `steal-the-polish` | Where to lift finished UI polish from, plus five ready kits: dashboard tiles, login card, pay button ring, scroll sequence, liquid glass nav. | [GPT kit](custom-gpt/steal-the-polish) |
| `rusty-hero` | The product colour hero: pick a product and the whole section takes its colour. Comes with the CSS and JS. | [GPT kit](custom-gpt/rusty-hero) |
| `prototype` | Builds several genuinely different versions of one UI piece so you can compare and choose. | [GPT kit](custom-gpt/prototype) |

### Animation

| Skill | What it does | Kit |
|---|---|---|
| `animate` | Builds an animation in the right order: should it move, why, which curve, how it exits. | [GPT kit](custom-gpt/animate) |
| `animation-vocabulary` | "What is that bouncy thing called?" Turns a vague description into the exact term. | [GPT kit](custom-gpt/animation-vocabulary) |
| `find-animation-opportunities` | Finds the places a UI should animate, and rejects the rest. | [GPT kit](custom-gpt/find-animation-opportunities) |
| `improve-animations` | Audits a project's motion and writes a prioritised fix plan. | [GPT kit](custom-gpt/improve-animations) |
| `review-animations` | A strict review of motion code against a high craft bar. | [GPT kit](custom-gpt/review-animations) |

### Writing and working

| Skill | What it does | Kit |
|---|---|---|
| `stop-slop` | Strips AI writing patterns out of prose. | [GPT kit](custom-gpt/stop-slop) |
| `caveman` | Ultra short replies that say the same thing in far fewer words. | [GPT kit](custom-gpt/caveman) |
| `gauntlet-loop` | Turns any goal into a builder and harsh critic loop that runs until the work beats a real benchmark. | [GPT kit](custom-gpt/gauntlet-loop) |
| `verify-loop` | Adds a "Done means" checklist to a skill or prompt so the work gets checked before it is handed back. | [GPT kit](custom-gpt/verify-loop) |

## Good to know

- These skills were first written for Claude Code. Each one tells ChatGPT how to handle the steps that assume a terminal or a browser preview: it does the step in the chat or hands you the code to run.
- The Claude version also has `watch` and `img2threejs`. They need tools installed on your own computer, so they are not here.
- The `skills` folder holds the same skills unzipped, if you would rather build your own zip.

## Credits

Most of these are other people's open source work, kept here with their licences. [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) lists every author and where the original lives. `ui-signals`, `steal-the-polish`, `rusty-hero` and `verify-loop` are ours, under the MIT licence in [LICENSE](LICENSE).

If you wrote one of these and want it credited differently, open an issue.
