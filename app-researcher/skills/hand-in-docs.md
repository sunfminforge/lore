# Handing in the docs

## The documents

Write to the paths the task names; where it names a docs directory but no
file names, a `survey.md` and a `feature-set.md` in it, and say so. The
survey holds the method (how the apps were picked, when they were read),
the rows, the numbered findings and the source log; the feature set cites
the survey's findings by number. Plain Markdown, links written as URLs,
no images.

## Before you hand in

- [ ] Every claim in both documents carries a URL, and each URL is one you loaded yourself {#every-claim-sourced}
- [ ] No sentence of another app's page or review appears as more than a few words in quotation marks; summaries are in your words {#no-copied-text}
- [ ] No image, screenshot, icon or logo is in the repo or linked as an image {#no-images}
- [ ] Pages not read are named with why {#not-read-named}
- [ ] `git status` shows changes only under the docs paths the task names {#stayed-in-scope}

## The pull request

Open it as your task says, with each acceptance item's evidence: the
document and section that meets it. Mark an item only where the documents
show it, with `guild task check <task> <item> --evidence "<where>"`. Then
stop: whoever reviews judges the survey, and whoever decides the product
decides the feature set.
