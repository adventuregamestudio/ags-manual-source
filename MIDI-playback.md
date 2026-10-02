## MIDI playback

MIDI is handled differently from other sound formats. MIDI files do not contain sampled audio, but instead contain control data intended to be received by MIDI compatible instruments. Since **AGS 3.6.0**, in order to play a MIDI file within the AGS engine the control data is passed to a software-based synthesizer known as TiMidity++ (or simply "Timidity").

**NOTE:** Timidity is built into the AGS engine and does not have to be installed separately.

**NOTE:** Prior to **AGS 3.6.0** MIDI playback on Windows would use a software-based synthesizer that was provided with the operating system. This is no longer the case; the following requirements now apply to Windows too.

In addition to the MIDI data and in order to generate audio, Timidity also requires Gravis Ultrasound compatible (GUS) patch files. These files provide a mapping between the MIDI data being passed in and synthesizer voices which generate audio output. The location of the patch files and the MIDI instruments which they may represent are determined through the presence of a Timidity configuration file.

### Where to get patch files

While it is possible to create custom Gravis Ultrasound compatible patch files, the majority of use cases are more easily covered by downloading a pre-configured bundle of compatible patch files from the Internet. As an example of a working set of patch files which are provided with Timidity configurations, the following archive contains the patches which were originally distributed with the Gravis Ultrasound driver:

http://www.gamers.org/pub/idgames/music/dgguspat.zip

**IMPORTANT:** It is recommended to ensure that you have the legal right to distribute and make use of third party patch files. The examples discussed here do not confirm any legal right or endorsement with regard to their usage.

### How to configure Timidity

The Timidity configuration file should be named "timidity.cfg" and should be present in the top-level of the game directory **or** at one of the following system paths:

**On Windows:**<br>
* `c:\timidity\timidity.cfg`

**On Linux and macOS**<br>
* `/etc/timidity.cfg`
* `/usr/local/share/timidity/timidity.cfg`

It is also possible to indicate a specific configuration file to use by setting its path as the value of the environment variable `TIMIDITY_CFG`.

When distributing a game with its own configuration and patch files it is likely preferable to have the Timidity configuration file in the top-level of the game directory (so that it is located automatically) and the referenced patch files located within a sub-directory. The `dir` directive within a Timidity configuration can be used to specify an additional search path for patch files. For example, if all patch files are located within a sub-directory named "my_patches" all that is needed is the relative path name:

```
dir ./my_patches
```

The path to a patch directory can also be absolute. An absolute path which is relative to the configuration file can be specified through the use of the special token `$basedir`, which expands to the path of the directory which contains the configuration file:

```
dir $basedir/my_patches
```
