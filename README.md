# garrett-mkdocs

[docs.garrettsweeney.com](https://docs.garrettsweeney.com/)

## Generate Anki Decks

The sample deck format is in `anki-starter.md`

```bash
# Get pip
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
# Install
python3 get-pip.py
# Install package
pip install markdown-anki-decks
# Create decks directory if DNE
mkdir -p decks
# Create decks
mdankideck docs decks
```
