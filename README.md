*NOTE: this is a fork of the original project, that is no longer being maintened, and has been updated to work with SUBLIME TEXT 3 and clj deps. Many thanks to jasongilman for his great work!*

SublimeClojureSetup
===================

A description of how I setup Sublime Text for Clojure development. It's a bit hacky but it works if you prefer to use Sublime Text for your editing. This is specific to my own workflow on a Mac but should be mostly applicable to development on other platforms with Sublime Text.

Initially based on instructions here: https://gist.github.com/jamesmacaulay/5457344

## Why Sublime Text?

I have tested many other code editors but always find myself returning to Sublime Text with SublimeREPL. And why is that? Even though its setup is hacky and is far from perfect, no other editor can match its raw speed and convenience of keybindings shortcuts. As with many things, picking and setting up your code editor is more art than science and what works best for you is up to your specific tastes and requirements.

## Prerequisites

  * Sublime Text 3
  * Java
  * [Leiningen 2](http://leiningen.org/)

### Leiningen

Install it then create `~/.lein/profiles.clj`. See [`profiles.clj`](profiles.clj) in this repo for contents.

## Sublime Text 3 With Clojure

This documents how to setup Sublime Text 3 with Clojure. It uses the sublime text [package manager](https://sublime.wbond.net/installation#st2). Follow the installation instructions to install the package manager into Sublime Text 3.
If you already have the package manager installed make sure all packages are up to date.

   * Bring up the command pallet - __cmd + shift + P__
   * Select: __Package Control: Upgrade/Overwrite All Packages__
   * It should finish successfully without any feedback. You can see sublime logs by hitting __ctrl + ~__


### How to perform basic tasks

#### Open a Clojure REPL

The Sublime REPL is used to open a REPL within Sublime Text. It can be opened by:

  * Invoke the command pallet - __cmd + shift + P__
  * Type `repl` and select __SublimeREPL: Clojure__
    * Doing this repeatedly will make it the first one to come up when typing `repl`
  * Alternately it can be opened with the key combination __alt + super + l__.

__Very Important__: Most of the time you'll want a REPL open for the current project. Sublime REPL only knows to connect the REPL to the current project if you start the REPL with a Clojure file open in the project and your cursor is in it.

Alternatively, if you wish to run the REPL with clj deps, you'll need to  to open the SublimeREPL package (`~/Library/Application\ Support/Sublime\ Text\ 3/Packages/SublimeREPL` in OSX, `~/config/sublime-text-3/Packages/SublimeREPL` in Ubuntu) and update the contents of `config/Clojure/Default.sublime-commands` and `config/Clojure/Main.sublime-menu` according to this [pull request](https://github.com/wuub/SublimeREPL/pull/537/commits/47bad8803f7eb6a3050ece1e88ea8b37f8e0947e#diff-c699bfc1a6f163eadf899a507a7fcfe4). Then, 

  * Invoke the command pallet - __cmd + shift + P__
  * Type `repl` and select __SublimeREPL: Clj__

#### Keybindings

These are setup in the instructions Sublime Text setup instructions below.

  * Bindings interacting with the REPL (Require a single Clojure REPL open and running.)
    * __alt + super + l__ - Starts a new Clojure REPL. Make sure to have cusor in an open Clojure file.
    * __ctrl + d__ - Exit the repl
    * __ctrl + l__ - Clear the repl
    * __alt + cmd + r__ - Refresh all code in project by running user/reset. It will fail if a project does not define the user/reset function.
    * __alt + cmd + shift + r__ - The super refresh. If you delete files or rename things sometimes clojure.tools.namespace will have issues. This runs (clojure.tools.namespace.repl/clear) prior to calling reset.
    * __alt + cmd + b__ - Transfer text from the current block to the repl and executes it.
        * This is useful when you have sample code in a clojure file open next to a REPL. Put your cursor within a block of the sample code and invoke the keystroke. The closest block around or near the cursor will be executed in the REPL within the namespace of the file it comes from.
    * __alt + cmd + s__ - Transfers selected text to the repl and executes it. Similar to alt + cmd + b.
    * __alt + cmd + x__ - Runs tests from current test file. Refreshes code in project first.
      * Invoke the keystroke with your cursor in the test file. It will run all the tests in the file in the REPL.
    * __alt + cmd + t__ - Runs a selected test var. Configurable whether this does a refresh first or not.
    * __alt + cmd + a__ - Runs all the tests in the project. Refreshes code in project first. The REPL will still be responsive while tests are running. You can cancel the running tests by refreshing the repl with alt + cmd + r.
    * __alt + cmd + d__ - Print documentation of the selected function.
      * Select a function name in a file including the namespace if part of the call and hit the keystroke.
    * __alt + cmd + c__ - Print source code of the selected function.
    * __alt + cmd + n__ - Print the vars in the selected namespace or namespace alias.
    * __alt + cmd + shift + n__ - Print the documentation of the vars in the selected namespace or namespace alias.
    * __alt + cmd + o__ - Opens the file and scrolls to the line where the selected var is defined. Also works on selected namespaces and with code in jar files.
      * Assumes the `subl` command line argument works. See https://www.sublimetext.com/docs/2/osx_command_line.html
    * __alt + cmd + p__ - Pretty print the value that was last returned in the repl.
  * Other useful keystrokes (Built into Sublime)
    * __ctrl + m__ - Jumps cursor to close or beginning of current block. Press repeatedly to go back and forth.
    * __ctrl + shift + m__ - Select code within current block. Repeated pressing expands selection.
      * This is very useful for selecting a block of Clojure code to cut and paste in a new area.

### Setup

#### Install These Packages

Install these packages using the package manager.

  * SublimeREPL
  * lispindent
  * BracketHighlighter
  * EnhancedClojure

#### Sublime Preference Files

##### Fix Match Brackets

Add this code to your user preferences

  * Add the following lines to `~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/Preferences.sublime-settings`

```
// This needs to be disabled since we're using Bracket Highlighter for highlighting brackets
"match_brackets": false
```

##### Fix identification of Clojure Symbols

Sublime Text doesn't correctly identify Clojure symbols.

  * Create `~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/Clojure.sublime-settings` with the following contents.

```
{
  "extensions":
  [
    "cljs"
  ],
  "word_separators": "\\()\"',;@$%^&|[]{}`~"
}
```


##### Disable auto-pairing of single quotes

Clojure uses single quote characters by themselves like `(def my-literal-list '(1 2 3))`. Sublime Text will automatically close single quotes. This becomes annoying when writing Clojure code in sublime text. Turn it off by following these steps:

  * Open `~/Library/Application\ Support/Sublime\ Text\ 3/Packages/Default/Default (OSX).sublime-keymap`
  * Search for "Auto-pair single quotes" which should be on line 274 or so.
  * Comment out the block of about 30 lines directly following that comment to disable pairing of single quotes only.

##### Add Leiningen to SublimeREPL Path

This file updates SublimeREPL settings so leiningen in on the path. Update this file to include the directory where you installed leiningen.

  * Edit `~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/SublimeREPL.sublime-settings` with the following changes

```
{
  // Has to include path to lein
  "default_extend_env":
  {
    "PATH": "REPLACE_ME_WITH_LEIN_DIR:{PATH}:/usr/local/bin"
  }
}
```
#### Configure Lisp Indentation Settings

The lispindent.sublime-settings defines which forms use function style indentation. I've added additional ones that are typically used in Clojure Applications

    * `cp lispindent.sublime-settings ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/`

#### Speed up text transfer in SublimeREPL

Change `~/Library/Application\ Support/Sublime\ Text\ 3/Packages/SublimeREPL/config/Clojure/Main.sublime-menu` line 22 from
```
"osx":  ["lein", "repl"]
```
to:
```
"osx":  ["lein", "trampoline", "run", "-m", "clojure.main"]
```

This greatly improves the speed at which text is sent from a Clojure window to the REPL. Based on answer here http://stackoverflow.com/questions/20835788/is-it-normal-to-have-really-slow-text-transfer-in-sublime-text-2-with-the-clojur

##### Setup Keybindings

We'll setup some keybindings in Sublime Text to make it easier to send code to the repl, run tests, etc.

  * Append the [`clojure_keybindings.sublime-keymap`](clojure_keybindings.sublime-keymap) in this repo to `~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/Default (OSX).sublime-keymap`
    * Keymap files contain JSON so make sure they're valid when saving or Sublime Text will fail with errors.

##### Clojure Helpers

The [`ClojureHelpers.py`](ClojureHelpers.py) file provides some helper functions that make working with Sublime REPL and Clojure a little better. These are associated with key bindings. You can also add your own helpers to this file.

  * Copy `ClojureHelpers.py` to `~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/`
    * `cp ClojureHelpers.py ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/`

##### Clojure Snippets

Copy the file `clojure_snippets.sublime-snippet` to `~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/clojure_snippets.sublime-snippet`. It contains some useful completions.

