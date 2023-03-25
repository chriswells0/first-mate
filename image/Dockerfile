FROM curlimages/curl:latest

USER root

RUN apk --no-cache upgrade \
	&& apk --no-cache add jq

COPY semver.sh /
RUN chmod +x /semver.sh

RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
RUN chmod +x ./kubectl \
	&& mv ./kubectl /usr/local/bin

USER curl_user

