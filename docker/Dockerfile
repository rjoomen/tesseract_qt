ARG TAG
FROM ghcr.io/tesseract-robotics/tesseract_planning:${TAG}

SHELL ["/bin/bash", "-c"]

ENV DEBIAN_FRONTEND noninteractive

USER root

# Install the dependency
COPY .add-gazebo-ppa /tmp/add_gazebo_ppa.sh
RUN apt install -y wget \
  && ./tmp/add_gazebo_ppa.sh \
  && if [[ $(lsb_release -cs) != noble ]] ; then \
  apt install -y libgz-rendering7-dev libgz-common5-dev libgz-math7-dev \
  && apt install -y qt-advanced-docking-system \
  ; else \
  apt install -y libgz-rendering8-dev libgz-common5-dev libgz-math7-dev \
  && apt install -y libqt-advanced-docking-system-dev \
  ; fi

# Bind mount the source directory so as not to unnecessarily copy source code into the docker image
ARG WORKSPACE_DIR=/opt/tesseract_qt
RUN --mount=type=bind,target=${WORKSPACE_DIR}/src/tesseract_qt \
  cd ${WORKSPACE_DIR} \
  && rosdep install \
    --from-paths ${WORKSPACE_DIR}/src \
    -iry

# Build the repository
# Bind mount the source directory so as not to unnecessarily copy source code into the docker image
RUN --mount=type=bind,target=${WORKSPACE_DIR}/src/tesseract_qt \
  source /opt/tesseract/install/setup.bash \
  && source /opt/tesseract_planning/install/setup.bash \
  && cd ${WORKSPACE_DIR} \ 
  && colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release -DBUILD_RENDERING=OFF -DBUILD_STUDIO=OFF \
  && rm -rf build log
