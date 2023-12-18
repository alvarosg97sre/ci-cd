Documentación del Proceso de CI/CD para el Portafolio de Álvaro
===============================================================

Introducción
------------

Este documento describe el proceso de Integración Continua y Despliegue Continuo (CI/CD) implementado para el portafolio personal de Álvaro, alojado en [GitHub](https://github.com/alvarosg97sre/portfolio). El propósito principal de este portafolio es mostrar el trabajo de Álvaro como un profesional de SRE y aprender más sobre DevOps y SRE. Este portafolio consta de una página web básica con HTML, CSS y JavaScript.

Proceso de CI: Construcción de la Imagen Docker
-----------------------------------------------

### Desencadenantes de CI

El proceso de Integración Continua se inicia automáticamente cuando se realiza un `push` a la rama `main` del repositorio del portafolio o cuando se crea un `pull request` hacia esta rama.

### Pasos de Construcción

1.  Checkout del Código: Se usa `actions/checkout@v3` para obtener el código fuente más reciente del repositorio.

2.  Definición del Tag de la Imagen: Se genera un tag para la imagen Docker basado en los primeros 7 dígitos del SHA del commit actual usando el comando `echo "::set-output name=tag::${GITHUB_SHA::7}"`.

3.  Construcción de la Imagen Docker: Se construye una imagen Docker utilizando `docker build`, etiquetando la imagen con el nombre `alvarosg97/portfolio` y el tag generado en el paso anterior.

4.  Autenticación en Docker Hub: Se realiza la autenticación en Docker Hub para permitir la subida de la imagen.

5.  Subida de la Imagen a Docker Hub: La imagen Docker construida se sube a Docker Hub.

Proceso de CD: Despliegue en Kubernetes
---------------------------------------

### Infraestructura de Kubernetes

El cluster de Kubernetes está alojado en Digital Ocean. Los archivos y configuraciones necesarios para crear y configurar este cluster están disponibles en otro repositorio: [kubernetes-portfolio](https://github.com/alvarosg97sre/kubernetes-portfolio).

### Pasos de Despliegue

1.  Configuración de kubectl: Se utiliza `steebchen/kubectl@v2.0.0` para interactuar con el cluster de Kubernetes.

2.  Actualización del Deployment de Kubernetes: Se actualiza el Deployment `mi-portfolio` en el cluster de Kubernetes para usar la nueva imagen Docker. Esto se logra con el comando `kubectl set image`.

3.  Verificación del Despliegue: Se verifica el estado del despliegue utilizando `kubectl rollout status` para asegurar que la actualización se haya completado correctamente.

