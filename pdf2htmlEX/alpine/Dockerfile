FROM alpine:3.8 as build

ENV REFRESHED_AT 20181027

RUN apk --update add alpine-sdk xz poppler-dev pango-dev m4 libtool perl autoconf automake coreutils python-dev zlib-dev freetype-dev glib-dev cmake 
RUN cd / && \
    git clone https://github.com/BWITS/fontforge.git && \
    cd fontforge && \
    ./bootstrap --force && \
    ./configure --without-iconv && \
    make && \
    make install
RUN cd / && \
    git clone git://github.com/coolwanglu/pdf2htmlEX.git && \
    cd pdf2htmlEX && \
    export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig && \
    cmake . && make && sudo make install
RUN cd / && \
    git clone https://github.com/source-foundry/ttfautohint-build.git && \
    cd ttfautohint-build && make && make install && cd / &&\


FROM alpine:3.8
    
RUN apk add libpng python freetype glib libintl libxml2 libltdl cairo poppler pango curl imagemagick && \
    rm -rf /var/lib/apt/lists/* && \
    rm /var/cache/apk/* && \
    rm -rf /fontforge /libspiro /libuninameslist /pdf2htmlEX /ttfautohint-build

COPY --from=build /usr/local /usr/local/

VOLUME /pdf
WORKDIR /pdf
COPY convertepub.sh finishepub.sh /
RUN chmod a+x /convertepub.sh /finishepub.sh

CMD ["/usr/local/bin/pdf2htmlEX"]
