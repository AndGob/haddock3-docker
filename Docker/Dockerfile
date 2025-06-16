#docker build -t haddock3 -f .\Docker\Dockerfile .
FROM ubuntu
LABEL author="Andrea Gobbini"
LABEL mantainer="Andrea Gobbini"
LABEL mail="gobbini.andrea@gmail.com"
LABEL version="0.0.3"
LABEL name='haddock3' category='docking'

# Install necessary dependencies
RUN apt-get -y update && \
    apt-get -y install wget bzip2 build-essential gfortran

# Install Miniconda
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /tmp/miniconda.sh && \
    bash /tmp/miniconda.sh -b -p /opt/conda && \
    rm /tmp/miniconda.sh

# Set environment variables for conda
ENV PATH=/opt/conda/bin:$PATH

# Install Java
RUN apt-get -y install openjdk-8-jdk   
# Replace with the desired version (8–15)

# Create a conda environment and install Python and pip
RUN conda create -y -n myenv python=3.9 && \
    conda init bash && \
    echo "conda activate myenv" >> ~/.bashrc

# Install haddock3 in the conda environment
RUN /opt/conda/envs/myenv/bin/pip install haddock3

# Set environment variables
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
ENV PATH=$JAVA_HOME/bin:$PATH

# Set Python3 as the default Python version
#RUN update-alternatives --set python /opt/conda/envs/myenv/bin/python

# Cleanup
RUN apt-get clean

# install C shell console required to run installation of CNS
RUN apt-get -y install csh

# Copy the CNS and haddock folders into the container
COPY ../CNS/cns_solve_1.3_all.tar.gz /opt/
COPY ../haddock3 /opt/haddock3

# extract CNS files and proceed with the installation
RUN tar -xvzf /opt/cns_solve_1.3_all.tar.gz -C /opt/

# ensure getarch is executable and set CNS_ARCH
#RUN chmod +x /opt/cns_solve_1.3/bin/getarch

# enter the directory and edit the CNS solve env to work with haddock3
RUN cd /opt/cns_solve_1.3 && \
    sed -i 's@_CNSsolve_location_@'"$PWD"'@' cns_solve_env

# Patch CNS with HADDOCK custom files
#RUN cp -r /opt/haddock3/varia/cns1.3/[bis]* /opt/cns_solve_1.3/

# Compiling CNS
#RUN cd /opt/cns_solve_1.3 && \
#    #chmod +x /opt/cns_solve_1.3/bin/getarch && \
#    export CNS_ARCH=`intel-x86_64bit-linux` && \
#    make install compiler=gfortran

# Set the working directory
WORKDIR /opt/

# Create a script to activate the conda environment and run commands
RUN echo '#!/bin/bash\nsource /opt/conda/etc/profile.d/conda.sh\nconda activate myenv\nexec "$@"' > /usr/local/bin/start.sh && \
    chmod +x /usr/local/bin/start.sh

ENTRYPOINT ["/usr/local/bin/start.sh"]
CMD ["/bin/bash"]
