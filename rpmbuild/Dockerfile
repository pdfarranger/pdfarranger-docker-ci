FROM fedora:latest
RUN dnf -y install 'dnf-command(builddep)' rpm-build rpmdevtools
RUN dnf -y builddep pdfarranger
RUN mkdir /pdfarranger
COPY ./pdfarranger-build /pdfarranger/
