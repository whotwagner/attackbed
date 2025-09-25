FROM debian:12

RUN apt-get update && \
    apt-get install -y dovecot-imapd dovecot-lmtpd dovecot-mysql postfix rsyslog vim mutt supervisor && \
    apt-get clean

# Add this line to set your mail domain
RUN echo "attackbed.com" > /etc/mailname

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

VOLUME ["/opt/mailconfig"]

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]