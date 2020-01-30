# German National Hosting Plugins
Plugin scaffolding for the German National Hosting Plugins

### Checkout the sample gnh-plugin
 - `git clone https://github.com/lockss/gnh-plugins.git`\
   `git checkout develop`

 - Plugin files are located in the src and test trees under org/lockss/plugin/gnh.
 - Additional files can be added for different plugins by adding them in folders below that root.
   
 - The early alpha release is missing support for json plugins and the missing support is found in the clockss directory.

### Creating a Signing Key

 - To build plugins to be used in a production environment, a private
   signing key must be created.  For development and testing in a
   development environment no key is needed and this section may be skipped.

 - In addition to the signing key, a public certificate will be generated,
   which will be used by the LOCKSS system to validate plugins.  If
   standard LOCKSS-supplied plugins will also be used, this certificate
   should be combined with the existing certificates in the standard
   LOCKSS-supplied keystore.

 - Checkout the develop branch of the lockss-core project:\
	`git clone https://github.com/lockss/lockss-core.git`\
	`cd lockss-core`\
	`git checkout develop`

 - If combining certs, copy src/main/java/org/lockss/plugin/lockss.keystore
   to some file \<cert_file\>.

 - Generate a signing keypair by running `./test/scripts/genkey`.  (-h for help)
    - Supply key details on command line or answer prompts.
    - Use a secure passphrase.
    - To "LOCKSS Keystore location:" answer \<cert_file\> from above, or a new keystore file if not combining with the standard keystore.
	- To "LOCKSS Keystore password" accept the default with \<enter\>.  This is a public keystore and doesn't need a secure password.
	- To "Trust this certificate?" answer "yes".
	- The generated private keystore file (default \<alias\>.keystore), and
      the alias will be used when building plugins.
	- The cert file must be made available to the production LOCKSS services.  

### Building and Signing Plugins.

 - In the gnh-plugins directory:
    - For development: `mvn package -Dtesting`\.  This will create an plugin signed with a non-secret key, suitable for use in the development environment.
    - For production: `mvn package -Dkeystore.location=<private_keystore_file> -Dkeystore.alias=<alias>`.  Supply the keystore passphrase when prompted, or on the command line with `-Dkeystore.password=<password>`.
    - The plugin jar(s) will be written to target/pluginjars .

### Using Plugins.

 - To load a signed or unsigned plugin into the development and testing
   environment, pass the path to the plugin jar to runcluster with `-jar
   <plugin_jar>`.  Either org.lockss.plugin.registryJars or
   org.lockss.plugin.registry must also be set.  See
   [runcluster](https://github.com/lockss/laaws-dev-scripts/tree/develop/runcluster)
   for details.

 - For production:
    - Copy target/pluginjars/*.jar to plugin registry dir on your prop server.
    - Copy keystore \<cert_file\> to \<cert_keystore_url\> on prop server.
    - In lockss.xml, set org.lockss.plugin.keystore.location to \<cert_keystore_url\>.

### Testing Plugins.

- See the testing framework in the runcluster directory of the [laaws-dev-scripts project](https://github.com/lockss/laaws-dev-scripts/tree/develop/runcluster).
