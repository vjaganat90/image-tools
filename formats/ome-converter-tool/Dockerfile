FROM polusai/bfio:2.5.0

# Trust current CAs: refresh the system bundle (OpenSSL) and certifi (requests).
RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates \
    && rm -rf /var/lib/apt/lists/* \
    && pip3 install --no-cache-dir --upgrade certifi

# bfio starts its Bio-Formats JVM through scyjava, which fetches a JRE and Maven with
# cjdk into $HOME/.cache/cjdk. Fetch them once into a shared cache whose index never
# expires, so a run as any user with any HOME (a CWL runner's) needs no network.
ENV CJDK_CACHE_DIR=/opt/cjdk CJDK_INDEX_TTL=inf
RUN python3 -c 'import bfio; bfio.start()' && chmod -R a+rwX /opt/cjdk

# environment variables defined in polusai/bfio
ENV EXEC_DIR="/opt/executables"
ENV POLUS_IMG_EXT=".ome.tif"
ENV POLUS_TAB_EXT=".csv"
ENV POLUS_LOG="INFO"
ENV NUM_THREADS=2
ENV NUM_WORKERS=4

# Work directory defined in the base container
WORKDIR ${EXEC_DIR}

# TODO: Change the tool_dir to the tool directory
ENV TOOL_DIR="formats/ome-converter-tool"

# Copy the repository into the container
RUN mkdir image-tools
COPY . ${EXEC_DIR}/image-tools

# Install the tool
RUN pip3 install "${EXEC_DIR}/image-tools/${TOOL_DIR}" --no-cache-dir

# Set the entrypoint
# TODO: Change the entrypoint to the tool entrypoint
ENTRYPOINT ["python3", "-m", "polus.images.formats.ome_converter"]
CMD ["--help"]
