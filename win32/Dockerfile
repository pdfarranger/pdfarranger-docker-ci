FROM alpine:3.18.4
RUN apk add pacman-makepkg curl libgcc py3-distutils-extra py3-setuptools intltool wine
RUN mkdir -p /etc/pacman.d \
 && curl https://raw.githubusercontent.com/msys2/MSYS2-packages/master/pacman-mirrors/mirrorlist.mingw | sed 's|$repo|x86_64|g' > /etc/pacman.d/mirrorlist.mingw64 \
 && echo -e '[mingw64]\nInclude = /etc/pacman.d/mirrorlist.mingw64' >> /etc/pacman.conf \
 && pacman-key --init \
 && curl -O https://repo.msys2.org/msys/x86_64/msys2-keyring-1~20230703-1-any.pkg.tar.zst \
 && pacman -U --noconfirm msys2-keyring* \
 && rm msys2-keyring* \
 && pacman-key --populate msys2
# alexpux@gmail.com signature is not accepted
RUN echo -e "5\ny\n" | GNUPGHOME=/etc/pacman.d/gnupg gpg --no-tty --command-fd 0 --expert --edit-key AD351C50AE085775EB59333B5F92EFC1A47D45A1 trust
RUN pacman -Sy --noconfirm mingw-w64-x86_64-gtk3 mingw-w64-x86_64-python-gobject \
 mingw-w64-x86_64-python-cairo mingw-w64-x86_64-poppler mingw-w64-x86_64-gcc \
 mingw-w64-x86_64-python-lxml mingw-w64-x86_64-qpdf mingw-w64-x86_64-pybind11 \
 mingw-w64-x86_64-gettext mingw-w64-x86_64-gnutls mingw-w64-x86_64-python-pip \
 mingw-w64-x86_64-python-pillow mingw-w64-x86_64-libhandy \
 mingw-w64-x86_64-python-cx_Freeze
# Creating loaders.cache with gdk-pixbuf-query-loaders.exe does not
# work because gdiplus.dll is not found
ADD loaders.cache /mingw64/lib/gdk-pixbuf-2.0/2.10.0/loaders.cache
RUN wine reg add "HKLM\\System\\CurrentControlSet\\Control\\Session Manager\\Environment" /f /v PATH /t REG_SZ /d "z:\\mingw64\\bin;c:\\windows\\system32" \
 && wineserver --wait
RUN cd /mingw64/share/glib-2.0/schemas \
 && wine cmd /c /mingw64/bin/glib-compile-schemas.exe . \
 && wineserver --wait
ENV WINEDEBUG=-all
RUN SETUPTOOLS_USE_DISTUTILS=stdlib wine \
 python3.exe -m pip install pikepdf==8.5.0 img2pdf==0.4.4 keyboard darkdetect \
 && wineserver --wait
