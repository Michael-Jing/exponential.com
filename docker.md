```
FROM continuumio/miniconda

RUN mkdir -p /app/bin/mlocr

WORKDIR /app/bin/mlocr

COPY environment.yaml environment.yaml

RUN conda env update -f environment.yaml

COPY . .

ENV PATH /opt/conda/envs/mlocr/bin:$PATH

ENV PYTHONPATH /app/bin/mlocr:$PYTHONPATH

WORKDIR /app/bin/mlocr

ENTRYPOINT [ "/opt/conda/envs/mlocr/bin/python" ]

CMD [ "app.py" ]

```
