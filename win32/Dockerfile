FROM archlinux
RUN curl https://raw.githubusercontent.com/msys2/MSYS2-packages/master/pacman-mirrors/mirrorlist.mingw64 > /etc/pacman.d/mirrorlist.mingw64 \
 && echo -e '[mingw64]\nInclude = /etc/pacman.d/mirrorlist.mingw64' >> /etc/pacman.conf \
 && echo -e '[multilib]\nInclude = /etc/pacman.d/mirrorlist' >> /etc/pacman.conf \
 && pacman-key --init \
 && pacman-key --recv-keys AD351C50AE085775EB59333B5F92EFC1A47D45A1 \
 && pacman-key --lsign-key AD351C50AE085775EB59333B5F92EFC1A47D45A1
RUN pacman -Sy --noconfirm mingw-w64-x86_64-gtk3 mingw-w64-x86_64-python-gobject \
 mingw-w64-x86_64-python-cairo mingw-w64-x86_64-poppler mingw-w64-x86_64-gcc \
 mingw-w64-x86_64-python-lxml mingw-w64-x86_64-qpdf mingw-w64-x86_64-pybind11 \
 mingw-w64-x86_64-gettext mingw-w64-x86_64-gnutls mingw-w64-x86_64-python-pip \
 mingw-w64-x86_64-python-pillow wine-staging
ENV WINEDEBUG=-all
RUN wine /mingw64/bin/gdk-pixbuf-query-loaders.exe > /mingw64/lib/gdk-pixbuf-2.0/2.10.0/loaders.cache \
 && cd /mingw64/share/glib-2.0/schemas \
 && wine cmd /c /mingw64/bin/glib-compile-schemas.exe . \
 && wine /mingw64/bin/python3.exe -m pip install keyboard 'img2pdf>=0.3.4' 'pikepdf>=1.15.1' https://github.com/jeromerobert/cx_Freeze/zipball/pdfarranger \
 && wineserver --wait
