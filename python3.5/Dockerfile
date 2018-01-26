FROM java:8

RUN mv /etc/apt/source2.list /etc/apt/source.list
RUN apt-get update
RUN apt-get install python3.5
RUN apt-get install pip3
RUN curl -o numpy_mkl.whl https://download.lfd.uci.edu/pythonlibs/n1rrk3iq/numpy-1.14.0+mkl-cp37-cp37m-win_amd64.whl
RUN pip3 install numpy_mkl.whl
RUN pip3 install scipy
RUN rm -rf numpy_mkl.whl
