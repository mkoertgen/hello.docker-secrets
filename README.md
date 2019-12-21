# hello.docker-secrets

Evaluating various secret management methods with docker:

- [Docker Stack](#docker-stack)
- ...

## Docker Stack

Manage the stack

```console
docker stack deploy --compose-file docker-compose.yml secrets-demo
docker stack ps secrets-demo
docker stack rm secrets-demo
```

Optionally, use an orchestrator differing from your default by adding `[--orchestrator swarm|kubernetes]`

### Docker Swarm

Manage secrets

```console
$docker swarm init

$docker secret create secret-password ./secrets/secret-password.txt
pi2591zumd8h7y9w8uwmlc9ie

$docker secret ls
ID                          NAME                DRIVER              CREATED             UPDATED
pi2591zumd8h7y9w8uwmlc9ie   secret-password                         5 seconds ago       5 seconds ago
```

### Kubernetes (k8s)

Manage secrets

```console
$kubectl create secret generic secret-password --from-file=./secrets/secret-password.txt
secret/secret-password created

$kubectl get secrets
NAME                  TYPE                                  DATA   AGE
default-token-dxh7z   kubernetes.io/service-account-token   3      53s
secret-password       Opaque

$kubectl describe secrets/secret-password
Name:         secret-password
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
secret-password.txt:  18 bytes
```

See also:

- [kubernetes secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

## Links

- [The rabbit hole is deep when trying to switch from environment variables file to Docker secrets](http://blog.code4hire.com/2018/06/the-rabbit-hole-is-deep-when-trying-to-switch-from-environment-variables-file-to-docker-secrets/)

### Ansible

- [Ansible docker_swarm](https://docs.ansible.com/ansible/latest/modules/docker_swarm_module.html)

### Vault (HashiCorp)

- [Your secret's safe with me: Securing container secrets with Vault](https://www.hashicorp.com/resources/securing-container-secrets-vault)
- ...

### Donet Core

- [Accessing Docker Swarm Secrets from ASP.NET Core](https://www.jamessturtevant.com/posts/Acessing-Docker-Swarm-Secrets-From-ASPNET-Core/)
- [API Series Part 4 - Secrets Management with Docker Secrets](https://jack-vanlightly.com/blog/2017/9/26/api-series-part-4-docker-secrets)

A docker-specific configuration provider

- [Dotnet Core ConfigurationProvider for Docker Swarm Secrets](https://josefottosson.se/dotnet-core-configurationprovider-for-docker-swarm-secrets/)
- [joseftw/JOS.DockerSwarmSecretsConfigurationProvider](https://github.com/joseftw/jos.dockerswarmsecretsconfigurationprovider)

#### Protect secrets in development

Alternatively, separate docker-specific secrets from `dotnet user-secrets` (probably during container startup / entrypoint)

- [Safe storage of app secrets in development in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/security/app-secrets?view=aspnetcore-3.1&tabs=windows)

However, this seems to be suitable for development only and requires dotnet SDK.

#### Azure KeyVault

- [Azure Key Vault Configuration Provider in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/security/key-vault-configuration?view=aspnetcore-3.1)
