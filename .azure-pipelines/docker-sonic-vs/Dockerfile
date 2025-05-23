FROM docker-sonic-vs

ARG docker_container_name
ARG need_dbg

COPY ["debs", "/debs"]

# Remove existing packages first before installing the new/current packages. This is to overcome limitations with
# Docker's diff detection mechanism, where only the file size and the modification timestamp (which will remain the
# same, even though contents have changed) are checked between the previous and current layer.
RUN dpkg --purge libswsscommon python3-swsscommon sonic-db-cli libsaimetadata libsairedis libsaivs syncd-vs swss sonic-eventd libdashapi

RUN apt-get update

RUN apt install -y /debs/libdashapi_1.0.0_amd64.deb \
            /debs/libswsscommon_1.0.0_amd64.deb \
            /debs/python3-swsscommon_1.0.0_amd64.deb \
            /debs/sonic-db-cli_1.0.0_amd64.deb \
            /debs/libsaimetadata_1.0.0_amd64.deb \
            /debs/libsairedis_1.0.0_amd64.deb \
            /debs/libsaivs_1.0.0_amd64.deb \
            /debs/syncd-vs_1.0.0_amd64.deb \
            /debs/swss_1.0.0_amd64.deb

RUN if [ "$need_dbg" = "y" ] ; then dpkg -i /debs/libsairedis-dbgsym_1.0.0_amd64.deb ; fi

COPY ["start.sh", "/usr/bin/"]

RUN pip3 install scapy==2.5.0

RUN apt-get -y install software-properties-common libdatetime-perl libcapture-tiny-perl build-essential libcpanel-json-xs-perl git python3-protobuf
