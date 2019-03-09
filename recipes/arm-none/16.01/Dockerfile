FROM camino.azurecr.io/gcc-rene-partial/arm-none:16.01 as partial

FROM eclipse/cpp_gcc

USER root
RUN mkdir /opt/gcc-rene
COPY --from=partial /opt/gcc-rene/ /opt/gcc-renet/ 
USER user

RUN echo 'export PATH="$PATH:/opt/gcc-rene/bin"' >> /home/user/.bashrc
