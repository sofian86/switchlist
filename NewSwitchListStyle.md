# Creating New SwitchList Styles #

(This documents how to add new switchlist styles by writing code in Objective-C.  See http://code.google.com/p/switchlist/wiki/SwitchListTemplatesInHTML for an easier way to create your own switch lists using HTML only.)

One of SwitchList's neat features is the ability to print the crew's work orders in a form that matches what the real crews would use on your railroad.  Although SwitchList supports a range of switchlist templates (including a Southern Pacific-style narrow switchless, the Large Type version better suited for garage layouts, or the San Francisco Belt Railroad's B-7 form), you may want to duplicate a particular railroad's format.  Luckily, it's possible with a bit of programming to contribute your favorite switchlist format to SwitchList.

SwitchList has two styles of switchlists it prefers.

## Pretty Switchlists ##
The first, "pretty", switchlists, imitate printed forms with handwritten entries.  Creating these switchlists is a bit complicated; look at the SwitchListView class for hints on writing your own.

## Computer-generated / Typed Switchlists ##

The second style of switchlist imitates computer-generated paperwork, and are easy to create even if you have't had a lot of programming experience.  The computer-generated switchlists are subclasses of the TextSwitchListView.  Check out the PICLReport or SwitchListReport classes for ideas about how to write your own, or follow the steps below.

# Creating Your Own Computer-Generated Switchlist Style #

There are five steps you need to do to create your own switchlist:

1) Create a new class that is a subclass of the TextSwitchListView.  Create the .h and .m file for the class.

2) Define a headerString method that names the message that should appear at the top of each switchlist:

```
// what kind of report?
- (NSString*) typeString {
	return [NSString stringWithFormat: @"My favorite switchlist for train %@",[train_ name]];
}
```


3) Define a contents method that provides the string content - all the lines - of the switchlist to be generated.  You'll use the [train](self.md) switchlist to get information on the train and its cars to display.

```
 - (NSString*) contents {
	return [NSString stringWithFormat: @"I'd put the text of my switchlist here!\n"];
}
```

4) Define the expectedColumns field to name the maximum line length in your report.  SwitchList will automatically choose a font that will fit the lines on the current printed page.

```
- (int) expectedColumns {
	return 80;
}
```

5) Add a new SwitchListStyle enum in SwitchListAppDelegate.h to name the number that should be stored in the preference file when your switchlist is the preferred one for a user.

```
enum SwitchListStyle {
	Undefined=0,
	SanFranciscoBeltLineB7Style=6,
        MyNewSwitchListStyle=7  // I changed this!
};
```

5) In -[init](SwitchListAppDelegate.md) method, add new lines to the init method to specify a human-readable name for your new switchlist:

```
	[indexToSwitchListNameMap_ setObject: @"My SwitchList" forKey: [NSNumber numberWithInt: MyNewSwitchListStyle]];
```

and associate the enum value with your class:

```
	[indexToSwitchListClassMap_ setObject: [MySwitchList class] forKey: [NSNumber numberWithInt: MyNewSwitchListStyle]];
```

With these two changes, the SwitchList user interface will automatically add your new switch list to the preferences dialog box, and will create new switchlists formatted in your style when requested.

Once you've created your new switchlist format, tell people about it on the switchlist e-mail list, and get it added as an official part of SwitchList!
