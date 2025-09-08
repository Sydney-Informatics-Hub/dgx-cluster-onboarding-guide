# SIH Quarto Training template

This repository can serve as a template for making training materials using [Quarto](https://quarto.org/) from:

- [Jupyter notebooks aka `.ipynb` files](#for-python-ipynb)
- [R content - `.qmd` files](#for-r-qmd) which mix R (and Python via reticulate, if needed) and md sections
- [Plain old `.md` files](#for-markdown-md-or-qmd)

To:

- Support this cross-language functionality
- Keep the repo as lean as possible
- Ensure as little unexpected behaviour as possible, please:

## Contributing to this repository

1. **Do not** under any circumstances commit to the main branch without review by someone else;
2. Fork the repository, make and test changes, then put in a pull request;
3. Please do not add GitHub actions or other "enhacements" that only work for your favourite programming language or workflow, and/or require a lot of extra files to work.

While GitHub actions seem like an amazing enhancement, we often develop materials that use custom libraries, renv or conda environments, docker builds etc.

So if you'd like to use actions:

1. Create your custom training workshop repository for your content
2. Delete what you don't need from this template
3. Create your custom actions script
4. Add a note to the bottom of this readme linking to your actions page, so others can use it as an example when creating training using a similar combination of environments and language.

## File location

We recommend the following structure for all published content:

- Store `index.qmd`, `setup.qmd` files in the root directory
- Store notebooks (.ipynb), R files (as .qmd or .rmd), and markdown lessons (.qmd or .md) in the `notebooks` folder
- Store figures in a `figs` folder

## How to use this template

For all languages/sources:

1. Use the big green button "Use this template" this repository
2. Edit `index.qmd` to change the main landing page. This is basically a markdown file.
3. Edit or create `setup.qmd` to change the Setup instruction pages. Same - basically a md file.
4. Edit `_quarto.yml` to change the dropdown menu options.
5. Add additional `*.md` files to the root dir to have them converted to html files (and add them to `_quarto.yml` to make them navigable), if you'd like.
6. Edit this Readme in your fork to reflect the content of your workshop.

If you want to use the command line instead of VSCode/RStudio (as described below), run the below commands (after activating the correct Python environment, if needed)

```bash
quarto render
# First time you create the file, add them to be tracked by github, e.g.
git add docs/*
git commit -am "your comments"
git push
```

You can browse the result locally by exploring the html files created (note: sometimes figures display locally but not on web and the other way around too.)

### For Python .ipynb

- You will need to have Jupyter and Quarto installed to convert the notebooks and render them for the web.
- [Recommended, not essential] Use VSCode's [Quarto Extension](https://quarto.org/docs/tools/vscode.html) to render the project (recommended b/c it's easier/nice).

1. Delete `notebooks/01b-exampleRcontent.qmd` and `01c-exampleMDcontent.md`
2. Create notebooks in the `notebooks` folder (for example `notebooks/1_cont.ipynb`). Have a look at what the syntax for Challenges, Objectives, Key Points and Questions is supported in `01a-fundamentals.ipynb`, and use similar syntax across other notebooks where needed.
3. Execute all the cells in your Jupyter notebook(s) until you're happy with the output/
4. Add links to your content to the navigation configuration in `_quarto.yml`. For example, to link to the rendered page for `notebooks/1_cont.ipynb`, add a link to `notebooks/1_cont.html` in `_quarto.yml`

Note:

Building from Jupyter notebooks will **not** re-render all of the notebooks unless you use the `quarto render notebook.ipynb --execute` command

### For R .qmd

- `.qmd` is called "Quarto Markdown", and basically works just like Rmd.
- If using R, you will need rmarkdown, xml2 and X to have the notebooks generate properly and link out to the documentation, as specified in the `_quarto.yml` file.
- Building from R will by default re-render all of the outputs.
- Sometimes you have to delete everything in your cache :(

1. Delete `notebooks/01a-fundamentals.ipynb`, `environment.yml` and `01c-exampleMDcontent.md`
2. Create notebooks in the `notebooks` folder (for example `notebooks/1_cont.qmd`). Have a look at what the syntax for Challenges, Objectives, Key Points and Questions is supported in `01b-exampleRcontent.qmd`, and use similar syntax across other .qmd files where needed.
3. Add links to your content to the navigation configuration in `_quarto.yml`. For example, to link to the rendered page for `notebooks/1_cont.qmd`, add a link to `notebooks/1_cont.html` in `_quarto.yml`
4. Type `quarto render` in the Terminal in RStudio (not the R command line, the Terminal tab!) - or use the buttons.

### For markdown .md or .qmd

1. Delete `notebooks/01a-fundamentals.ipynb`,`01b-exampleRcontent.qmd` and `environment.yml`
2. Create files in the `notebooks` folder (for example `notebooks/1_cont.md`). Have a look at what the syntax for Challenges, Objectives, Key Points and Questions is supported in `01c-exampleMDcontent.md`, and use similar syntax across other .md files where needed.
3. Add links to your content to the navigation configuration in `_quarto.yml`. For example, to link to the rendered page for `notebooks/1_cont.md`, add a link to `notebooks/1_cont.html` in `_quarto.yml`
4. Type `quarto render` in the terminal - or use VSCode's "Render Quarto project' command using the command pallette instead.

## Publish with GitHub pages

To publish Quarto websites to GitHub pages, you can:
* Render sites on your local machine (see above)
* Use the `quarto publish` command
* Use [GitHub Actions](https://github.com/quarto-dev/quarto-actions)

### Quarto publish command

Enable GitHub pages in your repository settings:
* Go to GitHub repository settings
* Scroll down to "GitHub Pages" section and select the following:
    * `Source: deploy from a branch`
    * `Branch:main`
      * `Directory: /root`
    * `Save`

Run the quarto publish command (assuming you are working in terminal)
```bash
quarto publish
```

Your Quarto project will be rendered at a URL with this format:
```html
https://pages.github.sydney.edu.au/informatics/<NAME-OF-YOUR-REPO>/
```

### GitHub Actions

Here are some example actions.

#### Auto render and publish quarto
Add this action to automatically `quarto render && quarto publish`. Only works well if your repo uses markdown files exclusively, cannot execute python/R code for example. In the current form, it expects "Github Pages" to be deployed on the `gh-pages` branch from `/(root)`, i.e. the [standard configuration](https://github.com/Sydney-Informatics-Hub/training-template/settings/pages).

```
# Example from https://github.com/quarto-dev/quarto-actions/blob/main/examples/quarto-publish-example.yml
name: Quarto Publish

on:
  push:
    branches: main

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v3

      - name: Set up Quarto
        uses: quarto-dev/quarto-actions/setup@v2
        with:
          tinytex: true

      - name: Render Quarto Project
        uses: quarto-dev/quarto-actions/render@v2

      - name: Publish to GitHub Pages (and render)
        uses: quarto-dev/quarto-actions/publish@v2
        with:
          target: gh-pages
```

### Themes, Aesthetic and Branding

If you'd like to use a more generic and possibly neutral theme, go to the `_quarto.yaml` and change the format section to:

```yaml
format:
  html:
   toc: true
   theme:
      light: flatly
      dark: darkly
   css: styles.scss
   code-link: true
   code-fold: false
```

If you'd like to use the USYD Masterbrand Ochre, go to the `_quarto.yaml` and change the format section to:

```yaml
format:
  html:
    theme: simplex
    css: [lesson.css, bootstrap-icons.css]
    toc: true
    code-overflow: wrap
    highlight-style: github
```

## Examples of template use, with GitHub actions
