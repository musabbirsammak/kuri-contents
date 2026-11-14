# content.json schema

`content/content.json` is one JSON object. All text that parents see is stored as a language map, for example `{ "bn": "…" }`, so more languages can be added later as `"en"` and so on.

## Top level

| Field | Type | Meaning |
|---|---|---|
| `schemaVersion` | number | Version of this file format. Changes only when the structure changes, together with the app code. |
| `contentVersion` | number | Raised by 1 for each content release. The app uses it to know when to reload content. |
| `builtFrom` | string | Where the first version came from. Informational. |
| `languages` | string[] | Language codes present in the text maps. |
| `domains` | array | Development areas (see below). |
| `routines` | array | Daily routine moments used to order tasks. |
| `ageGroups` | array | The 12 age groups. |
| `tasks` | array | Daily play tasks. |
| `milestones` | array | Milestone checklist items. |
| `safetyTips` | array | Safety tips per age group. |
| `facts` | array | "আপনি কি জানেন?" facts per age group, each with a research reference. |
| `parentingReflections` | array | Reflections for parents, each with a research reference and an age range. |
| `cards` | object | The three escalation cards. |
| `strings` | object | Other app text (consent screen, buttons, onboarding…). |
| `helpPlaces` | array | Entries on the "কোথায় যাব?" screen. |

## Common item fields

Tasks, milestones, safety tips, facts and parenting reflections share these fields (parenting reflections have `fromGroup` and `toGroup` instead of `groupId`):

| Field | Type | Meaning |
|---|---|---|
| `id` | string | Permanent ID. Never changed or reused. |
| `groupId` | string | Age group, `G01`–`G12`. |
| `text` | `{bn}` | The text parents see. |
| `status` | `"draft"` \| `"reviewed"` | Expert review state. |
| `reviewerNote` | string, optional | A question or comment for the reviewer. Not shown in the app. |
| `retired` | `true`, optional | Hidden in the app but kept so the ID is never reused. |

## `domains[]`

| Field | Meaning |
|---|---|
| `id` | `S` social/emotional, `L` language/communication, `C` thinking/learning, `M` movement |
| `name` | `{bn, en}` display name |

## `routines[]`

| Field | Meaning |
|---|---|
| `id` | `morning`, `meal`, `housework`, `bath`, `care`, `play`, `outing`, `afternoon`, `evening`, `bedtime`, `anytime` |
| `order` | Position in the day, used to sort a day's tasks. `anytime` tasks can go anywhere. |
| `name` | `{bn}` label |

## `ageGroups[]`

| Field | Meaning |
|---|---|
| `id` | `G01`–`G12` |
| `order` | 1–12 |
| `name` | `{bn}` group name |
| `startMonths`, `endMonths` | Age window in months (corrected age for babies born early, until 24 months) |
| `checkpointMonths` | Age of the checklist at the end of the window |
| `doctorCheckSuggestion` | `null`, `"general"`, `"autism"` or `"general_and_autism"`: show the one-time "ask a doctor about a development check" suggestion at this checkpoint |
| `summary` | `{bn}` (optional): what many children do at this age and which play helps most. Shown on the Today screen. |
| `notes[]` | Extra messages: `id`, `when` (`doctor_card`, `doctor_card_preterm`, `on_request`), `text` |

## `tasks[]`

ID pattern: `T<group>-<n>`, for example `T04-01`.

| Field | Meaning |
|---|---|
| `why` | `{bn}` (optional): why this play helps, what the child practises. Shown under the task. |
| `safety` | `{bn}` or `null`: safety line shown with the task |
| `domains` | One or more domain IDs |
| `routine` | A routine ID |
| `milestoneIds` | Milestones this task supports. Used to prioritise tasks for milestones answered "এখনো না" or "নিশ্চিত নই". |

## `milestones[]`

ID pattern: `M<checkpoint months>-<domain><n>`, for example `M09-S3`.

| Field | Meaning |
|---|---|
| `checkpointMonths` | Checklist age |
| `domain` | Domain ID |
| `allowNotApplicable` | `true` if parents may answer "প্রযোজ্য নয়" (culture-dependent items such as fork use) |
| `source` | `"CDC 2022"` or `"Kuri team"` |

## `safetyTips[]`

ID pattern: `SF<group>-<n>`, for example `SF04-01`. Only the common fields.

## `facts[]`

ID pattern: `F<group>-<n>`, for example `F04-02`. The app shows one at random on the group's screen, with a button for another, and can list them all.

| Field | Meaning |
|---|---|
| `summaryEn` | What the paper found, in English, for reviewers. Not shown in the app. |
| `reference` | The study behind the fact (see below). |

## `parentingReflections[]`

ID pattern: `P<n>`, for example `P03`. Short, kind reflections that help parents love the child as they are: warmth over comparison, praise for effort, love without conditions, no shaming or hitting. Shown at random, or all together on request, while the child's age group is between `fromGroup` and `toGroup`.

| Field | Meaning |
|---|---|
| `fromGroup`, `toGroup` | First and last age group (`G01`–`G12`) in which it is shown |
| `title` | `{bn}` short heading |
| `text` | `{bn}` the reflection |
| `summaryEn` | What the paper found, in English, for reviewers. Not shown in the app. |
| `reference` | The study behind it (see below). |

## `reference`

Used by facts and parenting reflections.

| Field | Meaning |
|---|---|
| `citation` | Authors (year). Title. Journal, volume(issue), pages. |
| `doi` | The paper's DOI, for example `10.1126/science.7375928`, or `null` if it has none |
| `url` | Link the app opens. Usually `https://doi.org/<doi>`; a PubMed link when there is no DOI. |

## `cards`

| Key | Meaning |
|---|---|
| `standard` | Calm "talk to a doctor" card. `body` uses `{name}` and `{list}`. `actions[]` are its buttons. |
| `lostSkills` | Shown when a milestone once answered "হ্যাঁ" is later answered "এখন আর করে না". |
| `emergency` | Always one tap away. `signs[]`, `footer`, `callLabel`, `callNumber`. |
| `welcome` | "কেন কুঁড়ি?" on the first screen. `title`, `reasons[]` (each `text`, `summaryEn`, `reference`, see facts), `closing`. Say only what the studies found: they tested parenting programmes, not this app. |

## `strings`

A flat map from a key (used in the app code) to `{bn}` text or a list of `{bn}` items. Words in curly braces, such as `{weeks}`, `{actual}`, `{corrected}` and `{count}`, are filled in by the app and must be kept.

Keys used by facts and parenting reflections: `facts_title`, `reflections_title`, `show_another`, `show_all`, `source_label`, `source_open`, `source_language_note`, `facts_average_note`.

## `helpPlaces[]`

| Field | Meaning |
|---|---|
| `id` | Permanent ID |
| `name` | `{bn}` or `{en}` |
| `phone` | Optional phone number |
| `enabled` | Shown in the app only when `true` |
| `status` | `draft`, `verify` or `reviewed` |
| `reviewerNote` | Optional |
