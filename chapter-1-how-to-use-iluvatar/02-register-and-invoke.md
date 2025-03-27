# Simple Ping, Register, and Invoke Tutorial

In this tutorial, we will run a pre-built hello world image on Iluvatar and interact with it via the HTTP API. For now, we will use the following image:
docker.io/alfuerst/hello-iluvatar-action



Later, we'll walk through building your own image and running it.

Confirm that your worker node is accessible by sending a simple ping request:
```bash
curl "http://127.0.0.1:8080/ping"
```

If the worker is running properly, you should receive a 'pong' response.

## Registering the Hello World Function
Next, register the hello world function using the pre-built image.

Run the following command to register the function:
```bash
curl -X PUT http://127.0.0.1:8080/register \
-H "Content-Type: application/json" \
-d '{
      "function_name": "hello",
      "version": "1",
      "image": "docker.io/alfuerst/hello-iluvatar-action",
      "memory": 512,
      "cpu": 1,
      "isolate": "docker",
      "compute": "CPU"
    }'
```
What does this mean?
- This command tells Iluvatar to register a function called hello (version 1) using the specified Docker image. 
- memory and cpu needed for your function
- 'isolate' tells iluvatar to run the image on a docker runtime(another option is containerd)
- 'compute' tells to run on a CPU(you can also have it as GPU for ML based functions)

If there are no errors, you should recieve {"ok": "function registered"}

## Invoking the Hello World Function

After successfully registering the function, you can invoke it using the HTTP API.
### Synchronous Invocation
Pass query parameters as key/value pairs. For example, to pass a parameter name:
```bash
curl "http://127.0.0.1:8080/invoke/<your-function-name>/<your-version>?name=Joe"
```
This will synchronously invoke the function and return the result if there are no errors.

### Asynchronous Invocation
For asynchronous execution, use the /async_invoke endpoint:
```bash
curl "http://127.0.0.1:8080/async_invoke/<your-function-name>/<your-version>?name=World"
```
This call returns a cookie string.
example output
```json
{
    "cookie": "FASD-31FA-FA2E-DFAS-2DAS"
}
```

You can check the result later by using:
```bash
curl "http://127.0.0.1:8080/invoke_async_check/<COOKIE_STRING>"
```

Once you’ve confirmed that the hello world image is working as expected, the next part of this tutorial will guide you through building your own custom function image using py2lambda and then running it on Iluvatar.