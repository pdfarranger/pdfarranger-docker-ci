FROM alpine:3.13.5
RUN apk add  py3-pip py3-distutils-extra py3-pikepdf py3-cairo py3-atspi \
 py3-wheel py3-gobject3 py3-pillow poppler-glib gtk+3.0 intltool xvfb dbus \
 appstream desktop-file-utils
RUN pip install https://gitlab.com/dogtail/dogtail/-/archive/6817c16/dogtail-6817c16.tar.gz coverage python-dateutil img2pdf
