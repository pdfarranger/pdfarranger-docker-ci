# Docker file for pdfarranger ci tests using pikepdf and qpdf built from source
FROM ubuntu:24.04

RUN apt-get update && apt-get -y install python3-pip python3-distutils-extra python3-cairo \
 python3-wheel python3-gi python3-pillow gir1.2-poppler-0.18 gtk+3.0 intltool xvfb dbus \
 appstream desktop-file-utils gsettings-desktop-schemas curl bash python3-lxml \
 pybind11-dev python3-dev cmake libhandy-1-0 libglib2.0-bin libgnutls28-dev \
 fonts-cantarell git python3-virtualenv zlib1g libjpeg-turbo8-dev libssl-dev \
 python3-venv gettext python3-dogtail python3-dateutil img2pdf python3-pyatspi \
 python3-pytest
