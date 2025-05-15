FROM jupyter/base-notebook

# Install Voila and ipywidgets
RUN pip install --no-cache-dir voila ipywidgets

# Set working directory (must come *before* COPY for permissions to be correct)
WORKDIR /home/jovyan

# Copy all repo contents into working directory
COPY . .

# Set permissions (optional but good practice for mounted volumes or CI/CD)
USER root
RUN chown -R jovyan:users /home/jovyan
USER jovyan

# Clean up pip cache (more effective)
RUN rm -rf /home/jovyan/.cache /home/jovyan/.local/share/jupyter/lab/staging

# Default command to run Voila
CMD ["voila", "quiz.ipynb", "--port=8888", "--no-browser", "--Voila.base_url=/", "--Voila.trust_xheaders=True"]
