# The Libre Publish plugin directory

The plugins and themes [Libre Publish](https://github.com/Martin-Business-Consultants/libre-publish)
lists in Settings › Plugins and Settings › Themes, with one-click Install.

`directory.json` names each one and pins it to the commit of the release
that was reviewed. It's signed (`directory.json.sig`, Ed25519) and the CMS
checks the signature against the public key it ships with, so it refuses a
list changed by anyone without the key. Installing from the directory
downloads exactly the pinned commit, so a release that is retagged or
replaced afterwards can't put other code on a site.

## Adding a plugin or theme

1. Release it on GitHub: a tag `v<version>` on a public repository, with
   its gemspec at the root (the gemspec's name is its key). See
   `docs/plugins.md` and `docs/themes.md` in Libre Publish.
2. Open a pull request adding an entry to `directory.json`:

   ```json
   {
     "key": "seo_plus",
     "kind": "plugin",
     "name": "SEO+",
     "description": "One sentence on what it adds.",
     "repo": "you/cms-seo-plus",
     "author": "You",
     "homepage": "https://github.com/you/cms-seo-plus",
     "version": "1.0.0",
     "commit": "<the full SHA v1.0.0 points at>",
     "requires": ">= 1.12"
   }
   ```

   `kind` is `plugin` or `theme`. `requires` is the Libre Publish versions it
   works with.
3. It's reviewed: the code at that commit, what it registers, its
   migrations, and that it only depends on gems the core bundles. CI runs
   `bin/check` (every field, and that the tag points at the commit).
4. Once merged, the workflow signs the list. Sites see it within a day, at
   their next update check.

An update is the same: a pull request moving `version` and `commit`.

## The key

The private key is the `DIRECTORY_SIGNING_KEY` secret here, and a copy is
kept offline by the maintainers. To sign by hand:
`bin/sign path/to/plugin-directory-signing.pem`.
