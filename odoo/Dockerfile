FROM docker.io/bitnami/odoo:16.0.20230315-debian-11-r2

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    nano \
    git \
  && rm -rf /var/lib/apt/lists/*

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN mkdir -p /mnt/oca-addons && cd /mnt/oca-addons && \
  OCA_REPOS=( \
    l10n-thailand \
    web \
    server-ux \
    partner-contact \
    hr \
    reporting-engine \
  ) && \
  for REPO in "${OCA_REPOS[@]}"; do \
    git clone --depth 1 --branch 16.0 https://github.com/OCA/${REPO}.git && \
    rm -rf ${REPO}/{setup,LICENSE,README.md,requirements.txt} && \
    mv ${REPO}/* . && \
    rm -r ${REPO} ; \
  done
 
RUN /opt/bitnami/odoo/venv/bin/pip3 install \
  num2words==0.5.12

# install amincheloh/odoo-addons in same folder with oca-addons preserve backward compat
RUN cd /mnt/oca-addons && \
  REPO=odoo-addons && \
  git clone --depth 1 --branch 16.0 https://github.com/amincheloh/${REPO}.git && \
  rm -rf ${REPO}/{setup,LICENSE,README.md,requirements.txt} && \
  mv ${REPO}/* . && \
  rm -r ${REPO}

RUN sed -i -E -e 's#(addons_path.+)#\1,/mnt/oca-addons#' /opt/bitnami/scripts/odoo/bitnami-templates/odoo.conf.tpl
