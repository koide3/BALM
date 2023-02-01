FROM ros:kinetic

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
 && apt-get install -y --no-install-recommends git wget nano libomp-dev build-essential python3-pip \
                                               libopencv-dev ros-kinetic-pcl-ros ros-kinetic-rviz \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /root
RUN git clone https://github.com/Livox-SDK/Livox-SDK.git \
  && mkdir -p Livox-SDK/build \
  && cd Livox-SDK/build \
  && cmake .. \
  && make -j$(nproc) \
  && make install \
  && rm -rf /root/Livox-SDK

RUN mkdir -p /root/catkin_ws/src
WORKDIR /root/catkin_ws/src

RUN git clone https://github.com/Livox-SDK/livox_ros_driver.git

COPY . /root/catkin_ws/src/BALM

WORKDIR /root/catkin_ws
RUN /bin/bash -c '. /opt/ros/kinetic/setup.bash; catkin_make -DCMAKE_BUILD_TYPE=Release'

RUN sed -i "6i source \"/root/catkin_ws/devel/setup.bash\"" /ros_entrypoint.sh

WORKDIR /

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]
