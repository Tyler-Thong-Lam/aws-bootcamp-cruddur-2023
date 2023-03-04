# Week 2 — Distributed Tracing

## Observaility
## Distributed Tracing
###HoneyComb
  
   We will follow the instruction to install these files:
   We will add these to the  ```requirements txt```
   
```
   from opentelemetry import trace
   from opentelemetry.instrumentation.flask import FlaskInstrumentor
   from opentelemetry.instrumentation.requests import RequestsInstrumentor
   from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
   from opentelemetry.sdk.trace import TracerProvider
   from opentelemetry.sdk.trace.export import BatchSpanProcessor
   from opentelemetry.sdk.trace.export import ConsoleSpanExporter, SimpleSpanProcessor
```
   
