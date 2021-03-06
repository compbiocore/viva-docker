FROM jupyter/base-notebook

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

# Julia dependencies
# install Julia packages in /opt/julia instead of $HOME
ENV JULIA_DEPOT_PATH=/opt/julia
ENV JULIA_PKGDIR=/opt/julia
ENV JULIA_VERSION=1.1.0

RUN mkdir /opt/julia-${JULIA_VERSION} && \
    cd /tmp && \
    wget -q https://julialang-s3.julialang.org/bin/linux/x64/`echo ${JULIA_VERSION} | cut -d. -f 1,2`/julia-${JULIA_VERSION}-linux-x86_64.tar.gz && \
    echo "80cfd013e526b5145ec3254920afd89bb459f1db7a2a3f21849125af20c05471 *julia-${JULIA_VERSION}-linux-x86_64.tar.gz" | sha256sum -c - && \
    tar xzf julia-${JULIA_VERSION}-linux-x86_64.tar.gz -C /opt/julia-${JULIA_VERSION} --strip-components=1 && \
    rm /tmp/julia-${JULIA_VERSION}-linux-x86_64.tar.gz
RUN ln -fs /opt/julia-*/bin/julia /usr/local/bin/julia

# Show Julia where conda libraries are \
RUN mkdir /etc/julia && \
    echo "push!(Libdl.DL_LOAD_PATH, \"$CONDA_DIR/lib\")" >> /etc/julia/juliarc.jl && \
    # Create JULIA_PKGDIR \
    mkdir $JULIA_PKGDIR && \
    chown $NB_USER $JULIA_PKGDIR && \
    fix-permissions $JULIA_PKGDIR

USER $NB_UID

# Add Julia packages. Only add HDF5 if this is not a test-only build since
# it takes roughly half the entire build time of all of the images on Travis
# to add this one package and often causes Travis to timeout.
#
# Install IJulia as jovyan and then move the kernelspec out
# to the system share location. Avoids problems with runtime UID change not
# taking effect properly on the .local folder in the jovyan home dir.
RUN julia -e 'import Pkg; Pkg.update()' && \
    julia -e "using Pkg; pkg\"add PlotlyJS ArgParse IJulia GeneticVariation VariantVisualization \
    InstantiateFromURL\"; pkg\"precompile\"" && \
    # move kernelspec out of home \
    mv $HOME/.local/share/jupyter/kernels/julia* $CONDA_DIR/share/jupyter/kernels/ && \
    chmod -R go+rx $CONDA_DIR/share/jupyter && \
    rm -rf $HOME/.local && \
    fix-permissions $JULIA_PKGDIR $CONDA_DIR/share/jupyter

RUN mkdir $HOME/notebook
RUN mkdir $HOME/notebook/data
COPY VIVA.ipynb $HOME/notebook
WORKDIR $HOME/notebook

ENTRYPOINT ["tini", "-g", "--", "xvfb-run"]
CMD ["jupyter", "lab"]
