# PEVŠ FI Bc. Thesis LaTeX Template
[![CC0](https://img.shields.io/badge/License-CC0-lightgrey.svg)](http://creativecommons.org/publicdomain/zero/1.0/)

Official LaTeX template for bachelor thesis at [Pan-European University](https://www.paneurouni.com), Faculty of Informatics

## Features

- 📚 Bibliography according to STN ISO 690
- 🔧 Easy customization
- 🤖 Automatic list generation
- 📝 Glossary/Acronym support
- 🖼️ Figure management
- 💻 Code listing support

## Prerequisites

- LaTeX distribution (e.g., TexLive)
- VSCode (optional)

## Setup

### Getting the Template
There are three ways to get started:

1. **Use as Template** (Recommended)
   - Visit the [GitHub repository](https://github.com/PEVS-FI/bc-thesis-template)
   - Click the green "Use this template" button in the top right
   - Select "Create a new repository"
   - Follow the prompts to create your own repository

3. **Download Release**
   - Visit the [GitHub repository](https://github.com/PEVS-FI/bc-thesis-template)
   - Download latest release
   - Extract the downloaded file to your preferred location

### Customizing Content
After obtaining the template:

- Edit `main.tex` and fill out the metadata section
- Replace `essentials/assignment.pdf` with your own assignment
- Edit `essentials/abstract.tex` with your abstract
- Edit `essentials/acknowledgment.tex` and `essentials/declaration.tex` if necessary

> **Note**: Remove *Consultant* row from the table in `titlepage-alt.tex` if not needed.


### VS Code Setup

1. Install LaTeX Workshop extension
2. Set citation backend to `biblatex`
3. Add `makeglossaries` and `biber` to `latex-workshop.latex.tools`
4. Add a build recipe to `latex-workshop.latex.recipes`

Example build recipe:
```json
{
    "name": "pdflatex ➞ makeglossaries ➞ biber ➞ pdflatex × 2",
    "tools": [
        "pdflatex",
        "makeglossaries",
        "biber",
        "pdflatex",
        "pdflatex"
    ]
} 
```
To use a specific recipe for your document, add this line at the beginning of `main.tex` (replace *recipe-name*):
```tex
%!LW recipe=recipe-name
```

### Building the Document

Either use VS Code's LaTeX Workshop build button or run:

```bash
pdflatex main
makeglossaries main
biber main
pdflatex main
pdflatex main
```

### Project Structure

```
.
├── essentials/            
│   ├── abstract.tex       # Abstract in SK/EN
│   ├── acknowledgment.tex # Acknowledgments
│   ├── acronyms.tex       # List of acronyms
│   ├── appendices.tex     # List of appendices
│   ├── bibliography.bib   # References
│   ├── declaration.tex    # Author's declaration
│   ├── titlepage.tex      # Main title page
│   ├── titlepage-alt.tex  # Secondary title page
│   └── assignment.pdf     # Assignment
├── examples/              
│   └── examples.tex       # Example usage demonstrations
├── main.tex               # Main document
└── README.md              # This file
```
## Usage

### Adding content

You can start writing your thesis in the **Document Body** section of the `main.tex` file:
```tex
%%%%%%% DOCUMENT BODY %%%%%%%
\section{Úvod}
Your content here...
```

For better organization, split your content into separate files:
```tex
% %%%%%%% DOCUMENT BODY %%%%%%%
\include{chapters/introduction}
\include{chapters/implementation}
\include{chapters/summary}
```

The `examples` section is mostly defined by the university, use it as a reference.
> **Note**: Remove `\include{examples/examples}` from `main.tex` in your final build.

### Bibliography
References are stored in `essentials/bibliography.bib` and displayed using the **STN ISO 690** norm. Examples are provided in file.

```bibtex
@book{borgman2003from,
author = {Borgman, Christine L.},
title = {From {Gutenberg} to the Global Information Infrastructure},
subtitle = {Access to Information in the Networked World},
location = {Cambridge (Mass.)},
publisher = {The MIT Press},
date = {2003},
pagetotal = {xviii, 324},
isbn = {0-262-52345-0},
langid = {english},
}
```

You can optionally set the citation style in `main.tex` bibliography section with `style=style-name`.

Available styles are: `iso-authoryear`(default) and `iso-numeric`. More info in the **Citation** section.

> **Note**: Editing bibliography requires rebuilding your document using the full build recipe.

### Citation
Template supports two citation styles:

1. **Author-Year Style** (`iso-authoryear`)
   - Using `\parencite{}` will add parenthesis around the reference
   - Format: `\parencite{key}` → (Author, Year)
   - Example: `\parencite{knuth1984}` → (Knuth, 1984)
   ---
   - Using `\textcite{}` will add reference in text
   - Format: `\textcite{key}` → Author, Year
   - Example: `\textcite{knuth1984}` → Knuth, 1984


2. **Numeric Style** (`iso-numeric`)
   - Using `\cite{}` will add number `[X]` in text and sort references by occurence   
   - Format: `\cite{key}` → [1]
   - Example: `\cite{knuth1984}` → [1]
   

> **Note**: Change citation style by modifying the `style` parameter in `main.tex`:
> ```tex
> \usepackage[
>     style=iso-authoryear, % or iso-numeric
> ]{biblatex}
> ```

### Acronyms
Acronyms are stored in `essentials/acronyms.tex`.

#### Adding New Acronyms
Add new acronyms to the end of `essentials/acronyms.tex` using this template:
```tex
\newacronym
    [description={Your description here}]
    {label}{SHORT}{Long Form}
```

Example:
```tex
\newacronym
    [description={Visual modeling language for software design and architecture}]
    {UML}{UML}{Unified Modeling Language}
```

#### Using Acronyms
Use acronyms in your text with the `\gls{label}` command:
- **First use displays:** "Long Form (SHORT)"
- **Subsequent uses display only:** "SHORT"

Example:
```tex
First use: \gls{UML} will show "Unified Modeling Language (UML)"
Second use: \gls{UML} will show just "UML"
```

> **Note**: Adding or modifying acronyms requires rebuilding your document using the full build recipe.

### Images

#### Default Configuration
All images are automatically centered on the page and sized to 90% of text width.

> **Note**: Default settings can be modified in `main.tex` in the **Default image options** section

#### Basic Usage
Insert an image with caption:
```tex
\begin{figure}[h]
    \includegraphics{path/to/your-image}
    \caption{Your caption here}
    \label{fig:unique-label}
\end{figure}
```

You can override default settings per image with arguments like:
```tex
\includegraphics[height=0.75\textwidth,angle=90]{image/to/rotate.png}
```
### Code Blocks

Add a new codeblock and specify the language with:
```tex
\begin{lstlisting}[language=Python, caption={Your caption}, label={lst:python-example}]    
Your code here...
\end{lstlisting}
```

> **Note**: Default settings can be modified in `main.tex` in the **Code blocks** section

## License

This work is dedicated to the public domain under the CC0 1.0 Universal license.

[![CC0](https://i.creativecommons.org/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, author has waived all copyright and related or neighboring rights to this work.