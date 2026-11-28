# TASK — 2: Translate project Markdown to English

ID source: local sequential numbering.

## Original user request

> так все .md файле в проекте что найдешь переведи на англ - для каждого файла при переводе подымай отдельного суб агента - 1 суб агент 1 переведеный файл

## English translation of original request

> Translate every `.md` file you find in the project into English. Assign a separate subagent to translate each file: one subagent per translated file.

## Additional user requirements

- Do not launch the Pi CLI; use a separate subagent per document through the available agent runtime.
- Assign one distinct subagent to each Markdown file being translated.
- Translate faithfully and completely, preserving the source meaning, structure, examples, links, and technical details.
- Keep translated project documents focused on Pi and do not name alternate agent runtimes.

## Acceptance criteria

- Every Markdown file present in the project inventory is assigned to a separate subagent for translation or English-language review.
- Russian content is translated into faithful English without shortening or changing the documented workflow.
- Existing English-only content is reviewed and left intact unless required to keep terminology consistent with the Pi-focused project.
- Markdown structure, code blocks, paths, identifiers, commands, links, tables, and role names remain valid.
- The task report records the inventory, assigned workers, updated paths, and verification results.

## Scope notes

- Inventory is taken from the repository workspace, including hidden project directories, while excluding `.git`.
- This task file preserves the original user request verbatim as required by the repository workflow; it is task metadata, not a translation target.