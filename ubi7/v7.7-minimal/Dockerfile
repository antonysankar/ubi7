FROM registry.access.redhat.com/ubi7/ubi-minimal 

LABEL name="ubi7"
LABEL version="7.7-minimal"
LABEL com.redhat.license_terms="https://www.redhat.com/licenses/eulas"


COPY dsop-fix*.sh /

RUN chmod +x /dsop-fix*.sh && for i in $(ls /dsop-fix*.sh); do sh ${i}; done && rm -rf /dsop-fix*.sh
RUN rm -rf /var/cache/yum/ /var/tmp/* /tmp/* /var/tmp/.???* /tmp/.???*

ENV container oci
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Purposely not setting USER as this is a builder image 
HEALTHCHECK --timeout=30s CMD curl -f http://localhost:80 || exit 1
CMD ["/bin/bash"]
