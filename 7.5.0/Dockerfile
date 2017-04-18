# © Copyright IBM Corporation 2015, 2016
# Modified by Tom Donohue
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM ubuntu:14.04

RUN export DEBIAN_FRONTEND=noninteractive \
  # The URL to download the MQ installer from in tar.gz format
  && MQ_URL=http://127.0.0.1/mqadv_dev75_linux_x86-64.tar.gz \
  # The MQ packages to install
  && MQ_PACKAGES="MQSeriesRuntime-*.rpm MQSeriesServer-*.rpm MQSeriesMsg*.rpm MQSeriesJava*.rpm MQSeriesJRE*.rpm MQSeriesGSKit*.rpm MQSeriesSamples-*.rpm" \
  # Optional: Update the command prompt
  && echo "mq:7.5" > /etc/debian_chroot \
  # Install additional packages required by this install process and the runtime scripts
  && apt-get update -y \
  && apt-get install -y \
    bash \
    bc \
    curl \
    rpm \
    tar \
  # Download and extract the MQ installation files
  && mkdir -p /tmp/mq \
  && cd /tmp/mq \
  && curl -LO $MQ_URL \
  && tar -zxvf ./*.tar.gz \
  # Recommended: Create the mqm user ID with a fixed UID and group, so that the file permissions work between different images
  && groupadd --gid 1000 mqm \
  && useradd --uid 1000 --gid mqm --home-dir /var/mqm mqm \
  && usermod -G mqm root \
  && cd /tmp/mq \
  # Accept the MQ license
  && ./mqlicense.sh -text_only -accept \
  # Install MQ using the RPM packages
  && rpm -ivh --force-debian --prefix /opt/mqm $MQ_PACKAGES \
  # Recommended: Set the default MQ installation (makes the MQ commands available on the PATH)
  && ln -s /lib64 /usr/ \
  && /opt/mqm/bin/setmqinst -p /opt/mqm -i \
  # Clean up all the downloaded files
  && rm -rf /tmp/mq

COPY *.sh /usr/local/bin/
COPY *.mqsc /etc/mqm/

# Support the latest functional cmdlevel by default
ENV MQ_QMGR_CMDLEVEL=802

RUN chmod +x /usr/local/bin/*.sh

# Always use port 1414 (the Docker administrator can re-map ports at runtime)
EXPOSE 1414

# Always put the MQ data directory is in a Docker volume
VOLUME /var/mqm

ENTRYPOINT ["mq.sh"]
