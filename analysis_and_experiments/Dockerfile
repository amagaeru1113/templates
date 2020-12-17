FROM python:3.8.5

ENV PYTHONUNBUFFERED=1

RUN echo "alias klab='kedro jupyter lab --ip=0.0.0.0 --allow-root'" >> /root/.bashrc
RUN echo "alias mfui='mlflow ui --host 0.0.0.0'" >> /root/.bashrc
RUN echo "alias kviz='kedro viz --host 0.0.0.0'" >> /root/.bashrc


WORKDIR /lab

COPY poetry.lock pyproject.toml ./

RUN apt-get update \
    && apt-get install -y --no-install-recommends git wget tree vim \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN pip install -U pip \
    pip install poetry

RUN poetry config virtualenvs.create false \
    && poetry install

