# miyoomini-sqlite3

This is a build of the [sqlite3](https://sqlite.org/download.html) source files for the Miyoo Mini.

## Setup

To use sqlite in a Miyoo Mini project, add the following to the [union-miyoomini-toolchain](https://github.com/shauninman/union-miyoomini-toolchain):

Add two lines to the [`./Dockerfile`](https://github.com/shauninman/union-miyoomini-toolchain/blob/main/Dockerfile):

        zip \
        sqlite3 \                    <<< add this line
      && rm -rf /var/lib/apt/lists/*
    
    RUN mkdir -p /root/workspace
    WORKDIR /root
    
    COPY support .
    RUN ./setup-toolchain.sh
    RUN cat setup-env.sh >> .bashrc
    RUN ./setup-sqlite.sh            <<< add this line
    
Add a script at [`./support/`](https://github.com/shauninman/union-miyoomini-toolchain/tree/main/support)`setup-sqlite.sh`:
```
#!/bin/sh
wget https://www.sqlite.org/2022/sqlite-autoconf-3390000.tar.gz
tar xvzf sqlite-autoconf-3390000.tar.gz
cd sqlite-autoconf-3390000/

./configure --host=arm-linux --prefix=/opt/miyoomini-toolchain/usr/arm-linux-gnueabihf/sysroot/usr CC=/opt/miyoomini-toolchain/usr/bin/arm-linux-gnueabihf-gcc

make
make install
```

Remember to run `chmod a+x ./support/setup-sqlite.sh` to make it executable.
