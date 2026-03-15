FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

ARG USER_UID=1000
ARG USER_GID=1000

# Remap vscode user/group to match host UID/GID (avoids permission errors on bind mounts)
RUN groupmod --gid $USER_GID vscode 2>/dev/null || true && \
    usermod --uid $USER_UID --gid $USER_GID vscode && \
    chown -R $USER_UID:$USER_GID /home/vscode

COPY --from=jdxcode/mise /usr/local/bin/mise /usr/local/bin/

# make sure mise is activated in both zsh and bash. Might be overridden by a user's dotfiles.
RUN echo 'eval "$(mise activate bash)"' >> /home/vscode/.bashrc && \
  echo 'eval "$(mise activate zsh)"' >> /home/vscode/.zshrc
