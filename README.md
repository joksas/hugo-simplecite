# Hugo Simple Cite

Citations in [Hugo] websites. Inspired by [Hugo Cite].

## Motivation

[Hugo Cite] provides a straightforward way to reference bibliographic entries inside [Hugo] documents. However, it also requires loading an external CSS file for proper display of tooltips. This is sub-optimal if one wants to include the documents in an RSS feed where it is the convention that *the reader* is responsible for the presentation. Of course, a parameter could be introduced to disable the tooltip but I figured it would be easier to just start a new project because I want to make a few other subjective changes. I decided to call this project **Hugo Simple Cite**.

## Installation

### Download

```text
git submodule add https://github.com/joksas/hugo-simplecite themes/hugo-simplecite
```

### Include in Hugo config file

Include the theme in your `config.toml`:
```text
theme = ["your", "other", "themes", "hugo-simplecite"]
```

### Add optional CSS

**Hugo Simple Cite** will work fine without CSS, but a stylesheet can be included to improve the appearance of some of the elements, e.g. to put square brackets around the indices of the ordered list of references. If you wish to include the CSS, insert these two lines in your HTML head template:
```html
{{ $simpleciteStyle := resources.Get "scss/hugo-simplecite.scss" | css.Sass | resources.Minify | resources.Fingerprint }}
<link rel="stylesheet" type="text/css" href="{{ $simpleciteStyle.Permalink }}">
```

## Usage

### Format

Just like [Hugo Cite], **Hugo Simple Cite** uses CSL-JSON for managing bibliography. If you use BibTeX files, you can convert them to CSL-JSON using [Pandoc](https://pandoc.org/):
```text
pandoc bibtex-file.bib -t csljson -o bib.json
```

Name your CSL-JSON file `bib.json` and place it in the same folder as your article's Markdown file.

### Shortcodes

#### `bibliography`

`{{< bibliography >}}` prints all bibliographic entries in `bib.json` as an [unordered HTML list](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ul).
By default, the entries will be sorted by year, in descending order.

The sorting may be controlled with the optional page parameter `bibliography_sort`, with possible values:
- `"date_descending"` (default): List entries by year, in descending order
- `"json_order"`: List entries in the same order as in the `bib.json` file

#### `bibentry`

`{{< bibentry "bibentryId" >}}` prints a bibliographic entry with `id` "`bibentryId`".

#### `cite`

`{{< cite "bibentryId" >}}` produces an in-text citation for bibliographic entry with `id` "`bibentryId`". At the moment, only Vancouver (numeric) system is implemented; Harvard (author⁠—date) system (like the one in [Hugo Cite]) may be implemented in the future.

You can also provide the reference portion such as a page as the second positional parameter.
For example, `{{< cite "bibentryId" "p. 5" >}}` produces an in-text citation for the bibliographic entry with `id` "`bibentryId`" and adds `p. 5` in the citation.

#### `references`

`{{< references >}}` prints bibliographic entries cited in the document as an [ordered list](https://html.com/tags/ol/).

### Example

Suppose we have a CSL-JSON file with two entries:
```json
[
  {
    "DOI": "10.1093/mind/LIX.236.433",
    "author": [
      {
	"family": "Turing",
	"given": "A. M."
      }
    ],
    "container-title": "Mind",
    "id": "Turing1950",
    "issue": "236",
    "issued": {
      "date-parts": [
	[
	  1950,
	  10
	]
      ]
    },
    "page": "433-460",
    "title": "I.—Computing Machinery and Intelligence",
    "type": "article-journal",
    "volume": "LIX"
  },
  {
    "author": [
      {
	"family": "Shannon",
	"given": "Claude E"
      }
    ],
    "container-title": "The Bell system technical journal",
    "id": "Shannon1948",
    "issue": "3",
    "issued": {
      "date-parts": [
	[
	  1948
	]
      ]
    },
    "page": "379-423",
    "title": "A mathematical theory of communication",
    "type": "article-journal",
    "volume": "27"
  }
]
```

We can produce in-text citations and a reference list in the following way:
```markdown
## Information Theory and AI

The concepts of both information theory {{< cite "Shannon1948" >}} and
artificial intelligence {{< cite "Turing1950" >}} were formally introduced in
the middle of the twentieth century. {{< cite "Shannon1948" >}} is considered
to be Shannon's most influential work.

## References

{{< references >}}
```

This will be rendered as
![rendered version](https://user-images.githubusercontent.com/46974359/121787086-07cb8280-cbbc-11eb-994e-542598c65977.gif)

* The tooltip is implemented using standard `title` attribute in an HTML tag.
* The citations also hyperlink to the entries in the reference list.
* The same entries can be referenced multiple times in the text; they will appear only once in the reference list.

## Configuration

Currently, only IEEE referencing style is implemented (for the most popular entry types). Feel free to contribute.

As mentioned earlier, you can include the CSS file that comes with **Hugo Simple Cite**. Alternatively, you can apply your own styling. I've included custom classes for most HTML tags used in the shortcodes so that the users could easily select relevant elements.

## TODOs

- [ ] Implement Harvard citation system (and a way to switch between it and the Vancouver system).
- [ ] Add more referencing styles (and a way to switch between them).
- [ ] Allow to specify CSL-JSON file in a different location.

[Hugo]: https://gohugo.io/
[Hugo Cite]: https://github.com/loup-brun/hugo-cite
