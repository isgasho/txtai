# API

![api](../images/api.png#only-light)
![api](../images/api-dark.png#only-dark)

txtai has a full-featured API, backed by [FastAPI](https://github.com/tiangolo/fastapi), that can optionally be enabled for any txtai process. All functionality found in txtai can be accessed via the API.

The following is an example configuration and startup script for the API.

Note: This configuration file enables all functionality. For memory-bound systems, splitting pipelines into multiple instances is a best practice.

```yaml
# Index file path
path: /tmp/index

# Allow indexing of documents
writable: True

# Enbeddings index
embeddings:
  path: sentence-transformers/nli-mpnet-base-v2

# Extractive QA
extractor:
  path: distilbert-base-cased-distilled-squad

# Zero-shot labeling
labels:

# Similarity
similarity:

# Text segmentation
segmentation:
    sentences: true

# Text summarization
summary:

# Text extraction
textractor:
    paragraphs: true
    minlength: 100
    join: true

# Transcribe audio to text
transcription:

# Translate text between languages
translation:

# Workflow definitions
workflow:
    sumfrench:
        tasks:
            - action: textractor
              task: storage
              ids: false
            - action: summary
            - action: translation
              args: ["fr"]
    sumspanish:
        tasks:
            - action: textractor
              task: url
            - action: summary
            - action: translation
              args: ["es"]
```

Assuming this YAML content is stored in a file named index.yml, the following command starts the API process.

```bash
CONFIG=index.yml uvicorn "txtai.api:app"
```

uvicorn is a full-featured production ready server with support for SSL and more. See the [uvicorn deployment guide](https://www.uvicorn.org/deployment/) for details.

## Connect to API

The default port for the API is 8000. See the uvicorn link above to change this.

txtai has a number of language bindings which abstract the API (see links below). Alternatively, code can be written to connect directly to the API. Documentation for a live running instance can be found at the `/docs` url (i.e. http://localhost:8000/docs). The following example runs a workflow using cURL.

```bash
curl \
  -X POST "http://localhost:8000/workflow" \
  -H "Content-Type: application/json" \
  -d '{"name":"sumfrench", "elements": ["https://github.com/neuml/txtai"]}'
```

## Local instance

A local API instance can be instantiated. In this case, the API runs internally, without any network connections, providing the same consolidated functionality. This enables running txtai in Python with configuration.

The configuration above can be run in Python with:

```python
from txtai.api import API

app = API(index.yml)

# Run action
app.workflow("sumfrench", ["https://github.com/neuml/txtai"])
```

See this [link for a full list of methods](./methods).

## Supported language bindings

The following programming languages have bindings with the txtai API:

- [JavaScript](https://github.com/neuml/txtai.js)
- [Java](https://github.com/neuml/txtai.java)
- [Rust](https://github.com/neuml/txtai.rs)
- [Go](https://github.com/neuml/txtai.go)

See the link below for a detailed example covering how to use the API.

| Notebook  | Description  |       |
|:----------|:-------------|------:|
| [API Gallery](https://github.com/neuml/txtai/blob/master/examples/08_API_Gallery.ipynb) | Using txtai in JavaScript, Java, Rust and Go | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/neuml/txtai/blob/master/examples/08_API_Gallery.ipynb) |
