dasein-cloud-joyent
===================

The Dasein Cloud Joyent submodule to the [Dasein Cloud](https://github.com/greese/dasein-cloud) project provides
an implementation of the Dasein Cloud API for the Joyent Cloud and the SDC platform.

* [Get started with Dasein Cloud](https://github.com/greese/dasein-cloud)
* [Get started with Dasein Cloud + Joyent](https://github.com/greese/dasein-cloud-joyent/wiki)

Installation
-------------------

### Maven

You need [maven](https://maven.apache.org/) to build this library. After you installed maven run set properties defined
in [Configuration](#configuration) or add '-DskipTests=true'. Then run:

    $ mvn clean install

Then you can use dasein-cloud-joyent/target/dasein-cloud-joyent-{version}.jar as an implementation of Dasein Cloud.

Usage
-------------------

Here is some basic usage of Dasein Cloud Joyent submodule:

### Add maven dependency

    <dependencies>
      <!-- API -->
      <dependency>
        <groupId>org.dasein</groupId>
        <artifactId>dasein-cloud-core</artifactId>
        <version>2014.07.1</version>
        <scope>compile</scope>
        <optional>false</optional>
      </dependency>
      <!-- implementation -->
      <dependency>
        <groupId>org.dasein</groupId>
        <artifactId>dasein-cloud-joyent</artifactId>
        <version>2014.07.1</version>
        <scope>runtime</scope>
        <optional>false</optional>
      </dependency>
    </dependencies>

### Set variables

Add or modify maven profile in pom.xml according your configuration.

    <profiles>
      <profile>
        <id>default</id>
        <activation>
          <activeByDefault>true</activeByDefault>
        </activation>
        <properties>
          <providerClass>org.dasein.cloud.joyent.SmartDataCenter</providerClass>
          <endpoint>https://us-west-1.api.joyentcloud.com</endpoint>
          <storageUrl>https://us-east.manta.joyent.com</storageUrl>
          <sshKeyShared>yourKeyName</sshKeyShared>
          <sshKeySecret>----BEGIN RSA PRIVATE KEY-----
                        your private RSA key
                        -----END RSA PRIVATE KEY-----</sshKeySecret>
          <sshKeyPassword>yourKeyPassword_Optional</sshKeyPassword>
          <accountNumber>yourAccount</accountNumber>
          <cloudName>Joyent Cloud</cloudName>
          <providerName>Joyent</providerName>
          <regionId>us-west-1</regionId>
          <proxyHost>yourProxyHost_Optional</proxyHost>
          <proxyPort>yourProxyPort_Optional</proxyPort>
        </properties>
      </profile>
    </profiles>

Or you can set system variables in Java using this snippet:

    Properties props = new Properties();
    InputStream inputStream = getClass().getResourceAsStream("dsn.properties");
    props.load(inputStream);
    for (Map.Entry<Object, Object> entry : props.entrySet()) {
        System.setProperty(String.valueOf(entry.getKey()), String.valueOf(entry.getValue()));
    }

dsn.properties:

    DSN_PROVIDER_CLASS=org.dasein.cloud.joyent.SmartDataCenter
    DSN_ENDPOINT=https://us-west-1.api.joyentcloud.com
    DSN_REGION=us-west-1
    DSN_ACCOUNT=your@account.id
    DSN_CLOUD_NAME=Joyent Cloud
    DSN_CLOUD_PROVIDER=Joyent
    DSN_CUSTOM_STORAGE_URL=https://us-east.manta.joyent.com
    DSN_sshKey_SHARED=YourKeyName
    DSN_sshKey_SECRET=-----BEGIN RSA PRIVATE KEY-----\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA==\n\
    -----END RSA PRIVATE KEY-----
    DSN_sshKeyPassword=

### Upload sample

    package org.example.dasein.storage;

    import org.dasein.cloud.CloudException;
    import org.dasein.cloud.InternalException;
    import org.dasein.cloud.examples.ProviderLoader;
    import org.dasein.cloud.storage.Blob;
    import org.dasein.cloud.storage.BlobStoreSupport;

    import java.io.File;
    import java.io.UnsupportedEncodingException;


    public class SimpleClientTest {
        private static final String FILE_PATH = "/path/to/you/file/file.txt";
        private static final String OBJECT_NAME = "file.txt";
        private static final String OBJECT_PATH = "/username/stor/1/";

        public static void main(String[] args) throws ClassNotFoundException, InstantiationException,
                UnsupportedEncodingException, IllegalAccessException, CloudException, InternalException {
            BlobStoreSupport storage = new ProviderLoader().getConfiguredProvider().getStorageServices().getOnlineStorageSupport();
            Blob uploaded = storage.upload(new File(FILE_PATH), OBJECT_PATH, OBJECT_NAME);
            System.out.print(uploaded);
        }
    }

For more examples, check the included unit tests.
