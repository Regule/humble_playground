FROM ros:humble

# Arguments
ARG USERNAME=USERNAME
ARG USER_ID=1000

# User creation
RUN if id -u $USER_ID; then userdel `id -un $USER_ID`; fi
RUN groupadd --gid  $USER_ID $USERNAME
RUN useradd --uid $USER_ID --gid $USER_ID -m $USERNAME

# Apt-get configuration
#RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F42ED6FBAB17C654
#RUN apt update && sudo apt install ros2-apt-source ; rm /etc/apt/sources.list.d/ros2.list; rm /usr/share/keyrings/ros-archive-keyring.gpg
RUN rm /etc/apt/sources.list.d/ros2-latest.list && \
    apt update && apt install curl && \
    curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null


# Sudo configuration
RUN apt-get update
RUN apt-get install -y sudo
RUN echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME
RUN chmod 0440 /etc/sudoers.d/$USERNAME

# Update system and install extra packages
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y python3-pip ros-humble-desktop ros-humble-ros-gz

# Set default user and default shell
ENV SHELL /bin/bash
USER $USERNAME