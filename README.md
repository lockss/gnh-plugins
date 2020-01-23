# German National Hosting Plugins
Plugin scaffolding for the German National Hosting Plugins

#### Checkout the sample gnh-plugin
	`git clone https://github.com/lockss/gnh-plugins.git`

 - Plugin files are located in the src and test trees under org/lockss/plugin/gnh.
 - Additional files can be added for different plugins by adding them in folders below that root.
 - You need to generate a keystore and replace the path to the anon.keystore values in the pom file to match the path and name used in the the generate step below.
 - When building, you will be asked for the password to your keystore to sign the plugin jars.
 - The early alpha release is missing support for json plugins and the missing support is found in the clockss directory.

#### Creating a Keystore and Signing Key

- Checkout the lockss-core project:
	`git clone https://github.com/lockss/lockss-core.git`

- If you want to load standard LOCKSS plugins in addition to your own:
 1. Copy of src/main/java/org/lockss/plugin/lockss.keystore to top level as anon.keystore.
 2. Generate a signing keypair by running ./test/scripts/genkey on develop branch to avoid warnings about proprietary keystore type.  Generates
    - Supply key details on command line or answer prompts.  
    - Use a secure password for this part.
    - To "Import certificate into LOCKSS public keystore?" answer "y".
    - To "LOCKSS Keystore location:" answer anon.keystore from above, or a new keystore file if not merging with standard keystore.
    - To "LOCKSS Keystore password" accept the default with <enter>.  This is a public keystore and doesn't need a secure password.
    - To "Trust this certificate?" answer "yes".
 3. Modify the pom file to reflect the name of the keystore.
    - If you have kept the name anon.keystore you do not need to modify the pom.
    - <keystore.file>${basedir}/anon.keystore</keystore.file>
    - <keystore.alias>anon</keystore.alias>

### Building and Signing Plugins.

 - In gnh-plugins directory:
    - `mvn package`
    - Follow the prompts and provide a password when requested.

#### Moving Plugins to Prop Server

- Copy target/pluginjars/*.jar to plugin registry dir on your prop server.
- Copy <FOO>.keystore to KEYSTORE_URL on prop server.
- In lockss.xml, set org.lockss.plugin.keystore.location to KEYSTORE_URL.

### Testing Plugins.

- The testing framework can be found in the github project. See http://github.com/lockss/laaws-dev-scripts.


