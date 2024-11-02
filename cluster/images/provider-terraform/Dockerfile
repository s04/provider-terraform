FROM alpine:3.20.3
RUN apk --no-cache add ca-certificates bash git curl unzip

ARG TARGETOS
ARG TARGETARCH

ENV TERRAFORM_VERSION=1.8.0
ENV TF_IN_AUTOMATION=1
ENV TF_PLUGIN_CACHE_DIR=/tf/plugin-cache

# Download Terraform from HashiCorp's releases
RUN curl -s -L https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_${TARGETOS}_${TARGETARCH}.zip -o terraform.zip \
  && unzip -d /usr/local/bin terraform.zip \
  && rm terraform.zip \
  && chmod +x /usr/local/bin/terraform \
  && mkdir -p ${TF_PLUGIN_CACHE_DIR} \
  && chown -R 2000 /tf

# Download the latest provider binary from the official releases
RUN PROVIDER_VERSION=v0.18.0 && \
  curl -s -L https://github.com/upbound/provider-terraform/releases/download/${PROVIDER_VERSION}/provider-terraform_${TARGETOS}_${TARGETARCH} -o /usr/local/bin/crossplane-terraform-provider && \
  chmod +x /usr/local/bin/crossplane-terraform-provider

USER 65532
ENTRYPOINT ["crossplane-terraform-provider"]
