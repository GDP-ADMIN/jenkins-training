FROM debian:wheezy
MAINTAINER Roy Inganta Ginting <roy.i.ginting@gdplabs.id>

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && \
    apt-get install -y openjdk-7-jdk ant unzip zip wget git curl tree php5-cli php5-fpm \
    php5-dev php-pear php5-mysql php5-mcrypt php5-gd php5-curl php5-xsl && \
    rm -rf /var/lib/apt/lists/*

# PHP QA tools
ENV PHAR_URLSRC https://phar.phpunit.de
RUN curl $PHAR_URLSRC/phpunit-4.1.6.phar -o /usr/local/bin/phpunit && \
    chmod +x /usr/local/bin/phpunit && \
    curl $PHAR_URLSRC/phploc-2.0.5.phar -o /usr/local/bin/phploc && \
    chmod +x /usr/local/bin/phploc && \
    curl $PHAR_URLSRC/phpcpd-2.0.1.phar -o /usr/local/bin/phpcpd && \
    chmod +x /usr/local/bin/phpcpd && \
    curl $PHAR_URLSRC/phpdcd-1.0.2.phar -o /usr/local/bin/phpdcd && \
    chmod +x /usr/local/bin/phpdcd && \
    curl http://static.pdepend.org/php/2.0.3/pdepend.phar -o /usr/local/bin/pdepend && \
    chmod +x /usr/local/bin/pdepend && \
    curl http://static.phpmd.org/php/2.1.3/phpmd.phar -o /usr/local/bin/phpmd && \
    chmod +x /usr/local/bin/phpmd && \
    curl http://phpdox.de/releases/phpdox.phar -o /usr/local/bin/phpdox && \
    chmod +x /usr/local/bin/phpdox && \
    curl http://get.sensiolabs.org/security-checker.phar -o /usr/local/bin/php-security-checker && \
    chmod +x /usr/local/bin/php-security-checker && \
    curl http://get.sensiolabs.org/php-cs-fixer.phar -o /usr/local/bin/php-cs-fixer && \
    chmod +x /usr/local/bin/php-cs-fixer && \
    curl https://github.com/Mayflower/PHP_CodeBrowser/releases/download/1.1.1/phpcb-1.1.1.phar -o /usr/local/bin/phpcb && \
    chmod +x /usr/local/bin/phpcb

RUN pear install PHP_CodeSniffer && \
    pecl install xdebug && \
    echo "zend_extension=`php-config --extension-dir`/xdebug.so" > /etc/php5/mods-available/xdebug.ini && \
    php5enmod xdebug && \
    php5enmod mcrypt

ENV JENKINS_VERSION 1.565.3
ENV JENKINS_HOME /var/jenkins_home

ADD init.groovy /tmp/WEB-INF/init.groovy.d/tcp-slave-angent-port.groovy
ADD jenkins.sh /usr/local/bin/jenkins.sh

RUN mkdir /usr/share/jenkins/ && \
    useradd -d "$JENKINS_HOME" -m -s /bin/bash jenkins && \
    curl -L http://updates.jenkins-ci.org/download/war/$JENKINS_VERSION/jenkins.war -o /usr/share/jenkins/jenkins.war && \
    cd /tmp && \
    zip -g /usr/share/jenkins/jenkins.war WEB-INF/init.groovy.d/tcp-slave-angent-port.groovy && \
    rm -rf /tmp/WEB-INF && \
    chmod 644 /usr/share/jenkins/jenkins.war && \
    chmod 755 /usr/local/bin/jenkins.sh

VOLUME $JENKINS_HOME

EXPOSE 8080
EXPOSE 50000

USER jenkins

ENTRYPOINT ["/usr/local/bin/jenkins.sh"]
