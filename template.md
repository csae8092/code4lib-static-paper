<!-- start of template -->
 
# Title
## Subtitle
 
Author1: name
Author1: affiliation
Author1: affiliation additional
Author1: email
 
Author2: name
Author2: affiliation
Author2: affiliation additional
Author2: email
 
## About the authors
Maximum of 50 words bio for author 1. 
Maximum of 50 words bio for author 2.
 
Repeat the above as necessary.
 
<!-- Preamble separator. Do not remove this. -->
 
## Abstract
A max. 200 words abstract.
 
## Heading, e.g. Introduction
Do *not* number headings or subheadings. Use plain text throughout. E.g., i.e. lorem ipsum donec congue ante vitae nisi scelerisque, quis dapibus nibh feugiat. Vivamus tempus velit non nisl tristique, non luctus quam pretium etc. 
 
In-text citations will be auto-generated from codes that authors will use in the text. For this please see the section “Creating and handling citations and references” in the author instructions below.
 
Footnotes should be added in square brackets with a leading caret[^1]. Add footnotes to a section titled “Notes” as shown below. 
 
You can use *italics* and **bold** for emphasis, italics preferred. 
 
Direct quotes are “inlined in the running text” (Tompson 2014). Longer quotes, or examples can be “block quoted”:
 
> It was a dark and stormy night.
>
> The author cried out in agony on realizing the file had just been irrevocably deleted.
[The caption and or source should follow between square brackets. Excerpt from “Hapless” (Jackson 1987).]
 
Including images is easy:
 
![Caption text](/path/to/file/image_name.jpg)
 
E.g.: ![Fol. 192v. in Cod.poet.et phil.fol.22; Württembergische Landesbibliotheek.](/assets/images/WLB_philpoet22_fol_54r.jpg)
 
Code may also be cited in the submission. For this surround it with ‘back ticks’:
 
`
c = "hello world!"
print( c )
`
 
For more possibilities (lists, tables, etc.) please refer https://www.markdownguide.org/basic-syntax/. 
 
### Subheading
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. 
 
## Conclusion
 
## Notes
[^1]: This is an endnote.
 
<!-- end of template -->
 
 
## Author instructions
Please adhere to the template. If you have layout requirements that are not supported by the basic Markdown described above please *contact* us and ask so we can hack a solution together that works for all. Do not use extended Markdown syntax (apart from footnotes as described above) without prior consulting an editor.
 
Using a Markdown editor is recommended to check if your document is conformant and error free (https://www.markdownguide.org/tools/).
 
### Creating and handling citations and references
To reduce the cognitive load of painstaking and error prone bibliography tasks on both authors *and* editors we have decided that all authors will use the [@bibtexkey] references markup in their Markdown documents. Instead of typing for instance “(Jackson 2007)” inline in the text, authors can use the unique key that Zotero exports for each bibliography. To use these keys, follow these steps:
 
* Make sure you have finalized your text so that you do not need to add or remove references to your bibliography anymore.
* Open your sub collection folder in the online Zotero bibliography for the special issue.
* Select all items from your sub collection (do *not* select the folder, use Ctrl-A to select all *items* in the collection).
* Click on the button “export” in the menubar of Zotero.
* Select “BibTex”.
* Zotero will now generate a file called “export-data.bib”. Save this in an appropriate place on your device (or locate it in your Downloads folder).
 
![Screenshot exporting bibliography from Zotero.](./assets/images/exportBibliographyFromZotero.png)
*Exporting your bibliography from Zotero.*
 
* Do not be fooled by the extension “.bib”. It is just a plain text file that you can open in any text editor.
* Once opened in a text editor you will find that each item has a unique key
 
![Example export-data.bib](./assets/images/export-data_bib_inSublimeText.png)
*Example export-data.bib.*
 
* The key is the first code after the curly bracket with which each item opens. In the screenshot, for instance, visible keys are “_british_2024”, “_tei_2024”, “zotero-item-8465” and “zotero-item-8459”.
* **Carefully replace all citations in your text with the appropriate bibtexkey markup. Suppose that “(Jackson 2007)” refers to the bibliographic item with the key “zotero-item-8465”, then the text in that place becomes “[@zotero-item-8465]”. Be sure to *also* remove parentheses.**
* If a citation requires some additional information (such as a specific page) you can include that information between square brackets as a suffix to the key: “[@zotero-item-8465][p.:13-132]”.
* Excellent! You're done. Submit your materials. Bibliography and citations will be auto-generated in the right style in the publishing process.
 
*This process is really best done once you have finalized your text.* If you would have to edit, remove, or add an entry in your Zotero collection, there is a chance that your keys may change. In which case you will have to check and align all references once again. 
 
### What should be cited and/or referenced?
Normal citation rules apply: evidence your argument by rooting it in appropriate literature references. 
 
We ask you to also provide references generously for terms more technical than the blatant obvious (e.g. HTML, MSWord do not need references, NPM and TypeScript do). There is an obviously somewhat arbitrary boundary between which technical terms should be referenced and those that really do not need further explanation. In case of doubt we ask you kindly to err on the side of over referencing terms. We do appreciate this can be drudgery, but we want to be of service to a wide and potentially unexperienced audience. 
 
In the case of technical terms a link (to e.g. a Wikipedia page, GitHub io page, etc.) will be normally fine. It is fine to “inline” these, i.e. [text of the link](https://url.of.link). Or you can add them as endnotes. In case you refer to bespoke software, especially when produced as non-commercial or academic endeavor, software citation rules should be followed to credit the developers/authors of such code (see: https://www.software.ac.uk/publication/how-cite-and-describe-software).
 
### Submitting
Send a zip file to Matteo Romanello (matteo.romanello@uzh.ch). If the zip file is large email servers may refuse it (maximum sizes differ from as low as 5MB to 25MB). In that case please make the zip file downloadable or use an online service for large file delivery. The zip file should contain your text file(s) in the main folder and images and other assets to be published on the journal's pages in sub folders "images" and "assets" (properly linked from the text files as needed).
```
