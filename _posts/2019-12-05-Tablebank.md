---
layout: post
title:  "Table Detection"
date:   2019-12-05 10:10:05 +0800
categories: jekyll update
---
I am currently working on table detection and I will use this post to keep a memo of things I learned
in the process
# System Requirement
I finally installed detectron successfully, but I think that use a prebuilt docker image is the easiest way to get started, a Dockerfile is located in the docker folder
* Python2 with conda
* Caffe2
Install Caffe2 by installing prebuilt pytorch binary

{% highlight bash %}
conda install pytorch torchvision cudatoolkit=9.0 -c pytorch

{% endhighlight %}
* Cocoapi

{% highlight bash %}
# COCOAPI=/path/to/clone/cocoapi
git clone https://github.com/cocodataset/cocoapi.git $COCOAPI
cd $COCOAPI/PythonAPI
pip install cython
# Install into global site-packages
make install
# Alternatively, if you do not have permissions or prefer
# not to install the COCO API into global site-packages
python setup.py install --user

{% endhighlight %}
* Detectron

{% highlight bash %}
git clone https://github.com/facebookresearch/detectron
pip install -r detectron/requirements.txt
cd detectron && make
# in order to not accidently interfier with others, use CUDA_VISIBLE_DEVICES to specify the gpu you want to use
python detectron/detectron/tests/test_spatial_narrow_as_op.py
{% endhighlight %}

# Inference with Tablebank pretrained model
{% highlight bash %}
python tools/infer_simple.py --cfg $CONFIG_MODEL --output-dir /tmp/detectron-tablebank --image-ext jpg \
    --wts $MODEL_PATH /home/shr/TableBank/data/Sampled_Detection_data/Latex/images
{% endhighlight %}

1. Download weights and configuration file from [Model Zoo]
# Resources
[detectron2]



[detectron2]: https://gilberttanner.com/blog/detectron-2-object-detection-with-pytorch
[Model Zoo]: https://github.com/doc-analysis/TableBank/blob/master/MODEL_ZOO.md#table-detection
