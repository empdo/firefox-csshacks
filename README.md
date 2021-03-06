# Collection of random CSS hacks for Firefox

This repository contains various styles to modify appearance of Mozilla Firefox. These stylesheets are mostly self-contained and can be mixed with each other somewhat freely, but there are no promises about compatibility with third-party styles.

In the case that a particular style relies on another style, the fact will be noted at the start of the file that requires so.

Stylesheets in this repository are tested only on Windows 10 and to a lesser amount on Linux. Most of them should also work on OSX and Windows7, but there may be wrong behavior especially when native widgets such as window titlebar or window control buttons are being styled.

# Style categories

The files themselves are only separated to *chrome* and *content* sub-folders. Files have a one or more *tag* applied to them as listed in `tags.csv` file.

You can browse the tag-categorized files by [using this simple ui](https://mrotherguy.github.io/firefox-csshacks/)

# Loading user*.css files

0. Go to about:config and set the pref `toolkit.legacyUserProfileCustomizations.stylesheets` to `true` to make Firefox load userChrome.css and userContent.css

1. Find your profile folder, if Firefox is running you can find by going to `about:support` and there should be a button with label "Open Folder" under application basics
2. Create a new folder to the profile folder and name it "chrome"
3. `userChrome.css` and `userContent.css` files should be created inside this chrome-folder.

Clone this repository or individual files inside that newly created chrome-folder.
In the end you should have a folder structure like this:

```
<profile_folder>
|_chrome
|   |_chrome
|   |_content
|   |_userChrome.css
|   |_userContent.css
|_extensions
|_prefs.js
...
all other profile folders and files
...
```

In short, create a parent chrome folder to the same directory where `prefs.js` is - the main profile folder. Firefox loads `userContent.css` and `userChrome.css` files only from that `chrome`-folder.

# Usage

Stylesheets are divided in to chrome and content folders. The difference is that styles inside "content" affect web-pages whereas styles inside "chrome" affect browser UI.

Use stylesheets under "chrome" in `userChrome.css`

Use stylesheets under "content" in `userContent.css`

The above is not a technical requirement but the particular styles *generally* won't do anything when loaded in wrong context. 

You can import the stylesheets with @-rule import like this:

```css
@import url("path/filename.css");
```

A good habit would be to load each separate style without modifications using @import statements, and then apply your own modifications in userChrome.css after all imports. This makes it easier for you to update the files from the repository since your modifications will be preserved.

Example `userChrome.css`:

```css
@import url(chrome/tab_close_button_always_on_hover.css);
@import url(chrome/tab_loading_progress_throbber.css);
@import url(chrome/button_effect_scale_onclick.css);

:root{
  --toolbar-bgcolor: rgb(36,44,59) !important;
  --uc-menu-bkgnd: var(--toolbar-bgcolor);
  --arrowpanel-background: var(--toolbar-bgcolor) !important;
  --autocomplete-popup-background: var(--toolbar-bgcolor) !important;
  --uc-menu-disabled: rgb(90,90,90) !important;
  --lwt-toolbar-field-focus: rgb(36,44,59) !important;
}

``` 

## Important!

Note that all `@import` rules need to be placed before any other rules in the file, including @namespace rules. Additionally, the order of imported files is just as important as the order of rules within one file.

I would strongly advice using @import to include styles instead of copying contents directly to userChrome.css even with just a few file "components". The technical reason for this is that some files rely on @namespace rules and those only apply on file level such that a @namespace applies to every selector in that file (and in that file only). On top of that, @imports make managing multiple files much easier.

Import any *_patch.css files *after* their base stylesheet.
Import the shared window_control_support.css *before* other stylesheets.

Additionally, you are advised to import theme_ files before any other modules.

# Theme

** NOTE ** Theme files are mostly out-of-date as of 2020-05-22

Stylesheets prefixed with `theme_` require `theme_color_variables.css` to be imported.

Example userChrome.css resulting in rather complete dark blueish-grey UI:

```css
@import url(theme_color_variables.css);
@import url(theme_sidebar.css);
@import url(theme_toolbars.css);
@import url(theme_popups_and_menus.css);

/* Your other rules here */
```

You can use individual modules from theme such as to only include popups_and_menus. But it would still be required that you import the theme_color_variables.css or you'll have to manually edit all the colors.
