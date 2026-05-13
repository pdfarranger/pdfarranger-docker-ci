FROM alpine:3.23.4
RUN apk add pacman-makepkg curl libgcc py3-distutils-extra py3-setuptools intltool wine
RUN mkdir -p /etc/pacman.d \
 && curl https://raw.githubusercontent.com/msys2/MSYS2-packages/master/pacman-mirrors/mirrorlist.mingw | sed 's|$repo|x86_64|g' > /etc/pacman.d/mirrorlist.mingw64 \
 && echo -e '[mingw64]\nInclude = /etc/pacman.d/mirrorlist.mingw64' >> /etc/pacman.conf \
 && curl -O https://repo.msys2.org/msys/x86_64/msys2-keyring-1~20260214-1-any.pkg.tar.zst \
 && echo -e "[options]\nSigLevel = PackageOptional Never\n[msys]\nServer = https://msys2.org" > /tmp/temp_pacman.conf \
 && pacman -U --noconfirm --config /tmp/temp_pacman.conf msys2-keyring-*.pkg.tar.zst \
 && rm /tmp/temp_pacman.conf \
 && pacman-key --init \
 && pacman-key --populate msys2
RUN pacman -Sy --noconfirm \
 mingw-w64-x86_64-gtk3 \
 mingw-w64-x86_64-python-gobject \
 mingw-w64-x86_64-python-cairo \
 mingw-w64-x86_64-poppler \
 mingw-w64-x86_64-python-pikepdf \
 mingw-w64-x86_64-gettext \
 mingw-w64-x86_64-gnutls \
 mingw-w64-x86_64-img2pdf \
 mingw-w64-x86_64-python-pip \
 mingw-w64-x86_64-libhandy \
 mingw-w64-x86_64-python-cx-freeze \
 mingw-w64-x86_64-python-dateutil \
 mingw-w64-x86_64-cc

ENV XDG_RUNTIME_DIR=/tmp
ENV WINEDEBUG=-all
ENV DISPLAY=
RUN wine /mingw64/bin/gdk-pixbuf-query-loaders.exe --update-cache \
 && wineserver --wait
RUN wine reg add "HKLM\\System\\CurrentControlSet\\Control\\Session Manager\\Environment" /f /v PATH /t REG_SZ /d "z:\\mingw64\\bin;c:\\windows\\system32" \
 && wineserver --wait
RUN cd /mingw64/share/glib-2.0/schemas \
 && wine cmd /c /mingw64/bin/glib-compile-schemas.exe . \
 && wineserver --wait
RUN wine \
 python3.exe -m pip install --break-system-packages keyboard \
 && wineserver --wait

# python-msilib has a new compression type which does not work in
# this environment. Revert to the old compression tcompTYPE_MSZIP
RUN apk add git \
 && git clone https://github.com/marcelotduarte/python-msilib.git \
 && cd python-msilib \
 && git checkout 0.6.0 \
 && git revert --no-commit 181b5a0 \
 && wine python3.exe -m pip install --break-system-packages /python-msilib

