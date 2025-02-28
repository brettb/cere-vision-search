This project introduces the development of a comprehensive image search and tagging application. The platform utilizes open-source local multimodal LLMs, such as the MiniCPM-V vision model and the Chroma Vector Database, to deliver high-performance and seamless image search capabilities.

**Features**

* **Folder Selection and Image Discovery**: Upon initial launch, the application prompts users to specify a directory, the application will then recursively scans the folder and its subfolders to identify images for tagging and embedding for similarity search
* **Image Indexing**: A JSON-based index is initialized to track images within the selected folder. This index is dynamically updated to reflect any new or deleted images.
* **Intelligent Tagging**: The application employs the MiniCPM-V CV model with Ollama to generate descriptive tags for each image. This includes identifying elements/styles, creating concise descriptions, and extracting any text present within the images.
* **Vector Database Storage**: Image metadata (path, tags, description, text content) is stored in a ChromaDB vector database for efficient vector search.
* **Natural Language Search**: Users can search for images using natural language queries. The application performs a hybrid full-text search and vector search on the stored metadata to retrieve relevant images.
* **User Interface**: A user-friendly web interface built with Tailwind CSS and Vue3 is provided for browsing and interacting with images.
* **Image Grid**: Images are displayed in a responsive grid layout.
* **Image Modal**: Clicking on a thumbnail opens a modal that displays the image along with its tags, description, and extracted text.
* **Progress Tracking**: Real-time progress is shown during batch processing of images, including the number of processed and failed images.

## Prerequisites

-   **Python 3.10+**: Ensure Python is installed on your system. You can install from: https://www.python.org/downloads/
-   **Ollama**: Install Ollama to run the MiniCPM-V model and others. You can choose an installer for your platform at: https://ollama.com/download
-   **ChromaDB**: ChromaDB will be installed as a Python package via pip.

## Installation

1. **Clone the repository:**

    ```bash
    git clone https://github.com/brettb/cere-vision-search.git
    cd cere-vision-search
    ```

2. Create and activate a virtual environment (recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install Python dependencies:**

    ```bash
    pip install -r requirements.txt
    ```

4. **Pull and Start the Ollama model:**

    Ensure the lminicpm-v model is running in Ollama. You may need to pull and start the model using Ollama's command-line interface. 

    Pull and run the Minicpm-v model:

    ```bash
    ollama run minicpm-v
    ```
Please experiment with other models to determine the perfect balance in acuracy and performance for your needs and system capabilities. I chose a mid-sized mode that performs great for our needs, you may find a smaller model or a larger model is better suited for your needs.

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

## Project Structure

-   `main.py`: Contains the FastAPI backend logic, including API endpoints for image processing, searching, and serving static files.
-   `image_processor.py`: Handles image processing using Ollama and updates the metadata.
-   `index.html`: The main HTML file for the frontend user interface with Tailwind CSS and Vue3.
-   `vector_db.py`: Handles the vector database (ChromaDB) operations.

## API Endpoints

- `GET /`: Serves the main web interface
- `POST /images`: Scans a folder for images and returns their metadata
- `GET /image/{path}`: Retrieves a specific image file
- `POST /search`: Performs hybrid (full-text + vector) search on images
- `POST /refresh`: Rescans the current folder for new or removed images
- `POST /process-image`: Processes a single image using Ollama to generate tags, description, and extract text
- `POST /update-metadata`: Updates metadata for a specific image
- `GET /check-init-status`: Checks if the vector database needs initialization