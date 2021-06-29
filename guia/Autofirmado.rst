Crear certificados autofirmados
=========================================

**Creamos el un directorio de trabajo**::

	mkdir certs
	cd certs


**Creamos el Root Key**::

	openssl genrsa -des3 -out rootCA.key 4096

**Creamos un certificado Root autofirmado**. Poner atención cuando le consulten <Common Name (e.g. server FQDN or YOUR name)> aqui se debe especificar el nombre correcto del servidor a nivel de DNS o el nombre que se utilizara en los archivos HOSTS::

	openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.crt


**Creamos un certificado por cada servidor - Creamos el certificado key**::

	openssl genrsa -out registry.key 2048

**Creamos un request** (csr) ::

	openssl req -new -key registry.key -out registry.csr

**Verificamos el contenido del request csr's**::

	openssl req -in registry.csr -noout -text

**Generamos el certificado utilizando el request crs** firmada con la llave y certificado Root::

	openssl x509 -req -in registry.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out registry.crt -days 500 -sha256

Verificamos el contenido del certificado::

	openssl x509 -in registry.crt -text -noout



