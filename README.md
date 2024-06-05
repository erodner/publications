# publication git

This is the publication repository of the computer vision group of the University of Jena.

## Small howto
1. clone the publication git repo (``git clone gogs@triton.inf-cv.uni-jena.de:ComputerVisionJena/Publications.git``)
2. Add your bibtex entry to the ``paper.bib``
3. Commit your changes and push to gitlab repo (``git commit -am "your nice log message" && git push origin master``)
4. Add your PDF and Teaser image to ``/home/dbv/publications`` with the name corresponding to the BibTex key used
4. Refresh publication database by browsing url: https://pub.inf-cv.uni-jena.de/refresh

## Important usage hints
* after updating the publication git, run a database refresh: https://pub.inf-cv.uni-jena.de/refresh
* URL for Lehrstuhl publication overview: https://www.inf-cv.uni-jena.de/publications.html
* URL to raw publication overwiev: https://pub.inf-cv.uni-jena.de/
* a person's publications: https://pub.inf-cv.uni-jena.de/author/Rodner (note: does not work for umlauts in names.)
* Umlauts in names: handle it using the seach function (eg. https://pub.inf-cv.uni-jena.de/search/Ruehle)
* List all Publications given a search string: https://pub.inf-cv.uni-jena.de/search/Johannes
* Windows users: Use Linux-style newline characters!


## Some rules to preserve consistency
* special characters and the & are NOT allowed!
* use full names as author names seperated with the word "and" instead of commas
* use {\"u}, {\"a}, {\"o} to specify Umlaute
* use the note field for text like "to be published", "submitted", etc.
* if you have the word Award or award in the note field, additional highlighting is added automatically
* keep all entries up to date with page number specifications etc
* add the abstract of a paper to the abstract field in the entry
* PDFs and teaser images have to be placed in ``/home/dbv/publications`` with the name corresponding to the BibTex key used
* Springer is fine with PDFs of the publications on your own personal page or a research group page. However, you should add a link to their webpage (or a similar reference) in the PDF. Limited and paid access to publications is simply something, nobody should accept anymore in research.
* link to the source code of your project with the ``code`` keyword in the BibTex entry
* you can even add HTML to your abstract to for example link to your supplementary material or to other example images
* Naming of Conferences:
  *No "Proceedings of th Xth ..." doesn't matter at all... :)
  * For IEEE conferences, add IEEE before "Conference on ..."
  * add the commbon acronym in brackets, e.g., IEEE Conference on Computer Vision and Pattern Recognition (CVPR)
  * Workshop Proceedings: Conf-Acro Workshop on foo and bar (ConfAcro-WS), e.g., ECCV Workshop on Parts and Attributes (ECCV-WS)
* Important for journal articles: add the 'volume' and the 'number' (i.e. issue) entry! A 'volume' entry without a 'number' entry will be ignored in the visualization.
