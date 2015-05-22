# Introduction #

Steps for building a new release.

# Details #

To make a distribution:

1) Re-run `/Developer/Applications/Utilities/Help Indexer`

2) Update version number in `InfoPlist.strings`

3) Update Help Index.  "cd src; hiutil -C -a SwitchList.help -f SwitchList.help/SwitchList.help.helpindex; cd .." Check-in changed `Help Indexer` file and `InfoPlist.strings` file.

4) Run the script `build/switchlistTag SwitchList.version.with.dots`.  (Replace SwitchList.version.with.dots with `SwitchList.0.7.1` or some such string.)

5) Run `build/makeDistribution SwitchList.version.with.dots`

6) Test

7) Copy `/tmp/SwitchList-version.dmg` to `/Volumes/rbowdidge/Sites/railroad/SwitchListDmgs/SwitchList-version.dmg`

8) Copy` /tmp/SwitchList-version.dmg` to `/Volumes/rbowdidge/Sites/railroad/SwitchList.dmg`

9) Update web page to show new version.

10) Announce on SwitchList-discuss
~