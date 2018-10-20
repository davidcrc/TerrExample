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

Guarde el archivo, luego aplicar:

```
sudo terraform plan -out config.tfplan
```

Si el plan esta bien y sin error, aplícar:

```
sudo terraform apply
```

Comprobar que el contenedor se está ejecutando:
```
sudo docker ps -a
```

+ Pagina de Simulacion:

https://www.katacoda.com/courses/terraform/deploy-nginx