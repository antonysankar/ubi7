FROM registry.access.redhat.com/ubi7/ubi

LABEL name="ubi7"
LABEL version="7.7"
LABEL com.redhat.license_terms="https://www.redhat.com/licenses/eulas"
LABEL dcar_status="approved"

COPY dsop-fix*.sh /

RUN chmod +x /dsop-fix-{1,2}.sh && for i in $(ls /dsop-fix-{1,2}.sh); do sh ${i}; done
RUN yum update -y --disablerepo=unified_platform_ubi8_appstream --disablerepo=unified_platform_ubi8_os && rm -rf /var/cache/yum/ /var/tmp/* /tmp/* /var/tmp/.???* /tmp/.???*
RUN chmod +x /dsop-fix-3.sh && for i in $(ls /dsop-fix-3.sh); do sh ${i}; done && rm -rf /dsop-fix*.sh

ENV container oci
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Purposely not setting USER as this is a builder image 
HEALTHCHECK --timeout=30s CMD curl -f http://localhost:80 || exit 1
CMD ["/bin/bash"]
