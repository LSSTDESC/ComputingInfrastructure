# LSST DESC Notes: Use Cases

Here are some simple example use cases for the LSST DESC Notes.


### Search for _X_ in all LSST DESC Notes

From a single point of entry, search all Notes' content for string X and
be given an accurate, well-formatted table of possibilities.

Some of Notes these maybe private (to DESC): these results should be visible to (some) DESC members but not the world.


### Request a Compendium of _K_ LSST DESC Notes

Select K Notes from an index page, and be returned a single PDF file
that can be read efficiently offline.

The cross-references between Notes in this compendium would, ideally,
point locally (within the PDF) and not to any web view of the Note.


### Automatically generate `LaTeX` for an LSST DESC Notes

From an `rst` or `md` format Note, automatically generate a `LaTeX` file with associated figures, and a bibtex database. These products would then be more easily munged into PhD theses or journal papers.
