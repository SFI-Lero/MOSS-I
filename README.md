# Framework for Managing University Open Source Software
 👉 [Download the latest **Framework for Managing University Open Source Software** PDF](https://github.com/SFI-Lero/MOSS-I/tree/main/output/FrameworkManagingUniversityOSS.pdf)

Welcome to the source repostory for the _"Framework for Managing University Open Source Software"_ document! This document is a results of the [NORF](https://norf.ie/) funded [MOSS-I project](https://github.com/OpenIrelandNetwork/MOSS-I). We aim to continuously update and improve the document, and welcome contributions from the community. Furthermore, we encourage individuals and organisations to fork this repository, to create and formulate their distinct versions.  

## Repository organisation 
### Documents and outputs 
This repository contains the main source for the **Framework for Managing University Open Source Software** document in the form of a markdown document in the main folder called `FrameworkManagingUniversityOSS.md` (to learn more about markdown, see [this "cheatsheet"](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)).  Using the processes detailed below this markdown file is converted to a PDF file called [`FrameworkManagingUniversityOSS.pdf`](https://github.com/SFI-Lero/MOSS-I/tree/main/output/FrameworkManagingUniversityOSS.pdf) contained in the [`output`](https://github.com/SFI-Lero/MOSS-I/tree/main/output) folder.

### Template 
The PDF document produced here is generated from the markdown using a custom version of the [Eisvogel template](https://github.com/Wandmalfarbe/pandoc-latex-template). The main changes implemented here are the rendering of authors, the addition of DOI links, the addition of Orcid and ROR links, and the modification of the header and footer. The template used here can be found in the `/assets/templates` folder and is called [eisvogel.tex](https://github.com/SFI-Lero/MOSS-I/tree/main/assets/templates/eisvogel.tex). 

### Reference style
The reference style currently used is the IEEE style as per the `ieee.csl` file provided in the main repository. Other styles may be found in the [Citation Style Language - Style Repository](https://github.com/citation-style-language/styles). If an alternative style is used its `.csl` file should be added to this repository, and the "pandoc command" in the YAML defining the GitHub action should be adjusted.  

### GitHub action 
The GitHub is defined by a YAML file (see below) found in the `.github/workflows` folder. The action (to convert the markdown document to a pdf file) is triggered uppon a push to the repository. 

## Locally processing the markdown to PDF conversion
Assuming [pandoc](https://pandoc.org/installing.html) is installed you can run the following locally (from a terminal navigated to the main project folder) to build the PDF:
```
pandoc FrameworkManagingUniversityOSS.md --output=output/FrameworkManagingUniversityOSS.pdf --template assets/templates/eisvogel.tex --listings --number-sections --bibliography=bibliography.bib --citeproc --csl ieee.csl
```

## GitHub action based markdown to PDF conversion
The below reproduces a so called [YAML](https://en.wikipedia.org/wiki/YAML) file which configures a GitHub action to trigger the updating of the repository hosted PDF when a _push_ occurs. Here the YAML file is called `markdown2pdf.yml` and is found in the [`.github/workflows`](https://github.com/SFI-Lero/MOSS-I/tree/main/.github/workflows) folder. Glossing over the YAML file one may see it sets up pandoc, issues a command using pandoc to export the `FrameworkManagingUniversityOSS.md` file to a file called `FrameworkManagingUniversityOSS.pdf` using the template called `eisvogel.tex`. Once completed the next step configures git and adds and commits the updated PDF file. Finally the PDF is pushes to the repository.  
```
name: markdown2pdf

on: [push, pull_request]

jobs:
  convert_via_pandoc:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
      - uses: docker://pandoc/extra:3.5
        with:
          args: FrameworkManagingUniversityOSS.md --output=output/FrameworkManagingUniversityOSS.pdf --template assets/templates/eisvogel.tex --listings --number-sections --bibliography=bibliography.bib --citeproc --csl ieee.csl
      - uses: actions/upload-artifact@v4
        with:
          name: output
          path: output
      - name: Commit files # transfer the new pdf files back into the repository
        run: |
          git config --local user.name ${{ github.actor }}
          git config --local user.email ${{ github.actor }}@users.noreply.github.com
          git add .
          git commit -m "Updating pdfs"
      - name: Push changes # push the output folder to your repo
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          force: true
  ```

## How to contribute
If you'd like to help improve this project, we welcome all contributions! Your contribution can be anything, e.g. language changes, code improvements, or documentation. For more information we refer to the [contributing guidelines](https://github.com/SFI-Lero/MOSS-I/blob/main/CONTRIBUTING.md). All contributors and team members are expected to act in accordance to the [project's code of confuct](https://github.com/SFI-Lero/MOSS-I/blob/main/CODE_OF_CONDUCT.md).

## Licenses 
The code/software shared here is licensed under the [MIT open source license](https://github.com/SFI-Lero/MOSS-I/blob/main/LICENSE). All other assets shared here, such as documents and images, are licensed under the [Creative Commons CC-BY-4.0 license](https://creativecommons.org/licenses/by/4.0/). 
