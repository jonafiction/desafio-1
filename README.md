# Desafio 1, creacion EC2 con github actions

Este flujo de trabajo de GitHub Actions automatiza la creación de una instancia EC2 en AWS cada vez que se realiza un `push` a la rama `main` del repositorio. Este archivo YAML se encarga de lanzar una nueva instancia EC2 y configurarla con parámetros básicos.

## Descripción del flujo de trabajo

El archivo YAML realiza los siguientes pasos:

1. **Checkout del código**: Se obtiene la versión más reciente del código desde el repositorio.
2. **Configuración de AWS CLI**: Se configura AWS CLI con las credenciales de AWS utilizando los secretos almacenados en GitHub.
3. **Creación de la instancia EC2**: Se crea una nueva instancia EC2 con una imagen especificada, tipo de instancia `t2.micro`, y una etiqueta de nombre.

## Pasos detallados

### 1. Checkout del código

Se utiliza la acción `actions/checkout@v4` para asegurarse de que el flujo de trabajo tenga acceso al último código disponible en el repositorio.

```yaml
- name: Checkout código
  uses: actions/checkout@v4

### Access AWS CLI
La primera parte de nuestro github actions necesitará nuestra Access key y secret Access key de aws, por ende cree los secrets en github actions: AWS_ACCESS_KEY_ID y AWS_SECRET_ACCESS_KEY.

- name: Configurar AWS CLI
  uses: aws-actions/configure-aws-credentials@v1
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: us-east-1




### Creación EC2
La instancia creada fue de tipo t2 ya que es la free tier y además esta sería una instancia básica que podríamos usar por ejemplo para un entorno de development (obviamente no para producción).
Como AMI se utilizó Linux 2 que habíamos usado previamente en clases (su id es ami-0166fe664262f664c) y la tenía previamente guardada en una variable de entorno de github actions a la que le puse AMI_ID_LINUX2.

- name: Crear instancia EC2
  run: |
    aws ec2 run-instances \
      --image-id ami-0166fe664262f664c \
      --count 1 \
      --instance-type t2.micro \
      --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=InstanciaDesafio1}]'






