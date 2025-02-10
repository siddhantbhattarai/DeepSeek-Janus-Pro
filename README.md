# How to Use DeepSeek Janus-Pro Locally
Learn how to set up the DeepSeek Janus-Pro project, build your own Docker image, and run the Janus web application locally on your laptop.

## Contents
1. What is the DeepSeek Janus-Series?
2. Setting up the Janus Project
3. Building and Running the Docker Image
4. Testing the Janus Pro Model
   - Testing the Multimodal Understanding
   - Testing Text-to-Image Generation
5. Conclusion

---

### What is the DeepSeek Janus-Series?
DeepSeek, a Chinese AI startup, has disrupted the global AI industry, shaking major tech giants with its state-of-the-art models in text generation, reasoning, vision models, and image generation. The Janus-Series represents its cutting-edge multimodal models designed for advanced visual and text-based tasks.

#### **1. Janus**
Janus is an autoregressive framework that separates visual encoding into distinct pathways for understanding and generation tasks. It uses a unified transformer architecture to achieve superior performance on multimodal tasks.

#### **2. JanusFlow**
JanusFlow incorporates rectified flow generative modeling with autoregressive language modeling, offering superior performance on benchmarks compared to other vision-language models.

#### **3. Janus-Pro**
Janus-Pro is the most advanced version of the series, optimized with better training strategies, expanded datasets, and improved multimodal understanding and text-to-image generation capabilities.

> **Read More:** DeepSeek's Janus-Pro: Features, DALL-E 3 Comparison & More.

---

### Setting up the Janus Project
To set up and run Janus-Pro locally, we will modify the original code, build our own Docker image, and deploy the model using Docker Desktop.

#### **Step 1: Install Docker Desktop**
Download and install the latest version of Docker Desktop from the [official Docker website](https://www.docker.com/).

##### **Note for Windows Users:**
If you are on Windows, install the Windows Subsystem for Linux (WSL) by running the following command in your terminal:

```bash
wsl --install
```

#### **Step 2: Clone the Janus Repository**
Clone the Janus repository from GitHub and navigate to the project directory:

```bash
git clone https://github.com/deepseek-ai/Janus.git
cd Janus
```

#### **Step 3: Modify the Demo Code**
Navigate to the demo folder and open the file `app_januspro.py` in your preferred code editor.

##### **Changes to Make:**
1. **Model Name:** Replace `deepseek-ai/Janus-Pro-7B` with `deepseek-ai/Janus-Pro-1B` to load a lighter version (4.1 GB), suitable for local use.
2. **Update the Demo Launch:** Modify the last line to ensure compatibility with the Docker URL and port:

```python
demo.queue(concurrency_count=1, max_size=10).launch(
    server_name="0.0.0.0", server_port=7860
)
```

#### **Step 4: Create a Docker Image**
Create a Dockerfile in the root directory of the project with the following content:

```dockerfile
# Use the PyTorch base image
FROM pytorch/pytorch:latest

# Set the working directory inside the container
WORKDIR /app

# Copy the current directory into the container
COPY . /app

# Install necessary Python packages
RUN pip install -e .[gradio]

# Set the entrypoint for the container to launch your Gradio app
CMD ["python", "demo/app_januspro.py"]
```

##### **Dockerfile Breakdown:**
- Uses the PyTorch base image.
- Sets the working directory inside the container.
- Copies project files to `/app`.
- Installs required dependencies.
- Launches the Gradio application.

---

### Building and Running the Docker Image

#### **Step 1: Build the Docker Image**
Run the following command in the terminal to build the Docker image:

```bash
docker build -t janus .
```

Building the image can take 10 to 15 minutes, depending on your internet speed.

#### **Step 2: Run the Docker Container**
Start the container with GPU support and port mapping:

```bash
docker run -it -p 7860:7860 -d -v huggingface:/root/.cache/huggingface -w /app --gpus all --name janus janus:latest
```

Open Docker Desktop and navigate to the **Containers** tab to confirm that the Janus container is running.

##### **Checking Logs:**
Click on the Janus container and navigate to the **Logs** tab. The container will download the model file from the Hugging Face Hub. Once complete, the logs will indicate that the application is running.

#### **Access the Application:**
Visit the following URL in your browser to access the Janus web application:

```
http://localhost:7860/
```

> **Note:** If you face issues, refer to the updated version of the Janus project at [kingabzpro/Janus](https://github.com/kingabzpro/Janus).

---

### Testing the Janus Pro Model
The web application has a clean interface and functions smoothly. Let's test the multimodal understanding and text-to-image generation capabilities.

#### **Testing the Multimodal Understanding**
1. Upload an image containing text and graphical elements.
2. Ask the model to describe the content and analyze details.

#### **Testing Text-to-Image Generation**
1. Provide a text prompt, such as "A futuristic city skyline during sunset."
2. Observe the image generated by the Janus Pro model.

Both features demonstrate impressive results, making Janus-Pro a strong contender for advanced AI tasks.

---

### Conclusion
DeepSeek's Janus-Pro is a groundbreaking multimodal model that redefines AI capabilities in visual understanding and text-to-image generation. By setting up the project locally and leveraging Docker, users can harness its powerful features seamlessly.

In this tutorial, we explored the Janus series, set up the Janus-Pro project, built a Docker image, and tested the model's multimodal capabilities.

> **Explore More:** DeepSeek Janus-Pro: Features, DALL-E 3 Comparison & More.

