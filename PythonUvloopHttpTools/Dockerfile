FROM python:3.6.2

LABEL author=roberto.prevato@gmail.com

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Run python's package manager and install the flask package
RUN pip install -r requirements.txt

# Configure ports
EXPOSE 80

# Run apt-get, to install the SSH server, and supervisor
RUN apt-get update \ 
    && apt-get install -y supervisor \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# start scripts
COPY scripts/runapp.sh /usr/bin/

# supervisor config
ADD supervisor/app.conf /etc/supervisor/conf.d/

# Run the chmod command to change permissions on above file in the /bin directory
RUN chmod 755 /usr/bin/runapp.sh

# Default environmental variables
ENV SERVER_PORT 80
ENV GUWORKERS 4
ENV GUWORKERS_CONNECTIONS 1001

# run commands in supervisor
CMD ["supervisord", "-n"]