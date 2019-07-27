FROM julia:stretch

MAINTAINER Computational Biology Core <cbc-help@brown.edu>

USER root

RUN apt-get update

RUN apt-get install -yq --no-install-recommends \
   libcairo2-dev \
   unzip \
   bzip2 \
   wget \
   gettext \
   librsvg2-2 \
   hdf5-tools \
   libgtk-3-0 \
   libgconf-2-4 \
   libx11-xcb-dev \
   libxtst-dev \
   libxss1 \
   libnss3 \
   libssl1.0-dev \
   libasound2 \
   libx11-xcb1 \
   libgtk2.0-0 \
   xvfb \
   x11-utils \
   libx11-dev \
   qt5-default \
   && apt-get clean


ENV DISPLAY=:99

#Install Electron
RUN wget https://github.com/electron/electron/releases/download/v4.0.4/electron-v4.0.4-linux-x64.zip
RUN unzip -q electron-v4.0.4-linux-x64.zip -d atom

#Install Julia Packages

RUN julia -e 'import Pkg; Pkg.update()' && \
    julia -e "using Pkg; pkg\"add PlotlyJS ArgParse IJulia GeneticVariation VariantVisualization#develop  \
    InstantiateFromURL\"; pkg\"precompile\"; pkg\"resolve\""


RUN mkdir /data /script
RUN chmod 777 /data /script
COPY viva /script
RUN chmod 777 /script/viva
WORKDIR /data

ADD entrypoint.sh /script/entrypoint.sh
RUN chmod 777 /script/entrypoint.sh

ENTRYPOINT ["/script/entrypoint.sh"]
