# Building Operator Up Docs

## Setup Development Environment

First make sure you have poetry installed, then initialize your virtual environment with the following commands:

Install the requirements:
```bash
poetry install
```

Start the venv shell:
```bash
poetry shell
```

## Start The Local Development Server
you can start adding and hacking away on the documenting with live reload.
We use this to ensure all documentation nicely fits on the page and any content added will be seamless for users.

```bash
mkdocs serve
```