FROM ros:noetic

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
 && apt-get install -y --no-install-recommends wget nano libomp-dev build-essential python3-pip \
                                               ros-noetic-pcl-ros ros-noetic-rviz \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /root/catkin_ws/src
WORKDIR /root/catkin_ws/src
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash; catkin_init_workspace'

COPY . /root/catkin_ws/src/BALM

WORKDIR /root/catkin_ws
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash; catkin_make -DCMAKE_BUILD_TYPE=Release'

RUN sed -i "6i source \"/root/catkin_ws/devel/setup.bash\"" /ros_entrypoint.sh

WORKDIR /

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]
