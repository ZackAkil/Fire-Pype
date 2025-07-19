# 🔥🐍 Fire-Pype

**The Flask framework that makes Google Cloud feel like magic.**

Build scalable real-time systems with intelligent caching, file processing, and observability - all optimized for Flask backends.

## 🎯 The Problem

Building Flask apps with Google Cloud is **painful**:

- ❌ Manual Firestore read/write cycles
- ❌ Complex file upload/download logic
- ❌ Serverless logging hell (thousands of tiny logs)
- ❌ No easy way to test with real production data
- ❌ Manual endpoint documentation and discovery

Fire-Pype **eliminates this pain** with intelligent decorators and automation.

## 🚀 Core Features

### 🔥 Smart Firestore Integration

```python
@app.route('/analyze/<doc_id>')
@fire_pype.analyze_and_update(collection='documents')
def analyze_document(doc_id, firestore_doc):
    return AnalysisResult(
        new_firestore_data={'status': 'completed'},  # Auto-updates Firestore
        confidence_score=0.95,
        processing_time=2.3
    )
```

### 📁 Automated File Processing

```python
@fire_pype.process_media(input_field='video_url', output_field='processed_url')
def process_video(doc_id, firestore_doc, local_input_path):
    output_path = generate_processed_video(local_input_path)
    return MediaProcessingResult(generated_files=[output_path])  # Auto-uploads to GCS
```

### 🧠 Intelligent Logging

```python
@fire_pype.pragmatic_logging(default_level='summary')
def complex_operation():
    fire_pype.log_step("Processing data")
    # Instead of 5+ log entries, you get ONE:
    # "Request abc123: Operation completed | Steps: 5 | Duration: 2.3s | Memory: 45MB"
```

### 🔄 Real-Time Pipeline Breadcrumbs

```python
@fire_pype.realtime_pipeline(pipeline_id='document_processing')
def process_document(doc_id):
    fire_pype.pipeline_step("📄 Document uploaded", metadata={'size': '2.3MB'})
    fire_pype.pipeline_step("🔍 OCR processing", metadata={'pages': 5})
    fire_pype.pipeline_step("✅ Validation complete", metadata={'status': 'approved'})
    # UI shows: "Document uploaded → OCR processing → Validation complete"
```

### 💾 Smart Caching Helpers

```python
def get_data(use_cache=True):
    if use_cache:
        # FirePype helper methods for caching
        cached_result = fire_pype.get_cached('api_results', key='data')
        if cached_result:
            return cached_result

    result = {'status': 'success'}  # Your actual logic here

    if use_cache:
        fire_pype.cache_result('api_results', key='data', data=result, ttl=3600)

    return result
```

### 🕰️ Time Machine Testing

```python
@fire_pype.log_process(enable_favoriting=True)
def process_user_data(user_id: str):
    # Run in production, favorite good runs in the UI
    # Fire-Pype automatically generates E2E tests from favorites!
    return complex_analysis(user_id)
```

### 📁 Pattern-Based File Routing

```python
@file_trigger(pattern="uploads/{department}/{user_id}_{type}.{ext}")
def process_upload(department, user_id, type, ext, file_data):
    # Automatically triggered based on file path patterns
    return route_by_type(type, file_data)
```

## 🖥️ Free Developer UI

Built-in web interface for monitoring and testing:

- **📊 Real-time Process Dashboard**
- **🔍 Automatic Endpoint Discovery**
- **🕰️ Event Replay Interface**
- **⭐ Test Suite Builder** (favorite runs → E2E tests)
- **🎯 Cross-Endpoint Testing** (test new features with real data)
- **🔄 Live Pipeline Breadcrumbs** (real-time processing progress)

## 🚀 Progressive Adoption

Start simple, grow as needed:

### 🟢 **Basic** (5 minutes)

```python
from flask import Flask
from fire_pype import FirePype

app = Flask(__name__)
fire_pype = FirePype(app, config={
    'firestore_project': 'your-project-id',
    'storage_bucket': 'your-bucket-name'
})

@app.route('/api/data')
def get_data(use_cache=True):
    if use_cache:
        cached_result = fire_pype.get_cached('api_results', key='data')
        if cached_result:
            return cached_result

    result = {'status': 'success'}  # Your actual logic here

    if use_cache:
        fire_pype.cache_result('api_results', key='data', data=result, ttl=3600)

    return result
```

### 🟡 **Intermediate** (Add logging & file processing)

```python
@app.route('/process')
@fire_pype.log_request(level='info')
@fire_pype.process_media(input_field='file_url')
def process_data(file_data):
    return {'processed': True}
```

### 🔴 **Advanced** (Full feature set)

```python
@fire_pype.analyze_and_update(collection='documents')
@fire_pype.pragmatic_logging(default_level='summary')
@fire_pype.log_process(enable_favoriting=True)
def enterprise_workflow(doc_id, firestore_doc):
    return complex_analysis(firestore_doc)
```

## 📦 Quick Start

```bash
pip install fire-pype
```

```python
from flask import Flask
from fire_pype import FirePype

app = Flask(__name__)
fire_pype = FirePype(app, config={
    'firestore_project': 'your-project-id',
    'storage_bucket': 'your-bucket-name',
    'enable_dev_ui': True,  # Free developer interface
    'enable_logging': True,
    'cache_ttl': 3600
})

@app.route('/api/data')
def get_data(use_cache=True):
    if use_cache:
        cached_result = fire_pype.get_cached('api_results', key='data')
        if cached_result:
            return cached_result

    result = {'status': 'success'}  # Your actual logic here

    if use_cache:
        fire_pype.cache_result('api_results', key='data', data=result, ttl=3600)

    return result
```

## 🎯 Why Fire-Pype?

| Feature             | Traditional Flask         | With Fire-Pype                                                           |
| ------------------- | ------------------------- | ------------------------------------------------------------------------ |
| **Caching**         | Manual Redis setup        | `fire_pype.get_cached()` + `fire_pype.cache_result()` (FirePype methods) |
| **File Processing** | Complex GCS logic         | `@fire_pype.process_media()`                                             |
| **Logging**         | Thousands of tiny logs    | One meaningful log per request                                           |
| **Testing**         | Manual test data creation | Auto-generate from production runs                                       |
| **Documentation**   | Manual API docs           | Automatic endpoint discovery                                             |

## 🔄 Advanced Features

### Dynamic Configuration

```python
@app.route('/process/<tenant_id>')
def process_for_tenant(tenant_id):
    fire_pype.set_config({
        'firestore_project': f'tenant-{tenant_id}-project',
        'storage_bucket': f'tenant-{tenant_id}-bucket',
        'cache_ttl': 1800
    })
    return process_data()
```

### Atomic Pub/Sub Management

Prevents costly retry loops with automatic dead-letter queues and retry counting.

### Dynamic Event Rerunability

Replay any past event with full context - perfect for debugging and analysis.

### Test Collections

Tag and organize tests: `#user-auth`, `#payment-processing`, `#media-uploads`

## 🏢 Production Ready

- **Zero Configuration**: Works out of the box
- **Performance Optimized**: Built for Flask's request-response cycle
- **Scalable**: Handles any scale from startup to enterprise
- **Observable**: Built-in monitoring, logging, and debugging tools

## 🚀 What's Next?

- **Rate Limiting**: Built-in API rate limiting
- **Health Checks**: Automatic health check endpoints
- **Metrics Export**: Prometheus/OpenTelemetry integration
- **Plugin System**: Extensible architecture for custom decorators

---

**Start building better Flask apps today!** 🚀

_Built with ❤️ for the Flask community_
