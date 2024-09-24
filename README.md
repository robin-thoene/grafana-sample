# grafana-sample

## About

This is a sample setup for running services on a single machine that includes:

- proxy
  - expose services to the public
  - load balance
  - HTTPS with "Let's Encrypt"
  - base auth for securing OTEL endpoints
- tracing backend
- Grafana for visualization

**This is just a sample and is not meant to be used as is in production!**

## Local setup

To run this sample locally follow these instructions.

### Prerequisites

#### Podman

You need to have podman installed. Furthermore the podman socket must be enabled for your current user. Assuming you are using Linux with systemctl,
you can do this with the following command:

```shell
systemctl --user start podman.socket
```

#### Environment variables

This setup needs a set of environment variables to be set. You can do this by creating a new `.env` file in the projects root directory. The
environment files content can be initialized with the following (insert your specific values!):

```shell
# Values needed to enable the Azure AD SSO login for the Grafana server.
GRAFANA_AZURE_AD_TENANT_ID=
GRAFANA_AZURE_AD_CLIENT_ID=
GRAFANA_AZURE_AD_CLIENT_SECRET=
OTEL_EXPORTER_OTLP_TRACES_HEADERS="Authorization=Basic YOUR_VALUE"
OTEL_EXPORTER_OTLP_LOGS_AUTH_HEADER_VALUE="YOUR_VALUE"
```

#### Base auth

To enable the basic authentication middleware that secures the OTEL endpoints, you need to create a new file `basicauth_users` in the projects root
directory. Inside this file you can specify the USERNAME:PASSWORD value pair. This pair can be generated using this command:

```shell
htpasswd -n USERNAME
```

An example content inside this file could look like this:

```shell
test:$apr1$PPUeXmv4$N6awkJY4biWwKFeJ/KslA.
```

This line corresponds to a user **test**, with a password **test**.

### Starting the sample

To start the sample, be sure you have followed the [prerequisites](### Prerequisites) first. Then use the following command:

```shell
podman-compose -f base_compose.yml up -d
```

To start a sample application that produces traces:

```shell
dotnet run --project ./dotnet-api/DotnetApi.csproj
```
