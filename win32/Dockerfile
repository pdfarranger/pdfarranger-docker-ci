FROM alpine:3.17.0
RUN apk add pacman-makepkg curl libgcc py3-distutils-extra py3-setuptools intltool wine
RUN mkdir -p /etc/pacman.d \
 && curl https://raw.githubusercontent.com/msys2/MSYS2-packages/master/pacman-mirrors/mirrorlist.mingw | sed 's|$repo|x86_64|g' > /etc/pacman.d/mirrorlist.mingw64 \
 && echo -e '[mingw64]\nInclude = /etc/pacman.d/mirrorlist.mingw64' >> /etc/pacman.conf \
 && pacman-key --init \
 && curl -O https://repo.msys2.org/msys/x86_64/msys2-keyring-1~20221024-1-any.pkg.tar.zst \
 && pacman -U --noconfirm msys2-keyring* \
 && rm msys2-keyring* \
 && pacman-key --populate msys2
RUN pacman -Sy --noconfirm mingw-w64-x86_64-gtk3 mingw-w64-x86_64-python-gobject \
 mingw-w64-x86_64-python-cairo mingw-w64-x86_64-poppler mingw-w64-x86_64-gcc \
 mingw-w64-x86_64-python-lxml mingw-w64-x86_64-qpdf mingw-w64-x86_64-pybind11 \
 mingw-w64-x86_64-gettext mingw-w64-x86_64-gnutls mingw-w64-x86_64-python-pip \
 mingw-w64-x86_64-python-pillow mingw-w64-x86_64-libhandy \
 mingw-w64-x86_64-python-setuptools-scm
RUN cd /usr/bin && ln -s wine64 wine
# Creating loaders.cache with gdk-pixbuf-query-loaders.exe does not
# work because gdiplus.dll is not found
ADD loaders.cache /mingw64/lib/gdk-pixbuf-2.0/2.10.0/loaders.cache
RUN wine reg add "HKLM\\System\\CurrentControlSet\\Control\\Session Manager\\Environment" /f /v PATH /t REG_SZ /d "z:\\mingw64\\bin;c:\\windows\\system32" \
 && wineserver --wait
RUN cd /mingw64/share/glib-2.0/schemas \
 && wine cmd /c /mingw64/bin/glib-compile-schemas.exe . \
 && wineserver --wait
ENV WINEDEBUG=-all
RUN wine python3.exe -m pip install wheel packaging python-dateutil keyboard 'https://github.com/jeromerobert/cx_Freeze/zipball/db061df6f0b6bfc190a9557470e4436dc743dc8a' darkdetect \
 && wineserver --wait
RUN curl -s -L 'https://files.pythonhosted.org/packages/1e/bf/e003c274ea2cf178448357549b444059276b674c7d6a7601320be873d133/pikepdf-6.2.6.tar.gz' | tar xz \
 && cd pikepdf-* && wine python3.exe -m pip install --no-build-isolation . \
 && cd .. && rm -rf pikepdf-* \
 && wine python3.exe -m pip install 'img2pdf==0.4.4' \
 && wineserver --wait
