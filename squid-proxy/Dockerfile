FROM debian:8
RUN apt-get -y update
RUN apt-get install -y curl supervisor git openssl build-essential libssl-dev wget vim curl unzip autoconf libtool automake ed
WORKDIR /apps/
RUN wget -O squid-5.7.tar.gz http://www.squid-cache.org/Versions/v5/squid-5.7.tar.gz 
RUN tar xzf squid-5.7.tar.gz \
    && CPU=$(( `nproc --all`-1 )) \
    && cd /apps/squid-5.7/ \
    && ./configure --prefix=/apps/squid --enable-icap-client --enable-ssl --with-openssl --enable-ssl-crtd --enable-security-cert-generators=file --enable-auth --enable-basic-auth-helpers="NCSA" --with-default-user=nobody \
    && make -j$CPU \
    && make install \
    && cd /apps \
    && rm -rf /apps/squid-5.7
ADD . /apps/

RUN mkdir -p /var/log/supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY zyte-smartproxy-ca.crt /usr/local/share/ca-certificates/zyte-smartproxy-ca.crt
RUN update-ca-certificates

RUN chmod +x /apps/init_and_run.sh

EXPOSE 3128
EXPOSE 3129
