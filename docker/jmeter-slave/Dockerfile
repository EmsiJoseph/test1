# Use a base image with Java pre-installed (OpenJDK 8)
FROM openjdk:8

# Set the JMeter version as a build argument (default is 5.6.3)
ARG JMETER_VERSION=5.6.3
ARG CMDRUNNER_JAR_VERSION=2.3
ARG JMETER_PLUGINS_MANAGER_VERSION=1.10
ENV JMETER_HOME=/opt/apache-jmeter
ENV JMETER_LIB_FOLDER=${JMETER_HOME}/lib/
ENV JMETER_PLUGINS_FOLDER=${JMETER_LIB_FOLDER}ext/

WORKDIR ${JMETER_HOME}
RUN  apt-get -y update \
    && apt-get install -y wget gnupg curl

# Install JMeter
RUN wget https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-${JMETER_VERSION}.tgz && \
    tar -xzf apache-jmeter-${JMETER_VERSION}.tgz && \
    mv apache-jmeter-${JMETER_VERSION}/* /opt/apache-jmeter && \
    rm -r apache-jmeter-${JMETER_VERSION}

WORKDIR ${JMETER_PLUGINS_FOLDER}
RUN wget https://repo1.maven.org/maven2/kg/apc/jmeter-plugins-manager/${JMETER_PLUGINS_MANAGER_VERSION}/jmeter-plugins-manager-${JMETER_PLUGINS_MANAGER_VERSION}.jar

# Download Command Runner and move it to lib folder
WORKDIR ${JMETER_LIB_FOLDER}
RUN wget https://repo1.maven.org/maven2/kg/apc/cmdrunner/${CMDRUNNER_JAR_VERSION}/cmdrunner-${CMDRUNNER_JAR_VERSION}.jar


# Install JMeter plugins from the specified text file
COPY plugins.txt /plugins.txt
RUN cat /plugins.txt | tr ',' '\n' | xargs java -jar cmdrunner-${CMDRUNNER_JAR_VERSION}.jar --tool org.jmeterplugins.repository.PluginManagerCMD install

# Set environment variables
ENV PATH="$JAVA_HOME/bin:${PATH}"
ENV PATH="$JMETER_HOME/bin:${PATH}"

WORKDIR ${JMETER_HOME}/bin
RUN cd . > jmeter.log

WORKDIR ${JMETER_HOME}/bin
RUN cd . > jmeter.server.log

WORKDIR ${JMETER_HOME}/bin

RUN ln -sf /dev/stdout ./jmeter-server.log
ENTRYPOINT ["sh", "-c", "sed -i 's/^/jmeter-server.log: /' ./jmeter-server.log && \
 ./jmeter-server -Dserver.rmi.localport=1099 -Dserver_port=1099 -Jserver.rmi.ssl.disable=true | \
  sed 's/^/jmeter-server: /'"]
