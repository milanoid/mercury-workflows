FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

ARG USERNAME
ARG USER_UID=1000
ARG USER_GID=${USER_UID}

# Ensure all future commands run as root (necessary for user creation)
USER root

# Remove any existing user that would collide with the requested UID/GID or name
RUN set -eu; \
    if getent passwd "${USER_UID}" >/dev/null; then userdel -r "$(getent passwd "${USER_UID}" | cut -d: -f1)" || true; fi; \
    if getent group  "${USER_GID}" >/dev/null; then groupdel   "$(getent group  "${USER_GID}" | cut -d: -f1)" || true; fi; \
    if getent passwd "${USERNAME}" >/dev/null; then userdel -r "${USERNAME}" || true; fi


# Add host user (gracefully skip if root)
RUN if [ "${USERNAME}" != "root" ]; then \
      groupadd --gid ${USER_GID} ${USERNAME} || true; \
      useradd -s /bin/bash -m -u ${USER_UID} -g ${USER_GID} ${USERNAME}; \
      mkdir -p /etc/sudoers.d; \
      echo "${USERNAME} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/${USERNAME}; \
      chmod 0440 /etc/sudoers.d/${USERNAME}; \
    fi

COPY --from=jdxcode/mise /usr/local/bin/mise /usr/local/bin/

# make sure mise is activated in both zsh and bash. Might be overridden by a user's dotfiles.
RUN echo 'eval "$(mise activate bash)"' >> /home/${USERNAME}/.bashrc && \
    echo 'eval "$(mise activate zsh)"'  >> /home/${USERNAME}/.zshrc && \
    chown ${USERNAME}:${USERNAME} /home/${USERNAME}/.bashrc /home/${USERNAME}/.zshrc
