
FROM bitnami/kubectl:latest

USER root

RUN apt update && \
	apt upgrade && \
	apt-get install --no-install-recommends --no-install-suggests --yes curl && \
	apt-get clean

COPY semver.sh /
RUN chmod +x /semver.sh

USER 1001
