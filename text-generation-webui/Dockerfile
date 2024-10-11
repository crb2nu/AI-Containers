FROM rocm/rocm-terminal:6.2

# Switch to rocm-user
USER rocm-user
WORKDIR /home/rocm-user

# Clone the text-generation-webui repository
RUN git clone https://github.com/oobabooga/text-generation-webui.git

WORKDIR /home/rocm-user/text-generation-webui

USER root
RUN rm -r /home/rocm-user/text-generation-webui/extensions/silero_tts
RUN rm -r /home/rocm-user/text-generation-webui/extensions/coqui_tts

USER rocm-user
# Run the setup script with specified environment variables
RUN GPU_CHOICE=B USE_CUDA118=FALSE LAUNCH_AFTER_INSTALL=FALSE INSTALL_EXTENSIONS=TRUE ./start_linux.sh --verbose

# Copy the CMD_FLAGS.txt into the application directory with appropriate ownership
COPY --chown=rocm-user:appgroup CMD_FLAGS.txt /home/rocm-user/text-generation-webui/


# Expose necessary ports for the application
EXPOSE 7860 5000 5005

# Set the working directory to the application directory
WORKDIR /home/rocm-user/text-generation-webui

RUN export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

# Set umask and HOME environment variable, then start the application
CMD ["bash", "-c", "umask 0002 && export HOME=/home/rocm-user/text-generation-webui && ./start_linux.sh"]