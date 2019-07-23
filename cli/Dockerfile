FROM julia:stretch

MAINTAINER Computational Biology Core <cbc-help@brown.edu>

USER root

#Install Cairo and other deps

RUN apt-get update
RUN apt-get install -y libcairo2-dev \
                       unzip \
                       wget \
                       gettext \
                       librsvg2-2 \
                       hdf5-tools \
                       libgtk2.0-0 \
                       libgconf-2-4 \
                       libx11-xcb-dev \
                       libxtst-dev \
                       libxss1 \
                       libnss3 \
                       libssl1.0-dev \
                       libasound-dev \
                       xvfb

#Install Electron
RUN wget https://github.com/electron/electron/releases/download/v4.0.4/electron-v4.0.4-linux-x64.zip
RUN unzip -q electron-v4.0.4-linux-x64.zip -d atom

#Install Julia Packages

RUN julia -e 'import Pkg; Pkg.update()' && \
    julia -e "using Pkg; pkg\"add Blink Rsvg PlotlyJS ArgParse GeneticVariation \
    DelimitedFiles VariantVisualization IJulia InstantiateFromURL\"; pkg\"precompile\""


RUN mkdir /data /script
RUN chmod 777 /data /script
COPY viva /script
RUN chmod 777 /script/viva

WORKDIR /data


ENTRYPOINT ["/script/viva"]
