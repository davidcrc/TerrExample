# TerrExample

## 1. Instalar Terraform

wget https://releases.hashicorp.com/terraform/0.11.9/terraform_0.11.9_linux_amd64.zip

unzip terraform_0.11.9_linux_amd64.zip

sudo mv terraform /usr/local/bin/

terraform --version 

## 2. Intalar docker (Ubuntu 18.04)

https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04

## 3. Configurar, plan y apply!

Comenzar con una definición básica de contenedor de NGINX Docker en una configuración mínima de Terraform: crear un archivo main.tf:

```hcl
# Configure Docker provider and connect to the local Docker socket
provider "docker" {
  host = "unix:///var/run/docker.sock"
}

resource "docker_image" "nginx" {
  name = "nginx:1.11-alpine"
}

resource "docker_container" "nginx-server" {
  name  = "nginx-server"
  image = "${docker_image.nginx.latest}"

  ports {
    internal = 80
  }

  volumes {
    container_path = "/usr/share/nginx/html"
    host_path      = "/home/scrapbook/tutorial/www"
    read_only      = true
  }
}
```

Una vez que se ha definido la configuración, necesitamos crear un plan de ejecución. Terraform describe las acciones requeridas para lograr el estado deseado. El plan se puede guardar usando -out. Aplicaremos el plan de ejecución en el siguiente paso.

```
sudo terraform plan -out config.tfplan
```

Una vez que se haya creado el plan, debemos aplicarlo para alcanzar el estado deseado. Usando el CLI, terraform extraerá cualquier imagen requerida y lanzará nuevos contenedores. La salida indicará los cambios y la configuración resultante.

```
sudo terraform apply
```

Puede usar la CLI de Docker para ver los cambios y ver el contenedor recién lanzado. Comprobar que el contenedor se está ejecutando:
```
sudo docker ps -a
```

![](source/docker.png?raw=true "Funcionamieto de Docker ")

Podemos revisar las configuraciones y cambios futuros:
```
sudo terraform show
```
![](source/show.png?raw=true "Show docker config")

+ Pagina de Simulacion:

https://www.katacoda.com/courses/terraform/deploy-nginx


## 4. Otras herramientas similares

- Las herramientas de orquestación de configuración, que incluyen Terraform y AWS CloudFormation, están diseñadas para automatizar la implementación de servidores y otra infraestructura.

- Las herramientas de administración de la configuración como Chef, Puppet y las demás en esta lista ayudan a configurar el software y los sistemas en esta infraestructura que ya se ha aprovisionado.

### 4.1 AWS CloudFormation

<img src="https://dnp94fjvlna2x.cloudfront.net/wp-content/uploads/2018/04/CF-logo.png" width="128">

Al igual que Terraform, AWS CloudFormation es una herramienta de orquestación de configuración que le permite codificar su infraestructura para automatizar sus implementaciones.

Las principales diferencias radican en que CloudFormation está profundamente integrado y solo se puede utilizar con AWS, y las plantillas de CloudFormation se pueden crear con YAML además de JSON.

CloudFormation le permite obtener una vista previa de los cambios propuestos en su pila de infraestructura de AWS y ver cómo podrían afectar sus recursos, y administra las dependencias entre estos recursos.

Para garantizar que la implementación y actualización de la infraestructura se realice de manera controlada, CloudFormation usa Desencadenadores de reversión para revertir las pilas de infraestructura a un estado implementado anterior si se detectan errores.

### 4.2 Azure Resource Manager y Google Cloud Deployment Manager

**Azure Resource Manager** le permite definir la infraestructura y las dependencias para su aplicación en plantillas, organizar recursos dependientes en grupos que se pueden implementar o eliminar en una sola acción, controlar el acceso a los recursos a través de los permisos de los usuarios y más.
<center>
<img src="https://dnp94fjvlna2x.cloudfront.net/wp-content/uploads/2018/04/azure-resource-manager.jpg" width="128">
</center>

**Google Cloud Deployment Manager**  ofrece muchas características similares para automatizar su pila de infraestructura GCP. Puede crear plantillas utilizando YAML o Python, obtener una vista previa de los cambios que se realizarán antes de la implementación, ver sus implementaciones en la interfaz de usuario de la consola y mucho más.

<center>
<img src="https://dnp94fjvlna2x.cloudfront.net/wp-content/uploads/2018/04/Deployment_Manager.png" width="128">
</center>
