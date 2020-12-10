
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
  lcov \
  jsoncpp-devel \
  libmicrohttpd-devel \
  libyui-devel \
  libyui-ncurses-devel \
  libyui-qt-devel \
  libyui-rest-api-devel \
  libzypp-devel \
  openSUSE-release-ftp \
  obs-service-source_validator \
  perl-JSON \
  pkg-config \
  'pkgconfig(Qt5Core)' \
  'pkgconfig(Qt5Gui)' \
  'pkgconfig(Qt5Svg)' \
  'pkgconfig(Qt5Widgets)' \
  'pkgconfig(Qt5X11Extras)' \
  "rubygem(coveralls-lcov)" \
  "rubygem(libyui-rake)" \
  "rubygem(rubocop)" \
  ShellCheck \
  rpm-build \
  tack \
  yast2-devtools \
  && zypper clean --all \
  && rm -rf /usr/lib*/ruby/gems/*/cache/ \
  && rm -rf /usr/share/doc/ \
  && find /usr/lib/locale/* -maxdepth 1 | grep -v -E "(en_US|cs_CZ|es_ES|de_DE|C.utf8)" | xargs rm -rf \
  && find /usr/share/locale -name "*.mo" -delete

COPY libyui-travis /usr/local/bin/
RUN chmod a+x /usr/local/bin/libyui-travis
ENV LC_ALL=en_US.UTF-8
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

# run some smoke tests to make sure there is no serious issue with the image
RUN c++ --version

# this is a bit tricky as the libyui/rake loads some files at the initialization
RUN mkdir -p package && echo > package/test.spec && \
  echo -e 'SET(VERSION_MAJOR "42")\nSET(VERSION_MINOR "42")\nSET(VERSION_PATCH "42")' \
  > VERSION.cmake && rake -t -r libyui/rake -V && rm -rf package && rm VERSION.cmake
