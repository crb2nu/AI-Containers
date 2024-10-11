FROM rocm/rocm-terminal:6.2

# Switch to rocm-user
USER rocm-user
WORKDIR /home/rocm-user
# Clone the stable-diffusion-webui repository
RUN git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git

USER root
RUN apt-get update && apt-get install -y --no-install-recommends software-properties-common

RUN add-apt-repository ppa:deadsnakes
RUN apt-get update && apt-get install -y --no-install-recommends python3.10 python3.10-venv

USER rocm-user
# Set the working directory to the application directory
WORKDIR /home/rocm-user/stable-diffusion-webui

RUN rm ./webui-user.sh

# Copy the webui-user.sh into the application directory with appropriate ownership
COPY --chown=rocm-user:appgroup webui-user.sh /home/rocm-user/stable-diffusion-webui/


# Expose necessary ports for the application
EXPOSE 7862


RUN export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

# Set umask and HOME environment variable, then start the application
CMD ["bash", "-c", "umask 0002 && export HOME=/home/rocm-user/stable-diffusion-webui && ./webui.sh"]