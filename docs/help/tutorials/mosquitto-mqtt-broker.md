# Mosquitto MQTT Broker

Eclipse Mosquitto is an open source (EPL/EDL licensed) message broker that implements the MQTT protocol and is a good choice to use in an Arrowhead Local Cloud when MQTT is required.

## Install

For the installation possibilities please consult with their [official site](https://mosquitto.org/download/).

## Access Control

In a properly secured Local Cloud, when MQTT is required, the access to the broker and topics should also be controlled. Connecting clients should be authenticated and reading/writing of topics for service providing should be limited to the actual service provider clients.

### Client authentication

In order to set up client authentication a "password file" has to be created which contains user name and password combinations for the clients. 

- It is recommended that Arrowhead Core Systems have unique credentials.
- It is recommended that service providing application systems have unique credentials.
- It is acceptable that application systems with service consumption purpuse only, share a common credential.

You can manage the users by using the `mosquitto_passwd` command (which comes with the broker installation).

**Create the password file with the first user:**

```
mosquitto_passwd -c /etc/mosquitto/users <username>
```

You will be prompted to set a password and the `users` file will be created. Only the hashed version of the password is stored in the file.

**Add additional users to the file (without overwriting it):**

```
mosquitto_passwd /etc/mosquitto/users <username>
```

**Modify the broker configuration:**

The Mosquitto configuration file usually located at `/etc/mosquitto/mosquitto.conf`. To enable password authentication add the following lines:

```
allow_anonymous false
password_file /etc/mosquitto/users
```

After making changes, always restart the broker!

### Topic read control

Having read or write access control on the topics used for service providing ensures, that only the actual service providers are allowed

- to write to a publish kind service topic, and
- to read a request-response kind service topic.

**Create an access control file**

Create an `accesctrl` file under the `/etc/mosquitto` folder with a similar content:

```
# Arrowhead Core Systems

user serviceregistry
topic read arrowhead/serviceregistry/system-discovery
topic read arrowhead/serviceregistry/service-discovery

...

# Publish kind service topics

user <username_a>
topic write <its/specific/service/topic/>

user <username_b>
topic write <its/specific/service/topic/>

...

# Request-response kind service topics

user <username_c>
topic read <its/specific/service/topic/>

user <username_d>
topic read <its/specific/service/topic/>

...

```

**Modify the broker configuration:**

The Mosquitto configuration file usually located at `/etc/mosquitto/mosquitto.conf`. To enable topic access control add the following line:

```
acl_file /etc/mosquitto/accesctrl
```

After making changes, always restart the broker!

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

**At the end of this process your Mosquitto configuration file should contain something similar:**

```
cafile /path/to/ca.crt
certfile /path/to/public.crt
keyfile /path/to/private.key
```

After making changes, always restart the broker!