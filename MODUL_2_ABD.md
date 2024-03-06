---
Dates: 05/03/2024
Author: Abdurrahman Al-atsary
---

**Installasi Hive + HiveQL dan Apache Derby (Metastore)**

_**Overview**_

Infrastruktur Hive merupakan simbolis sebagai pengolahan data di dalam Data warehouse (Gudang Data). Dengan kata lain data-data yang ada didalam file HDFS (Hadoop Distributed File System) akan dapat diolah jika adanya hive ini data data dapat diolah dengan baik, dikarnakan hive merupakan bahasa yang dipakai RDBMS sama seperti MySQL dan lainnya.

Ikuti Langkah berikut untuk penginstallan di dalam WSL maupun Linux Ubuntu:

1. Periksa hadoop sudah ada didalam OS anda dengan cara : `hadoop version`

2. Install source yang akan dibutuhkan didalam penginsallan Hive dan Metadata Apache Derby

Link : [Apache Derby](https://db.apache.org/derby/releases/release-10_14_2_0.html), [Apache Hive](https://dlcdn.apache.org/hive/)

3. Lalu, lakukan untar pada kedua file diatas dengan cara ketikan baris perbaris.

```bash
tar -zxvf apache-hive-3.1.2-bin.tar.gz
tar -zxvf db-derby-10.14.2.0-bin.tar.gz
```

4. Lakukan pengeditan didalam lingkungan linux/WSL di file `.bashrc` dengan mengetikan `sudo nano .bashrc`

5. Cari konfigurasi dari path hadoop sebelumnya, kemudian dibawahnya ditambahkan skrip berikut :

```bash
export HIVE_HOME="/home/miroslav/apache-hive-3.1.2-bin"

export PATH=$PATH:$HIVE_HOME/bin
```

**Notes : Ubah bagian "miroslav" dari nama username di dalam unix kalian.** kemdudian save dengan `ctrl + s` setalah itu `crtl + x` untuk keluar.

6. Kemudian kita kedalam folder dari hadoop : `cd hadoop-3.3.6/etc/hadoop`. Lalu kita cari didalam list filenya `core-site.xml`

7. Buka `core-site.xml` pastikan sudah ada script xml berikut didalam tag `<configuration>`:

```xml
<property>
    <name>hadoop.proxyuser.dikshant.groups</name>
    <value>*</value>
</property>
<property>
    <name>hadoop.proxyuser.dikshant.hosts</name>
    <value>*</value>
</property>
<property>
    <name>hadoop.proxyuser.server.hosts</name>
    <value>*</value>
</property>
<property>
    <name>hadoop.proxyuser.server.groups</name>
    <value>*</value>
</property>
```

8. selanjutnya kita hidupkan server hadoop terlebih dahulu : `start-all.sh`

9. Kemudian buat beberapa folder dan file yang ada didalam File system hadoop seperti berikut :

```bash
hdfs dfs -mkdir /tmp
hdfs dfs -mkdir /user
hdfs dfs -mkdir /user/hive
hdfs dfs -mkdir /user/hive/warehouse
```

**Notes : Lakukan baris per baris**

**Adding information** : Untuk pengecekan apakah direktori/folder sudah dibuat atau tidak dapat mengetikan `hdfs dfs -ls -R / `

10. Lakukan permission didalam untuk folder-folder yang ada didalam hadoop dengan cara :

```bash
hdfs dfs -chmod ugo+rwx /tmp
hdfs dfs -chmod ugo+rwx /user/hive/warehouse
```

11. Guava library issue

Kita dapat membuang guava yang ada didalam hive dan mengganti agar sama versinya dengan guava yang ada dalam hadoop dengan

```bash
rm $HIVE_HOME/lib/guava-19.0.jar
cp $HADOOP_HOME/share/hadoop/common/lib/guava-27.0-jre.jar $HIVE_HOME/lib/
```

**Notes : Ikuti baris per baris**

12. kemudian server hive dan apache derby dapat digunakan dengan cara `hive` kan berjalan dengan baik didalam server hadoop.
