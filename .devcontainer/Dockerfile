FROM ros:humble

# Set up environment variables
ENV DEBIAN_FRONTEND=noninteractive \
    ROS_DISTRO=humble \
    ROS_WS=/workspace

# Create non-root user "rosdev" with matching UID and GID
ARG USERNAME=rosdev
ARG USER_UID=1000
ARG USER_GID=1000

RUN groupadd --gid ${USER_GID} ${USERNAME} && \
    useradd -m -s /bin/bash --uid ${USER_UID} --gid ${USER_GID} ${USERNAME}

# Update base-os, install dependencies, remove sudo if not required/desired
RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    python3-colcon-common-extensions \
    ros-humble-rmw-cyclonedds-cpp \
    ros-dev-tools \
    ros-humble-ign-ros2-control \
    ros-humble-ignition-cmake2-vendor \
    ros-humble-rviz2 \
    ignition-fortress \
    git \
    bash-completion \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Set up the rosdev user's environment. Remove the sudoers line if you do not want to be able to install
# additional packages as root.
RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> /home/${USERNAME}/.bashrc && \
    echo "source ${ROS_WS}/install/setup.bash" >> /home/${USERNAME}/.bashrc && \
    mkdir -p ${ROS_WS} && chown -R ${USERNAME}:${USERNAME} ${ROS_WS} && \
    echo "rosdev ALL=(ALL) NOPASSWD: /usr/bin/apt, /usr/bin/dpkg, /usr/bin/apt-get, /usr/bin/apt-cache" | sudo tee /etc/sudoers.d/rosdev && \
    chmod 0440 /etc/sudoers.d/rosdev 

USER ${USERNAME}
WORKDIR ${ROS_WS}

CMD ["/bin/bash"]

