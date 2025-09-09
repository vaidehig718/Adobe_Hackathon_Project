FROM python:3.10

WORKDIR /app

# Copy requirements first (better caching)
COPY Challenge_1b/requirements.txt .

ENV PIP_DEFAULT_TIMEOUT=300
RUN pip install --no-cache-dir -r requirements.txt

# Copy only Challenge_1b code
COPY Challenge_1b/ .

# Pre-download the Hugging Face model
RUN python -c "from sentence_transformers import SentenceTransformer; SentenceTransformer('sentence-transformers/all-MiniLM-L6-v2')"

ENTRYPOINT ["python", "main_pipeline.py"]
