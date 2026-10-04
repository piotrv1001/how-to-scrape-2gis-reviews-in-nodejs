# How to Scrape 2GIS Reviews in Node.js

This example calls the [2GIS Reviews Scraper](https://apify.com/piotrv1001/2gis-reviews-scraper) through Apify's Node.js client. It uses an existing Actor; it does not implement a scraper.

## What this example does

- Calls the Actor with one public 2GIS place URL
- Requests up to 20 recent reviews for that place
- Waits for the run to finish
- Fetches the dataset and prints the rows

## Prerequisites

Node.js, an Apify account, and an Apify API token.

## Installation

```bash
npm install
```

## Environment setup

Copy `.env.example` to `.env` and replace the sample token with your Apify API token.

## Usage

```bash
npm start
```

The example uses one Dubai place page. For several branches, add each place URL. `maxReviewsPerPlace` applies to each place; a repeated run returns recent reviews again, so deduplicate by `placeId` and `reviewId`.

## Code example

```js
import { ApifyClient } from 'apify-client';
import 'dotenv/config';

// Initialize the ApifyClient with your Apify API token
// Set APIFY_TOKEN in your .env file (copy .env.example to get started)
const client = new ApifyClient({
    token: process.env.APIFY_TOKEN,
});

// Prepare Actor input
const input = {
    startUrls: [{ url: 'https://2gis.ae/dubai/firm/70000001006697500' }],
    maxReviewsPerPlace: 20,
    sortBy: 'date_created',
    locale: 'en_US',
};

// Run the Actor and wait for it to finish
const run = await client.actor("piotrv1001/2gis-reviews-scraper").call(input);

// Fetch and print Actor results from the run's dataset (if any)
console.log('Results from dataset');
console.log(`💾 Check your data here: https://console.apify.com/storage/datasets/${run.defaultDatasetId}`);
const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach((item) => {
    console.dir(item);
});

// 📚 Want to learn more 📖? Go to → https://docs.apify.com/api/client/js/docs
```

## Example output

`sample-output.json` is **illustrative**, not a captured run. Review rows can contain `placeId`, `reviewId`, full `text`, `rating`, `dateCreated`, `source`, and `ownerAnswerText`. A null rating is not zero. Review text remains in the author's language even if `locale` changes.

## Use cases

- Review recent customer feedback for a location
- Find low-rated reviews without a visible owner reply
- Track new reviews across repeated runs by stable IDs
- Compare recurring issues across several branches

## Try the actor on Apify

**[Open the 2GIS Reviews Scraper on Apify](https://apify.com/piotrv1001/2gis-reviews-scraper)**

## Related resources

- [How to Scrape 2GIS Reviews and Find Unanswered Feedback](https://www.falconscrape.com/blog/how-to-scrape-2gis-reviews-and-find-unanswered-feedback)

## License

MIT
