FROM alpine:3.15.0
RUN apk add py3-pip py3-distutils-extra py3-cairo py3-atspi py3-lxml \
 py3-wheel py3-gobject3 py3-pillow poppler-glib gtk+3.0 intltool xvfb dbus \
 appstream desktop-file-utils gsettings-desktop-schemas curl bash \
 py3-pybind11-dev python3-dev py3-pybind11 py3-wheel qpdf-dev g++
# With qpdf 10.4 we can install pikepdf from 2.2.5 to 4.2.0
RUN pip install https://gitlab.com/dogtail/dogtail/-/archive/6817c16/dogtail-6817c16.tar.gz coverage python-dateutil img2pdf pikepdf==4.2.0
