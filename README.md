
Using biblatex:
```
pandoc document.md --output=output/document.pdf --template assets/templates/eisvogel.tex --listings --number-sections --biblatex --bibliography=bibliography.bib 
```

Using citeproc:
```
pandoc document.md --output=output/document.pdf --template assets/templates/eisvogel.tex --listings --number-sections --bibliography=bibliography.bib --citeproc 
```


```
name: markdown2pdf

on: push

jobs:
  convert_via_pandoc:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
      - uses: docker://pandoc/extra:3.5
        with:
          args: document.md --output=output/document.pdf --template assets/templates/eisvogel.tex --listings --number-sections --bibliography=bibliography.bib --citeproc 
      - uses: actions/upload-artifact@v4
        with:
          name: output
          path: output
      - name: Commit files # transfer the new pdf files back into the repository
        run: |
          git config --local user.name "Kevin-Mattheus-Moerman"
          git add .
          git commit -m "Updating pdfs"
      - name: Push changes # push the output folder to your repo
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          force: true
  ```