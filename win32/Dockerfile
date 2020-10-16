FROM alpine:20200917
RUN apk add pacman wine curl freetype
RUN ln -s /usr/bin/wine64 /usr/bin/wine
RUN mkdir -p /etc/pacman.d \
 && curl https://raw.githubusercontent.com/msys2/MSYS2-packages/master/pacman-mirrors/mirrorlist.mingw64 | grep -v sourceforge.net > /etc/pacman.d/mirrorlist.mingw64 \
 && echo -e '[mingw64]\nInclude = /etc/pacman.d/mirrorlist.mingw64' >> /etc/pacman.conf \
 && pacman-key --init \
 && curl -O http://repo.msys2.org/msys/x86_64/msys2-keyring-r21.b39fb11-1-any.pkg.tar.xz \
 && pacman -U --noconfirm msys2-keyring-r21.b39fb11-1-any.pkg.tar.xz \
 && rm msys2-keyring* \
 && pacman-key --populate msys2
RUN pacman -Sy --noconfirm mingw-w64-x86_64-gtk3 mingw-w64-x86_64-python-gobject \
 mingw-w64-x86_64-python-cairo mingw-w64-x86_64-poppler mingw-w64-x86_64-gcc \
 mingw-w64-x86_64-python-lxml mingw-w64-x86_64-qpdf mingw-w64-x86_64-pybind11 \
 mingw-w64-x86_64-gettext mingw-w64-x86_64-gnutls mingw-w64-x86_64-python-pip \
 mingw-w64-x86_64-python-pillow
ENV WINEDEBUG=-all
RUN wine /mingw64/bin/gdk-pixbuf-query-loaders.exe > /mingw64/lib/gdk-pixbuf-2.0/2.10.0/loaders.cache \
 && cd /mingw64/share/glib-2.0/schemas \
 && wine cmd /c /mingw64/bin/glib-compile-schemas.exe . \
 && wine /mingw64/bin/python3.exe -m pip install keyboard 'img2pdf==0.3.6' 'pikepdf==1.16.1' https://github.com/jeromerobert/cx_Freeze/zipball/pdfarranger \
 && wineserver --wait
