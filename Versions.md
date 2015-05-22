# Introduction #

Master list of public releases of SwitchList.


# Details #
Version 1.2.9.2 (8/6/2014).  New Style tab, and switchlist templates can now include custom strings that can be changed in the SwitchList UI.  Railroad Letterhead style added.  Minor bug fixes.  Download from http://www.vasonabranch.com/railroad/SwitchList.1.2.9.2.dmg

Version 1.2.9 (2/23/2014).  Allow variable days to unload cars, hints about how busy each siding is, annul button works.  File format is incompatible with past versions.

Version 1.2.0 (9/30/2012). "Suggest Cargos..." now shows existing cargos for each industry. New Chicago example. Minor bug fixes.

Version 1.1.4 (9/26/2012). Return missing files for converting older layout file to new version, fix problems launching on Mac OS X 10.5

Version 1.1.3 (9/22/2012). Add "Import Template..." menu item to simplify adding new custom switchlist styles.

Version 1.1.2 (9/5/2012). Add "Suggest Cargo" feature that gives suggestions about new cargos to create for your industries.

Version 1.1.1 (9/3/2012). SwitchList now suggests cargos, cargo rates can be specified in days or months, support multiple trains in same path.

Version 1.0.0 (8/11/2012). Better random choice of cars to accept new cargo.

Version 0.9.7.3 (8/1/2012). Fixes crash on PowerPC machines during startup.

Version 0.9.7 (7/31/2012) Improvements in HTML switchlist templates, better handwritten font by default, UI switch for towns, new logo.

Version 0.9.6 (3/24/2012). Bug fixes, documentation improvements.

Version 0.9.5 (3/20/2012). Respects maximum train length, allows station names with commas.

Version 0.9.4 (3/5/2012). Fixes crash in layout with many yards, removes problem with switchlists getting clipped during printing, improved documentation, web interface home page now customizable.

Version 0.9.3 (12/20/2011). Documentation fixes, and allow hovering over cargo name in freight car table to show full description.

Version 0.9.2 (12/4/2011). Fixes crash in some switchlist styles if a car’s reporting marks contain no space between initials and number.

Version 0.9.1 (11/5/2011). Fix badly formatted reports for some alternate switchlist templates. Improvements to PICL reports.

Version 0.9.0 (11/4/2011). Limits on siding capacity. Custom reports in HTML. Improvements to Web Server.

Version 0.8.1 (9/11/2011).  Fix problems with iPhone display and Line Printer switchlist style.

Version 0.8.0 (9/10/2011).  User-defined switchlist templates now supported, with documentation on how to create them.

Version 0.7.4 (8/23/2011).  Simplifies messing with the web server display by using HTML templates and a templating engine.

Version 0.7.3 (8/21/2011).  This release is all about improving the web server feature.  The web server now shows a window when it's active showing the URL for reaching the server, and uses IP addresses ("10.0.0.7") instead of long names to make typing in the server's address easier.  The web interface also now has a list of cars grouped by industry for easier setup.

Version 0.7.2 (6/21/2011). This release provides a much-improved PICL report switchlist (thanks to James McNab!).  It also includes many cleanup changes to the switchlist display and printing code.  These changes make it easy to write your own switch list format, and may make the code work better on non-US sized paper.

5/9/2011: Announced that SwitchList is now an open source project.

Version 0.7.0 (4/23/2011).  New switchlists, fixed rate cargos, better documentation.

Changes include:
  * New switchlists:
    * San Francisco Belt Railroad Form B-7
    * modern PICL report contributed by James McNab
  * Fixed "Print all" that prints all switchlists for all trains at once.
  * New "fixed rate" cargos that ship a fixed number of cars each day.
  * Fix problem with Web mode where some operations couldn't be done unless the layout was called "Vasona Branch".
  * Allow problem window messages to be copied.
  * Search fields in each window now search items with drop-down boxes.
  * Better error messages when industries and towns are unreachable.
  * Documentation on how to add door spotting instructions to your switchless.

Version 0.6.6 (3/6/2011). Fix incorrect behavior for industry names with apostrophes, problems opening files when an industry is associated with no towns. Change “Add more loads” button to add at least two new loads.

Version 0.6.5 (2/26/2011). Generate correct number of cars per session based on cars/week. Improve typewritten switchlists with better font and removal of car type key at bottom. Allow overriding font choices. Fix bug where handwritten dates didn’t match spaces on form. New documentation in Apple Help format.

Version 0.6.4 (2/20/2011). New Southern Pacific-style switchlist, realistic squiggles on duplicate entries, print “empty” instead of “unassigned” when a car is not reserved for a particular cargo.

Version 0.6.3 (2/5/2011). Add support for spotting cars at specific industry doors, new documentation, tool tips to show exact cargo in freight car. Import list of freight cars from text file.

Version 0.6.2 (11/17/2010). Clean up user interface, get rid of 1 yard/town limit, new icon. Printing document now prints switchlists for all trains.

Version 0.6.1 (11/9/2010). Added car types tab and UI for car types. File format changed to v3 to have CarType objects rather than just strings.

Version 0.5.3 (10/31/2010). Crash on clearing loads. Add layout info tab. Let “layout date” appear on printed switchlists.

Version 0.5.2 (10/31/2010). Fix crashes when reopening preference or help windows.

Version 0.5.1, (10/31/2010). First official release. PowerPC/Intel binary.

# File Formats #

SwitchList-0.6.6 ([r19](https://code.google.com/p/switchlist/source/detail?r=19)): File format 2.

SwitchList-??? ([r174](https://code.google.com/p/switchlist/source/detail?r=174)): File format 3.  Add car type.

SwitchList-0.8.9 ([r170](https://code.google.com/p/switchlist/source/detail?r=170): File format 4.  Add siding length.

SwitchList-1.1.0 ([r424](https://code.google.com/p/switchlist/source/detail?r=424)): File format 5.  Adds cargo rates.

SwitchList-1.2.9 ([r609](https://code.google.com/p/switchlist/source/detail?r=609)): File Format 7.  Days to unload.

# Identifying A File Version #

If you get an unknown layout file, you can guess at the version by checking some magic numbers in the file.  Open it in a text editor; the top section has some XML with the title "NSStoreModelVersionHashes" which lists the magic hashes expected for each kind of object in the program (FreightCar, Industry, etc).  Here's a few of the hashes for three classes:

```
Freight Car:
Version 7: r609: vc47I3prTXdN+JishWjEF/xHAXyScG3vhiwjlnCH1iE=
Version 5: r424: +qbD31B2psgfXVeVrl9nsCioBM1BN+7sQ+frmm8YJnU=
Version 4: r170: MUhOO9br8bKwJQlhCHa55b7PqTk0lkESEXxY47q44nA=
Version 3: r174: MUhOO9br8bKwJQlhCHa55b7PqTk0lkESEXxY47q44nA=
Version 2: r9:   MUhOO9br8bKwJQlhCHa55b7PqTk0lkESEXxY47q44nA=

Industry:
Version 7: r609: mz5wbMeLvFUtzQFIMaUMKGDXPZ8WRH00d97GBGepFiU=
Version 5: r424: mz5wbMeLvFUtzQFIMaUMKGDXPZ8WRH00d97GBGepFiU=
Version 4: r170: mz5wbMeLvFUtzQFIMaUMKGDXPZ8WRH00d97GBGepFiU=
Version 3: r174:
Version 2: r9:   pPJRlCkz3ktRCi2LozJuj13iWJ4duNyZcKaZS55HukM=

CarType:
Version 7: r609: dEmE//DowqbI9VHlNrNhfTjLYtCFzUdxU47V9zJ1OEc=
Version 5: r424: dEmE//DowqbI9VHlNrNhfTjLYtCFzUdxU47V9zJ1OEc=
Version 4: r170: dEmE//DowqbI9VHlNrNhfTjLYtCFzUdxU47V9zJ1OEc=
Version 3: r174:
Version 2: r9:   dEmE//DowqbI9VHlNrNhfTjLYtCFzUdxU47V9zJ1OEc=
```

So, if you have an unknown file, and the FreightCar hash starts with "vc47", then it's probably a version 7 file from version 1.2.9.