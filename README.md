# GoOse

*HTML Content / Article Extractor in Go*

[![Build Status](https://secure.travis-ci.org/advancedlogic/GoOse.png?branch=master)](https://travis-ci.org/advancedlogic/GoOse?branch=master)
[![Coverage Status](https://coveralls.io/repos/advancedlogic/GoOse/badge.svg?branch=master&service=github)](https://coveralls.io/github/advancedlogic/GoOse?branch=master)
[![Go Report Card](https://goreportcard.com/badge/github.com/pavelbazika/GoOse)](https://goreportcard.com/report/github.com/pavelbazika/GoOse)
[![GoDoc](https://godoc.org/github.com/pavelbazika/GoOse?status.svg)](http://godoc.org/github.com/pavelbazika/GoOse)

## Description

GoOse is a powerful Go library and command-line tool for extracting article content and metadata from HTML pages. This is a Go port of the original "Goose" library, completely rewritten and modernized for contemporary Go development.

**Key Features:**
- 🚀 Extract clean article text from web pages
- 📰 Extract article metadata (title, description, keywords, images)
- 🖼️ Advanced image extraction and top image detection
- 🎥 Video content detection and extraction
- 🌐 Multi-language support with stopwords
- 🔧 Command-line interface for easy integration
- 📦 Clean library API for programmatic use
- ⚡ High performance with concurrent processing support

Originally licensed to Gravity.com under the Apache License 2.0. Go port written by Antonio Linari.

## Installation

### As a Library
```bash
go get github.com/pavelbazika/GoOse
```

### As a CLI Tool
```bash
# Install directly
go install github.com/pavelbazika/GoOse/cmd/goose@latest

# Or build from source
git clone https://github.com/pavelbazika/GoOse.git
cd GoOse
make build
# Binary will be available at ./bin/goose
```

## Quick Start

### Command Line Usage

```bash
# Extract article from URL (text output)
goose convert https://example.com/article

# Extract article with JSON output
goose convert https://example.com/article --format json

# Save output to file
goose convert https://example.com/article --output article.txt

# Show version
goose version

# Show help
goose help
```

### Library Usage

```go
package main

import (
	"fmt"
	"log"

	"github.com/pavelbazika/GoOse/pkg/goose"
)

func main() {
	// Create a new GoOse instance
	g := goose.New()

	// Extract from URL
	article, err := g.ExtractFromURL("https://edition.cnn.com/2012/07/08/opinion/banzi-ted-open-source/index.html")
	if err != nil {
		log.Fatal(err)
	}

	// Print extracted content
	fmt.Println("Title:", article.Title)
	fmt.Println("Description:", article.MetaDescription)
	fmt.Println("Keywords:", article.MetaKeywords)
	fmt.Println("Content:", article.CleanedText)
	fmt.Println("URL:", article.FinalURL)
	fmt.Println("Top Image:", article.TopImage)
	fmt.Println("Authors:", article.Authors)
	fmt.Println("Publish Date:", article.PublishDate)
}
```

### Advanced Configuration

```go
package main

import (
	"github.com/pavelbazika/GoOse/pkg/goose"
)

func main() {
	// Create configuration
	config := goose.Configuration{
		Debug:          false,
		TargetLanguage: "en",
		UserAgent:      "MyApp/1.0",
		Timeout:        30, // seconds
	}

	// Create GoOse with custom configuration
	g := goose.NewWithConfig(config)

	// Extract from raw HTML
	html := "<html><body><article><h1>Title</h1><p>Content...</p></article></body></html>"
	article, err := g.ExtractFromRawHTML(html, "https://example.com")
	if err != nil {
		// Handle error
	}

	// Use the extracted article
	_ = article
}
```

## Project Structure

GoOse follows standard Go project layout:

```
├── cmd/goose/          # CLI application
├── pkg/goose/          # Public library API
├── internal/           # Private application code
│   ├── crawler/        # Web crawling logic
│   ├── extractor/      # Content extraction
│   ├── parser/         # HTML parsing utilities
│   ├── types/          # Shared data types
│   └── utils/          # Utility functions
├── docs/               # Documentation
├── sites/              # Test HTML files
└── Makefile           # Build automation
```

## Development

### Prerequisites

- Go 1.21 or later
- Make (for build automation)

### Getting Started

1. **Clone the repository:**
   ```bash
   git clone https://github.com/pavelbazika/GoOse.git
   cd GoOse
   ```

2. **Install dependencies:**
   ```bash
   make deps
   ```

3. **Build the project:**
   ```bash
   make build
   ```

4. **Run tests:**
   ```bash
   make test
   ```

5. **Run all quality checks:**
   ```bash
   make qa
   ```

### Available Make Commands

```bash
make help          # Show all available commands
make build         # Build the CLI binary
make install       # Install CLI to GOPATH/bin
make test          # Run all tests
make test-race     # Run tests with race detection
make coverage      # Generate coverage report
make format        # Format source code
make lint          # Run linters
make qa            # Run all quality checks
make clean         # Clean build artifacts
make tidy          # Clean up go.mod and go.sum
```

### Development Workflow

1. Make changes to the code
2. Run `make format` to format your code
3. Run `make qa` to ensure quality
4. Run `make test` to verify functionality
5. Commit your changes

## API Reference

### Main Types

- **`goose.Goose`** - Main extractor instance
- **`goose.Article`** - Extracted article data
- **`goose.Configuration`** - Extractor configuration

### Key Methods

- **`goose.New()`** - Create new extractor with default config
- **`goose.NewWithConfig(config)`** - Create extractor with custom config
- **`ExtractFromURL(url)`** - Extract article from URL
- **`ExtractFromRawHTML(html, url)`** - Extract from HTML string

For complete API documentation, run:
```bash
go doc github.com/pavelbazika/GoOse/pkg/goose
```

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes following the coding standards
4. Run the full test suite (`make qa`)
5. Commit your changes (`git commit -m 'Add amazing feature'`)
6. Push to the branch (`git push origin feature/amazing-feature`)
7. Open a Pull Request

Please ensure your code:
- ✅ Passes all tests (`make test`)
- ✅ Follows Go formatting standards (`make format`)
- ✅ Passes linting checks (`make lint`)
- ✅ Has appropriate test coverage
- ✅ Includes documentation for public APIs

## Roadmap

### Current Status
- ✅ Modern Go modules support
- ✅ CLI interface with Cobra
- ✅ Comprehensive test coverage
- ✅ Standard Go project layout
- ✅ Build automation with Make

### Planned Improvements
- [ ] Enhanced error handling and logging
- [ ] Plugin architecture for custom extractors
- [ ] Performance optimizations
- [ ] Additional output formats (XML, YAML)
- [ ] Docker containerization
- [ ] Advanced image processing
- [ ] Batch processing capabilities

## License

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.

## Acknowledgments

- **@Martin Angers** for goquery
- **@Fatih Arslan** for set
- **Go Team** for the amazing language and net/html
- **Original Goose contributors** at Gravity.com
- **Community contributors** for ongoing improvements
