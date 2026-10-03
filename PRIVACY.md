# Privacy policy: Nero Labs Data Tools plugin

Last updated: 3 October 2026

This policy covers the Nero Labs Data Tools plugin for Claude. The plugin is published by Nero Engine Group Ltd, trading as Nero Labs, registered in England and Wales (company number 16521432), registered office 66 Paul Street, London, EC2A 4NA. We are registered with the Information Commissioner's Office under number ZB914271. Contact: founder@neroengine.com.

## What the plugin is

The plugin is a set of written instructions (skills) and one connector entry. It contains no code that runs on your device and it has no server of its own. It collects nothing and stores nothing.

## Where your data goes

When you ask Claude to run one of the tools, Claude sends the inputs you give it (Apify dataset IDs, links to files, Google Sheets, websites or recordings, and rows of data) to Apify's hosted MCP server at mcp.apify.com, signed in with your own Apify account. Apify runs the matching Nero Labs Actor in your Apify account and keeps the results in your account's storage under Apify's own retention rules and privacy policy (https://apify.com/privacy-policy).

Some Actors pass your data to one outside service to do their job:

- Bulk Transcription and Call Score send the audio of the recordings you supply to OpenAI (api.openai.com) for transcription and review. OpenAI does not use API data to train its models by default.
- Bulk PageSpeed & Lighthouse Checker sends each page address to Google's PageSpeed Insights API.
- HTTP Request Sender sends your rows to the API address you choose.

Every other tool does its work inside the Apify run.

## What Nero Labs sees

As the Actor developer, Nero Labs sees the run counts and the charges that Apify reports to developers. We do not see the contents of your inputs or your results, and we do not sell or share any data.

## Your choices

You decide what to send. You can delete runs, datasets and stored files at any time in your Apify Console, and you can disconnect the connector in Claude at any time. Do not send personal data you are not allowed to process; if you do send personal data, you remain responsible for it as its controller.

## Complaints

Contact founder@neroengine.com first. You can also complain to the ICO at https://ico.org.uk/make-a-complaint/.
