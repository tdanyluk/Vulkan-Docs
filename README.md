Vulkan API Documentation Project
================================

Obsolecence Warning
-------------------

**Note:** Starting with the 1.0.25 Vulkan Specification update, this
extension branch, `1.0-VK_IMG_filter_cubic`, is **obsolete**.

We have moved from a branch-per-extension model to a *single-branch model*.
In the new model, the `1.0` branch is the only actively maintained and
current branch. It includes all registered and published extensions. The
`1.0` branch may be built with many different combination of those
extensions, if desired.

This branch will not be deleted. However, it is out of date, and will become
further out of date as time passes. Please replace any use of this branch
with the `1.0` branch instead.

This repository contains formal documentation of the Vulkan API. This
includes the main API Specification, the reference (man) pages, the XML API
Registry, and related tools and scripts.

Repository Structure
--------------------

```
README.md               This file
ChangeLog.txt           Change log summary
doc/specs/              Main documentation tree
    vulkan/             Vulkan specification
        appendices/     Appendices - one file each
        chapters/       Chapters - one file each
        config/         asciidoc configuration
        images/         Images (figures, diagrams, icons)
        man/            Reference (manual) pages for API
        enums/          Includeable snippets for enumerations from vk.xml
        flags/          Includeable snippets for flags from vk.xml
        protos/         Includeable snippets for prototypes from vk.xml
        structs/        Includeable snippets for structures from vk.xml
        validity/       Includeable validity language from vk.xml
    misc/               Related specifications (GL_KHR_vulkan_glsl)
src/spec/               XML API Registry (vk.xml) and related scripts
src/vulkan/             Vulkan headers, generated from the Registry
```

Building the Specification and Reference Pages
----------------------------------------------

To build the documents, you need to install, at a minimum:

* GNU-compatible make
* asciidoc (http://www.methods.co.nz/asciidoc/)

On some systems/build targets you may also need:

* dblatex
* source-highlight

These tools are known to work on several varieties of Linux, MacOS X, and
Cygwin running under Microsoft Windows.

There are several make targets in doc/specs/vulkan :

* make xhtml - Build one large HTML specification document.
* make pdf - Build one large PDF specification document.
* make chunked - Build an HTML document broken into one file per chapter.
* make manhtml - Make HTML API reference (all man pages as one big file).
* make manpdf - Make a one-giant PDF API reference.
* make manhtmlpages - Make man pages as one-file-per-API.
* make manpages - Make man pages as nroff Unix-style (real) man pages.
* make allchecks - Run the validation rules on the specification.

The outputs will be written to $(OUTDIR), which defaults to out/ at the root
of the checked-out git repository.

To build PDF outputs (make pdf, make manpdf), you need a2x (part of the
asciidoc) package, dblatex and a LaTeX processor. The PDF builds are
currently configured to use a2x to go from asciidoc markdown to docbook, and
then run the result through dblatex to go from there to LaTeX and then
through your LaTeX processor to PDF.

Spec Validation
---------------

There are a couple of validation tools which look for inconsistencies and
missing material between the specification and ref pages, and the canonical
description of the API in vk.xml :

* checkinc
* checklinks
* allchecks - both checkinc and checklinks

They are necessarily heuristic since they're dealing with lots of
hand-written material. To use them you'll also need to install:

* python3

The '''checkinc''' target uses Unix filters to determine which autogenerated
API include files are used (and not used) in the spec. It generates several
output files, but the only one you're likely to care about is
'''actual.only'''. This is a list of the include files which are *not*
referenced anywhere in the spec, and probably correspond to undocumented
material in the spec.

The '''checklinks''' target validates the various internal tagged links in
the man pages and spec (e.g. the '''fname:vkFuncBlah''',
'''sname:VkStructBlah''', etc.) against the canonical description of the API
in vk.xml . It generates two output files, manErrs.txt and specErrs.txt,
which report problematic tags and the filenames/lines on which those tags
were found.


Generating Headers and Related Files
------------------------------------

The header file (src/vulkan/vulkan.h) and many parts of the specification
and reference page documents are generated from descriptions in the XML API
Registry (src/spec/vk.xml). All the generated files are checked into the
repository, and should not be modified directly. If you change vk.xml,
you can regenerated these files by going to src/spec and running:

* make clobber (get rid of all old generated files)
* make full_install (regenerate all the files)
