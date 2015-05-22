# Introduction #

SwitchList 1.1.1 includes a new "Suggested Cargos" feature where SwitchList can guess at your likely industry from its name and suggest several cargos that might be appropriate for the industry.   All SwitchList's advice comes from knowledge of industries stored in the typicalIndustry.plist file.

You can modify this file to add new potential industry classes and cargos.

[Here](http://code.google.com/p/switchlist/source/browse/trunk/src/resources/typicalIndustry.plist) is the current version of the file.

# Details #

typicalIndustry.plist is a text file in Apple's ["plist"](http://en.wikipedia.org/wiki/Property_list) format - a file using XML tags (values in angle brackets) to mark what each item is.)  The file is an array of potential industry classes (generalized descriptions of an industry.)  Each industry is stored as a dictionary of key/value pairs.  The values in the plist are:

`IndustryClass:` string name describing the general class of industry.  Used in the pull-down menu for changing the type of industry.

`Synonyms:` a list of sample industry names that match this industry class.

`Cargo:` array of potential cargos.  Each cargo is a dict containing a `Name` (kind of cargo) `Incoming` (boolean value, true if cargo is coming to the industry, false otherwise), and `Rate` (percentage of cargos of this type.)  The rates of all cargos should sum to 100.

Eventually, Cargo will have two other values for helping pick cargos appropriate for a particular layout: `Era` (decade where cargo makes sense) and `Country` (for distinguishing between Canadian, US, British, or German railroad cargos).  My plan is to add controls to the Layout tab to set the era and location of the layout to help automatic cargo suggestions.

# Example #

Here is the portion of the typicalIndustry.plist file for a fresh fruit packer:

```
 <dict>
    <key>IndustryClass</key><string>fresh fruit packer</string>
    <key>Synonyms</key>
    <array>
      <string>United Fruit Co.</string>
      <string>Produce</string>
      <string>Lo Blue Packing Co.</string>
      <string>Sunkist Packing</string>
      <string>Sunkist</string>
      <string>Citrus</string>
    </array>
    <key>Cargo</key>
    <array>
      <dict>
        <key>Name</key><string>fruit</string>
        <key>Incoming</key><false/>
        <key>Rate</key><integer>100</integer>
      </dict>
    </array>
  </dict>
  <dict>
```