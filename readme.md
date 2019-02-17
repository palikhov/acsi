# ACSI V0.2

Automated Character Sheet Internationalizer (ACSI) is a simple program to place ['i18n'-language tags](https://wiki.roll20.net/Character_Sheet_i18n) into existing [HTML character sheets](https://github.com/Roll20/roll20-character-sheets) made for the [Roll20 platform](https://en.wikipedia.org/wiki/Roll20). The aim is to automate large parts of the currently manual process of introducing `i18n`-tags to existing character sheets, and lower the bar for translating character sheets. 

V0.2 At it's current state, it can only properly create i18n-tags for the most uniform text sections that doesn't contain many linebreaks or special characters. Further it does not play well with neither roll templates or if a sheet contains a sheetworker section.

ACSI written in C, and have some unix commands sprinkled in, so it needs to be run in a linux enviroment to work. 

## Initiate ACSI

Just run `make` command in your command line while in the same  directory as the associated ACSI-files, and ACSI will be compiled, removing all *.o files while doing so.

### Make options

There exist a few make commands to do other things than just a clean build:

build,b:	builds without removing the .o files
clean,c:	removes all .o files
t:		removes all files with the .clean suffix, thus removing previously cleaned files

## Use
After running the `make` command, you need to make a copy of the file you want to change and place it in the same folder as ASCI is located.

You can now call ASCI from the command line(when your command line have the ASCI as active directory): `./acsi <filename>.html` 

You give the ACSI a html filename from the same directory, and ACSI will insert the i18n-tags as it sees fit, then saving the output as a new file with "i18n-"  as prefix.
If everything worked out as it should, it should display a short `ASCI finished succesfully!` on the command line.

## Changelog

### V0.2 (2019-02-18)
- code cleanup
- fixed credits typo
- rewrote readme

### V0.1
- make data tags for text displayed in html elements, `<p>like this text</p>`
- treats all text; comments, roll templates & sheetworkers alike, best to avoid if your sheet contains much of it
- autogenerated tags are at times clumsy, and duplicates may arise as it's currently taken in consideration

## Development roadmap

### V0.5 (Alpha)
- makes less mistakes with creating tags
- ignore `cs>` , `cs<` , `cf>`, `cf<` (credits to David for noticing)
- Ignores html comments, sheet workers and roll templates
- doesn't make duplicate tags

### V.1.0 and beyond
- makes tags for `titles`
- create own `translation.json` files
- push changes make to `translation.json` back to the sheet(like adjusting tag-names)
- translate roll templates
- notice if files already are translated, and would only improve on missing things
- translate sheetworkers
- improve ACSI's user friendliness



