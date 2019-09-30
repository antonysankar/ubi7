FROM registry.access.redhat.com/ubi8/ubi:8.0

LABEL name="ubi8"
LABEL version="8.0"
LABEL com.redhat.license_terms="https://www.redhat.com/licenses/eulas"
LABEL dcar_status="notapproved"


COPY dsop-fix*.sh /

RUN chmod +x /dsop-fix*.sh && for i in $(ls /dsop-fix*.sh); do sh ${i}; done && rm -rf /dsop-fix*.sh
RUN yum update -y && rm -rf /var/cache/yum/ /var/tmp/* /tmp/* /var/tmp/.???* /tmp/.???*

ENV container oci
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Purposely not setting USER as this is a builder image 
HEALTHCHECK --timeout=30s CMD curl -f http://localhost:80 || exit 1
CMD ["/bin/bash"]
