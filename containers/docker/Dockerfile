
# Copyright (c) 2020, Oracle and/or its affiliates.
# Licensed under the Universal Permissive License v 1.0 as shown at https://oss.oracle.com/licenses/upl.

FROM oraclelinux:7-slim
LABEL "provider"="Oracle" \
      "issues"="https://github.com/oracle/oci-designer-toolkit/issues" \
      "version"="0.9.2" \
      "description"="OKIT Web Server Container." \
      "copyright"="Copyright (c) 2020, Oracle and/or its affiliates."
SHELL ["/bin/bash", "-c"]
ENV PYTHONIOENCODING=utf8 \
    PYTHONPATH=":/okit/visualiser:/okit/okitweb:/okit" \
    FLASK_APP=okitweb \
    FLASK_DEBUG=1 \
    LANG=en_GB.UTF-8 \
    LANGUAGE=en_GB:en \
    LC_ALL=en_GB.UTF-8 \
    PATH=/root/bin:${PATH}
# Expose Ports
EXPOSE 80
EXPOSE 443
# Install new yum repos
RUN yum install -y \
    oracle-softwarecollection-release-el7 \
    oraclelinux-developer-release-el7 \
# Update base image
 && yum update -y \
# Install additional packages
 && yum install -y \
        python36 \
        python3-pip \
#        python36-pyyaml \
#        python36-oci-sdk \
#        rh-nginx114 \
 && rm -rf /var/cache/yum \
# Upgrade pip
 && python3 -m pip install --upgrade pip==20.0.2 \
# Install required python modules
 && pip3 install --no-cache-dir \
        flask==1.1.1 \
        gunicorn==20.0.4 \
        oci==2.6.0 \
        pyyaml==5.2 \
# Create Workspace
 && mkdir -p /okit/{config,log,visualiser,okitweb,workspace}
# Copy source code
COPY okitweb /okit/okitweb
COPY visualiser /okit/visualiser
COPY containers/oci/* /root/.oci/
# Add entrypoint to automatically start webserver
ENTRYPOINT ["gunicorn"]
CMD ["--workers=4", "--limit-request-line", "0", "--timeout", "120", "--bind=0.0.0.0:80", "okitweb.wsgi:app"]
