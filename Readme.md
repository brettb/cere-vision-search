This project will be an introduction to creating a full featured image search and tagging application. To keep things simple, we will start with a Computer Vision model served by a locally hosted Ollama or an OpenAi compatible server and a Chroma Vector Store.


## Prerequisites

-   **Python 3.10+**: Ensure Python is installed on your system.
-   **Ollama**: You have a choice to install with pip via the requirements.txt, install locally with a binary downloaded from the [Ollama website](https://ollama.com/download) or as a Docker container. I will also host a copy on my laptop that will be available when I am.

-   **ChromaDB**: ChromaDB will be installed as a Python package via pip or you can install with Docker.

## Installation

1. **Clone the repository:**

    ```bash
    https://github.com/brettb/cere-vision-search.git
    cd 
    ```

2. Create and activate a virtual environment (recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   
   ```
3.   **Install Python dependencies:** (Be sure to comment out Ollama if you dont want to use it locally installed via Python)
     Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
   
  To optionally run Chroma via Docker you can build the container with the following command 
  'sudo docker compose up -d --build'. Otherwise Chroma will run on the local ststem and utilize the launch directory to store a local copy of the database.
  
  . **Pull and Start the Ollama model:**

    Ensure the Llama 3.2 Vision model is running in Ollama. You may need to pull and start the model using Ollama's command-line interface. 

    Download the installer from [here](https://github.com/ollama/ollama) and install Ollama.

    Pull the llava-llama3:latest Vision model:

    ```bash
    ollama pull llava-llama3:latest
    ```

## Usage

1. **Run the FastAPI backend:**

    ```bash
    uvicorn main:app --host 127.0.0.1 --port 8001
    ```

    This will start the server on `http://127.0.0.1:8001`.

2. **Access the web interface:**

    Open your web browser and navigate to `http://127.0.0.1:8001`.

3. **Select a folder:**

    Enter the path to the folder containing your images and click "Open Folder". The application will scan the folder and display the found images.

    The first time you open a folder, the application will scan through all the images and process them and initialize the vector database (ChromaDB might also download an embedding model). This might take a while depending on your network speed and the number of images in the folder.

4. **Process images:**

    -   **Process All**: Click the "Process All" button to start tagging all unprocessed images. The progress will be displayed on the screen.
    -   **Process Individual Images**: Click the "Process Image" button in the image modal for a specific image to process it individually.

5. **Search images:**

    Enter your search query in the search bar and click "Search". The application will display images matching your query.

6. **Refresh images:**

    When new images are added to the folder, you can click the "Refresh" button to rescan the folder and update the image list.

  
  
