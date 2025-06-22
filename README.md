# Desafio Terraform + AWS - Infrastructure as Code

Este projeto demonstra o uso do Terraform para provisionar infraestrutura na AWS, incluindo a criação de uma instância EC2 com deploy automático de um site web usando Ansible.

## 📋 Descrição do Projeto

O projeto automatiza a criação dos seguintes recursos na AWS:

- **3 Security Groups**: 
  - HTTP (porta 80)
  - SSH (porta 22) 
  - Egress (saída de dados)
- **1 Key Pair**: Para acesso SSH seguro
- **1 Instância EC2**: t2.micro com Amazon Linux 2
- **Deploy automático**: Site web via Nginx usando Ansible

## 🗂️ Estrutura de Arquivos

```
├── provider.tf        # Configuração do provider AWS
├── key_pair.tf        # Criação do par de chaves SSH
├── security_group.tf  # Configuração dos Security Groups
├── data.tf           # Data source para AMI do Amazon Linux
├── ec2.tf            # Configuração da instância EC2
├── outputs.tf        # Outputs do projeto (IP público, URL)
├── variables.tf      # Variáveis do projeto
├── user_data.sh      # Script de inicialização da instância
├── playbook.yaml     # Playbook Ansible para configuração
└── README.md         # Este arquivo
```

## ⚙️ Pré-requisitos

- [Terraform](https://www.terraform.io/downloads) instalado
- [AWS CLI](https://aws.amazon.com/cli/) configurado
- Conta AWS (AWS Educate compatível)

## 🚀 Como usar

### 1. Preparação
```bash
# Clone ou extraia o projeto
cd iac-com-terraform-e-aws

# Configure seu IP público no arquivo variables.tf
# Descubra seu IP em: https://www.whatismyip.com/
```

### 2. Configurar credenciais AWS
```bash
aws configure
```
Insira:
- Access Key ID
- Secret Access Key  
- Region: us-east-1
- Output format: json

### 3. Executar Terraform
```bash
# Inicializar Terraform (baixa providers)
terraform init

# Verificar o plano de execução
terraform plan

# Aplicar as configurações (criar infraestrutura)
terraform apply
# Digite 'yes' quando solicitado
```

### 4. Acessar o site
Após alguns minutos, você verá a saída:
```
instance_public_ip = "54.123.456.789"
website_url = "http://54.123.456.789"
```

Acesse a URL no navegador usando `http://` (não https).

### 5. Destruir recursos (opcional)
```bash
terraform destroy
# Digite 'yes' quando solicitado
```

## 🔧 Configurações Importantes

### IP Público no variables.tf
**OBRIGATÓRIO**: Altere o IP no arquivo `variables.tf`:
```hcl
variable "meu_ip_publico" {
    default = "SEU.IP.AQUI/32"  # Substitua pelo seu IP real
}
```

### Região AWS
O projeto está configurado para `us-east-1`. Para usar outra região, altere no `provider.tf`.

## 📖 Componentes Explicados

### Terraform
Ferramenta de Infrastructure as Code que permite definir recursos de nuvem usando código declarativo.

### Security Groups
Funcionam como firewall virtual, controlando o tráfego de entrada e saída da instância.

### User Data
Script executado automaticamente na primeira inicialização da instância EC2.

### Ansible
Ferramenta de automação que configura o servidor (instala Nginx, faz deploy do site).

## 🎯 Recursos Criados

- **aws_security_group.http_sg**: Permite tráfego HTTP (porta 80)
- **aws_security_group.ssh_sg**: Permite SSH (porta 22) do seu IP
- **aws_security_group.egress_all_sg**: Permite todo tráfego de saída
- **aws_key_pair.ec2_key_pair**: Par de chaves para acesso SSH
- **aws_instance.web_server**: Instância EC2 t2.micro

## 💡 Troubleshooting

### Site não carrega
- Aguarde 5-10 minutos após o `terraform apply`
- Verifique se está usando `http://` e não `https://`
- Confirme se o IP no `variables.tf` está correto

### Erro de credenciais
- Verifique `aws configure`
- Confirme as credenciais do AWS Educate

### Erro de permissões
- Certifique-se de que sua conta AWS tem as permissões necessárias
- No AWS Educate, verifique se os serviços EC2 estão disponíveis

## 📝 Observações

- Este projeto é compatível com AWS Educate
- A instância t2.micro está dentro do free tier
- Sempre execute `terraform destroy` para evitar custos
- O deploy completo leva cerca de 5-10 minutos

## Saiba mais

- [Documentação do Terraform](https://developer.hashicorp.com/terraform)
- [Documentação do Provider AWS do Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Lista de Providers do Terraform](https://registry.terraform.io/browse/providers)
- [Documentação da AWS](https://docs.aws.amazon.com/pt_br/)

## 👨‍💻 Autor

Projeto desenvolvido como parte do desafio de Infrastructure as Code com Terraform e AWS.

