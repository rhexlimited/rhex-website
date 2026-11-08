# RHEX Reality Debate Show (The President)

Static pages, no build step. They share the main site's logo (`../assets/img/rhex-logo.png`), brand colours, fonts and the `rhex-theme` light/dark setting.

| Page | URL |
| --- | --- |
| Show page with all contestants | `/debate/` |
| A contestant's shareable voting page | `/debate/contestant.html?c=<number>` (e.g. `?c=1120`) |

## Adding a contestant

Edit `data/contestants.js` and copy one block in `contestants`. Put photos in `debate/assets/contestants/` and set `photo` to e.g. `"assets/contestants/1120.jpg"` (portrait, 4:5 works best). Without a photo, a gold monogram is shown.

## Payments (Paystack)

1. Paste your Paystack **public** key (`pk_live_…`) into `paystackPublicKey` in `data/contestants.js`. Never put the secret key in these files.
2. Until a real key is set, the vote button is disabled and visitors see "Online voting isn't switched on yet".
3. Each payment is `votes × votePrice` naira. The transaction carries `metadata.contestant_number`, `metadata.contestant_name` and `metadata.votes`, and its reference looks like `RHEX-PRES-1120-…`, so every vote can be traced in the Paystack dashboard (Transactions → filter or export).

## Counting votes

The browser can't be trusted to count votes. To show live tallies, add a server-side Paystack webhook (`charge.success`) that verifies the payment with the secret key and adds `metadata.votes` to the contestant's total. Until then, tally from the Paystack dashboard export.
