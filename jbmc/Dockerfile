FROM openjdk:8u131-jdk-alpine

# Compilation dependencies
RUN apk add --no-cache apkbuild-cpan gcc g++ make git bison flex \
      perl-libwww unzip perl-lwp-protocol-https bash ccache parallel \
      nano emacs vim

# Clone CBMC repo
RUN git clone https://github.com/diffblue/cbmc.git 

# Compile CBMC/JBMC stuff
RUN mkdir -p /cbmc/src/ipasir \
    && cd /cbmc/src \
    && make minisat2-download \
    && make \
    && cd ../ \
    && mkdir ipasir

ADD ipasir/* /cbmc/ipasir/

WORKDIR /cbmc

RUN cd /cbmc/ \
  && make -C src IPASIR=../../ipasir LIBSOLVER=$(pwd)/ipasir/libipasir.a

ENV PATH $PATH:/cbmc/src/jbmc/

CMD bash