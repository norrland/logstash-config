# Logstash configs

## Intro

Some collected snippets for configuring logstash to manage syslog messages
and pflog/tcpdump output.

### Syslogd

I collect logs from my OpenBSD hosts. To prepare this I've done as
following.

Add hostname to logs:

`$ doas rcctl set syslogd flags "-h"`

Add your logstash host as destination:
/etc/syslog.conf

`*.*   @tcp://logstash:5514`

### pflogd

As pflogd is storing the binary logs in /var/log/pflog, we want to parse
this with tcpdump.

tcpdump -> logger:

`# tcpdump -ttt -ner /var/log/pflog | logger -t pflog -p local0.notice`

-ttt will generate the correct timestamp for the parsing later on. logger
will send the message to syslog.
