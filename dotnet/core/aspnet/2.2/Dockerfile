FROM mcr.microsoft.com/dotnet/core/aspnet:2.2

ARG DT_ENVIRONMENT_ID=""
ARG DT_PAAS_TOKEN=""

RUN apt-get update && \
    apt-get install -y wget unzip && \
    mkdir -p /opt/dynatrace/oneagent && \
    if [ "$DT_ENVIRONMENT_ID" != "" ] && [ "$DT_PAAS_TOKEN" != "" ]; \
    then \
        echo "TESTING" && \
        TEMP_FILE=$(mktemp) && \
        wget --no-check-certificate -O $TEMP_FILE "https://$DT_ENVIRONMENT_ID.live.dynatrace.com/api/v1/deployment/installer/agent/unix/paas/latest?Api-Token=$DT_PAAS_TOKEN&include=dotnet" && \
        unzip -o -d /opt/dynatrace/oneagent $TEMP_FILE && \
        rm -f $TEMP_FILE ; \
    fi

ENV LD_PRELOAD ""
