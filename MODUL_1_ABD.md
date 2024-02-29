# Instalasi Modul 1 ABD (Apache Hadoop)

1. Pertama, pastikan ada WSL didalam windows kalian.

- Jika tidak ada bisa dilakukan dengan :
  1. Buka Command Line (Administrator) : Ketikan `wsl --install`
  2. Ditunggu untuk penyelesaiannya lalu bisa buka **Microsoft Store**, cari **Ubuntu Versi 22.04** kemudian install.
  3. Tunggu hingga selesai semua instalasi.
  4. Lalu, Restart Laptop Anda.

2. Kedua, buka Ubuntu-nya yang ada didalam windows

3. Tunggu sebentar, lalu setup untuk username dan passwordnya

**Notes: Username dan Password harap dihafal karna sebagai kunci kita untuk setup nanti.**

4. Lalu installasi java dengan mengetikan pada command line : `sudo apt install openjdk-8-jdk --headless`

5. Kemudian ketik `java -version` untuk pengecekan apakah versi java yang terinstall yang sudah benar di versi 8.

6. Lalu buka directory dengan mengetikan : `cd /usr/lib/jvm` dan copas bagian folder javanya _**(Letakan didalam Notepad)**_.

7. Kemudian ketik dalam terminal kembali `cd` untuk balik ke homenya.

8. Kemudian,ketik `sudo nano .bashrc` untuk mengedit file .bashrc (Environment dari Linux)

9. Lalu edit dengan ke arah paling bawah dari semua code dan copas code berikut :

```txt
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$PATH:/usr/lib/jvm/java-8-openjdk-amd64/bin
export HADOOP_HOME=~/hadoop-3.3.6/
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
export HADOOP_STREAMING=$HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.3.6.jar
export HADOOP_LOG_DIR=$HADOOP_HOME/logs
export PDSH_RCMD_TYPE=ssh



```

10. Lalu, tambahkan baris berikut :
