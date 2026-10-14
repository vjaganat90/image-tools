FROM python:3.13-slim

# Trust current CAs: refresh the system bundle (OpenSSL) and certifi (requests).
RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates \
    && rm -rf /var/lib/apt/lists/* \
    && pip3 install --no-cache-dir --upgrade certifi

# environment variables defined in polusai/bfio
ENV EXEC_DIR="/opt/executables"
ENV POLUS_IMG_EXT=".ome.tif"
ENV POLUS_TAB_EXT=".csv"
ENV POLUS_LOG="INFO"

# Work directory defined in the base container
WORKDIR ${EXEC_DIR}

# When building from repo root: -f utils/bbbc-download-plugin/Dockerfile .
COPY utils/bbbc-download-plugin/pyproject.toml ${EXEC_DIR}
COPY utils/bbbc-download-plugin/VERSION ${EXEC_DIR}
COPY utils/bbbc-download-plugin/README.md ${EXEC_DIR}
COPY utils/bbbc-download-plugin/src ${EXEC_DIR}/src

RUN pip3 install ${EXEC_DIR} --no-cache-dir

ENTRYPOINT ["python3", "-m", "polus.plugins.utils.bbbc_download"]
CMD ["--help"]
