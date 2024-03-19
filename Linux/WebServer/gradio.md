
Gradio是一个开源Python包，能够为机器学习模型、API或者任意Python函数构建演示或Web应用程序

自动生成交互式Web页面，支持多种输入输出格式，同时还支持生成能外部网络访问的链接，能够让别人体验你的AI算法
- 自动生成可交互页面
- 代码简洁
- 自定义多种输入输出
- 支持生成可外部访问的链接并进行分享
## Installation

```bash
python>=3.8
pip install gradio
```
## Usage

###  class Interface

```python
@document("launch", "load", "from_pipeline", "integrate", "queue")
class Interface(Blocks):
    """
    Interface is Gradio's main high-level class, and allows you to create a web-based GUI / demo
    around a machine learning model (or any Python function) in a few lines of code.
    You must specify three parameters: (1) the function to create a GUI for (2) the desired input components and
    (3) the desired output components. Additional parameters can be used to control the appearance
    and behavior of the demo.

    Example:
        import gradio as gr

        def image_classifier(inp):
            return {'cat': 0.3, 'dog': 0.7}

        demo = gr.Interface(fn=image_classifier, inputs="image", outputs="label")
        demo.launch()
    Demos: hello_world, hello_world_3, gpt2_xl
    Guides: quickstart, key-features, sharing-your-app, interface-state, reactive-interfaces, advanced-interface-features, setting-up-a-gradio-demo-for-maximum-performance
    """


    def __init__(
        self,
        fn: Callable,
        inputs: str | Component | list[str | Component] | None,
        outputs: str | Component | list[str | Component] | None,
        examples: list[Any] | list[list[Any]] | str | None = None,
        cache_examples: bool | None = None,
        examples_per_page: int = 10,
        live: bool = False,
        title: str | None = None,
        description: str | None = None,
        article: str | None = None,
        thumbnail: str | None = None,
        theme: Theme | str | None = None,
        css: str | None = None,
        allow_flagging: str | None = None,
        flagging_options: list[str] | list[tuple[str, str]] | None = None,
        flagging_dir: str = "flagged",
        flagging_callback: FlaggingCallback | None = None,
        analytics_enabled: bool | None = None,
        batch: bool = False,
        max_batch_size: int = 4,
        api_name: str | Literal[False] | None = "predict",
        _api_mode: bool = False,
        allow_duplication: bool = False,
        concurrency_limit: int | None | Literal["default"] = "default",
        js: str | None = None,
        head: str | None = None,
        additional_inputs: str | Component | list[str | Component] | None = None,
        additional_inputs_accordion: str | Accordion | None = None,
        **kwargs,
    ):
        """
        Parameters:
            fn: the function to wrap an interface around. Often a machine learning model's prediction function. Each parameter of the function corresponds to one input component, and the function should return a single value or a tuple of values, with each element in the tuple corresponding to one output component.
            inputs: a single Gradio component, or list of Gradio components. Components can either be passed as instantiated objects, or referred to by their string shortcuts. The number of input components should match the number of parameters in fn. If set to None, then only the output components will be displayed.
            outputs: a single Gradio component, or list of Gradio components. Components can either be passed as instantiated objects, or referred to by their string shortcuts. The number of output components should match the number of values returned by fn. If set to None, then only the input components will be displayed.
            examples: sample inputs for the function; if provided, appear below the UI components and can be clicked to populate the interface. Should be nested list, in which the outer list consists of samples and each inner list consists of an input corresponding to each input component. A string path to a directory of examples can also be provided, but it should be within the directory with the python file running the gradio app. If there are multiple input components and a directory is provided, a log.csv file must be present in the directory to link corresponding inputs.
            cache_examples: If True, caches examples in the server for fast runtime in examples. If `fn` is a generator function, then the last yielded value will be used as the output. The default option in HuggingFace Spaces is True. The default option elsewhere is False.
            examples_per_page: If examples are provided, how many to display per page.
            live: whether the interface should automatically rerun if any of the inputs change.
            title: a title for the interface; if provided, appears above the input and output components in large font. Also used as the tab title when opened in a browser window.
            description: a description for the interface; if provided, appears above the input and output components and beneath the title in regular font. Accepts Markdown and HTML content.
            article: an expanded article explaining the interface; if provided, appears below the input and output components in regular font. Accepts Markdown and HTML content.
            thumbnail: path or url to image to use as display image when the web demo is shared on social media.
            theme: Theme to use, loaded from gradio.themes.
            css: custom css or path to custom css file to use with interface.
            analytics_enabled: Whether to allow basic telemetry. If None, will use GRADIO_ANALYTICS_ENABLED environment variable if defined, or default to True.
            batch: If True, then the function should process a batch of inputs, meaning that it should accept a list of input values for each parameter. The lists should be of equal length (and be up to length `max_batch_size`). The function is then *required* to return a tuple of lists (even if there is only 1 output component), with each list in the tuple corresponding to one output component.
            max_batch_size: Maximum number of inputs to batch together if this is called from the queue (only relevant if batch=True)
            api_name: defines how the endpoint appears in the API docs. Can be a string, None, or False. If set to a string, the endpoint will be exposed in the API docs with the given name. If None, the name of the prediction function will be used as the API endpoint. If False, the endpoint will not be exposed in the API docs and downstream apps (including those that `gr.load` this app) will not be able to use this event.
            allow_duplication: If True, then will show a 'Duplicate Spaces' button on Hugging Face Spaces.
            head: Custom html to insert into the head of the page. This can be used to add custom meta tags, scripts, stylesheets, etc. to the page.
           
        """
```

### class launch

```python
    def launch(
        self,
        inline: bool | None = None,
        inbrowser: bool = False,
        share: bool | None = None,
        debug: bool = False,
        max_threads: int = 40,
        auth: Callable | tuple[str, str] | list[tuple[str, str]] | None = None,
        auth_message: str | None = None,
        prevent_thread_lock: bool = False,
        show_error: bool = False,
        server_name: str | None = None,
        server_port: int | None = None,
        *,
        height: int = 500,
        width: int | str = "100%",
        favicon_path: str | None = None,
        ssl_keyfile: str | None = None,
        ssl_certfile: str | None = None,
        ssl_keyfile_password: str | None = None,
        ssl_verify: bool = True,
        quiet: bool = False,
        show_api: bool = True,
        allowed_paths: list[str] | None = None,
        blocked_paths: list[str] | None = None,
        root_path: str | None = None,
        app_kwargs: dict[str, Any] | None = None,
        state_session_capacity: int = 10000,
        share_server_address: str | None = None,
        share_server_protocol: Literal["http", "https"] | None = None,
        _frontend: bool = True,
    ) -> tuple[FastAPI, str, str]:
        """
        Launches a simple web server that serves the demo. Can also be used to create a
        public link used by anyone to access the demo from their browser by setting share=True.

        Parameters:
            inline: whether to display in the interface inline in an iframe. Defaults to True in python notebooks; False otherwise.
            inbrowser: whether to automatically launch the interface in a new tab on the default browser.
            share: whether to create a publicly shareable link for the interface. Creates an SSH tunnel to make your UI accessible from anywhere. If not provided, it is set to False by default every time, except when running in Google Colab. When localhost is not accessible (e.g. Google Colab), setting share=False is not supported.
            debug: if True, blocks the main thread from running. If running in Google Colab, this is needed to print the errors in the cell output.
            auth: If provided, username and password (or list of username-password tuples) required to access interface. Can also provide function that takes username and password and returns True if valid login.
            auth_message: If provided, HTML message provided on login page.
            prevent_thread_lock: If True, the interface will block the main thread while the server is running.
            show_error: If True, any errors in the interface will be displayed in an alert modal and printed in the browser console log
            server_port: will start gradio app on this port (if available). Can be set by environment variable GRADIO_SERVER_PORT. If None, will search for an available port starting at 7860.
            server_name: to make app accessible on local network, set this to "0.0.0.0". Can be set by environment variable GRADIO_SERVER_NAME. If None, will use "127.0.0.1".
            max_threads: the maximum number of total threads that the Gradio app can generate in parallel. The default is inherited from the starlette library (currently 40).
            width: The width in pixels of the iframe element containing the interface (used if inline=True)
            height: The height in pixels of the iframe element containing the interface (used if inline=True)
            favicon_path: If a path to a file (.png, .gif, or .ico) is provided, it will be used as the favicon for the web page.
            ssl_keyfile: If a path to a file is provided, will use this as the private key file to create a local server running on https.
            ssl_certfile: If a path to a file is provided, will use this as the signed certificate for https. Needs to be provided if ssl_keyfile is provided.
            ssl_keyfile_password: If a password is provided, will use this with the ssl certificate for https.
            ssl_verify: If False, skips certificate validation which allows self-signed certificates to be used.
            quiet: If True, suppresses most print statements.
            show_api: If True, shows the api docs in the footer of the app. Default True.
            allowed_paths: List of complete filepaths or parent directories that gradio is allowed to serve (in addition to the directory containing the gradio python file). Must be absolute paths. Warning: if you provide directories, any files in these directories or their subdirectories are accessible to all users of your app.
          
        Returns:
            app: FastAPI app object that is running the demo
            local_url: Locally accessible link to the demo
            share_url: Publicly accessible link to the demo (if share=True, otherwise None)
        Example: (Blocks)
            import gradio as gr
            def reverse(text):
                return text[::-1]
            with gr.Blocks() as demo:
                button = gr.Button(value="Reverse")
                button.click(reverse, gr.Textbox(), gr.Textbox())
            demo.launch(share=True, auth=("username", "password"))
        Example:  (Interface)
            import gradio as gr
            def reverse(text):
                return text[::-1]
            demo = gr.Interface(reverse, "text", "text")
            demo.launch(share=True, auth=("username", "password"))
        """
```

### ChatInterface
```python
class ChatInterface(Blocks):
    """
    ChatInterface is Gradio's high-level abstraction for creating chatbot UIs, and allows you to create
    a web-based demo around a chatbot model in a few lines of code. Only one parameter is required: fn, which
    takes a function that governs the response of the chatbot based on the user input and chat history. Additional
    parameters can be used to control the appearance and behavior of the demo.

    Example:
        import gradio as gr

        def echo(message, history):
            return message

        demo = gr.ChatInterface(fn=echo, examples=["hello", "hola", "merhaba"], title="Echo Bot")
        demo.launch()
    Demos: chatinterface_random_response, chatinterface_streaming_echo
    Guides: creating-a-chatbot-fast, sharing-your-app
    """

    def __init__(
        self,
        fn: Callable,
        *,
        chatbot: Chatbot | None = None,
        textbox: Textbox | None = None,
        additional_inputs: str | Component | list[str | Component] | None = None,
        additional_inputs_accordion_name: str | None = None,
        additional_inputs_accordion: str | Accordion | None = None,
        examples: list[str] | None = None,
        cache_examples: bool | None = None,
        title: str | None = None,
        description: str | None = None,
        theme: Theme | str | None = None,
        css: str | None = None,
        analytics_enabled: bool | None = None,
        submit_btn: str | None | Button = "Submit",
        stop_btn: str | None | Button = "Stop",
        retry_btn: str | None | Button = "🔄  Retry",
        undo_btn: str | None | Button = "↩️ Undo",
        clear_btn: str | None | Button = "🗑️  Clear",
        autofocus: bool = True,
        concurrency_limit: int | None | Literal["default"] = "default",
    ):
        """
        Parameters:
            fn: the function to wrap the chat interface around. Should accept two parameters: a string input message and list of two-element lists of the form [[user_message, bot_message], ...] representing the chat history, and return a string response. See the Chatbot documentation for more information on the chat history format.
            chatbot: an instance of the gr.Chatbot component to use for the chat interface, if you would like to customize the chatbot properties. If not provided, a default gr.Chatbot component will be created.
            textbox: an instance of the gr.Textbox component to use for the chat interface, if you would like to customize the textbox properties. If not provided, a default gr.Textbox component will be created.
            additional_inputs: an instance or list of instances of gradio components (or their string shortcuts) to use as additional inputs to the chatbot. If components are not already rendered in a surrounding Blocks, then the components will be displayed under the chatbot, in an accordion.
            additional_inputs_accordion_name: Deprecated. Will be removed in a future version of Gradio. Use the `additional_inputs_accordion` parameter instead.
            additional_inputs_accordion: If a string is provided, this is the label of the `gr.Accordion` to use to contain additional inputs. A `gr.Accordion` object can be provided as well to configure other properties of the container holding the additional inputs. Defaults to a `gr.Accordion(label="Additional Inputs", open=False)`. This parameter is only used if `additional_inputs` is provided.
            examples: sample inputs for the function; if provided, appear below the chatbot and can be clicked to populate the chatbot input.
            cache_examples: If True, caches examples in the server for fast runtime in examples. The default option in HuggingFace Spaces is True. The default option elsewhere is False.
            title: a title for the interface; if provided, appears above chatbot in large font. Also used as the tab title when opened in a browser window.
            description: a description for the interface; if provided, appears above the chatbot and beneath the title in regular font. Accepts Markdown and HTML content.
            theme: Theme to use, loaded from gradio.themes.
            css: custom css or path to custom css file to use with interface.
            analytics_enabled: Whether to allow basic telemetry. If None, will use GRADIO_ANALYTICS_ENABLED environment variable if defined, or default to True.
            submit_btn: Text to display on the submit button. If None, no button will be displayed. If a Button object, that button will be used.
            stop_btn: Text to display on the stop button, which replaces the submit_btn when the submit_btn or retry_btn is clicked and response is streaming. Clicking on the stop_btn will halt the chatbot response. If set to None, stop button functionality does not appear in the chatbot. If a Button object, that button will be used as the stop button.
            retry_btn: Text to display on the retry button. If None, no button will be displayed. If a Button object, that button will be used.
            undo_btn: Text to display on the delete last button. If None, no button will be displayed. If a Button object, that button will be used.
            clear_btn: Text to display on the clear button. If None, no button will be displayed. If a Button object, that button will be used.
            autofocus: If True, autofocuses to the textbox when the page loads.
            concurrency_limit: If set, this is the maximum number of chatbot submissions that can be running simultaneously. Can be set to None to mean no limit (any number of chatbot submissions can be running simultaneously). Set to "default" to use the default concurrency limit (defined by the `default_concurrency_limit` parameter in `.queue()`, which is 1 by default).
        """
```

### demo

#### text

```python
import gradio as gr

def greet(name, intensity):
    return "Hello " * intensity + name + "!"

demo = gr.Interface(
    fn=greet,
    inputs=["text", "slider"],
    outputs=["text"],
)

demo.launch(share=True)

```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240117121836.png)

#### image

```python
import gradio as gr
import torch
from torchvision import transforms
import requests
from PIL import Image
model = torch.hub.load('pytorch/vision:v0.6.0', 'resnet18', pretrained=True).eval()

# Download human-readable labels for ImageNet.
response = requests.get("https://git.io/JJkYN")
labels = response.text.split("\n")

def predict(inp):
  inp = Image.fromarray(inp.astype('uint8'), 'RGB')
  inp = transforms.ToTensor()(inp).unsqueeze(0)
  with torch.no_grad():
    prediction = torch.nn.functional.softmax(model(inp)[0], dim=0)
  return {labels[i]: float(prediction[i]) for i in range(1000)}
# 使用 Gr.Interface 的新方式创建输入和输出
inputs = gr.Image()
outputs = gr.Label(num_top_classes=3)
gr.Interface(fn=predict, inputs=inputs, outputs=outputs).launch(share=True)
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240117144659.png)


#### file

向服务器上传文件
```python

import os
import requests
import gradio as gr
import shutil
import tempfile

url = "http://39.105.47.197:8000/uploadfile/"

def generate_file(file_obj):

    # 返回新文件的的地址（注意这里）
    files = {"file": (file_obj, open(file_obj, "rb"))}

    # 使用 requests 发送文件到服务器
    response = requests.post(url, files=files)

    return f"File {file_obj} uploaded successfully!"

# 创建 Gradio 界面
iface = gr.Interface(fn=generate_file, inputs="file", outputs="text", live=True)

# 启动 Gradio 服务器
iface.launch(share=True)
```

