# JDK and libstdc++6 Upgrade Guide



## 1. Upgrading from JDK 17 to JDK 21

### Step 1. Check the current JDK version

```bash
java -version
```

### Step 2. Uninstall the old version (optional)

Depending on your operating system, choose whether to uninstall the old version of JDK.

Take Debian/Ubuntu as an example:

```bash
sudo apt remove openjdk-17-jdk
```

### Step 3. Install JDK 21

#### Ubuntu/Debian

Install using apt (if JDK 21 source is available):

```bash
sudo apt update
sudo apt install openjdk-21-jdk
```

#### CentOS/RHEL

```bash
sudo dnf install java-21-openjdk-devel
```

### Step 4. Verify the installation

```bash
java -version
```

The output should contain `openjdk version "21"`.



## 2. Update libstdc++.so.6 in the system

### Step 1. Check the current version

```bash
strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX
```

or：

```bash
ldd --version
```

### Step 2. Upgrade GCC and automatically upgrade libstdc++

#### Ubuntu/Debian

```bash
sudo apt update
sudo apt install libstdc++6
```

If the system repository version is too old, it is recommended to install the new version of gcc from Ubuntu toolchain PPA:

```bash
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt update
sudo apt install libstdc++6
```

#### CentOS/RHEL

New versions of GCC can be built using `yum` or manually:

```bash
sudo yum install libstdc++ libstdc++-devel
```

### Step 3. Confirm that the upgrade is successful

```bash
strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX
```

You should see a later version of GLIBCXX, such as `GLIBCXX_3.4.32`.
