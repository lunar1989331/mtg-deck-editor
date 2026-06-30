# MTG Deck Editor (Unofficial Fan Project)

A single-file, browser-based deck builder for *Magic: The Gathering*. No installation, no backend, no build step — just open the HTML file and start building.

> This is an unofficial, fan-made tool. *Magic: The Gathering* is a trademark of Wizards of the Coast LLC. This project is not affiliated with, endorsed, sponsored, or specifically approved by Wizards of the Coast.

## Features

- 🔍 **Card search** with autocomplete, powered by the [Scryfall](https://scryfall.com) API
- 🎨 **Filters** by color, type, rarity, and converted mana cost (CMC)
- 🃏 **Hover preview** for full-size card images
- 📊 **Deck statistics** — mana curve and color distribution charts (via Chart.js)
- ✅ **Legality checker** for supported formats
- 💾 **Local save/load** — decks are stored in your browser (`localStorage`), no account needed
- 📤 **Import / Export** — supports plain text, Arena, and Archidekt-style formats
- ⌨️ Keyboard shortcut: `Ctrl/Cmd + S` to save

## Getting Started

1. Download `mtg-deck-editor.html`
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
3. Start searching for cards and building your deck

That's it — no server, no `npm install`, no API key required.

> **Note:** Deck data is saved locally in your browser. Clearing your browser data (or switching browsers/devices) will lose saved decks unless you export them first.

## Tech Stack

- Vanilla HTML / CSS / JavaScript (no framework, no build tools)
- [Chart.js](https://www.chartjs.org/) for mana curve / color pie charts
- [Scryfall API](https://scryfall.com/docs/api) for card data and images

## Data Attribution

Card data and images are provided by [Scryfall](https://scryfall.com). This project is not produced, endorsed, supported, or affiliated with Scryfall or Wizards of the Coast.

## Contributing

This is a small personal project shared as open source. Issues and pull requests are welcome — feel free to fork and adapt it for your own use.

## License

Released under the [MIT License](LICENSE).
