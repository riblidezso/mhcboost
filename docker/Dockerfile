FROM continuumio/anaconda

#install keras
RUN pip install keras==1.0.7
ENV KERAS_BACKEND theano

#get compilers
RUN apt-get update && apt-get install -y make g++
RUN conda install -y gcc

#download and build  xgb
RUN git clone --recursive https://github.com/dmlc/xgboost && cd xgboost && make -j4 && cd python-package && python setup.py install
#install xgb python
ENV PYTHONPATH /xgboost/python-package:$PYTHOPATH

#install seaborn for fancy plotting
RUN conda install -y seaborn

#copy input data
COPY input /home/root/input

#copy the notebook
COPY netmhc_no_nestcv.ipynb /home/root/

WORKDIR /home/root

EXPOSE 9013
CMD ["jupyter", "notebook", "--no-browser","--port=9013"]
