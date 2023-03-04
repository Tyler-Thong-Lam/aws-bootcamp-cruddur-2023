# Week 2 — Distributed Tracing

## Observaility
## Distributed Tracing

### HoneyComb

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
Then add the env var to the ```backend-flask``` in the ``` docker-compose```
```
OTEL_EXPORTER_OTLP_ENDPOINT: "https://api.honeycomb.io"
OTEL_EXPORTER_OTLP_HEADERS: "x-honeycomb-team=${HONEYCOMB_API_KEY}"
OTEL_SERVICE_NAME: "${HONEYCOMB_SERVICE_NAME}"

# Set up env var
export HONEYCOMB_API_KEY=""
export HONEYCOMB_SERVICE_NAME="Cruddur"
gp env HONEYCOMB_API_KEY=""
gp env HONEYCOMB_SERVICE_NAME="Cruddur"

```

### CloudWatch

We will use the watchtower as ana adapter between python system login and cloudwatch logs;  it uses python boto sdk , we can plug our app logs directly to cloudwatch

```
# Configuring Logger to Use CloudWatch
LOGGER = logging.getLogger(__name__)
LOGGER.setLevel(logging.DEBUG)
console_handler = logging.StreamHandler()
cw_handler = watchtower.CloudWatchLogHandler(log_group='cruddur')
LOGGER.addHandler(console_handler)
LOGGER.addHandler(cw_handler)
LOGGER.info("some message")
```

```
@app.after_request
def after_request(response):
    timestamp = strftime('[%Y-%b-%d %H:%M]')
    LOGGER.error('%s %s %s %s %s %s', timestamp, request.remote_addr, request.method, request.scheme, request.full_path, response.status)
    return response
```

We are going to add message in an API endpoint

```LOGGER.info('Hello Cloudwatch! from  /api/activities/home')```

Set the env var in for ```docker-compose.yml```

```
      AWS_DEFAULT_REGION: "${AWS_DEFAULT_REGION}"
      AWS_ACCESS_KEY_ID: "${AWS_ACCESS_KEY_ID}"
      AWS_SECRET_ACCESS_KEY: "${AWS_SECRET_ACCESS_KEY}"
```
[CloudWatch](![cloudwatch log2](https://user-images.githubusercontent.com/93460271/222882756-3f5cb3d4-5c24-49b4-8a44-8d42074cec77.png)

### RollBar

For this, I learn the new thing that we can use this tool to real time tracking and monitoring solution for developers

[Proof of RollBar](![rollbar ](https://user-images.githubusercontent.com/93460271/222883393-206bf57d-83e7-439a-a91b-e7b61f57a691.png)

After following the instruction from the video, I can obseve the output of the roll bar 

[Hello World-RollBar](![Rollbar-Helloworld](https://user-images.githubusercontent.com/93460271/222883459-7049ab91-870a-4941-9560-6a9883824d6e.png)

### AWS X-ray

With this service, We can analyze and debug the applications

[AWS X-ray](https://aws.amazon.com/xray/)


[AWS Xray Architecture](![aws xray architecture](https://user-images.githubusercontent.com/93460271/222883870-d3a6f0bd-e06e-47f3-b75c-8248673b2e30.png)

[](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html)











