# Frequently Asked Questions

## My tool currently just uses one file, what should I do?

Most likely you should take the filename and make that (or something close to it) your directory name under `.tools/`. If your have a file, `.cslint`, that currently holds your tool settings, you'd create a folder `tools/cslint/`.

Then, you'll still need a file, but the follow-up question is, what should *that* file be called? You could name it `.cslint` still because "consistency" but a better name would probably be something like:

- `config.json`
- `settings.json`

Or, if you really want to avoid file extensions:

- `config`
- `settings`

If you chose `config.json` your app would expect to find the file in `tools/cslint/config.json` instead of in `.cslint` once the migration was complete.
