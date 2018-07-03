![UPPERSAFE](https://web.uppersafe.com/resources/images/uppersafe-color.svg)

# *UPPERSAFE Open Source Firewall*

[![Build status](https://travis-ci.org/dev2lead/uppersafe-osfw.svg?branch=master)](https://travis-ci.org/dev2lead/uppersafe-osfw) [![Python 3.4|3.5|3.6](https://img.shields.io/badge/python-3.4|3.5|3.6-yellow.svg)](https://www.python.org)

OSFW is a firewall, fully written in Python 3, that provides an IP / domain filtering based on a collection of threat intelligence public feeds.

It blocks in real time incoming and outcoming traffic considered as *malicious* (matching the filtering rules automatically set up for each threat).

It also provides a secure DNS service that blocks different kind of *malicious servers* (phishing websites, malware hosting, malvertising, C&C servers, etc).

## Components

OSFW includes 3 main components:

|Name|Description|
|-|-|
|`osfw-sensor`|In charge of monitoring and logging the requests blocked by the firewall|
|`osfw-syncfw`|In charge of collecting and syncing the threat intelligence feeds|
|`osfw-webapp`|In charge of managing the web interface|

## Quick start

Install the virtual environment:

    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt

Start the firewall components:

    bash run.sh

Attach a screen:

    screen -r osfw-sensor
    screen -r osfw-syncfw
    screen -r osfw-webapp

## Configuration

To enable the secure DNS service, simply create a symbolic link of the `unbound.conf` file to the unbound configuration directory with the following command:

    ln -s "$PWD/assets/unbound.conf" /etc/unbound/unbound.conf.d/osfw.conf

It is possible to customize the behaviour of the firewall by editing the default `config.yml` file.

One of the reasons you would want to edit this file is to unblock specific websites.
It happens that some legit and top ranked websites got blocked because of different purposes, most of the time one of the following:

- Their users can upload files on the main domain (file transfer providers or cloud storage providers)
- Their users can upload files or even web pages on a subdomain (hosting providers)
- Their users can perform URL redirect (link shortener websites)

To prevent these websites from being blocked, you can specify them as a list in the configuration file.

In case you want to edit the default list, you can use a magic keyword `.tld` that will match any top level domain and some specific second level domain names.
For example, `domain.tld` will match all of the following cases:

- `domain.uk`
- `domain.co.uk`
- `domain.com.uk`
- `domain.net.uk`
- `domain.org.uk`
- `domain.edu.uk`
- `domain.gov.uk`
- `domain.jp`
- `domain.co.jp`
- `domain.com.jp`
- `domain.net.jp`
- `domain.org.jp`
- `domain.edu.jp`
- `domain.gov.jp`

There is also a way to make a rule act as a subdomain wildcard, to do so you need to start the rule with a `.` such as the ones in the default configuration file.

## Dependencies

- python3 (see also `requirements.txt`)
- iptables
- unbound
- screen

## Support

Nicolas THIBAUT (nicolas[@]uppersafe[.]com)

https://www.patreon.com/dev2lead/memberships

## License

This software is provided under a GNU AGPLv3 License.
