# Set up a Debian stable (assumed jessie) container to run Anki in,
# for use until it works in stretch.
#
# Adapted from http://cloj.de/AnkiDocker/ , which in turn credits:
#   http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/
#   https://github.com/jfrazelle/dockerfiles/blob/master/spotify/Dockerfile
#
# Build:
#
#   cd to this file's directory, then:
#   # docker build -t debian-anki .
#
# Run:
#
#   $ ~/bin/anki-jessie

FROM debian:stable

RUN apt-get update && apt-get install -y \
  anki \
  dbus-x11 \
  fonts-vlgothic \
  ibus-mozc \
  locales \
  x11-xkb-utils

RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && locale-gen
RUN ln -nsf /usr/share/zoneinfo/America/Los_Angeles /etc/localtime

# Might only work if your host user and group IDs are both 1000.
ENV HOME /home/anki-user
RUN useradd --create-home --home-dir $HOME anki-user \
        && chown -R anki-user:anki-user $HOME

WORKDIR $HOME
USER anki-user
ENV LANG en_US.UTF-8
ENV XMODIFIERS @im=ibus
CMD /bin/bash -c "(/usr/bin/ibus-daemon -xd; /usr/bin/anki;)"


# The MIT License (MIT)
# 
# Copyright (c) 2016 Richard Möhn et al.
# 
# Permission is hereby granted, free of charge, to any person obtaining a
# copy of this software and associated documentation files (the
# "Software"), to deal in the Software without restriction, including
# without limitation the rights to use, copy, modify, merge, publish,
# distribute, sublicense, and/or sell copies of the Software, and to
# permit persons to whom the Software is furnished to do so, subject to
# the following conditions:
# 
# The above copyright notice and this permission notice shall be included
# in all copies or substantial portions of the Software.
# 
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
# OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
# MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
# IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
# CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
# TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
# SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
