FROM quay.io/uninett/jupyter-spark:20210514-6405497

LABEL maintainer = "Uninett As <system@uninett.no>"

USER root

RUN apt-get update && apt-get install -y --no-install-recommends \
        "eatmydata"   \
        "libglfw3" && \
    apt-get -y autoremove && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/*


RUN mkdir -p /var/log/tensorboard/ /usr/local/test-scripts && chmod o+w /var/log/tensorboard/  && \
    wget https://storage.googleapis.com/tensorflow_docs/docs/site/en/tutorials/images/cnn.ipynb -O /usr/local/test-scripts/tf-cnn.ipynb && \
    wget https://raw.githubusercontent.com/pytorch/examples/master/mnist/main.py -O /usr/local/test-scripts/pytorch-mnist.py
USER notebook
ENV PATH=/usr/local/mpi/bin:/usr/local/nvidia/bin:/usr/local/cuda/bin:${PATH} \
    LD_LIBRARY_PATH=/usr/local/mpi/lib:/usr/local/nvidia/lib:/usr/local/nvidia/lib64:${LD_LIBRARY_PATH} \
    NVIDIA_VISIBLE_DEVICES="" \
    NVIDIA_DRIVER_CAPABILITIES=all \
    PKG_KERAS_VERSION=2.3.1 \
    PKG_TENSORFLOW_VERSION=1.15 \
    PKG_TENSORFLOW_PROBABILITY_VERSION=0.9.0 \
    PKG_PYTORCH_VERSION=1.7.1 \
    PKG_XGBOOST_VERSION=1.3.3 \
    PKG_CATBOOST_VERSION=0.24.4 \
    PKG_LIGHTGBM_VERSION=3.1.1 \
    PKG_FASTAI_VERSION=1.0.60
    
# moved  installation of 'cudatoolkit=10.1' 'jupyter_contrib_nbextensions' (for fastai with notebooks) to jupyter-spark Dockerfile

 #    'mkl-service=2.3.0' \
 #    'libjpeg-turbo=2.*' \
 #    'libtiff=4.2.0' \

#needs conda : mkl-service=2.3.0 libjpeg-turbo=2.*  'libtiff=4.2.0'
#pygpu=0.7.6 is outdated, last release in 2018, only compatible with cuda 9.1 and documentation points to a dead link
#bcolz=1.2.1 has declared the package officially unmaintained on github, last release was 2018
#'python-graphviz=0.16' in conda becomes graphviz in pip

RUN CC="cc -mavx2" eatmydata pip install --no-cache-dir -I -U --force-reinstall \
    spacy==3.0.6 \
    jax==0.2.13 \
    graphviz==0.16  \
    pydot==1.4.2 \
    hyperopt==0.2.* \
    pycuda==2019.1.2 \
    "keras==$PKG_KERAS_VERSION" \
    keras-tqdm==2.0.* \
    pandas-summary==0.0.7 \
    sklearn_pandas==1.8.* \
    feather-format==0.4.* \
    plotnine==0.7.1 \
    isoweek==1.3.3 \
    kaggle==1.5.* \
    hypothesis==6.0.4 \
    mlflow==1.13.1 \
    "lightgbm==$PKG_LIGHTGBM_VERSION" \
    opencv-python==4.5.* \
    "tensorflow==$PKG_TENSORFLOW_VERSION" \
    "tensorflow-probability==$PKG_TENSORFLOW_PROBABILITY_VERSION" \
    "tensorboardx==2.0" \
    "xgboost==$PKG_XGBOOST_VERSION" \
    "catboost==$PKG_CATBOOST_VERSION" \
    sentencepiece \
    "fastai==$PKG_FASTAI_VERSION" \
    torch \
    torchtext \
    torchvision \
    nvidia-ml-py3 && \
    conda clean -y --all && \
    jupyter lab build

COPY start-notebook.sh /usr/local/bin/
