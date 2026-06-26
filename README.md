## o3-shop Childtheme

### Shopversion

o3-shop


### general information

This o3-shop child theme can be used for update-safe adjustments to the o3-shop theme.

The folder structure already exists, so you just have to copy the files you want to change from o3-shop into the corresponding folder.
If the child theme is activated, the files it contains are primarily used by the o3 shop.

Don't copy all the files into the child theme, just the ones you want to customize!
The o3-shop takes the files that are not in the child theme from the original o3 theme.


### Installation (short version)

`composer require o3-shop/o3-child`


### Installation (long version)

Create a connection to the server on which your o3-shop eShop is located via SSH client.
Go to your o3-shop project directory, where the composer.json file and the source and vendor folders are located.
Run the following command there: `composer require o3-shop/o3-child`


### Activate child theme

In the admin area under *Extensions → Themes → o3-child_ChildTheme* click the *Activate* button.
Then clear the cache and off you go!

The "parent theme" may need to be updated to the current version!

### Development

**Use this method if:** You want to customize the theme or develop new features.

| Requirement | Minimum Version | Notes |
|------------|----------------|-------|
| Node.js | v22.13.0 | [Download](https://nodejs.org/) |
| npm | 10.x | Included with Node.js |
| Gulp CLI | 3.0.0 | [Download](https://gulpjs.com/) |

#### Copy build files for existing theme

```bash
cp -r <DOCUMENT_ROOT>/vendor/o3-shop/o3-theme/build <DOCUMENT_ROOT>/source/Application/views/o3-child
cp -r <DOCUMENT_ROOT>/vendor/o3-shop/o3-theme/package.json <DOCUMENT_ROOT>/source/Application/views/o3-child
```
Available gulp tasks (run from the theme root):

| Command | What it does | When |
|---|---|---|
| `gulp` | Production build: minified JS/CSS, PurgeCSS pruning, asset optimization | before shipping |
| `gulp dev` | Watcher: rebuilds JS on `build/js/**` change, rebuilds CSS on `build/scss/**` change, clears the shop's Smarty `tmp/` on `**/*.tpl` and translation-file changes | day-to-day |

While running `gulp dev`, keep the shop in dev mode (theme settings
→ disable production mode) so the browser serves the un-minified
assets and source maps.

There's a small in-frontend **mode-tool** widget that surfaces which
mode the shop is currently in; enable it from the theme settings to
avoid the "why isn't my change showing up?" tax.

---

### PurgeCSS safelist

Production builds run PurgeCSS over the compiled CSS and drop any
class that isn't seen in the templates / JS. Classes constructed
dynamically slip through:

```smarty
{* dynamic class — PurgeCSS doesn't see "grid-view" or "line-view" anywhere literal *}
<div class="[{$type}]-view"></div>
```

Add those to the safelist in `gulpfile.js` (the PurgeCSS config
block, around line 81):

```javascript
safelist: [
    'grid-view',
    'line-view',
    /^custom-/,   // patterns work too
]
```

---

### Where to put new SCSS / JS

#### JavaScript

- **Main bundle** — `build/js/main.bundle.js`. `import` new modules
  here to ship them in the main page bundle.
- **Standalone widgets** — `build/js/widgets/`. Loaded explicitly
  from templates:

  ```smarty
  [{oxscript include="js/widgets/checkagb.js" priority=10}]
  ```

JavaScript shipped by modules can also be folded into
`main.bundle.js`; the file's header comments give worked examples.

#### SCSS

- **Main bundle** — `build/scss/main.bundle.scss`. `@import` your
  partials here to include them globally.

Same pattern for module SCSS: examples in the bundle file's comments.

---

### jQuery

The theme targets Bootstrap 5, which uses vanilla JS rather than
jQuery, so the global jQuery is not loaded by default.

If a third-party module genuinely needs jQuery, enable it from the
theme settings (same version the legacy wave theme used). Build-time
deprecation warnings around jQuery are harmless and will disappear
once Bootstrap moves past its current compat shims.

---

### PayPal compatibility

The PayPal module's template checks the active theme literal:

```smarty
[{if $oViewConf->getActiveTheme()=='flow'}]
```

If you're integrating PayPal with o3-theme, change `'flow'` to
`'o3-theme'` in that template. (Tracked separately as something the
module should handle itself.)

---

### Issues

Report bugs and feature requests on the umbrella project tracker:

<https://github.com/o3-shop/o3-shop/issues>

Tag with the project name **O3 Theme** so triage finds them.
Issues that are clearly theme-internal (build, SCSS, JS bundling)
can also go straight on this repo's tracker:
<https://github.com/o3-shop/o3-Theme/issues>.



### License

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.