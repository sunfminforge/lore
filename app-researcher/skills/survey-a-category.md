# Surveying a category

## Picking the apps

- Start from the store's own ranking for the category on the platform the task names (the App Store's charts and its search for the category's words), then lists and reviews by publications that test apps; take the apps most of them name, and a few newer or niche ones that do something the leaders do not. {#start-from-rankings}
- Take as many apps as the task asks; where it names none, about eight, and say that you chose the number and how you picked them. {#how-many-apps}
- Write down each app's store URL and ID first: the store page is the anchor every later claim about the app points back to; on the App Store, Apple's public lookup (`https://itunes.apple.com/lookup?id=<id>&country=<cc>`) gives its price, rating, rating count, last update and description as JSON in one fetch, far easier than reading the page, and its search (`/search?term=<words>&entity=software&country=<cc>`) finds the IDs. {#store-url-first}

## Reading each app the same way

One row per app, every column filled or marked "not found", each value with its URL:

- what it is, in one line, and for whom, as its store page says; {#row-what-and-for-whom}
- price: free, paid, subscription, in-app purchases, and what the free tier holds, as the page lists it; {#row-price}
- rating and how many ratings, with the date you read them, since both move; {#row-rating}
- its main features, as the page or its own site lists them, said in your words; {#row-features}
- what reviewers praise and complain of, read from a set a reviewer can recount: on the App Store its customer reviews feed (`https://itunes.apple.com/<cc>/rss/customerreviews/id=<id>/sortBy=mostRecent/json`, up to 50 recent reviews a page), else the review data in the store page's source; count with a short script over reviewer, date and text, developer replies apart, never by eye from the rendered page, which drops reviews; code each review from its text, not its stars, as a bug, a feature request, praise or an experience, one review often several; write every theme as n of m with the source of m, and where a store shows only a summary, say counts not obtained; {#row-praise-and-complaints}
- platforms, and when it was last updated. {#row-platforms-updated}

## The source log

- Keep a log as you read, one line per page: URL, date read, what it gave. Every claim in the survey names a line of it by its URL; a claim you cannot point back to is cut. {#source-log}
- Prefer the source that owns the fact: the app's store page or its own site for what it offers and costs, the store for ratings, a dated article or the reviews for what users find. A write-up of a write-up is cited only with the source it cites. A roundup written by the maker of one of the apps it ranks is marked as the maker's; where no independent article covers an app, write article not found rather than stretch one that barely names it. {#source-that-owns-it}
- A page that will not load without signing in, paying or installing is not read: it goes into the log as not read, and why. {#not-read-is-logged}
- An official table published as a PDF: the fetch tool reads no PDF text, so look first for an HTML copy that a university or government page republishes and cite both; else render the pages whole as images and read them, since rotated pages often run past their crop box and a crop cuts columns off; say in the log how the table was read. {#tables-in-pdfs}
- Fetch a store one page at a time: Apple's lookup and search allow about 20 calls a minute, and a store page answers 429 to quick repeats; on a 429, wait half a minute and retry that one page, then go on. {#fetch-singly}

## Across the category

After the rows, what the category shares and where it differs: features nearly every app has (the table stakes), features only a few have, the complaints that come back across apps, and the gaps no app fills. Each of these is a finding, numbered, citing the rows it rests on.
