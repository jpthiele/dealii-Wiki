This note tries to give best practices for DOIs for deal.II tutorial and code gallery contributions.

1. Create a DOI at zenodo.org with title "The deal.II tutorial step-XY: The Title" or "The deal.II code gallery: The Title":
   - Include the .cc, CMakeLists.txt, and all necessary parameter files
   - You can include a README.md which will be prominently displayed in the citation (example: https://doi.org/10.5281/zenodo.484156). This allows you to link to the current version of the tutorial and provide instructions and additional citations, etc..
   - Select type "Software" and provide keywords, description, etc.
   - You probably want "open access" and "LGPL 2.1" (this is necessary to be included in deal.II anyways)
   - Create a reference of type "has this upload as part" entry with URL https://github.com/dealii/dealii or https://github.com/dealii/code-gallery
2. Add your paper to the deal.II zenodo community: https://zenodo.org/communities/dealii/
3. Add a ``@dealiiTutorialDOI{link,imglink}`` entry to your tutorial (see https://github.com/dealii/dealii/pull/4145 and https://github.com/dealii/dealii/pull/4180 for examples)


See https://groups.google.com/d/topic/dealii-developers/eQw15ZZxNqc/discussion and https://github.com/dealii/dealii/pull/4205 for some discussion about this.