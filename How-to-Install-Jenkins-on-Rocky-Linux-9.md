Gist for https://youtu.be/2-L0WohfsqY

# Install Temurin JDK

From [CentOS/RHEL/Fedora Instructions](https://adoptium.net/installation/linux/#_centosrhelfedora_instructions):

* Create RPM repository

```
cat <<EOF > /etc/yum.repos.d/adoptium.repo
[Adoptium]
name=Adoptium
baseurl=https://packages.adoptium.net/artifactory/rpm/${DISTRIBUTION_NAME:-$(. /etc/os-release; echo $ID)}/\$releasever/\$basearch
enabled=1
gpgcheck=1
gpgkey=https://packages.adoptium.net/artifactory/api/gpg/key/public
EOF
```

* Install Temurin 11 (this will fail)
  * `dnf -y install temurin-11-jdk`

* Modify `/etc/yum.repos.d/adoptium.repo`
  * change `rocky/$releasever` to `rocky/8`

* Install Temurin 11 (this will succeed)
  * `dnf -y install temurin-11-jdk`


# Install Jenkins

From [Jenkins Redhat Packages](https://pkg.jenkins.io/redhat-stable/):

* Create RPM repository
  * `wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo`

* Add repository key
  * `rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key`

* Install Jenkins
  * `dnf -y install jenkins`

# Configure Jenkins

* `systemctl --full status jenkins`
* `systemctl edit jenkins`

```
[Service]
Environment="JAVA_OPTS=-Djava.awt.headless=true -Djava.net.preferIPv4Stack=true -Djava.io.tmpdir=/var/cache/jenkins/tmp/ -Dorg.apache.commons.jelly.tags.fmt.timeZone=America/New_York -Duser.timezone=America/New_York"
Environment="JENKINS_OPTS=--pluginroot=/var/cache/jenkins/plugins"
```

* `mkdir -p /var/cache/jenkins/tmp`
* `chown -R jenkins:jenkins /var/cache/jenkins/tmp`
* `systemctl show jenkins`
* `systemd-analyze verify jenkins.service`
* `systemctl start jenkins`
* `systemctl --full status jenkins`
* `journalctl -u jenkins`