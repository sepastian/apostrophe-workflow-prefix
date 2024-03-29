# Apostrophe Workflow Prefix

This repository demonstrates how apostrophe-workflow's `link-to-locale` generates invalid URLs containing `apos.prefix` twice, when specifying a prefix in `app.js.`

When running without a prefix everything works as expected.

# Setup

This project is based on [`apostrophe-boilerplate`](https://github.com/apostrophecms/apostrophe-boilerplate); [`apostrophe-workflow`](https://github.com/apostrophecms/apostrophe-boilerplate) has been added and configured with two locales; a locale switcher has been implemented in `views/layout.html`.

Two locales have been defined to be able to demonstrate switching between locales: `en-US` and `de-AT`, both children of the `default` locale.

Apostrophe workflow has been configured with basic options, according the project's `README.md`.

# Instructions to Reproduce the Error

First, test the app without a prefix set in `app.js`; everything should work as expexted. Then, set a prefix and test again; the links generated by the locale switcher will contain `apos.prefix` twice.

## Without Prefix

To test without a prefix first, make sure to comment out `prefix: '/pre' in `app.js`. Then start the app.

1) `docker-compose up mongo` to start Mongo
2) `npm i` in another terminal to install packages
3) `node app apostrophe-groups:add admin admin` to add group
4) `node app apostrophe-users:add admin admin` to add user
5) `npm start` to start the app

Login at `localhost:3000/login`, create the page _New Page` with slug `/new-page` in the `default` locale; export to the `en-US` locale (_New Page_/`/new-page`) and the `de-AT` locale (_Neue Seite_/`/neue-seite`), commit all three versions of the page.

The language selector in the top-right corner implemented in `views/layout.html` should display options for _United States_ and _Austria_. Clicking on _Austria_ should redirect to `/neue-seite`, clicking on _United States_ should redirect to `/new-page`.

## With Prefix

Stop the app. Uncomment `prefix: '/pre'` in `app.js`. (Re)start the app as described above.

Login at `localhost:3000/login`, then go to `/new-page` to display _New Page_. Now click on _Austria_ in the locale switcher in the to-right corner. The link generated is `/pre/pre/neue-seite`, which does not exist.

# Analysis

The `link-to-locale` route defined in `apostrophe-workflow/lib/routes.js` returns a link containing `apos.prefix` twice.

I'm not yet sure why this happens and how to fix it.
