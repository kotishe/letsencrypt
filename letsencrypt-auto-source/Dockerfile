# For running tests, build a docker image with a passwordless sudo and a trust
# store we can manipulate.

FROM ubuntu:trusty

# Add an unprivileged user:
RUN useradd --create-home --home-dir /home/lea --shell /bin/bash --groups sudo --uid 1000 lea

# Let that user sudo:
RUN sed -i.bkp -e \
      's/%sudo\s\+ALL=(ALL\(:ALL\)\?)\s\+ALL/%sudo ALL=NOPASSWD:ALL/g' \
      /etc/sudoers

# Install pip and nose:
RUN apt-get update && \
    apt-get -q -y install python-pip && \
    apt-get clean
RUN pip install nose

RUN mkdir -p /home/lea/letsencrypt

# Install fake testing CA:
COPY ./tests/certs/ca/my-root-ca.crt.pem /usr/local/share/ca-certificates/
RUN update-ca-certificates

# Copy code:
COPY . /home/lea/letsencrypt/letsencrypt-auto-source

USER lea
WORKDIR /home/lea

CMD ["nosetests", "-v", "-s", "letsencrypt/letsencrypt-auto-source/tests"]
