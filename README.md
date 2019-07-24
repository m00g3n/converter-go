# AsyncAPI Converter

## Overview

The AsyncAPI Converter converts AsyncAPI documents from versions 1.0.0, 1.1.0 and 1.2.0 to version 2.0.0-rc1. It supports both `json` and `yaml` formats on input and output. By default, the AsyncAPI Converter converts a document into the `json` format.

## Prerequisites

- `go` in version 1.11+

## Installation

 To install the AsyncAPI Converter package, run:

`go get github.com/asyncapi/converter-go`

## Usage

You can use the AsyncAPI Converter in the terminal or as a package.

### CLI

To use the AsyncAPI Converter in a terminal, build the application. Run:

```bash
git clone https://github.com/asyncapi/converter-go.git
cd ./converter-go
go build -o=asyncapi-converter ./cmd/api-converter/main.go
```

To convert a document use the following command:

```text
asyncapi-converter <document_path> [--toYAML] [--id=<id>]
```

where:

- `document_path` is a mandatory argument that is either `URL` or `file path` to asyncapi document
- `--toYAML` is an optional argument and allows to produce results in yaml format instead of json
- `--id` is an optional argument and allows to specify application id

minimal example that will convert `gitter-streaming` from version `1.2.0` to `2.0.0-rc1` in `json` format

```text
asyncapi-converter https://git.io/fjMPF
```

minimal example that will convert `gitter-streaming` from version `1.2.0` to `2.0.0-rc1` in `yaml` format

```bash
asyncapi-converter https://git.io/fjMPF --toYAML
```

specify the application id example:

```bash
asyncapi-converter https://git.io/fjMXl --id=urn:com.asynapi.streetlights
```

### As a package

```go
package main

import (
	"asyncapi-converter/pkg/asyncapi"
	"asyncapi-converter/pkg/converter/v2rc1"
	"log"
	"net/http"
	"os"
)

var (
	url = "https://git.io/fjMPF"
	id  = "urn:gitter-streaming"
)

func main() {
	// get gitter-streaming.yml
	resp, err := http.Get(url)
	handleError(err)

	// create yaml converter
	converter, err := v2rc1.NewYamlConverter(
		v2rc1.WithEncoding(asyncapi.Json),
		v2rc1.WithId(&id),
	)
	handleError(err)

	// convert document
	err = converter.Do(resp.Body, os.Stdout)
	handleError(err)
}

func handleError(err error) {
	if err != nil {
		log.Fatal(err)
	}
}
```
