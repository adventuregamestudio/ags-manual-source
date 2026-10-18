## `File` functions and properties

### `File.Copy`

```ags
static bool File.Copy(const string old_filename, const string new_filename);
```

Creates a copy of an existing file.
If there's already a file with the new name then it will be overwritten.
Any directory necessary is also created.
Returns false in any failure and true if it succeeds.

**NOTE:** You can only write to `$INSTALLDIR$`, `$SAVEGAMEDIR$` and `$APPDATADIR$`, 
so `new_filename` parameter must start with one of those.
See `File.Open` documentation for more information.

Example:

```ags
if (File.Copy("$SAVEGAMEDIR$/profile.dat", "$SAVEGAMEDIR$/profile.bak"))
{
  Display("A backup copy was made.");
}
```

will make a backup copy of the file 'profile.dat' in the save game directory, and tell if it succeeded.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`File.Open`](File#fileopen), 
[`File.Path`](File#filepath),
[`File.Rename`](File#filerename)

---

### `File.GetFileTime`

```ags
static DateTime* File.GetFileTime(const string filename);
```

Retrieves specified file's last write time.
Returns null if file does not exist.

Example:

```ags
void room_AfterFadeIn()
{
  String filename = "$SAVEGAMEDIR$/game_config.ini";
  DateTime *dt = File.GetFileTime(filename);
  if (dt == null) {
    Display("File '%s' not found.", filename);
  } else {
    Display("File '%s' was last modified on: %02d-%02d-%04d %02d:%02d:%02d", 
      filename, dt.DayOfMonth, dt.Month, dt.Year, dt.Hour, dt.Minute, dt.Second);
  }
}
```

This example looks for a custom config file placed in the save game directory
and tells its modified time if the file exists.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`File.Open`](File#fileopen),
[`File.Path`](File#filepath)

---

### `File.Open`

*(Formerly known as `FileOpen`, which is now obsolete)*

```ags
static File* File.Open(string filename, FileMode)
```

Opens a disk file for reading or writing, and returns a `File` object pointer which you may use to perform operations on this file, or *null* if engine failed to open a file for whatever reasons.

MODE is either `eFileRead`, `eFileWrite` or `FileAppend`, depending on
whether you want to write to or read from the file. If you pass
`eFileWrite` and a file called FILENAME already exists, it will be
overwritten.

`eFileAppend` opens an existing file for writing and starts adding
information at the end (i.e. the existing contents are not deleted).

When specifying a file path you may use tags that define either a special file or location.

Following *file tags* are supported:<br>
`$CONFIGFILE$`, which allows you to open player's configuration file for reading or writing. This is recommended to be used instead of telling exact path, because config file may be found in different places depending on game settings or personal player's setup.

Example:

```ags
File.Open("$CONFIGFILE$", eFileRead);
```

Following *location tags* are supported:

- `$DATA$`, which allows you to read files that are packaged inside the
game package, originally from a directory specified in the General Settings,
in the **Package custom data folder(s)** option, on the Compiler category.
  Files stored in this way can only be opened in read mode.  
- `$INSTALLDIR$`, which allows you to explicitly read files in the game
installation directory.  
- `$SAVEGAMEDIR$`, which allows you to write/read files in the save game
directory.  
- `$APPDATADIR$`, which allows you to write/read files to a folder on the
system which is accessible by and shared by all users. The example of
their use is below.  

Examples:

```ags
File.Open("$DATA$/datadir/book.txt", eFileRead);
File.Open("$INSTALLDIR$/game.dat", eFileRead);
File.Open("$SAVEGAMEDIR$/userinfo.txt", eFileWrite);
File.Open("$APPDATADIR$/scoretable.dat", eFileAppend);
```

**IMPORTANT:** For security reasons, if you open the file for writing,
then you can ONLY work with files in either `$SAVEGAMEDIR$` or
`$APPDATADIR$` locations. An attempt to write file in `$INSTALLDIR$`
will result in failure, and *null* is returned. Similarily, if you try to open a file for writing using an absolute path, or relative path that points to location outside of the game directory, it will be automatically rejected, and *null* is returned.

**IMPORTANT:** An attempt to write file into relative path *without specifying any location tag* will make AGS
automatically remap such path into `$APPDATADIR$`. This is done for backward compatibility. To ensure that your script does exactly what you want, please use location tags.

**NOTE:** You are allowed to have subdirectories in the path. If the file is opened for writing, AGS will be responsible for creating these subdirectories. If it fails to do so for any reason, the the file won't be opened.

`File.Open` returns a File* pointer, which you should store in a variable of that type. You may then use that pointer to perform reading or writing operations. After finishing work you should close the file with the `Close` function. There are only a limited number of file handles,
and forgetting to close the file can lead to problems later on.

Example:

```ags
File *output = File.Open("$SAVEGAMEDIR$/temp.tmp", eFileWrite);
if (output == null)
    Display("Error opening file.");
else {
    output.WriteString("test string");
    output.Close();
}
```

will open the file temp.tmp in the save game folder for writing. An
error message is displayed if the file could not be created. Otherwise,
it will write the string "test string" to the file and close it.

**NOTE:** Open file pointers are not persisted across save games. That
is, if you open a file, then save the game; then when you restore the
game, the File will not be usable and you'll have to open it again to
continue any I/O. The safest practice is not to declare any global File
variables.

*Compatibility:*

Subdirectories are created when opening a file for writing by AGS 3.5.0 and later versions.
$CONFIGFILE$ tag supported by AGS 3.5.1 and later versions.
$DATA$ tag supported by AGS 3.6.0 and later versions.

*See also:* [`File.Close`](File#fileclose),
[`File.Exists`](File#fileexists),
[`File.ReadStringBack`](File#filereadstringback),
[`File.WriteString`](File#filewritestring),
[General Settings](GeneralSettings#compiler)

---

### `File.Rename`

```ags
static bool File.Rename(const string old_filename, const string new_filename);
```

Renames an existing file.
If there's already a file with the new name then it will be overwritten.
Any directory necessary is also created.
Returns false in any failure and true if it succeeds.

**NOTE:** You can only write to `$INSTALLDIR$`, `$SAVEGAMEDIR$` and `$APPDATADIR$`,
so both parameters must start with one of those.
See `File.Open` documentation for more information.

Example:

```ags
void room_AfterFadeIn()
{
  String filename = "$SAVEGAMEDIR$/game_config.ini";
  String bkp = filename.Append(".bkp");
  bool success = File.Rename(filename, bkp);
  if (success) {
    Display("File renamed successfully.");
  } else {
    Display("Failed to rename the file.");
  }
}
```

In this example, a file is renamed as a backup, possibly in antecipation to a new write to disk.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`File.Open`](File#fileopen),
[`File.Path`](File#filepath),
[`File.Copy`](File#filecopy)

---

### `File.ResolvePath`

```ags
static String File.ResolvePath(string filename)
```

Resolves the script path into the system filepath, this should be used only for diagnostic purposes.

This will process all tokens and all rules described in `File.Open` and return the resulting path,
which can be helpful when debugging or when you want to present this information somewhere to help
debugging some problem in the game. This resolved path should not be re-used in subsequent `File.Open`
calls.

Example:

```ags
System.Log(eLogInfo, "The profile is stored at: %s", File.ResolvePath("$SAVEGAMEDIR$/profile.dat"));
```

will write the actual system path of the file "$SAVEGAMEDIR$/profile.dat" to the game log, which may help when a player reports a problem with their files.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`File.Open`](File#fileopen), [`File.Path`](File#filepath)

---

### `File.Close`

*(Formerly known as `FileClose`, which is now obsolete)*

```ags
File.Close()
```

Closes the file, and commits all changes to disk. You **must** call this
function when you have finished reading/writing the file.

Example:

```ags
File *output = File.Open("$SAVEGAMEDIR$/test.dat", eFileWrite);
output.WriteString("test string");
output.Close();
```

will open the file test.dat, write the string "test string", and close
it.

*See also:* [`File.Open`](File#fileopen)

---

### `File.Delete`

```ags
static File.Delete(string filename)
```

Deletes the specified file from the disk.

For security reasons this command only works with files in the
`$SAVEGAMEDIR$` and `$APPDATADIR$` directories.

**NOTE:** This is a static function, therefore you don't need an open
File pointer to use it. See the example below.

Example:

```ags
File.Delete("$SAVEGAMEDIR$/temp.tmp");
```

will delete the file "temp.tmp" from the app data directory, if it
exists.

*Compatibility:* Supported by **AGS 3.0.1** and later versions.

*See also:* [`File.Exists`](File#fileexists),
[`File.Open`](File#fileopen)

---

### `File.Exists`

```ags
static bool File.Exists(string filename)
```

Checks if the specified file exists on the file system.

*filename* may include any file tags or location tags, which are described for [`File.Open`](File#fileopen).

**NOTE:** This is a static function, therefore you don't need an open
File pointer to use it. See the example below.

Example:

```ags
if (!File.Exists("$SAVEGAMEDIR$/temp.tmp"))
{
    File *output = File.Open("$SAVEGAMEDIR$/temp.tmp", eFileWrite);
    output.WriteString("some text");
    output.Close();
}
```

will create the file "temp.tmp" if it doesn't exist

*Compatibility:* Supported by **AGS 3.0.1** and later versions.

*See also:* [`File.Delete`](File#filedelete),
[`File.Open`](File#fileopen)

---

### `File.GetFiles`

```ags
static String[] File.GetFiles(string fileMask, optional FileSortStyle fileSortStyle, optional SortDirection sortDirection)
```

Returns a dynamic array of filenames that match the specified file mask, optionally sorted using certain style and direction.
By default it sorts files by their names in ascending order.
If no matching files were found, returns an empty array (which Length is 0).

*fileMask* is a standard wildcard search expression such as `"*.dat"` or `"data*.*"`

*fileMask* may include any file tags or location tags, which are described for [`File.Open`](File#fileopen).

*FileSortStyle* determines which file property will be used when sorting the list of filenames. It can be:
* eFileSort_None - don't sort, the order will be unspecified;
* eFileSort_Name - sort by name (this sort is case insensitive for cross-platform compatibility);
* eFileSort_Time - sort by write time (time when this file was created or last written to).

*SortDirection* determines the order of sorting:
* eSortNoDirection - unspecified;
* eSortAscending;
* eSortDescending.

Example:

```ags
String files[] = File.GetFiles("$SAVEGAMEDIR$/*.dat", eFileSort_Time, eSortDescending);
if (files.Length > 0)
{
    Display("Last saved custom dat file is: %s", files[0]);
}
```

will search the save game directory for all files matching "*.dat" pattern, ordered by a time of writing in reverse order, and display the name of the most recently written one.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`ListBox.FillDirList`](ListBox#listboxfilldirlist)

---

### `File.ReadRawBytes`

```ags
int File.ReadRawBytes(char bytes[], int index, int count);
```

Reads up to "count" number of bytes and stores them in a provided dynamic array, starting with certain index.
Returns actual number of read bytes.

Example:

```ags
void room_AfterFadeIn()
{
  String filename = "$SAVEGAMEDIR$/example.dat";
  File* f = File.Open(filename, eFileRead);
  if(f == null) {
    Display("Error opening file '%s' for reading", filename);
    return;
  }
  
  int buf_len = 7;
  char buffer[] = new char[buf_len];
  
  int read = f.ReadRawBytes(buffer, 0, buf_len);
  f.Close();
  
  buffer[buf_len-1] = 0; // for safety
  
  Display("Read '%d' bytes\nbuffer got '%s'.", read, buffer);
}
```

In this example, it reads a file `"example.dat"`,
that has data that can be interpreted as text, from `File.WriteRawBytes` example.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`File.WriteRawBytes`](File#filewriterawbytes)

---

### `File.ReadInt`

*(Formerly known as `FileReadInt`, which is now obsolete)*

```ags
File.ReadInt()
```

Reads an integer from the file, and returns it to the script. Only
integers written with File.WriteInt can be read back.

Example:

```ags
int number;
File *input = File.Open("$SAVEGAMEDIR$/stats.dat", eFileRead);
number = input.ReadInt();
input.Close();
```

will open the file stats.dat, read an integer into number and then close
the file.

*See also:* [`File.ReadStringBack`](File#filereadstringback),
[`File.WriteInt`](File#filewriteint)

---

### `File.ReadRawChar`

*(Formerly known as `FileReadRawChar`, which is now obsolete)*

```ags
int File.ReadRawChar()
```

Reads a raw character from the input file and returns it. This function
allows you to read from files that weren't created by your game, however
it is recommended for expert users only.

Example:

```ags
File *input = File.Open("$SAVEGAMEDIR$/stats.txt", eFileRead);
String buffer = String.Format("%c", input.ReadRawChar());
input.Close();
```

will read a raw character from file stats.txt and writes it to the
string 'buffer'.

*See also:* [`File.ReadStringBack`](File#filereadstringback),
[`File.ReadRawInt`](File#filereadrawint),
[`File.WriteRawChar`](File#filewriterawchar)

---

### `File.ReadRawFloat`

```ags
float File.ReadRawFloat()
```

Reads the next raw 32-bit float from the file.

Example:

```ags
File *file = File.Open("$SAVEGAMEDIR$/settings.dat", eFileRead);
if (file != null)
{
  mouseSensitivity = file.ReadRawFloat();
  file.Close();
}
```

will load the mouse sensitivity setting from the file 'settings.dat' in the save game directory. If the file does not exist yet, File.Open returns null and the variable keeps its default value. Here `mouseSensitivity` is a global float variable, written to the file with WriteRawFloat.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`File.ReadStringBack`](File#filereadstringback),
[`File.WriteRawFloat`](File#filewriterawfloat),
[`File.ReadRawInt`](File#filereadrawint),
[`File.ReadRawChar`](File#filereadrawchar)

---

### `File.ReadRawInt`

*(Formerly known as `FileReadRawInt`, which is now obsolete)*

```ags
int File.ReadRawInt()
```

Reads a raw 32-bit integer from the input file and returns it to the
script. This allows you to read from files created by other programs -
however, it should only be used by experts as no error-checking is
performed.

Example:

```ags
int number;
File *input = File.Open("$SAVEGAMEDIR$/stats.txt", eFileRead);
number = input.ReadRawInt();
input.Close();
```

will read a raw integer from file stats.txt and put it into the integer
number.

*See also:* [`File.ReadStringBack`](File#filereadstringback),
[`File.WriteRawInt`](File#filewriterawint)
[`File.ReadRawChar`](File#filereadrawchar)

---

### `File.ReadRawLineBack`

*(Formerly known as `File.ReadRawLine`, which is now obsolete)*

```ags
String File.ReadRawLineBack()
```

Reads a line of text back in from the file and returns it. This enables
you to read in lines from text files and use them in your game.

**NOTE:** this command can only read back plain text lines from text
files. If you attempt to use it with binary files or files written with
commands like WriteString, WriteInt, etc then the results are
unpredictable.

Example:

```ags
File *input = File.Open("$SAVEGAMEDIR$/error.log", eFileRead);
if (input != null) {
    while (!input.EOF) {
        String line = input.ReadRawLineBack();
        Display("%s", line);
    }
    input.Close();
}
```

will display the contents of the 'error.log' file, if it exists

*See also:* [`File.WriteRawLine`](File#filewriterawline)

---

### `File.ReadStringBack`

*(Formerly known as `FileRead`, which is now obsolete)*<br>
*(Formerly known as `File.ReadString`, which is now obsolete)*

```ags
String File.ReadStringBack()
```

Reads a string back in from a file previously opened with File.Open, and
returns it. You should only use this with files which you previously
wrote out with File.WriteString. Do NOT use this function with any other
files, even text files.

Example:

```ags
File *input = File.Open("$SAVEGAMEDIR$/test.dat", eFileRead);
String buffer = input.ReadStringBack();
input.Close();
```

will open the file test.dat (which you have previously written with
File.WriteString) and read a string into the buffer. Then close the
file.

*See also:* [`File.Open`](File#fileopen),
[`File.WriteString`](File#filewritestring)

---

### `File.Seek`

```ags
int Seek(int offset, optional FileSeek origin);
```

Moves read/write position by *offset* bytes related to *origin*. Returns
new read/write position. This is usually used when you are reading file
and want to skip over some data, or writing a file and want to move back
and overwrite a piece of data in the previous section for some reason.

The *origin* is determined by one of the FileSeek types: eSeekBegin -
counts *offset* bytes starting from the file's beginning; *offset* must
be positive.<br>
eSeekCurrent - counts *offset* bytes starting from the current position;
*offset* may be either positive or negative.<br>
eSeekEnd - counts *offset* bytes starting from the file's end, going
backwards; *offset* must be positive.

If optional *origin* parameter is not specified, eSeekCurrent type is
used by default.

**IMPORTANT:** Do not use Seek on files which you have written with safe
data writing functions, such as WriteInt and WriteString. These
functions add additional data for the purpose of protection against
incorrect reading, and Seek ignores that protection. If you use Seek and
then try to ReadIntBack, for example, most probably you will receive a
reading error. Only use it on files that contain "raw" data, or when you
know written format precisely.

Example:

```ags
File *input = File.Open("$SAVEGAMEDIR$/test.dat", eFileRead);
int first_value = input.ReadRawInt();
input.Seek(256);
int second_value = input.ReadRawInt();
input.Close();
```

will open the file test.dat, read `first_value`, skip 256 bytes, read
`second_value`, and close the file.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`File.Position`](File#fileposition)

---

### `File.WriteRawBytes`

```ags
int File.WriteRawBytes(char bytes[], int index, int count);
```

Writes up to "count" number of bytes from the provided dynamic array, starting with certain index.
Returns actual number of written bytes.

Example:

```ags
void room_AfterFadeIn()
{
  String filename = "$SAVEGAMEDIR$/example.dat";
  File* f = File.Open(filename, eFileWrite);
  if(f == null) {
    Display("Error opening file '%s' for writing", filename);
    return;
  }
  
  int buf_len = 6;
  char buffer[] = new char[buf_len];
  buffer[0] = 'H';
  buffer[1] = 'e';
  buffer[2] = 'l';
  buffer[3] = 'l';
  buffer[4] = 'o';
  buffer[5] = '!';
  
  int written = f.WriteRawBytes(buffer, 0, buf_len);
  f.Close();
  
  Display("Wrote '%d' bytes", written);
}
```

Writes the string `"Hello!"` using write bytes to a file named `"example.dat"`.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`File.ReadRawBytes`](File#filereadrawbytes)

---

### `File.WriteInt`

*(Formerly known as `FileWriteInt`, which is now obsolete)*

```ags
File.WriteInt(int value)
```

Writes VALUE to the file. This allows you to save the contents of
variables to disk. The file must have been previously opened with
File.Open, and you can read the value back later with File.ReadInt.

Example:

```ags
int number = 6;
File *output = File.Open("$SAVEGAMEDIR$/stats.dat", eFileWrite);
output.WriteInt(number);
output.Close();
```

will open the file stats.dat and write the integer number in it.

*See also:* [`File.ReadInt`](File#filereadint),
[`File.WriteString`](File#filewritestring)

---

### `File.WriteRawChar`

*(Formerly known as `FileWriteRawChar`, which is now obsolete)*

```ags
void File.WriteRawChar(int value)
```

Writes a single character to the specified file, in raw mode so that
other applications can read it back. If you are just creating a file for
your game to read back in, use File.WriteInt instead because it offers
additional protection. Only use this function if you need other
applications to be able to read the file in.

This command writes a single byte to the output file - therefore, VALUE
can contain any value from 0 to 255.

Example:

```ags
File *output = File.Open("$SAVEGAMEDIR$/output.txt", eFileWrite);
output.WriteRawChar('A');
output.WriteRawChar('B');
output.WriteRawChar(13);
output.Close();
```

will write the text "AB", followed by a carriage return character, to
the file.

*See also:* [`File.ReadRawChar`](File#filereadrawchar),
[`File.WriteInt`](File#filewriteint)

---

### `File.WriteRawFloat`

```ags
void File.WriteRawFloat(float value)
```

Writes a raw 32-bit float to the file.

Example:

```ags
File *file = File.Open("$SAVEGAMEDIR$/settings.dat", eFileWrite);
if (file != null)
{
  file.WriteRawFloat(mouseSensitivity);
  file.Close();
}
```

will save the mouse sensitivity setting to the file 'settings.dat' in the save game directory, so that it can be loaded back with ReadRawFloat the next time the game runs. Here `mouseSensitivity` is a global float variable.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`File.ReadStringBack`](File#filereadstringback),
[`File.ReadRawFloat`](File#filereadrawfloat),
[`File.WriteRawInt`](File#filewriterawint),
[`File.WriteRawChar`](File#filewriterawchar)

---

### `File.WriteRawInt`


```ags
void File.WriteRawInt(int value)
```

Writes VALUE to the specified file, in raw mode so that
other applications can read it back. If you are just creating a file for
your game to read back in, use File.WriteInt instead because it offers
additional protection. Only use this function if you need other
applications to be able to read the file in.

This command writes a single integer to the output file - therefore, VALUE
can contain any value from -2147483648 to 2147483647.

Example:

```ags
File *output = File.Open("$SAVEGAMEDIR$/stats.dat", eFileWrite);
output.WriteRawInt(51);
output.Close();
```

will open the file stats.dat and write the integer number in it.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`File.ReadRawInt`](File#filereadrawint),
[`File.WriteInt`](File#filewriteint)

---

### `File.WriteRawLine`

*(Formerly known as `FileWriteRawLine`, which is now obsolete)*

```ags
File.WriteRawLine(string text)
```

Writes a string of text to the file in plain text format. This enables
you to read it back in Notepad or any text editor. This is useful for
generating logs and such like.

The TEXT will be printed to the file, followed by the standard newline
characters.

Example:

```ags
File *output = File.Open("$SAVEGAMEDIR$/error.log", eFileAppend);
output.WriteRawLine("There was an error playing sound1.wav");
output.Close();
```

will write an error line in the file error.log.

*See also:* [`File.ReadRawLineBack`](File#filereadrawlineback),
[`File.WriteString`](File#filewritestring)

---

### `File.WriteString`

*(Formerly known as `FileWrite`, which is now obsolete)*

```ags
File.WriteString(string text)
```

Writes TEXT to the file, which must have been previously opened with
File.Open for writing. The string is written using a custom format to
the file, which can only be read back by using File.ReadStringBack.

Example:

```ags
File *output = File.Open("$SAVEGAMEDIR$/temp.tmp", eFileWrite);
if (output == null) Display("Error opening file.");
else {
    output.WriteString("test string");
    output.Close();
}
```

will open the file temp.tmp for writing. If it cannot create the file,
it will display an error message. Otherwise, it will write the string
"test string" and close it.

*See also:* [`File.ReadStringBack`](File#filereadstringback),
[`File.Open`](File#fileopen),
[`File.WriteRawLine`](File#filewriterawline)

---

### `File.EOF`

*(Formerly known as `FileIsEOF`, which is now obsolete)*

```ags
readonly bool File.EOF
```

Checks whether the specified file has had all its data read. This is
only useful with files opened for **reading**. It returns 1 if the
entire contents of the file has now been read, or 0 if not.

Example:

```ags
File *output = File.Open("$SAVEGAMEDIR$/test.dat", eFileRead);
while (!output.EOF) {
    int temp = output.ReadRawChar();
    Display("%c", temp);
}
output.Close();
```

will display every character in the file test.dat, one by one, to the
screen.

*See also:* [`File.Error`](File#fileerror),
[`File.Open`](File#fileopen),
[`File.ReadStringBack`](File#filereadstringback)

---

### `File.Error`

*(Formerly known as `FileIsError`, which is now obsolete)*

```ags
readonly bool File.Error
```

Checks whether an error has occurred reading from or writing to the
specified file.

An error can occur if, for example, you run out of disk space or the
user removes the disk that is being read from.

This function only checks for errors while actually reading/writing
data. The File.Open function will return null if there was an error
actually opening or creating the file.

To find out whether all data has been read from a file, use
[`EOF`](File#fileeof) instead.

Example:

```ags
File *output = File.Open("$SAVEGAMEDIR$/test.dat", eFileWrite);
output.WriteInt(51);
if (output.Error) {
    Display("Error writing the data!");
}
output.Close();
```

will write a number to the file 'test.dat', and display a message if
there was a problem.

*See also:* [`File.EOF`](File#fileeof),
[`File.ReadStringBack`](File#filereadstringback)

---

### `File.Path`

```ags
readonly String File.Path
```

Gets the path to opened file.

Example:

```ags
File *output = File.Open("$SAVEGAMEDIR$/custom_save.txt", eFileWrite);
if(output) {
  System.Log(eLogInfo, "Opened \"%s\" for writing.", output.Path);
  output.WriteRawLine("won=false");
  output.Close();
}
```

Will write the full absolute path of the file in the debug log.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`File.Open`](File#fileopen), [`File.ResolvePath`](File#fileresolvepath)

---

### `File.Position`

```ags
readonly int File.Position
```

Gets current File's reading or writing position. This value means number
of bytes between file's beginning and current place you are reading from
or writing to.

This may be useful, for example, if you are passing the file pointer to
another script module function and want to know how much data that
function has read or written.

Example:

```ags
File *output = File.Open("$SAVEGAMEDIR$/test.dat", eFileWrite);
int old_pos = output.Position;
WriteCustomModuleData(output);
int new_pos = output.Position;
Display("Custom module has written %d bytes", new_pos - old_pos);
output.Close();
```

will open file, pass the file pointer to some custom function, then
display amount of data that function wrote.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`File.Seek`](File#fileseek)

