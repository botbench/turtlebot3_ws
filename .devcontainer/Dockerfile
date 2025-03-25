FROM ros:humble

# Set up environment variables
# Set the model to the robot you have, default is "burger"
ENV DEBIAN_FRONTEND=noninteractive \
    ROS_DISTRO=humble \
    ROS_WS=/workspace \
    TURTLEBOT3_MODEL=burger

# Create non-root user "rosdev" with matching UID and GID
ARG USERNAME=rosdev
ARG USER_UID=1000
ARG USER_GID=1000

RUN groupadd --gid ${USER_GID} ${USERNAME} && \
    useradd -m -s /bin/bash --uid ${USER_UID} --gid ${USER_GID} -G video,dialout ${USERNAME}

# Update base-os, install dependencies, remove sudo if not required/desired
RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    python3-colcon-common-extensions \
    python3-argcomplete \
    libboost-system-dev \
    build-essential \
    ros-humble-rmw-cyclonedds-cpp \
    ros-dev-tools \
    ros-humble-ign-ros2-control \
    ros-humble-ignition-cmake2-vendor \
    ros-humble-rviz2 \
    ros-humble-ros-gz-sim \
    ros-humble-ros-gz-bridge \
    ros-humble-ros-gz-image \
    ros-humble-hls-lfcd-lds-driver \
    ros-humble-turtlebot3-msgs \
    ros-humble-dynamixel-sdk \
    libudev-dev \
    ignition-fortress \
    git \
    bash-completion \
    sudo \
    apt-file \
    mesa-utils \
    less \
    && rm -rf /var/lib/apt/lists/*

# Set up the rosdev user's environment. Remove the sudoers line if you do not want to be able to install
# additional packages as root.
RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> /home/${USERNAME}/.bashrc && \
    echo "source ${ROS_WS}/install/setup.bash" >> /home/${USERNAME}/.bashrc && \
    echo "export TURTLEBOT3_MODEL=${TURTLEBOT3_MODEL}" >> /home/${USERNAME}/.bashrc && \
    echo 'export LDS_MODEL=LDS-02' >> /home/${USERNAME}/.bashrc && \
    mkdir -p ${ROS_WS} && chown -R ${USERNAME}:${USERNAME} ${ROS_WS} && \
    echo "rosdev ALL=(ALL) NOPASSWD: /usr/bin/apt, /usr/bin/apt-file, /usr/bin/dpkg, /usr/bin/apt-get, /usr/bin/apt-cache" | sudo tee /etc/sudoers.d/rosdev && \
    chmod 0440 /etc/sudoers.d/rosdev 

USER ${USERNAME}
WORKDIR ${ROS_WS}

CMD ["/bin/bash"]

