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


### Automatically generate `LaTeX` for an LSST DESC Note

From an `rst` or `md` format Note, automatically generate a `LaTeX` file with associated figures, and a bibtex database. These products would then be more easily munged into PhD theses or journal papers.


### Cite an LSST DESC Note in an ApJ Paper

LSST DESC Notes must be citeable objects, having at least a unique URL and at best (and perhaps not in all cases) an ADS-indexed DOI. The pros and cons of this approach should be weighed against posting to the arXiv (or publishing!) any DESC Note that we want to cite.  This would solve, e.g., the hosting problem, not that GitHub will not exist forever.

### Find LSST DESC Notes by author name

This is distinct from 'Search for _X_ in all LSST DESC Notes.  

### Browse an index of LSST DESC Notes by keyword

The most obvious example is browsing by DESC science working group, but other keywords could be defined.  Other (sub-)topical keywords could be defined.

### Find an LSST DESC Note by DESC Note number

This is relevant only if the LSST DESC Notes actually are assigned numbers as a convenient shorthand, something human readable like DN-1234, rather than, say, a DOI like 10.3847/0004-637X/826/1/76.

### See the version history of an LSST DESC Note and retrieve previous versions

LSST DESC Notes are not necessarily going to be static.  Updated versions intended for 'release' to the collaboration or to the outside world should not make previous versions (which may have been cited internally or by the outside world) disappear.

