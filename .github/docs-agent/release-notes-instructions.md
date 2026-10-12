# Release notes agent instructions

You draft release notes for the Products API when a pull request changes an
OpenAPI spec. A technical writer reviews everything you produce before it
merges. Your job is a solid first draft plus clear pointers to what needs a
human check.

## 1. Find what changed

Run:

    git diff origin/<BASE BRANCH>...HEAD -- api-specs/

Read the full spec files that changed, not only the diff, so you understand
where each change sits (which endpoint, which schema, request or response).

## 2. Classify each change

Use exactly one category per change:

| Category | Examples |
| --- | --- |
| **Breaking** | Removed endpoint, field, or parameter. Renamed field. Changed field type. A field or parameter that became required. A removed response code clients may rely on. |
| **Added** | New endpoint. New optional field or parameter. New response code. |
| **Changed** | Non-breaking behavior change, such as a new default value or a relaxed constraint. |
| **Deprecated** | Anything newly marked `deprecated: true`. |

Rules:
- A new field in `ProductInput` that is listed under `required` is Breaking,
  because existing create requests will start failing.
- The same field added to several schemas (for example `Product`,
  `ProductInput`, and `ProductUpdateInput`) is ONE release note entry.
- Changes that don't affect API users, such as reordering, whitespace, or
  edits to descriptions only, get no entry. Note them in the PR comment instead.

## 3. Write the entry

Edit `docs/release-notes.md`. Add entries under the `## Unreleased` heading,
in the matching `###` subsection (Breaking, Added, Changed, Deprecated).
Create a subsection only if it doesn't exist yet. Never edit released versions.

Style:
- Start each entry with the field or endpoint name in code font, then say
  what the user can now do or must now do.
- Second person, present tense. One or two sentences.
- Name the affected endpoints, for example `GET /products/{productId}`.
- For Breaking entries, add a second sentence telling users what to change.
- End each entry with the PR number in parentheses, for example `(#12)`.
- Take field names, types, and required status from the spec only. Never
  guess what a field means beyond its `description`.

Example:

    ### Added
    - `sku` field on products. Returned by `GET /products` and
      `GET /products/{productId}`, and accepted when you create or update a
      product. Optional. (#12)

If an entry for this PR already exists (search for its PR number), update it
instead of adding a duplicate. This keeps reruns clean when the PR gets new
commits.

## 4. Check the spec itself

For every field or parameter that was added or changed, check whether it has
a `description` and an `example`. Do NOT add or invent them. List any that are
missing in the PR comment so a writer can supply them.

## 5. Commit

Commit only `docs/release-notes.md`, with this message:

    docs: draft release notes for #<PR NUMBER> [skip ci]

Then push to the PR branch. Never change any other file.

## 6. Comment on the PR

Post one comment with `gh pr comment`, in this shape:

    **Release notes drafted** in `docs/release-notes.md`.

    **Entries:** <category>: <one-line summary>, one per line

    **Check:**
    - <each field missing a description or example>
    - <anything you were unsure how to classify, and why>

If there is nothing to check, write "Check: nothing flagged."
If no change needs a release note, commit nothing and comment:
"No release note needed: <reason>."
