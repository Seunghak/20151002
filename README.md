# 20151002

hadoop 설치 및 설정 방법

자바가 설치된 후에 hadoop 계정 추가하고 root에서 hadoop으로 옮겨간 후 명령어들을 실행
    useradd hadoop
    su - hadoop
    ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
    cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
    chmod 0600 ~/.ssh/authorized_keys
    
hadoop 다운로드 방법 (directory 생성 후에 URL을 이용하여 다운로드
    cd /home
    mkdir hadoop
    cd hadoop
    wget http://apache.tt.co.kr/hadoop/common/hadoop-2.7.1/hadoop-2.7.1.tar.gz
    tar -zxvf hadoop-2.7.1.tar.gz
    
컨피규어링
    vi $HOME/.bashrc vi로 밑에꺼 추가

    export JAVA_HOME=/usr/java/jdk1.8
    export HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar
    export HADOOP_HOME=/home/hadoop/hadoop-2.7.1
    export HADOOP_INSTALL=$HADOOP_HOME
    export HADOOP_MAPRED_HOME=$HADOOP_HOME
    export HADOOP_COMMON_HOME=$HADOOP_HOME
    export HADOOP_HDFS_HOME=$HADOOP_HOME
    export HADOOP_YARN_HOME=$HADOOP_HOME
    export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
    export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
    export JAVA_LIBRARY_PATH=$HADOOP_HOME/lib/native:$JAVA_LIBRARY_PATH
    
    실행방법 source $HOME/.bashrc
    
hadoop 설정값 추가할 것들
    vi $HADOOP_HOME/etc/hadoop/hadoop-env.sh 
    export JAVA_HOME=/usr/java/jdk1.8
    
    vi $HADOOP_HOME/etc/hadoop/core-site.xml

    <configuration> 
        <property>
            <name>fs.default.name</name>
            <value>hdfs://localhost:9000</value>
        </property>
    </configuration> 
    
    vi $HADOOP_HOME/etc/hadoop/hdfs-site.xml

    <configuration> 
        <property>
            <name>dfs.replication</name>
            <value>1</value>
        </property>

        <property>
            <name>dfs.name.dir</name>
            <value>file:///home/hadoop/hadoopdata/hdfs/namenode</value>
        </property>

        <property>
            <name>dfs.data.dir</name>
            <value>file:///home/hadoop/hadoopdata/hdfs/datanode</value>
        </property>
    </configuration> 
    
    cp $HADOOP_HOME/etc/hadoop/mapred-site.xml.template $HADOOP_HOME/etc/hadoop/mapred-site.xml
    
    vi  $HADOOP_HOME/etc/hadoop/mapred-site.xml

    <configuration> 
        <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
        </property>
    </configuration> 
    
    vi $HADOOP_HOME/etc/hadoop/yarn-site.xml

    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    
    set JAVA_HOME
    
hadoop 실행 시작
    hdfs namenode -format
    start-dfs.sh
    start-yarn.sh
    
테스트 문서 제작
    vim test.txt
    안에 아무 내용이나 생성

테스트 문서 생성 후 실행법
    hdfs dfs -mkdir /user   
    hdfs dfs -mkdir /user/hadoop  
    hdfs dfs -mkdir /user/hadoop/input  
    hdfs dfs -put test.txt /user/hadoop/input  
    
    wget https://raw.githubusercontent.com/kowonsik/CCL/master/WordCount.java

    hadoop com.sun.tools.javac.Main WordCount.java

    jar -cf wc.jar WordCount*.class

    hadoop jar wc.jar WordCount /user/hadoop/input /output
    
테스트 문서 실행한 결과 확인
    hdfs dfs -cat /output/part-r-00000



    

