# Go Command Line Template Tool `gotmpl`

![WIP](https://img.shields.io/badge/status-wip-red)

*This is a template for a project that I have been [wishing] for. If you
are interested in a fun small-to-mid size Go project I would be willing
to mentor/help you through this for free just to get it done.*

[wishing]: <https://github.com/rwxrob/wish>

`gotmpl` is a simple wrapper to the very powerful Go templating system.
All it does is combine data with one of any number of templates from
your personal library. The data can be passed as YAML, JSON, TOML, or
XML files or fed to standard input in any of those forms. The structured
format of the data is automatically detected saving you unnecessary
grief with extra arguments.

`gotmpl` is ideal for integration with terminal editors like Vi/m and
Emacs.

## Command Line Argument Data

Every argument after the name of the template is assumed to be an
argument that goes into the default `.Args` template variable *unless*
the *first* argument is the name of a file. This allows the very
powerful combination when used within editors such as `vi` that support
passing one or more lines from the file being edited to a program and
replacing those lines with the output of the program, sometimes called
command-line "filters".

More complicated data can either be passed in files or using the Bash
"here" `<<<` syntax, or anything else that passes data into standard
input (see Examples).

## Data Template Format Detection

YAML, JSON, TOML, and XML are all supported structured data formats.
Each is detected from either the obvious suffixes (`yaml/yml`,
`json/jsn`, `toml`, `xml`) or the first non-whitespace character in the
data (`-`,`{`,`[`,`<` respectively). This means that JSON data relying
on this detection must always be an "object" (map/hash/Dict) instead of
an array. If the first non-whitespace character is anything other than
these four special identifiers then the format is assumed to be TOML,
which allows for the easiest `key=value` combinations that look normal
as command line argument strings

## Template Directory

Templates are stored in a directory set by the `GOTMPL` environment
variable. If not set, the `gotmpl` directory within the
`os.UserConfigDir()` will be used by default. It is recommended that the
template directory itself be managed within a Git or other source
management of some kind in order to keep copies of templates as they are
created.

### Importance of Suffixes

You can name the files within the template directory whatever you like.
However, here's some help in deciding what to call them:

* `tmpl` is supported by the popular `vim-go` plugin
* `gotmpl` is supported by other graphic editors
* `html` has wide support for "mustache"-like templating

If your suffix is `html` or the first non-whitespace character of your
template is a left angle bracket (`<`) then `html/template` will be
implied and used. Otherwise `text/template` is assumed by default.

## Examples

When an argument isn't provided an interactive menu with a list of all
templates is displayed.

```
gotmpl add mytemplate.html
gotmpl add mytemplate.txt
gotmpl ls
gotmpl rm [mytemplate]
gotmpl edit [mytemplate]
gotmpl mytemplate data.yml
gotmpl mytemplate < data.yml
gotmpl mytemplate data.json
gotmpl mytemplate < data.json
gotmpl mytemplate <<< '{"some":"thing"}'
gotmpl mytemplate <<< "some=thing \n another=thing"
gotmpl mytemplate first second third "four th"
```
