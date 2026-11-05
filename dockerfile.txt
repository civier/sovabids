FROM python:3.14-slim-bookworm

ENV PYTHONUNBUFFERED=1

RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates git \
    && rm -rf /var/lib/apt/lists/*

COPY . ./src/sovabids

WORKDIR /src/sovabids

# The playbook clones upstream for the Flask app and installs the GUI package from PyPI.
RUN python -m pip install --no-cache-dir 'sovabids[gui]' \
    && python -m pip check

WORKDIR /

EXPOSE 5000

CMD ["python", "/src/sovabids/front/app/app.py"]