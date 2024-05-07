FROM debian:10

# dockerfile for GRPY

ARG DEBIAN_FRONTEND=noninteractive

# basic install stuff

RUN apt-get update
RUN apt-get update
RUN apt-get install -y --no-install-recommends apt-utils 
RUN apt-get install -y build-essential
RUN apt-get install -y git curl wget

# get GRPY

RUN git clone https://github.com/ehb54/GRPY.git
RUN apt-get install -y python
RUN apt-get install -y gfortran
# RUN apt-get install -y libblas3 liblapack3 liblapack-dev liblapacke-dev libblas-dev 
# RUN apt-get install -y libatlas-base-dev
RUN apt-get install -y libopenblas-dev

# for systems with default python3, need to link in python2
# RUN ln -s /bin/python2.7 /usr/bin/python

# search for lib names - for development
# RUN find . -name "lib*blas*"
# RUN ls -l /usr/lib64/lib*blas*
# RUN ls -l /usr/lib/liblap*
# RUN ls -l /etc/alternatives/liblap*

# atlas try
# RUN cd GRPY/GRPY/ThirdParty/plasma-installer_2.8.0 && ls && ./setup.py --cc=gcc --fc=gfortran --blaslib="-lblas -lcblas" --lapacklib="-llapack_atlas -llapack"

# openblas try
RUN cd GRPY/GRPY/ThirdParty/plasma-installer_2.8.0 && ls && yes d | ./setup.py --cc=gcc --fc=gfortran --blaslib="-lopenblas" --lapacklib="-llapack" --notesting

# for a more recent plasma try plasma 17 - but it has changes vs 2.8 so likely require adjusting plasmaGRPY.f

# RUN wget https://bitbucket.org/icl/plasma/downloads/plasma-17.1.tar.gz && tar zxvf plasma-17.1.tar.gz
# RUN ln -s /bin/python2.7 /usr/bin/python
# RUN cd plasma-17.1 && make 


## compile plasmaGRPY

RUN cd GRPY/GRPY/src/plasmaGRPY && \
gfortran -c -O2 -ffpe-summary='none' -I../../ThirdParty/plasma-installer_2.8.0/install/include plasmaGRPY.f && \
gfortran -o ../../bin/GRPYLinux/plasmaGRPY.exe -O2 -ffpe-summary='none' plasmaGRPY.o -L../../ThirdParty/plasma-installer_2.8.0/install/lib -lplasma -lquark -lcoreblasqw -lcoreblas -llapacke -lopenblas -L/GRPY/GRPY/ThirdParty/plasma-installer_2.8.0/install/lib -ltmg -llapack  -lpthread

VOLUME [ "/work" ]
WORKDIR /work
ENTRYPOINT [ "/GRPY/GRPY/bin/GRPYLinux/plasmaGRPY.exe" ]


