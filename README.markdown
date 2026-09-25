# nvALT 2

> **This fork** is a modernized, Apple silicon–only build of nvALT for current macOS.
> See [Building on Apple silicon](#building-on-apple-silicon) below.

A collaboration between Brett Terpstra (ttscoff) and David Halter (ElasticThreads) based on [DivineDominion's](github.com/divineDominion/nv) fork. nvALT adds a few features we'd been looking for (and let me get some coding practice).

![Screenshot](http://img.skitch.com/20110520-k5y4i6i3p8ciftq2dbs7rx64e7.jpg)

## Contents

- [Building on Apple silicon](#building-on-apple-silicon)
- [About nvALT](#about-nvalt)
- [What it is](#what-it-is)
- [Additional Features](#additional-features)
- [Customization](#customization)
- [Download](#download)
- [Credits](#credits)

## Building on Apple silicon

Requirements: an Apple silicon Mac and Xcode 16 or later. The app targets macOS 14 and later and is built for `arm64` only (no Rosetta needed).

1. Open `Notation.xcodeproj` in Xcode. Xcode fetches the one Swift package dependency ([swift-cmark](https://github.com/swiftlang/swift-cmark), used for the Markdown preview) automatically.
2. Pick the **Notation Release** scheme (optimized) or **Notation Develop** (debug), then build and run.

From the command line:

    xcodebuild -project Notation.xcodeproj -scheme "Notation Release" -derivedDataPath build/DerivedData build
    open build/DerivedData/Build/Products/ForBuilding/nvALT.app

Builds are signed ad hoc ("Sign to Run Locally"), which is enough to run the app on the Mac that built it. To distribute it, set your own signing team and notarize.

What changed from the original Intel build:

- **Markdown/MultiMarkdown preview** is rendered in-process with cmark-gfm (tables, footnotes, strikethrough, task lists, autolinks, smart punctuation) instead of launching the bundled Intel `multimarkdown` 4.7 executable for every refresh. A leading MultiMarkdown metadata block (or YAML front matter) is hidden from the preview, and headers get MultiMarkdown-style `id`s.
- **Encryption and hashing** use Apple's CommonCrypto instead of bundled 32/64-bit Intel OpenSSL libraries. The on-disk format is unchanged, so existing (encrypted) note databases open as before.
- **Link detection** in notes uses the system's `NSDataDetector` instead of the Intel-only AutoHyperlinks framework.
- **Sparkle** auto-updates are removed (the old feed serves the original Intel app). The "Check for Updates…" menu items are hidden.
- **HTML import as Markdown** relied on bundled Python 2 scripts, and macOS no longer ships Python 2; HTML files now import as rich text.
- **Textile** preview and **TaskPaper** conversion still use the system `perl` and `ruby`; if macOS stops shipping those, the preview says so instead of failing.

Preferences and notes are shared with the original nvALT (same bundle identifier, `net.elasticthreads.nv`).

## About nvALT

nvALT is a fork of the original [Notational Velocity][notational] with some additional features and some interface modifications. It is a work in progress. I'm not listing it as a beta, as that would imply that it was on its way to being its own product. It's an experiment, and I hope you enjoy it!

## What it is

Notational Velocity is a way to take notes quickly and effortlessly using just your keyboard. You press a shortcut to bring up the window and just start typing. It will begin searching existing notes, filtering them as you type. You can use &#x2318;-J and &#x2318;-K to move through the list. Enter selects and begins editing. If you're creating a new note, you just type a unique title and press enter to move the cursor into a blank edit area. Check out the descriptions at [notational.net][notational] for a more eloquent synopsis.

## Additional Features

nvALT adds:

* Widescreen (horizontal) layout option
* Shortcut (&#x2318;-&#x2325;-N) to collapse the notes panel
* Markdown, Textile and MultiMarkdown support with Preview window
* HTML source code tab in the Preview window for fast copy/paste to blogs, etc.
* Unique interface design changes
* Fixes for a couple of bugs/annoyances
* Customizable HTML and CSS files for the Preview window
    * You can use Javascript in the templates to do a few neat tricks

## Customization

Select "Open Custom CSS Folder" within the Preview menu, and the application's supprt folder will open. You will find two files:` template.html` and `custom.css`. If you're handy with HTML and CSS, feel free to customize these in whatever way you like. You can add Javascript as well, but you'll need to load external scripts from a url or using a full file:// path. If worst comes to worst, you can just delete or rename your customizations and the default files will be put back in place automatically when you select the menu item again.

## Download

More info and a download for the compiled binary can be found at [brettterpstra.com/projects/nvalt](http://brettterpstra.com/projects/nvalt/)

## Credits

* [Notational Velocity][notational]
* Code: The original Notational Velocity [source code][original source] by Zachary Schneirov
* Code: DivineDominion's [MultiMarkdown fork][DivineDominion]
* Inspiration: [Elastic Threads' version](http://elasticthreads.tumblr.com/nv) of Notational Velocity

[notational]: http://notational.net/
[original source]: https://github.com/scrod/nv
[DivineDominion]: https://github.com/DivineDominion/nv

