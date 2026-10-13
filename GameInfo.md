## `GameInfo` properties

GameInfo struct provides optional information about the game, which corresponds to the "Info" category section of the General Settings pane in the Editor.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

---

### `GameInfo.Title`

```ags
static readonly attribute String GameInfo.Title
```

Gets the game's title.

Example:

```ags
lblAboutTitle.Text = GameInfo.Title;
```

will show the game's title, as set in the General Settings, in the label lblAboutTitle, for example on an "About" screen.

---

### `GameInfo.Description`

```ags
static readonly attribute String GameInfo.Description
```

Gets the game's description.

Example:

```ags
lblAboutText.Text = GameInfo.Description;
```

will show the game's description, as set in the General Settings, in the label lblAboutText, for example on an "About" screen.

---

### `GameInfo.DeveloperName`

```ags
static readonly attribute String GameInfo.DeveloperName
```

Gets the game's developer's name.

Example:

```ags
lblAboutAuthor.Text = String.Format("A game by %s", GameInfo.DeveloperName);
```

will show the developer's name, as set in the General Settings, in the label lblAboutAuthor, for example on an "About" screen.

---

### `GameInfo.DeveloperURL`

```ags
static readonly attribute String GameInfo.DeveloperURL
```

Gets the game's developer's URL string.

Example:

```ags
lblAboutWebsite.Text = GameInfo.DeveloperURL;
```

will show the developer's web address, as set in the General Settings, in the label lblAboutWebsite, for example on an "About" screen.

---

### `GameInfo.Genre`

```ags
static readonly attribute String GameInfo.Genre
```

Gets the game's genre description.

Example:

```ags
lblAboutGenre.Text = GameInfo.Genre;
```

will show the game's genre, as set in the General Settings, in the label lblAboutGenre, for example on an "About" screen.

---

### `GameInfo.ReleaseDate`

```ags
static readonly attribute DateTime* GameInfo.ReleaseDate
```

Gets the game's release date as a DateTime instance.

Example:

```ags
DateTime *date = GameInfo.ReleaseDate;
lblAboutDate.Text = String.Format("Released on %d/%d/%d", date.DayOfMonth, date.Month, date.Year);
```

will show the game's release date, as set in the General Settings, in the label lblAboutDate, for example on an "About" screen.

---

### `GameInfo.Version`

```ags
static readonly attribute String GameInfo.Version
```

Gets the game's release version, represented as "X.Y.Z.W" string.

Example:

```ags
lblVersion.Text = String.Format("v%s", GameInfo.Version);
```

will show the game's version, as set in the General Settings, in the label lblVersion. Showing the version on the title screen helps players tell you which version they have when they report a problem.
