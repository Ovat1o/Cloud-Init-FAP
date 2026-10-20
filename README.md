# Provisionamento Automatizado com Cloud-Init e Vagrant

Atividade prática de DevOps demonstrando a inicialização declarativa de uma máquina virtual Ubuntu 22.04 LTS (Jammy) utilizando **Cloud-Init** e **Vagrant**.

---

## 🛠️ Especificações da Atividade

1. **Atualização de pacotes:** `package_update: true` para sincronizar os repositórios APT antes das instalações.
2. **Usuário Administrador:** 
   - Nome: `meu-usuario`
   - Grupo suplementar: `sudo`
   - Regra sudo sem senha: `ALL=(ALL) NOPASSWD:ALL`
   - Bloqueio de senha convencional: `lock_passwd: true`
   - Chave pública SSH: OpenSSH `ed25519` gerada no ambiente host (Windows) e injetada via `ssh_authorized_keys`.
3. **Pacotes solicitados:** Instalação declarativa via `packages` do servidor web `nginx` e da engine `docker.io`.
4. **Página personalizada:** Criação do arquivo `/var/www/html/index.html` com o módulo `write_files`, definindo layout estilizado, permissões `0644` e proprietário `www-data:www-data`.
5. **Execução de comandos (`runcmd`):**
   - `systemctl enable --now nginx`
   - `systemctl enable --now docker`
   - `usermod -aG docker meu-usuario`
6. **Mensagem final:** Registro da conclusão do provisionamento via diretiva `final_message`.

---

## 📁 Estrutura de Arquivos

```text
Cloud-Init/
├── Vagrantfile               # Definição da VM (Ubuntu 22.04, portas e rede privada)
├── cloud-init_setup.sh       # Script de preparação do NoCloud datasource
├── user-data                 # Arquivo de configuração cloud-config
├── README.md                 # Documentação do projeto
└── evidencias/               # Evidências e capturas de tela dos testes
    ├── 01-cloud-init.txt
    ├── 01-servicos-ativos.png
    ├── 02-pagina-web.png
    ├── 02-usuario-e-ssh.txt
    ├── 03-servicos-e-docker.txt
    ├── 04-validacao-web.txt
    └── EVIDENCIAS.md
```

---

## 🚀 Como Executar

### 1. Pré-requisitos
- Vagrant instalado
- Oracle VirtualBox instalado
- Chave SSH Ed25519 gerada no host (`ssh-keygen -t ed25519`)

### 2. Inicialização do Ambiente
No terminal (PowerShell), dentro da pasta `Cloud-Init`:

```powershell
vagrant up
```

> **Nota:** Caso o Cloud-Init seja limpo e necessite de nova leitura do `user-data` após o boot inicial, execute um reinício para aplicação completa:
> ```powershell
> vagrant reload
> ```

### 3. Validação dos Serviços

Para verificar o status do Cloud-Init e dos serviços instalados:

```powershell
# Acesso SSH via chave gerada
ssh -i "$HOME\.ssh\id_ed25519" -p 2222 meu-usuario@127.0.0.1

# Verificar status do Cloud-Init
cloud-init status --long

# Verificar serviços
systemctl status nginx
systemctl status docker
id meu-usuario
```

### 4. Acesso Web

Abra o navegador e acesse a porta encaminhada disponibilizada pelo Vagrantfile:
- **Porta mapeada:** `http://localhost:8080` (ou `http://localhost:2200` em caso de redirecionamento por porta ocupada)
- **IP da rede privada:** `http://192.168.56.10`

---

## 📸 Evidências

Todas as capturas de tela e logs de saída dos comandos de teste estão organizados na pasta [`evidencias/`](evidencias/EVIDENCIAS.md).

