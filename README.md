# How to use the SDK

Install the sdk 

```
pip install -i https://test.pypi.org/simple/ example-package-asifr-berhampore==0.0.1
```

Next you can import the package in your code

```
from example_package_asifr_berhampore.example_package_asifr_berhampore import TrafficProcessingSDK
```

Initialize the TrafficProcessingSDK

```
kafka_bootstrap_servers = "localhost:9092"
group_id = "traffic-processing-group"
sdk = TrafficProcessingSDK(kafka_bootstrap_servers, group_id)
```


You can now call sdk.proocess_request() method to send the data to the producer. It is recommended to create a middleware for that.

Example in Fast API:

```
@app.middleware("http")
async def some_middleware(request: Request, call_next):
    response = await call_next(request)
    response_body = [chunk async for chunk in response.body_iterator]
    response.body_iterator = iterate_in_threadpool(iter(response_body))
    print(f"response_body={response_body[0].decode()}")
    print(request.url.path)
    print(request.method)

    sdk.process_request(request.url.path, request.method, response_body[0].decode())
    return response
```
