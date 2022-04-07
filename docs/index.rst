Documentation for the DataJoint's DJTest Image
#############################################

| A base docker image for running tests related to DataJoint.
| For more details, have a look at `prebuilt images <https://hub.docker.com/r/datajoint/djtest>`_, `source <https://github.com/datajoint/djtest-docker>`_, and `documentation <https://datajoint.github.io/djtest-docker>`_.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

Launch Locally
**************

Debian
======
.. code-block:: shell

   docker-compose -f dist/debian/docker-compose.yaml --env-file config/.env up --build

Alpine
======
.. code-block:: shell

   docker-compose -f dist/alpine/docker-compose.yaml --env-file config/.env up --build

Features
********

- Adds testing libraries:
  - nose
  - nose-cov
  - coveralls
  - flake8
  - black
- Applies image compresssion

Usage Notes
***********

- 

Testing
*******

To rebuild and run tests locally, execute the following statements:

.. code-block:: shell

   set -a  # automatically export sourced variables
   . config/.env  # source config for build and tests
   docker buildx bake -f dist/${DISTRO}/docker-compose.yaml --set *.platform=${PLATFORM} --set *.context=. --load  # build image
   tests/main.sh  # run tests
   set +a  # disable auto-export behavior for sourced variables

Base Image
**********

Build is a child of `datajoint/djbase <https://datajoint.github.io/djbase-docker/>`_.