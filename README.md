# 🖥️ Proxmox Home Lab

Este repositório documenta a criação e configuração do meu Home Lab utilizando o **Proxmox VE** como plataforma de virtualização.

O objetivo do laboratório é praticar administração de servidores Linux, redes, virtualização, containers, monitoramento, segurança e automação.

---

## 📋 AmbienteO

| Item | Configuração |
|---|---|
| Hypervisor | Proxmox VE |
| Equipamento | Notebook |
| Hostname | `pve01` |
| FQDN | `pve01.home.arpa` |
| Rede | Ethernet |
| Gerenciamento | Proxmox Web Interface |
| Porta Web | `8006` |

---

# 1. Instalação do Proxmox VE

## 1.0 - Requisitos para instalação

- Instalação do [Ventoy](https://www.ventoy.net/en/download.html)
- Instalação da ISO do [Proxmox](https://www.proxmox.com/en/downloads/proxmox-virtual-environment)
- Necessário que durante a intalação a maquina esteja conectada ao cabo de rede

Obs: Recomento que não instale as ultimas versões pode pode ocasionar em Erro durante a instalação do Proxmox


## 1.1 Download da ISO

Foi realizado o download da ISO oficial do **Proxmox VE**.

Após o download, é recomendável validar o hash SHA256 da imagem antes de criar a mídia de instalação.

No Windows/PowerShell:

```powershell
Get-FileHash "C:\caminho\proxmox-ve.iso" -Algorithm SHA256
```

O hash obtido deve ser comparado com o SHA256 disponibilizado para a versão baixada.

---

## 1.2 Possiveis problemas

Inicialmente foi utilizado o **Ventoy** para inicializar a ISO do Proxmox.

Durante a tentativa de instalação gráfica foram apresentados os erros:

```text
error: invalid magic number
error: you need to load the kernel first
```

Para resolver esse problema foi necessário pegar uma versão desatualizada do Proxmox. Por esse motivo é necessário a validação do Hash.

---


# 2. Configuração da BIOS/UEFI

Antes da instalação foram verificadas as configurações de inicialização.

Configuração utilizada:

```text
Boot Mode: UEFI
Secure Boot: Disabled
CSM/Legacy: Disabled
USB Boot: Enabled
```

Em seguida, o computador foi inicializado pelo pendrive através do **Boot Menu**.

---

# 3. Instalação

No menu de inicialização do Proxmox foi selecionada a opção:

```text
Install Proxmox VE (Graphical)
```

Durante a instalação foram configurados:

- Disco de instalação;
- Localização e fuso horário;
- Senha do usuário `root`;
- E-mail administrativo;
- Interface de rede;
- Hostname/FQDN;
- Endereço IP;
- Gateway;
- Servidor DNS.

---

# 4. Configuração do Hostname


Pode prosseguir com as configurações da maneira que preferir.

Essas definições são feitas de padrão pelo proprio Proxmox 

Estrutura:

```text
pve01.home.arpa
│     │
│     └── Domínio interno do Home Lab
│
└── Servidor Proxmox
```

O domínio `home.arpa` será utilizado para resolução de nomes dentro da rede do laboratório.

---

# 5. Primeiro acesso

Após finalizar a instalação, o servidor foi reiniciado e inicializado pelo disco onde o Proxmox foi instalado.

O endereço IP pode ser verificado pelo terminal com:

```bash
ip a
```

ou:

```bash
hostname -I
```

A interface administrativa do Proxmox pode ser acessada através de:

```text
https://IP_DO_PROXMOX:8006
```

Exemplo:

```text
https://192.168.1.100:8006
```

Credenciais:

```text
Usuário: root
Realm: Linux PAM standard authentication
Senha: definida durante a instalação
```

---

# 6. Verificação dos serviços

Para verificar o serviço responsável pela interface web:

```bash
systemctl status pveproxy
```

Verificação do daemon do Proxmox:

```bash
systemctl status pvedaemon
```

Verificação de serviços com falha:

```bash
systemctl --failed
```

Verificação da porta Web:

```bash
ss -lntp | grep 8006
```

---

# 7. Configuração para notebook

Como o servidor Proxmox está instalado em um notebook, foi configurado para continuar funcionando mesmo com a tampa fechada.

Editar:

```bash
nano /etc/systemd/logind.conf
```

Configurar:

```ini
HandleLidSwitch=ignore
HandleLidSwitchExternalPower=ignore
HandleLidSwitchDocked=ignore
```

Aplicar:

```bash
systemctl restart systemd-logind
```

Ou reiniciar o servidor:

```bash
reboot
```

Dessa forma, fechar a tampa do notebook não suspende o servidor.

---

# 8. Troubleshooting

## Invalid magic number / You need to load the kernel first

### Sintomas

Durante o boot utilizando Ventoy:

```text
invalid magic number
you need to load the kernel first
```

# 8. Arquitetura inicial do Home Lab

Planejamento inicial:

```text
                    INTERNET
                       │
                 Roteador/Firewall
                       │
                       │
                 ┌─────┴─────┐
                 │   pve01   │
                 │  Proxmox  │
                 └─────┬─────┘
                       │
              ┌────────┼─────────┐
              │        │         │
             VM       LXC       VM
              │        │         │
           Docker     DNS    Monitoramento
```

---

# 9. Próximas etapas

- [x] Instalar Proxmox VE
- [x] Configurar hostname/FQDN
- [x] Configurar acesso pela rede
- [x] Habilitar funcionamento com a tampa fechada
- [ ] Configurar IP estático
- [ ] Configurar DNS interno
- [ ] Criar primeira VM Linux
- [ ] Criar primeiro container LXC
- [ ] Criar servidor Docker
- [ ] Implementar Pi-hole ou AdGuard Home
- [ ] Implementar monitoramento
- [ ] Configurar VPN para acesso remoto
- [ ] Criar rotina de backup
- [ ] Configurar VLANs
- [ ] Documentar topologia da rede

---

## 📚 Objetivo do laboratório

Este Home Lab será utilizado como ambiente de estudos e testes para:

- Linux;
- Proxmox VE;
- Virtualização;
- Containers LXC;
- Docker;
- Redes;
- DNS;
- DHCP;
- Firewall;
- VPN;
- Monitoramento;
- Backup;
- Automação;
- Segurança;
- Administração de servidores.

---

## ⚠️ Observações

Este ambiente possui finalidade de laboratório e aprendizado. Alterações realizadas em ambientes de produção devem ser previamente avaliadas, documentadas e testadas.
