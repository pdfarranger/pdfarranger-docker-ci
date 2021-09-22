FROM alpine:3.14.2 AS wine
LABEL image=jeromerobert/wine-mingw64 stage=wine
RUN apk add curl build-base flex bison bash gawk autoconf linux-headers
# Wine 6.4, 5.19, 5.15, 5.14 freeze. 5.1, 5.8, 5.13 work.
RUN curl https://dl.winehq.org/wine/source/5.x/wine-5.13.tar.xz | tar xJ
RUN curl -L https://github.com/wine-staging/wine-staging/archive/v5.13.tar.gz | tar xz
RUN cd wine-staging-*/patches && ./patchinstall.sh DESTDIR=/wine-5.13 imagehlp-BindImageEx
WORKDIR wine-5.13
RUN ./configure~ --enable-win64 --without-x --without-freetype
RUN make -j$(($(nproc)*2)) install
RUN find /usr/local -name "*.so" -exec strip {} \;

FROM alpine:3.14.2
RUN apk add pacman curl libgcc py3-distutils-extra py3-setuptools intltool
RUN mkdir -p /etc/pacman.d \
 && curl https://raw.githubusercontent.com/msys2/MSYS2-packages/master/pacman-mirrors/mirrorlist.mingw | sed 's|$repo|x86_64|g' > /etc/pacman.d/mirrorlist.mingw64 \
 && echo -e '[mingw64]\nInclude = /etc/pacman.d/mirrorlist.mingw64' >> /etc/pacman.conf \
 && pacman-key --init \
 && curl -O http://repo.msys2.org/msys/x86_64/msys2-keyring-1~20210213-2-any.pkg.tar.zst \
 && pacman -U --noconfirm msys2-keyring* \
 && rm msys2-keyring* \
 && pacman-key --populate msys2
RUN pacman -Sy --noconfirm mingw-w64-x86_64-gtk3 mingw-w64-x86_64-python-gobject \
 mingw-w64-x86_64-python-cairo mingw-w64-x86_64-poppler mingw-w64-x86_64-gcc \
 mingw-w64-x86_64-python-lxml mingw-w64-x86_64-qpdf mingw-w64-x86_64-pybind11 \
 mingw-w64-x86_64-gettext mingw-w64-x86_64-gnutls mingw-w64-x86_64-python-pip \
 mingw-w64-x86_64-python-pillow
COPY --from=wine /usr/local/ /usr/local/
RUN cd /usr/local/bin && ln -s wine64 wine
# Creating loaders.cache with gdk-pixbuf-query-loaders.exe does not
# work because gdiplus.dll is not found
ADD loaders.cache /mingw64/lib/gdk-pixbuf-2.0/2.10.0/loaders.cache
RUN wine reg add "HKLM\\System\\CurrentControlSet\\Control\\Session Manager\\Environment" /f /v PATH /t REG_SZ /d "z:\\mingw64\\bin;c:\\windows\\system32" \
 && wineserver --wait
RUN cd /mingw64/share/glib-2.0/schemas \
 && wine cmd /c /mingw64/bin/glib-compile-schemas.exe . \
 && wineserver --wait
RUN wine python3.exe -m pip install 'pikepdf==2.16.1' 'pybind11==2.7.1' packaging python-dateutil keyboard 'img2pdf==0.4.0' \
  https://github.com/jeromerobert/cx_Freeze/zipball/db061df6f0b6bfc190a9557470e4436dc743dc8a && wineserver --wait
ENV WINEDEBUG=-all
