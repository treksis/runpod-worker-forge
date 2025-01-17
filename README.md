# Stable Diffusion WebUI Forge | RunPod Serverless Worker

This is the source code for a [RunPod](https://runpod.io?ref=2xxro4sy)
Serverless worker that uses the [Stable Diffusion WebUI Forge API](
https://github.com/lllyasviel/stable-diffusion-webui-forge) for inference.

## Model

The model(s) for inference will be loaded from a RunPod
Network Volume.

## Extensions

This worker includes the following extensions:

1. ControlNet (built-in)
2. [ReActor](https://github.com/Gourieff/sd-webui-reactor)

## Known Issues

1. The SDXL Refiner model seems to be causing intermittent issues and has
   been removed.
2. After Detailer is not currently compatible with Forge and has been removed.

## Testing

1. [Local Testing](docs/testing/local.md)
2. [RunPod Testing](docs/testing/runpod.md)

## Installing, Building and Deploying the Serverless Worker

1. [Install Stable Diffusion WebUI Forge on your Network Volume](
docs/installing.md)
2. [Building the Docker image](docs/building.md)
3. [Deploying on RunPod Serveless](docs/deploying.md)
4. [Frequently Asked Questions](docs/faq.md)

## RunPod API Endpoint

You can send requests to your RunPod API Endpoint using the `/run`
or `/runsync` endpoints.

Requests sent to the `/run` endpoint will be handled asynchronously,
and are non-blocking operations.  Your first response status will always
be `IN_QUEUE`.  You need to send subsequent requests to the `/status`
endpoint to get further status updates, and eventually the `COMPLETED`
status will be returned if your request is successful.

Requests sent to the `/runsync` endpoint will be handled synchronously
and are blocking operations.  If they are processed by a worker within
90 seconds, the result will be returned in the response, but if
the processing time exceeds 90 seconds, you will need to handle the
response and request status updates from the `/status` endpoint until
you receive the `COMPLETED` status which indicates that your request
was successful.

### RunPod API Examples

#### Stable Diffusion WebUI Forge APIs

* [Get ControlNet Models](docs/api/forge/get-controlnet-models.md)
* [Get Embeddings](docs/api/forge/get-embeddings.md)
* [Get Face Restorers](docs/api/forge/get-face-restorers.md)
* [Get Hypernetworks](docs/api/forge/get-hypernetworks.md)
* [Get Loras](docs/api/forge/get-loras.md)
* [Get Memory](docs/api/forge/get-memory.md)
* [Get Models](docs/api/forge/get-models.md)
* [Get Options](docs/api/forge/get-options.md)
* [Get Prompt Styles](docs/api/forge/get-prompt-styles.md)
* [Get Real-ESRGAN Models](docs/api/forge/get-realesrgan-models.md)
* [Get Samplers](docs/api/forge/get-samplers.md)
* [Get Script Info](docs/api/forge/get-script-info.md)
* [Get Scripts](docs/api/forge/get-scripts.md)
* [Get Upscalers](docs/api/forge/get-upscalers.md)
* [Get VAE](docs/api/forge/get-vae.md)
* [Image to Image](docs/api/forge/img2img.md)
* [Image to Image with ControlNet](docs/api/forge/img2img-controlnet.md)
* [Interrogate](docs/api/forge/interrogate.md)
* [Refresh Checkpoints](docs/api/forge/refresh-checkpoints.md)
* [Refresh Loras](docs/api/forge/refresh-loras.md)
* [Set Model](docs/api/forge/set-model.md)
* [Set VAE](docs/api/forge/set-vae.md)
* [Text to Image](docs/api/forge/txt2img.md)
* [Text to Image with ReActor](docs/api/forge/txt2img-reactor.md)
* [Text to Image with InstantID](docs/api/forge/txt2img-instantid.md)

#### Helper APIs

* [File Download](docs/api/helper/download.md)
* [Huggingface Sync](docs/api/helper/sync.md)

### Optional Webhook Callbacks

You can optionally [Enable a Webhook](docs/api/helper/webhook.md).

### Endpoint Status Codes

| Status      | Description                                                                                                                     |
|-------------|---------------------------------------------------------------------------------------------------------------------------------|
| IN_QUEUE    | Request is in the queue waiting to be picked up by a worker.  You can call the `/status` endpoint to check for status updates.  |
| IN_PROGRESS | Request is currently being processed by a worker.  You can call the `/status` endpoint to check for status updates.             |
| FAILED      | The request failed, most likely due to encountering an error.                                                                   |
| CANCELLED   | The request was cancelled.  This usually happens when you call the `/cancel` endpoint to cancel the request.                    |
| TIMED_OUT   | The request timed out.  This usually happens when your handler throws some kind of exception that does return a valid response. |
| COMPLETED   | The request completed successfully and the output is available in the `output` field of the response.                           |

## Serverless Handler

The serverless handler (`rp_handler.py`) is a Python script that handles
the API requests to your Endpoint using the [runpod](https://github.com/runpod/runpod-python)
Python library.  It defines a function `handler(event)` that takes an
API request (event), runs the inference using the model(s) from your
Network Volume with the `input`, and returns the `output`
in the JSON response.

## Acknowledgements

- [Stable Diffusion WebUI Forge](https://github.com/lllyasviel/stable-diffusion-webui-forge)
- [Generative Labs YouTube Tutorials](https://www.youtube.com/@generativelabs)

## Additional Resources

- [Postman Collection for this Worker](RunPod_Forge_Worker.postman_collection.json)
- [Generative Labs YouTube Tutorials](https://www.youtube.com/@generativelabs)
- [Getting Started With RunPod Serverless](https://trapdoor.cloud/getting-started-with-runpod-serverless/)
- [Serverless | Create a Custom Basic API](https://blog.runpod.io/serverless-create-a-basic-api/)

## Community and Contributing

Pull requests and issues on [GitHub](https://github.com/ashleykleynhans/runpod-worker-forge)
are welcome. Bug fixes and new features are encouraged.

You can contact me and get help with deploying your Serverless
worker to RunPod on the RunPod Discord Server below,
my username is **ashleyk**.

<a target="_blank" href="https://discord.gg/pJ3P2DbUUq">![Discord Banner 2](https://discordapp.com/api/guilds/912829806415085598/widget.png?style=banner2)</a>

## Appreciate my work?

<a href="https://www.buymeacoffee.com/ashleyk" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>
