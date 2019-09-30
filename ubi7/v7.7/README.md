This readme needs to be adjusted for automation

# How to STIG the UBI image

## Build the openscap container from the Dockerfile

`sudo docker build -t openscap-ncp:v0.1.43 /tmp/nist-ncp/`

### Locate the Image ID and Tag the Image

`sudo docker images`

`REPOSITORY TAG IMAGE ID CREATED SIZE`

`openscap-ncp v0.1.43 99ca43e8df63 14 seconds ago 584 MB`

`sudo docker tag 99ca43e8df63 openscap-ncp:latest`

This openscap container will provide the side car capabilities to harden the base UBI image as described below.
## STIG the base UBI Containers

### Install the atomic utility and the OpenScap packages

`yum install atomic docker`

After the atomic tool is installed, you also need a scanner, which is used by the atomic scan command as a back end for scanning images and containers. Red Hat recommends choosing the OpenSCAP scanner bundled in the rhel7/openscap container image in the registry.access.redhat.com image registry:

`sudo systemctl docker start`

`atomic install registry.access.redhat.com/rhel7/openscap`

### Pull down the images using docker

`sudo docker pull registry.access.redhat.com/ubi7-dev-preview/ubi:latest`

`sudo docker pull registry.access.redhat.com/ubi7-dev-preview/ubi-minimal:latest`

`sudo docker pull registry.access.redhat.com/ubi7/ubi:latest`

`sudo docker pull registry.access.redhat.com/rhel7:latest`

### Run the scan
`sudo atomic scan --scan_type configuration_compliance --scanner_args xccdf-id=scap_org.open-scap_cref_ssg-rhel7-xccdf-1.2.xml,profile=xccdf_org.ssgproject.content_profile_ospp,report registry.access.redhat.com/ubi7/ubi`

`cp /var/lib/atomic/openscap/2019-03-20-21-23-56-714983/6e1a4a3ff71f98f1f1926a6688d031528810f68542d1bc94c95d745da9951919/report.html /home/ec2-user/ubi7-initial.html`

### Harden the container
`sudo atomic scan --remediate --scan_type configuration_compliance --scanner_args xccdf-id=scap_org.open-scap_cref_ssg-rhel7-xccdf-1.2.xml,profile=xccdf_org.ssgproject.content_profile_ospp,report registry.access.redhat.com/ubi7/ubi`

### Tag the hardened container
`docker tag 11e1966c2059 ubi7-hardened:latest`

### Scan the hardened container
`sudo atomic scan --scan_type configuration_compliance --scanner_args xccdf-id=scap_org.open-scap_cref_ssg-rhel7-xccdf-1.2.xml,profile=xccdf_org.ssgproject.content_profile_ospp,report ubi7-hardened:latest`

### Copy report out of container
`cp /var/lib/atomic/openscap/2019-03-20-21-39-48-945211/2fdd6805706862846b092c735e86aeab575c0a84dec4f4861a9f10b468d885d4/report.html /home/ec2-user/ubi7-hardened.html`
