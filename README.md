# lava-job-templates

Job Submit Example:

```bash
#!/bin/bash -x -e

# meta-data settings
export MACH_ARCH="arm"
#export DEVICE_TYPE="pico-imx7-ums"
export DEVICE_TYPE="pico-imx7"
export CONSOLE_DEVICE="ttymxc4"

# lava functional tests settings
export TEST_GIT_REPO="ssh://rdsw@10.88.88.8/volume1/internal_git/lava-functional-tests.git"
export TEST_DIRECTORY="tn-test-shell"
export TEST_DEFINITION="yocto-tests-basic.yaml"
export TEST_TITLE="yocto-qc-test-with-ums"

# download fileserver settings
export FILESERVER=10.88.88.12
export FILESERVER_PORT=8085
export FILESERVER_URL="http://${FILESERVER}:${FILESERVER_PORT}/"
export SDCARD_IMAGE_NAME=$(cat ./SDCARD_IMAGE_NAME) # "fsl-image-qt5-validation-imx-pico-imx7"

# using overlay job templates which boot DUT os, and then transfer overlay via wget
# Substitute with build info (nothing to do)
envsubst < u-boot-ums-template.yaml > lava_job.yaml

LAVA_JOB_DEFINITION=$(cat lava_job.yaml)

# squad settings
SQUAD_SERVER=10.88.88.12
SQUAD_SERVER_PORT=8082
SQUAD_LAVA_BACKEND=10.88.88.12:8080
SQUAD_TOKEN=fb5ff1c7e4425a89b61859cab995137eacb133e4
SQUAD_GROUP=TechNexion
SQUAD_PROJECT=yocto-sumo
VERSION=${BUILD_NUMBER}

# Submit to squad
curl \
    --header "Auth-Token: ${SQUAD_TOKEN}" \
    --form backend=${SQUAD_LAVA_BACKEND} \
    --form definition="${LAVA_JOB_DEFINITION}" \
    http://${SQUAD_SERVER}:${SQUAD_SERVER_PORT}/api/submitjob/${SQUAD_GROUP}/${SQUAD_PROJECT}/${VERSION}/jenkins

```

