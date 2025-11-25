## Font Preview

You cannot exactly *edit* fonts from within the AGS itself at the time being (unless you are using a plugin that allows that), but AGS lets you to preview the imported fonts and configure their runtime display properties.

On the Explore Project panel, you can expand the Fonts node to show the fonts which are imported for your game. Right clicking on the "Fonts" node itself allows you to create a new font. When creating a new font, or double-clicking on existing font within the tree, a new preview panel will open.

When a new font is created it clones the last one in the list. To overwrite an existing font, press the "Import over this font..." button. You may also reimport font from a different file or with a different size by going to the Font's properties, and clicking on "..." button at "Source Filename" or "Font Size" property respectively.

AGS supports two kinds of fonts: TrueType (TTF) and bitmap fonts, which are: SCI fonts (Sierra's font format) and WFN fonts (extended bitmap font format).

**NOTE:** Only TTF fonts have freely adjustable import sizes. Bitmap fonts have only one size, but may be scaled up using an integer multiplier.

**NOTE:** On Windows you cannot import fonts directly from the Windows Fonts folder. Unfortunately there is nothing that can be done about this, you must manually copy the font to another folder and import it from there.

Fonts can have outlines. For LucasArts-style speech, outlines are really
a necessity since they stop the text blending into the background and
becoming un-readable. To outline a font, either set the OutlineStyle to
"Automatic" to have AGS do it for you, or you can use a specific font
slot as the outline font (it will be drawn in black behind the main font
when the main font is used).

Every font have following optional properties:

-   **Character Spacing** - defines *additional* uniform distance between each two characters in a text line. Normally the distance between each pair of characters is dictated by the font itself (and it may depend on which characters are positioned nearby). With this property you may increase or decrease this distance by a flat number. Default value is 0.

    NOTE: this property is available since **AGS 3.6.3**.

-   **Line Spacing** - defines default distance (in pixels) between two lines of wrapped text. Setting this to 0 will make font use its own (logical) height as a vertical spacing. Having line spacing lower than font's height will make lines partially overlap.
-   **Logical Height** - lets select how the font's height will be defined whenever it's requested by the script or engine, for purposes such as arrangement of visual elements. This has following choices:

    - **Nominal height (import size)** - uses the Font Size number which you entered when importing this font.
    - **Real height (reported by font)** - uses the "height" property reported by imported font itself.
    - **User-defined value** - lets you specify your own value.
    
    The purpose of this setting is to fix up the font height measurements in case the value reported by the font itself seems incorrect. "Nominal height" is a backwards compatible choice, as this is the way older versions of AGS were accounting font's height. "Real height" is a sensible default for properly designed TTF fonts. But if neither of the above gives good results, then you may choose to set up your own value.
    
    NOTE: this property is available since **AGS 3.6.3**.
    
-   **Logical Height value** - if you set Logical Height property to the "User-defined value", then this property will become visible and lets you type the exact value, that will be used as a font's height in game. Normally you should not need this, but sometimes there are fonts that do not report their height correctly, so that's where this setting comes handy.

    NOTE: this property is available since **AGS 3.6.3**.

-   **Outline Style** - lets you assign an outline to this font. This setting has following choices:

    - **None** - no outline.
    - **Automatic** - draw outline using same font. This is achieved by drawing same text but with certain offset. If selected, then two more settings become available (see below).
    - **UseOutlineFont** - draw same text with another font behind this one, which will serve as outline. This assumes that the font chosen for "outline" is suitable for such purpose and is slightly bigger in size than the current one.
    
-   **Auto Outline Style** - an additional style of the Automatic outline.

    - **Squared** - the outline will have sharp "rectangular" edges and corners.
    - **Rounded** - the outline will have rounded corners.
    
-   **Auto Outline Thickness** - thickness of the Automatic outline (in pixels).
-   **Size Multiplier** - an integer scale multiplier applied to this font. 
    Note that it commonly makes sense to assign this for the bitmap fonts,
    as TTF fonts may be reimported with different font size. 
    When set for a TTF font it will simply be initialized with a higher
    size in game.
-   **TTF Font adjustment** - an adjustment of the true-type font metrics. This setting is purposed for backwards compatibility only.

    - **Do nothing** - the font will be read as-is. This is the default.
    - **Resize ascender to the nominal font height** - the font's ascender (a part above baseline) will be resized to match the font's import size. This setting is meant to fixup fonts created for older versions of AGS, where engine had a somewhat different font rendering. If used with normal fonts, this may actually make them look worse.

-   **Vertical Offset** - defines additional vertical offset applied to
    every drawn line of text (when using this font). This property is
    mainly meant to override particular font's misbehavior.

