
#!BuildTag: yast-rake

# This actually is registry.opensuse.org/yast/head/images/opensuse/tumbleweed:latest
# when building in OBS YaST:Head!
FROM opensuse/tumbleweed

# explicitly set the UTF-8 locale
ENV LC_ALL=en_US.UTF-8
ENV LANG=en_US.UTF-8

# do not install the files marked as documentation (use "rpm --excludedocs")
RUN sed -i -e "s/^.*rpm.install.excludedocs.*/rpm.install.excludedocs = yes/" /etc/zypp/zypp.conf

# import the YaST OBS GPG key
COPY YaST:Head.pub /usr/share/gpg-keys/
RUN rpm --import /usr/share/gpg-keys/YaST:Head.pub

RUN zypper addrepo --refresh http://download.opensuse.org/repositories/YaST:/Head/openSUSE_Tumbleweed yast

RUN zypper --non-interactive install --no-recommends \
  diffutils \
  git \
  hostname \
  which \
  "rubygem(ruby:3.3.0:yast-rake)" \
  osc \
  obs-service-format_spec_file \
  obs-service-source_validator \
  tar \
  bzip2 \
  sudo \
  && zypper clean --all \
  && rm -rf /usr/lib*/ruby/gems/*/cache/ \
  && find /usr/share/locale -name "*.mo" -delete
