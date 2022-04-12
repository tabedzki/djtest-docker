ARG BASE_IMAGE_HASH
ARG PY_VER
FROM datajoint/djbase:py${PY_VER}-debian-${BASE_IMAGE_HASH} as test_build

LABEL maintainerName="DataJoint Contributors" \
	maintainerEmail="service-health@datajoint.com" \
	maintainerCompany="DataJoint"
COPY --chown=anaconda:anaconda ./config/pip_requirements.txt /tmp/
RUN \
	umask u+rwx,g+rwx,o-rwx && \
	/entrypoint.sh echo "Requirements updated..." && \
	rm $PIP_REQUIREMENTS && \
	# Clean up
	find /opt/conda/conda-meta -user anaconda -exec chmod u+rwx,g+rwx,o-rwx "{}" \; && \
	conda clean -ya


# Squashed Final Image
FROM scratch
COPY --from=test_build / /
LABEL maintainerName="DataJoint Contributors" \
	maintainerEmail="service-health@datajoint.com" \
	maintainerCompany="DataJoint"
USER anaconda:anaconda
# Add conda to path here for use in downstream dockerfiles
ENV PATH /opt/conda/bin:$PATH
ENV HOME /home/anaconda
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV APT_REQUIREMENTS /tmp/apt_requirements.txt
ENV PIP_REQUIREMENTS /tmp/pip_requirements.txt
ENV CONDA_REQUIREMENTS /tmp/conda_requirements.txt
ENTRYPOINT ["/entrypoint.sh"]
WORKDIR /main
VOLUME /tmp/.X11-unix
CMD ["bash"]
