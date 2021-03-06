#author russell jarvis rjjarvis@asu.edu
#NEURON Dockerfile
#Set the base image to Ubuntu
FROM scidash/scipy-notebook-plus

#Get a whole lot of GNU core development tools
#version control java development, maven
#Libraries required for building MPI from source
#Libraries required for building NEURON from source
#Also DO this part as root.
USER root
RUN /opt/conda/bin/conda update conda
RUN apt-get update -y && apt-get upgrade -y 
RUN apt-get update && apt-get install -y wget bzip2 ca-certificates default-jre default-jdk maven automake libtool  \
                       wget python3 libpython3-dev libncurses5-dev libreadline-dev libgsl0-dev cython3 \
                       python3-pip python3-numpy python3-scipy python3-matplotlib python3-mock \
                       ipython3 python3-docutils \
                       python3-mpi4py cmake ssh


#Do the rest of the build  as user:
#This will create a more familiar environment to continue developing in.
#with less of a need to chown and chmod everything done as root at dockerbuild completion

RUN wget https://neuron.yale.edu/ftp/neuron/versions/v7.6/nrn-7.6.x86_64-linux.deb
RUN dpkg -i nrn-7.6.x86_64-linux.deb
ENV PATH /usr/bin/python/python:/opt/conda/bin:/opt/conda/bin/conda:/opt/conda/bin/python:$PATH

USER jovyan
ENV PATH /usr/bin/python/python:/opt/conda/bin:/opt/conda/bin/conda:/opt/conda/bin/python:$PATH
# Use numpy 1.12.1 until quantities is compatible with 1.13.
RUN sudo /opt/conda/bin/conda install -y scipy numpy==1.12.1 matplotlib
RUN sudo chown -R jovyan /home/jovyan
ENV HOME /home/jovyan
ENV PATH /opt/conda/bin:/opt/conda/bin/conda:/opt/conda/bin/python:$PATH

RUN wget https://neuron.yale.edu/ftp/neuron/versions/v7.6/nrn-7.6.x86_64-linux.deb
RUN sudo dpkg -i nrn-7.6.x86_64-linux.deb
RUN sudo git clone https://github.com/neuronsimulator/nrn
WORKDIR nrn
RUN sudo apt-get install flex -y
RUN sudo ./build.sh
RUN sudo ./configure --prefix=`pwd` --without-iv --with-nrnpython=/opt/conda/bin/python
#RUN sudo apt-get install bison -y
RUN sudo apt-get install byacc -y
RUN cat ./build.sh
RUN sudo make
RUN sudo make install
WORKDIR nrn/src/nrnpython
RUN python setup.py install

ENV NEURON_HOME $HOME/nrn-7.6/x86_64
ENV PATH $NEURON_HOME/bin:$PATH



