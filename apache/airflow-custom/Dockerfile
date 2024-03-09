

User
FROM apache/airflow:2.7.3-python3.11
USER root
RUN mkdir -p /var/lib/apt/lists/partial
RUN apt-get update -y && \
    apt-get install libpq-dev -y && \
    apt-get install -y openjdk-11-jdk && \
    apt-get install -y ant && \
    apt-get install -y wget && \
    apt-get install -y procps && \
    apt-get install -y --no-install-recommends \
         gcc \
         heimdal-dev &&\
    apt-get clean;

ENV JAVA_HOME /usr/lib/jvm/java-11-openjdk-amd64/
RUN export JAVA_HOME

ENV AIRFLOW__CORE__TEST_CONNECTION=Enabled
ENV SPARK_VERSION=3.5.0 \
HADOOP_VERSION=3 \
SPARK_HOME=/opt/spark \
PYTHONHASHSEED=1

# Download and uncompress spark from the apache archive
RUN curl -sL --retry 3 \
    "https://archive.apache.org/dist/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz" \
    | tar xz -C /opt && \
    ln -s /opt/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} /opt/spark && \
    chown -R airflow: /opt/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}
ENV SPARK_HOME=/opt/spark
ENV  PATH=$PATH:$SPARK_HOME/bin
RUN export SPARK_HOME=/opt/spark
RUN export PATH=$PATH:$SPARK_HOME/bin
# RUN mkdir -p /var/lib/apt/lists/partial
RUN apt-get update -y 
    
USER airflow
COPY requirements.txt /
RUN pip install --upgrade pip
RUN pip install --no-cache-dir "apache-airflow==${AIRFLOW_VERSION}" -r /requirements.txt
RUN rm /open/airflow/dags -fr