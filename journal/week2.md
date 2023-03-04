# Week 2 — Distributed Tracing

## Observaility
## Distributed Tracing
###HoneyComb

We will follow the instruction to install these files:

We will add these to the  ```requirements txt```

```
opentelemetry-api 
opentelemetry-sdk 
opentelemetry-exporter-otlp-proto-http 
opentelemetry-instrumentation-flask 
opentelemetry-instrumentation-requests

```

Adding these to ```app.py```
   
```
   from opentelemetry import trace
   from opentelemetry.instrumentation.flask import FlaskInstrumentor
   from opentelemetry.instrumentation.requests import RequestsInstrumentor
   from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
   from opentelemetry.sdk.trace import TracerProvider
   from opentelemetry.sdk.trace.export import BatchSpanProcessor
   from opentelemetry.sdk.trace.export import ConsoleSpanExporter, SimpleSpanProcessor
```

```
# Initialize tracing and an exporter that can send data to Honeycomb
provider = TracerProvider()
processor = BatchSpanProcessor(OTLPSpanExporter())
provider.add_span_processor(processor)
trace.set_tracer_provider(provider)
tracer = trace.get_tracer(__name__)
```

```
# Initialize automatic instrumentation with Flask
app = Flask(__name__)
FlaskInstrumentor().instrument_app(app)
RequestsInstrumentor().instrument()
```
