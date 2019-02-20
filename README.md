# ACSI
![version](https://img.shields.io/badge/version-0.31-yellowgreen.svg) ![platform](https://img.shields.io/badge/platform-Linux-lightgrey.svg) ![sheets translated](https://img.shields.io/badge/Sheets%20Translated-2-blue.svg) ![GitHub last commit](https://img.shields.io/github/last-commit/Anduh/acsi.svg)
![stars](https://img.shields.io/github/stars/Anduh/acsi.svg?style=social)

**Automated Character Sheet Internationalizer (ACSI)** is a simple program to partially automate the [internationalization](https://wiki.roll20.net/Character_Sheet_i18n) of existing [HTML Character Sheets](https://github.com/Roll20/roll20-character-sheets) made for the [Roll20 platform](https://en.wikipedia.org/wiki/Roll20). It takes a html file, creates a copy where it places `i18n`-tags according to some of the [Character Sheet i18n](https://wiki.roll20.net/Character_Sheet_i18n) standard.
Works at least on **Ubuntu 16.04 LTS**, probably on most other Linux distros.


At it's current state, ACSI can only create i18n-tags for [standard text](https://wiki.roll20.net/Character_Sheet_i18n#Standard_Text), that doesn't contain many line-breaks or special characters. It attempts to ignores sections inside `<script type="text/worker"> </script>` and `<rolltemplate> </rolltemplate>`. If a sheet contains lots of commented html code, ACSI might be thrown off and produce an unusable result. ACSI is also assumed to be used on a sheet containing no i18n-tags or html error, as it isn't yet capable of noticing existing tags or if the sheet contains html syntax errors.

ACSI isn't too exact when it searches for sections it want to translate or avoid, so especially sections containing `script` or `rolltemplate` outside the `<script>` and <rolltemplate> can introduce more errors. 

ACSI is written in C, and relies on a few Unix commands, so it needs to be run in a Linux enviroment to work(Project author uses Ubuntu 16.4 TLS). Doesn't rely on any advanced libraries. 

# Install/Run
- **1.** Download the project (`clone https://github.com/Anduh/acsi.git` is probably easiest)
- **2A.** Open the command line command in the `acsi` folder and run `make` command. ACSI should compile now from the files in the `/src` folder.
- **2B.** Install the `.deb` package from the `/linux/` folder

## Makefile options
There exist a few make commands to do other things than just a clean build:

**build,b**:	builds without removing the .o files

**clean,c**:	removes all .o files

**tclean, tc**:	removes all translated files(those with 'i18n-' prefix)

**test, t**:	builds a `test` instance you can have in parallel with your main `acsi` 

# Use
After having compiled ACSI, you need to place the html file you want translated in the same folder as ACSI is located. A testfile `testsheet.html` is provided so you can try it out right away(try run `./acsi testsheet.html`).

You can now call ACSI from the command line(when your command line have the ACSI as active directory): `./acsi <filename>`, where you replace `<filename>` with the name of your file(must be a `.html` file).

ACSI will produce a new file (with "i18n-" as prefix) where it insert the i18n-tags in the proper places without touching the original file.
If everything worked out as it should, it should display a short `<i18n-filename> created!` on the command line. 

## Optional
If you run `./acsi <filename> t`, ACSI will print some extra info while running the program that can be helpful for debugging.

# Changelog

## V0.31 (2019-02-20)
- add .gitattribute to force LF line endings

## V0.3 (2019-02-19)
- optional testmode added
- include a testfile

## V0.2 (2019-02-18)
- code/file cleanup
- fixed credits typo
- rewrote readme
- html comments, sheetworkers and rolltemplates doesn't  any longer have a negatively impact  when creating `i18n`-tags

## V0.1
- make data tags for text displayed in html elements, `<p>like this text</p>`
- treats all text; comments, roll templates & sheetworkers alike, best to avoid if your sheet contains much of it
- autogenerated tags are at times clumsy, and duplicates may arise as it's currently taken in consideration

# Project Aim and Development Roadmap
The primary primary focus of the project is to prioritize on implementing the largest timesavers when it comes to creating `i18n`-tags, and in the end to automate as much as possible of the process. As a secondary goal it serves to improve programming/coding/documentation habits of the participants.

ASCI has a "two birds, onestone" origin from a 'C/Unix'-programming course, which explains the not too optimal choice of C for a text-parsing program.
If the interests arise, ACSI could be adapted/rewritten with other libraries/modules/languages for ease of further development or just portability but it's of low priority.

## 'Alpha' (V.0.5)
The aim for the Alpha version is to have ACSI reliably create 40-70% of the i18n-tags for any given sheet, and not to get thrown off by stray html comments or special characters in unexpected places.

- Makes less mistakes with creating tags(improve operation syntax)
- Ignore `cs>` , `cs<` , `cf>`, `cf<` (credits to David for noticing)
- Avoid creating duplicate tags and giving same tagname to sections that aren't identical to eachother

## 'Main Goal'(V.1.0)
V.1.0 aims to reliably create 70-95% of i18n-tags for any given sheet, and provide more automation options, such as generating the associated `translation.json` file.

- make tags for [Element Attribute Text](https://wiki.roll20.net/Character_Sheet_i18n#Element_Attribute_Text) like `title`, `label` and `placeholder`
- make tags for [dropdown menu options](https://wiki.roll20.net/Character_Sheet_i18n#Dynamic_Key_Replacement)
- make tags for [Roll Templates](https://wiki.roll20.net/Character_Sheet_i18n#Roll_Templates)
- create the `translation.json` by itself (replicating the browser's [console.log()](https://wiki.roll20.net/Character_Sheet_i18n#Step_Two.2C_Generating_the_Translation_File) method)
- push changes made to a `translation.json` file back to the sheet(so it will be easier to change the autogenerated tags)

- make tags for the more complex [Variable Replacement](https://wiki.roll20.net/Character_Sheet_i18n#Variable_Replacement) and [Roll Queries](https://wiki.roll20.net/Character_Sheet_i18n#Roll_Queries) methods


## 'Nice To Have'(V.1.5) 
Option things that can be done that goes beyond the primary goal if ACSI
- notice if files already have tags, and would only add missing tags
- rewriting to not rely on linux-processes(getting rid of `unistd.h` and `sys/.h` libraries)
- improve userfriendliness, like a GUI
- creating a linux install package for ACSI, and implement most standard command arguments

# Glossary
- **RCSi18n**: [Roll20 Character Sheet i18n](https://wiki.roll20.net/Character_Sheet_i18n) standard
- **sheet**: [Character Sheet](https://wiki.roll20.net/Character_Sheets), consisting of a html and css file, as well as a translation file if it have been internationalized
- **tag, i18n-tag**: the unique tagname for a given text segment shown on a character sheet. It's the left side of the `translation.json`. Represented on the html sheet with the `data-i18n` [global attribute](https://www.w3schools.com/tags/ref_standardattributes.asp)
- **translation file**: the paired `translation.json` [needed]((https://wiki.roll20.net/Character_Sheet_i18n#Step_Two.2C_Generating_the_Translation_File)) to show a character sheet in Roll20
- **sheetworkers**: JavaScript included in the html sheet inside `<script>` tags [(Roll20Wiki page)](https://wiki.roll20.net/Sheet_Worker_Scripts)
- **roll templates**: html sections meant to change how dice rolls are display when rolled from buttons on the sheet [(Roll20Wiki page)](https://wiki.roll20.net/Roll_Templates)
