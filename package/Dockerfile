
#!BuildTag: libyui-devel

# NOTE: when building in OBS it actually is this image:
# https://build.opensuse.org/package/show/devel:libraries:libyui/opensuse-tumbleweed-image
# not the Docker hub image!!
FROM opensuse/tumbleweed

# do not install the files marked as documentation (use "rpm --excludedocs")
RUN sed -i -e "s/^.*rpm.install.excludedocs.*/rpm.install.excludedocs = yes/" /etc/zypp/zypp.conf

# import the OBS GPG key
COPY devel:libraries:libyui.pub /usr/share/gpg-keys/
RUN rpm --import /usr/share/gpg-keys/devel:libraries:libyui.pub

# Add the libyui repository
RUN zypper ar -f http://download.opensuse.org/repositories/devel:/libraries:/libyui/openSUSE_Tumbleweed/ libyui

RUN zypper --non-interactive install --no-recommends \
  boost-devel \
  brp-check-suse \
  brp-extract-appdata \
  cmake \
  doxygen \
  fontconfig-devel \
  gcc-c++ \
  git \
  graphviz \
  swig \
  python-devel \
  ruby-devel \
  graphviz-devel \
  jsoncpp-devel \
  libmicrohttpd-devel \
  libboost_test-devel \
  ncurses-devel \
  libzypp-devel \
  obs-service-source_validator \
  perl-JSON \
  pkg-config \
  'pkgconfig(Qt5Core)' \
  'pkgconfig(Qt5Gui)' \
  'pkgconfig(Qt5Svg)' \
  'pkgconfig(Qt5Widgets)' \
  'pkgconfig(Qt5X11Extras)' \
  "rubygem(libyui-rake)" \
  ShellCheck \
  rpm-build \
  tack \
  && zypper clean --all \
  && rm -rf /usr/lib*/ruby/gems/*/cache/ \
  && rm -rf /usr/share/doc/ \
  && find /usr/lib/locale/* -maxdepth 1 | grep -v -E "(en_US|cs_CZ|es_ES|de_DE|C.utf8)" | xargs rm -rf \
  && find /usr/share/locale -name "*.mo" -delete

ENV LC_ALL=en_US.UTF-8
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
