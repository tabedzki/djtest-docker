# DJTest

A base docker image for running tests related to DataJoint.

# Launch locally


`docker-compose -f dist/alpine/docker-compose.yml --env-file config/.env up --build`
OR
`docker-compose -f dist/debian/docker-compose.yml --env-file config/.env up --build`


# Notes

https://hub.docker.com/r/datajoint/djtest