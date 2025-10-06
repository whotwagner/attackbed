FROM debian:12

RUN apt-get update && \
    apt-get install -y dovecot-imapd dovecot-lmtpd dovecot-mysql postfix postfix-mysql rsyslog vim mutt supervisor && \
    apt-get clean

RUN echo "localhost" > /etc/mailname

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

VOLUME ["/opt/mailconfig"]

# Create vmail user mapped to your user_query uid/gid (uid=103, gid=8)
# GID 8 is typically "mail" on Debian; create user with that group.
RUN getent group 8 || groupadd -g 8 mail && \
    id -u vmail >/dev/null 2>&1 || useradd -u 103 -g 8 -d /var/mail -M vmail

# Mail storage and runtime dirs
RUN mkdir -p /var/mail && chown -R 103:8 /var/mail && chmod 0770 /var/mail
#RUN mkdir -p /var/spool/postfix /var/lib/postfix && chown -R postfix:postfix /var/spool/postfix /var/lib/postfix
RUN mkdir -p /var/lib/postfix /var/spool/postfix

# Postfix SASL socket path lives under /var/spool/postfix/private
RUN mkdir -p /var/spool/postfix/private && chown -R postfix:postfix /var/spool/postfix

EXPOSE 25 143 588

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]