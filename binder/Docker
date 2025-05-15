FROM jupyter/voila

# Install only necessary Python packages
RUN pip install --no-cache-dir \
    voila \
    ipywidgets

# Copy your repo contents (including notebook)
COPY . /home/jovyan

# Set working directory
WORKDIR /home/jovyan

# Clean up to reduce image size
RUN rm -rf ~/.cache
