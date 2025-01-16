# Mosquitto MQTT Broker

Eclipse Mosquitto is an open source (EPL/EDL licensed) message broker that implements the MQTT protocol and is a good choice to use in an Arrowhead Local Cloud when MQTT is required.

## Install

For the installation possibilities please consult with their [official site](https://mosquitto.org/download/).

## SSL with Arrowhead Certificate

Using [Arrowhead compliant broker certificate](../certificate-profiles.md#broker-profile) makes your MQTT Broker part of your Local Cloud when secure network communication (SSL) is required. However the certificates in the default `PKCS#12` format (`broker.p12` file for example) can't be directly utilized by the Eclipse Mosquitto. It requires a separated CA certificate file, a public certificate file and a private key file what you can extract from your `PKCS#12` file with the help of the following [openssl library](https://openssl-library.org/) commands:

**Extract the CA certificate:**

```
openssl pkcs12 -in your-broker-certificate.p12 -cacerts -nokeys -out ca.crt
```

Now you can configure the `ca.crt` file as your broker's CA certificate.

**Extract the public certificate:**

```
openssl pkcs12 -in your-broker-certificate.p12 -clcerts -nokeys -out public.crt
```

Now you can configure the `public.crt` file as your broker's public certificate.

**Extract the private key:**

```
openssl pkcs12 -in your-broker-certificate.p12 -nocerts -out private.key
```
- You may be prompted to enter the password for the `.p12` file.
- You will be prompted to add a passphrase to the extracted key.

Now you can configure the `private.key` file as your broker's private key.

Mosquitto will prompt for the private key password on startup. To avoid this (in secure environments), re-export the private key without a password:

```
openssl rsa -in private.key -out private.key
```

**At the end of this process your mosquitto configuration file should contain something similar:**

```
cafile /path/to/ca.crt
certfile /path/to/public.crt
keyfile /path/to/private.key
```