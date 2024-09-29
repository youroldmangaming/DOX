<img src="image_2024-09-15_111916647.png" width="150" height="100" alt="YOMG Lab Documentation">
# DOX - A Computer Scientists Lab NoteBook

<img src="comp.jpg"  alt="YOMG Lab Documentation">

For full source visit [github](https://github.com/youroldmangaming/DOX/).

# Notebook Overview

**DOX - A Computer Scientist's Lab Notebook** is a digital logbook, documenting the setup of various technical labs, hardware experiments, software configurations, and any testing performed. This site will serve as a reference for future experiments and a learning tool for others.

A lifetime of software development with experience in all roles, from greenfield self-starting software houses to fully enabled small to medium-sized teams, and then onto enterprise-level standards-based production (Software Engineer to Chief Technical Officer). The scope includes:

- Software Engineering
- Infrastructure Implementation
- Database, Security, and System Administration
- Service Delivery and Account Management
- Product Development and Marketing
- ITIL, Best Practices
- Project and Programme Delivery

This project will cover it all, with no simple demarcation, as these fields are all deeply intertwined.

## Core Features

### 1. Project and Experiment Logging
- Document detailed step-by-step instructions on setting up labs, configurations, experiments, and troubleshooting.

### 2. Categorization and Tagging
- Organize logs by categories (e.g., hardware setups, software configurations, Docker experiments) and tags (e.g., Raspberry Pi, LoRa, Docker, Ansible, IoT).

### 3. Version Control for Documentation
- Store all experiment logs in a version-controlled repository (e.g., Git), allowing for proper revision history.

### 4. Searchable Knowledge Base
- Implement search functionality to easily find past experiments and configurations.

### 5. Interactive Code/Command Blocks
- Provide code snippets and commands in the documentation with options to easily copy them.

### 6. Collaborative Notes and Comments
- Allow for collaborative notes where others can comment on existing entries or contribute new insights.


Interactive Code/Command Blocks:

Provide code snippets and commands in the documentation with options to copy them easily.

Collaborative Notes and Comments:
Option to add collaborative notes for others, allowing them to comment on existing entries or contribute new insights.

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.
* This is a test3.
## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.




## SmartyPants

SmartyPants converts ASCII punctuation characters into "smart" typographic punctuation HTML entities. For example:

|                |ASCII                          |HTML                         |
|----------------|-------------------------------|-----------------------------|
|Single backticks|`'Isn't this fun?'`            |'Isn't this fun?'            |
|Quotes          |`"Isn't this fun?"`            |"Isn't this fun?"            |
|Dashes          |`-- is en-dash, --- is em-dash`|-- is en-dash, --- is em-dash|


## KaTeX

You can render LaTeX mathematical expressions using [KaTeX](https://khan.github.io/KaTeX/):

The *Gamma function* satisfying $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$ is via the Euler integral

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$

> You can find more information about **LaTeX** mathematical expressions [here](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference).


## UML diagrams

You can render UML diagrams using [Mermaid](https://mermaidjs.github.io/). For example, this will produce a sequence diagram:

```mermaid
sequenceDiagram
Alice ->> Bob: Hello Bob, how are you?
Bob-->>John: How about you John?
Bob--x Alice: I am good thanks!
Bob-x John: I am good thanks!
Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

Bob-->Alice: Checking with John...
Alice->John: Yes... John, how are you?
```

And this will produce a flow chart:

```mermaid
graph LR
A[Square Rect] -- Link text --> B((Circle))
A --> C(Round Rect)
B --> D{Rhombus}
C --> D
```

---

**DOX - A Computer Scientist's Notebook**  
_Y0MG 1990-2024_  
[GitHub Repository](https://github.com/youroldmangaming/DOX/tree/master) | [Documentation Site](https://dox.youroldmangaming.com)

---
