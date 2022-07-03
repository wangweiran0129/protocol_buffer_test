# Protocol Buffer Intallation/Compilation (Java Version)
This repo demonstrates how I installed and compiled the Protocol Buffers in Java vesion, though I could still not import the systematical **com.google.protobuf** package. What I can guarantee is that I checked almost all the websites which could help me solve the problems including the Google Protocol Buffer official website, Stack Overflow and writing an email to Fabien Geyer. I also tried on different versions of `gradle` and `protoc`.

## [pbzlib-java by Fabien Geyer](https://github.com/fabgeyer/pbzlib-java)
In his README.md, he claimed that this library is used for simplifying the serialization and deserialization of [protocol buffer](https://developers.google.com/protocol-buffers/) objects to/from files. This is also the first step I tried to read and write data in Java.  

### Prerequisite:
- gradle (The installation of gradle can be found [here](https://gradle.org/install/))

### Specific Steps:
1. `$ git clone https://github.com/fabgeyer/pbzlib-java.git`
2. `$ cd pbzlib-java`
3. `$ gradle build`

### The Problems I Met and Some Measures I've done:

1. I first used the latest version of gradle, and there was a "BUILD FAILED" error. If you take a look at the `build.gradle` file, you will find it's quite old, and some keywords e.g., **compile** in **dependencies** has been replaced by **implementation** in the latest gradle version. The last git push of this repo was actually 2 years ago.

2. I tried to fix the `build.gradle` but failed. Therefore, I wrote an email to Fabien Geyer. In his reply, quoted "it was successfully tested with the version of Gradle packaged in Ubuntu 20.04, meaning Gradle v.4.4. It should also work Ubuntu 22.04". However, **gradle v4.4** couldn't work on my Mac (macOS Monterey with an Intel Chip). I tried some other gradle 4.* versions, and as far as I tested, **gradle 4.10.3** is the only version which can build successfully on my computer.

3. However, the package **com.google.protobuf.DynamicMessage** could still not be imported. I investigated some days online and also wrote another email to Fabien Geyer, but I received no reply. Considering this FOSS is for simplification reason, I thus switched to the original Protocol Buffer.

## [Protocol Buffer by Google](https://developers.google.com/protocol-buffers/)

### Specific Steps:

1. Download a pre-built binary from the release page. To match the protocol buffer in Geyer's `build.gradle` file, I downloaded [Protocol Buffers v3.11.0](https://github.com/protocolbuffers/protobuf/releases/tag/v3.11.0).

2. Follow the README.txt in the downloaded zip file.

3. Check whether the protocol buffer has been installed successfully by typing `protoc --version` in the terminal.

4. I remember the installation for the older release of protobuf compiler above has some compilation bugs, so I found one post on stack overflow [Installing Google Protocol Buffers on Mac](https://stackoverflow.com/questions/21775151/installing-google-protocol-buffers-on-mac) and also refered to the Homebrew official website on [how to install protobuf on mac](https://formulae.brew.sh/formula/protobuf@3.6#default).

5. Check whether the protocol buffer has been installed successfully by typing `protoc --version` in the terminal.

### Coding Examples:
In this repo, you will find one proto file which defines the data structure of the datasets, [`addressbook.proto`](https://github.com/wangweiran0129/protocol_buffer_test/blob/master/address_book/addressbook.proto), and two java codes [`AddPerson.java`](https://github.com/wangweiran0129/protocol_buffer_test/blob/master/address_book/AddPerson.java) which writes the data, and [`ListPeople.java`](https://github.com/wangweiran0129/protocol_buffer_test/blob/master/address_book/ListPeople.java) which reads the data. All of these three codes are given by [Protocol Buffer Basics: Java Tutorial](https://developers.google.com/protocol-buffers/docs/javatutorial). If `protoc` has been successfully installed on your computer, you will be able to run the compiler by the following command: 

`$ protoc -I=$SRC_DIR --java_out=$DST_DIR $SRC_DIR/addressbook.proto`  

This will generate a `com/example/tutorial/protos` subdirectory in the specified destination directory defined above, containing a few generated `.java` files.


### The Problems I Met and Some Measures I've done:

1. Two functions/methods in `AddPerson.java`: `mergeFrom(Message)` and `writeTo(CodeOuputStream)` from the type AddressBook.Builder referes to the missing type Message. If you switch to `AddressBook.java`, the same problem, i.e., the package `com.google.protobuf` does not exist.

2. I referred to the solution: [Compilation error of simple java files with Google Protocol Buffer](https://stackoverflow.com/questions/7095675/compilation-error-of-simple-java-files-with-google-protocol-buffer), but it didn't solve my problem:

    - `$ mdfind descriptor.proto` to find location of `descriptor.proto` and `cd` to that directory.
    - Run the compiler `$ protoc -I=$SRC_DIR --java_out=$DST_DIR $SRC_DIR/descriptor.proto`  

3. I also installed the [Protobuf Runtime Installation](https://github.com/protocolbuffers/protobuf#protocol-compiler-installation), but I couldn't `make java`, which:

    - Download [protobuf Java runtime .jar file](https://mvnrepository.com/artifact/com.google.protobuf/protobuf-java) from maven.
    - `export CLASSPATH=/path/to/protobuf-java-[version].jar`
    - `make java`

4. Later on, I upgraded both my `gradle` and `protoc` to the latest versios, but I could still not figure the problem out. The main problem is the package `com.google.protobuf`.