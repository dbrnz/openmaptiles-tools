# Use a separate docker for downloading to minimize final docker image
FROM bash as downloader

ENV IMPORT_DATA_DIR=/import
RUN mkdir -p /import \
    && cd "$IMPORT_DATA_DIR" \
    && wget --quiet http://osmdata.openstreetmap.de/download/water-polygons-split-3857.zip \
    && unzip -oj water-polygons-split-3857.zip -d "$IMPORT_DATA_DIR" \
    && rm water-polygons-split-3857.zip


FROM osgeo/gdal:alpine-normal-3.0.3
LABEL maintainer="YuriAstrakhan@gmail.com"

ENV IMPORT_DATA_DIR=/import
ENV WATER_POLYGONS_FILE="${IMPORT_DATA_DIR}/water_polygons.shp"

COPY --from=downloader "$IMPORT_DATA_DIR"/* ${IMPORT_DATA_DIR}/

WORKDIR /usr/src/app
COPY . /usr/src/app

CMD ["./import-water.sh"]
