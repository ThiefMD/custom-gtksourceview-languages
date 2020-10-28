# Custom Gtk.SourceView Languages

This is a set of [language specs](https://developer.gnome.org/gtksourceview/stable/lang-reference.html) used in [ThiefMD](https://thiefmd.com)

## Changes

* Markdown YAML Frontmatter support
* Code block language highlighting support
* Blockquote style applied to whole line
* Support for \~\~delete\~\~ tag
* Change in XML and HTML comment styling

## Usage

In this example, we packages the language-specs in our `Build.PKGDATADIR/gtksourceview-4/language-specs`.

```vala
    public Gtk.SourceLanguageManager get_language_manager () {
        if (thief_languages == null) {
            thief_languages = new Gtk.SourceLanguageManager ();
            string custom_languages = Path.build_path (
                Path.DIR_SEPARATOR_S,
                Build.PKGDATADIR,
                "gtksourceview-4",
                "language-specs");
            string[] language_paths = {
                custom_languages
            };
            thief_languages.set_search_path (language_paths);

            var markdown = thief_languages.get_language ("markdown");
            if (markdown == null) {
                warning ("Could not load custom languages");
                thief_languages = Gtk.SourceLanguageManager.get_default ();
            }
        }

        return thief_languages;
    }
```

Inside of our editor, we can do:

```vala
var languages = get_language_manager ();
var markdown_syntax = languages.get_language ("markdown");
Gtk.SourceBuffer buffer = new Gtk.SourceBuffer.with_language (markdown);
Gtk.SourceView view = new Gtk.SourceView.with_buffer (buffer);
```

## Credits

* Originally from [GNOME/gtksourceview](https://gitlab.gnome.org/GNOME/gtksourceview)