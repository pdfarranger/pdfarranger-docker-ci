FROM alpine:edge
RUN apk add py3-pip py3-distutils-extra py3-cairo py3-atspi py3-lxml \
 py3-wheel py3-gobject3 py3-pillow poppler-glib gtk+3.0 intltool xvfb dbus \
 appstream desktop-file-utils gsettings-desktop-schemas curl bash \
 py3-pybind11-dev python3-dev py3-pybind11 py3-wheel qpdf-dev g++ libhandy1 \
 ttf-cantarell
# With qpdf 10.6 we can install pikepdf from 2.2.5 (?) to 6.0.1
RUN pip install https://gitlab.com/dogtail/dogtail/-/archive/6817c16/dogtail-6817c16.tar.gz coverage python-dateutil img2pdf pikepdf==6.0.1
