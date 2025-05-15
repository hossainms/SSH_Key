FROM jupyter/base-notebook

# Install Voila and ipywidgets
RUN pip install --no-cache-dir voila ipywidgets

# Copy all repo contents
COPY . /home/jovyan

# Set working directory
WORKDIR /home/jovyan

# Clean up cache to reduce image size
RUN rm -rf ~/.cache
