# TerrExample

## 1. Instalar docker

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


