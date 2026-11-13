# কুঁড়ি (Kuri) — content

The content of **Kuri**, an offline Android app that helps Bangladeshi parents follow their child's development from birth to 5 years: daily play tasks, milestone checklists, safety tips and app text, all in Bengali.

The app bundles this content, so the app code and the content can be changed separately. Anyone can edit the content with the browser editor in this repository, without Android Studio.

> **Status:** draft. None of this content should reach parents until a Bangladeshi child-development professional has reviewed it. Every item carries a `status` (`draft` or `reviewed`) to track that review.

## What's here

```
content/content.json   all app content (the file the app reads)
editor/index.html      browser editor for content.json
docs/SCHEMA.md         what every field in content.json means
```

## Editing content

1. Pull the latest version:
   ```
   git pull
   ```
2. Open `editor/index.html` in **Chrome** or **Edge** (double-click the file).
3. Click **Open content.json** and choose `content/content.json`.
4. Make your changes, then press **Save** (or Ctrl+S). The editor checks your changes first and writes straight back to the file.
5. Review and commit:
   ```
   git diff
   git add content/content.json
   git commit -m "content: <what you changed>"
   git push
   ```

Firefox and Safari can't write to the opened file, so there Save downloads a new `content.json` instead. Move it over `content/content.json` before committing.

### What the editor does

- Lists each age group's tasks, milestones and safety tips, plus the shared cards, app text, help places and labels.
- Marks items `draft` or `reviewed` and holds reviewer notes.
- Links tasks to the milestones they support.
- Filters by status, reviewer notes and tasks with no linked milestones; searches across all items.
- Refuses to save if text is empty, an ID is duplicated, a link points to a missing milestone, or a `{placeholder}` was removed.
- Writes JSON in a fixed format, so `git diff` shows only the lines that actually changed.

## Rules for content

- **IDs never change and are never reused.** The app stores parents' answers by ID (`T04-01`, `M09-S3`, `SF04-01`). The editor creates IDs for new items and doesn't let you edit them.
- **Retire, don't delete.** A retired item stays in the file with `"retired": true` and the app hides it. Only items added in the current editing session can be deleted.
- **Keep `{placeholders}`** such as `{name}` and `{count}`. The app fills them in.
- **Never change the meaning of a reviewed item without setting it back to `draft`.**
- **Raise `contentVersion` by 1** (on the editor's Overview page) for each release that goes into the app.

## Using the content in the app

Copy `content/content.json` to `app/src/main/assets/content.json` in the Android project. Development builds can show all items; release builds should show only items with `"status": "reviewed"` that aren't retired.

## Sources

- Milestones are adapted and translated from the CDC *Learn the Signs. Act Early.* milestone checklists (2022 revision).
- Daily tasks follow the play-and-talk approach of the WHO/UNICEF *Care for Child Development* package, adapted to Bangladeshi households.
- Emergency signs follow the WHO/UNICEF IMCI general danger signs.

Translations and adaptations are pending expert review.
