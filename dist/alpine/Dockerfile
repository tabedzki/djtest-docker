ARG BASE_IMAGE_HASH
ARG PY_VER

FROM datajoint/djbase:py${PY_VER}-alpine-${BASE_IMAGE_HASH} as jup_build
USER dja:anaconda
COPY --chown=dja:anaconda ./config/pip_requirements.txt /tmp/
RUN \
    umask u+rwx,g+rwx,o-rwx && \
    export PY_VER=$(python -c "import sys;print(sys.version)" | \
        head -1 | awk '{print $1}') && \
    /entrypoint.sh echo "Requirements updated..." && \
    rm $PIP_REQUIREMENTS && \
    conda clean -ya && \
    find /opt/conda/conda-meta -user dja -exec chmod u+rwx,g+rwx,o-rwx "{}" \; && \
    chmod -R o+w /home/dja/.cache

# #Squashed Final Image
FROM scratch
COPY --from=jup_build / /
RUN chmod 4755 /startup && /startup -user=dja
LABEL maintainerName="Raphael Guzman" \
      maintainerEmail="raphael@vathes.com" \
      maintainerCompany="DataJoint"
USER dja:anaconda
ENV HOME /home/dja
ENV LANG C.UTF-8
ENV APK_REQUIREMENTS /tmp/apk_requirements.txt
ENV PIP_REQUIREMENTS /tmp/pip_requirements.txt
ENV CONDA_REQUIREMENTS /tmp/conda_requirements.txt
ENV PATH "/home/dja/.local/bin:/opt/conda/bin:$PATH"
ENTRYPOINT ["/entrypoint.sh"]
WORKDIR /main
VOLUME /tmp/.X11-unix
CMD ["/bin/sh", "-l"]