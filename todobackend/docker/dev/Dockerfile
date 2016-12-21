FROM balmainrob/todobackend-base:latest
MAINTAINER Robert Joscelyne <robert.joscelyne@gmail.com>

# Install dev/build dependencies
RUN apt-get update && \
    apt-get install -qy python-dev libmysqlclient-dev libxml2-dev libxslt1-dev

RUN apt-get install -qy python-dev  \
     build-essential libssl-dev libffi-dev \
     libxml2-dev libxslt1-dev zlib1g-dev \
     python-pip

# Activate virtual environment and install wheel support
RUN . /appenv/bin/activate && \
    pip install wheel --upgrade

# PIP environment variables (NOTE: must be set after installing wheel)
ENV WHEELHOUSE=/wheelhouse PIP_WHEEL_DIR=/wheelhouse PIP_FIND_LINKS=/wheelhouse XDG_CACHE_HOME=/cache

# OUTPUT: Build artefacts (Wheels) are output here
VOLUME /wheelhouse

# OUTPUT: Build cache
VOLUME /build

# OUTPUT: Test reports are output here
VOLUME /reports

# Add test entrypoint script
COPY scripts/test.sh /usr/local/bin/test.sh
RUN chmod +x /usr/local/bin/test.sh

# Set defaults for entrypoint and command string
ENTRYPOINT ["test.sh"]
CMD ["python", "manage.py", "test", "--noinput"]

# Add application source
COPY src /application
WORKDIR /application