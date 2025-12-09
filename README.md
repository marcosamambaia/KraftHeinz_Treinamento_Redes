# Topologia de Rede - Projeto CML

Este projeto faz parte de um **treinamento de rede para a equipe técnica da KraftHeinz**, utilizando **Cisco Modeling Labs (CML)** com um **roteador** e dois **switches**, configurados para suportar múltiplas VLANs e acesso à Internet via NAT.


---

##  Componentes

- **Roteador (Roteador)**
  - IOS versão 17.16
  - Interfaces configuradas:
    - `Ethernet0/0.10` → VLAN 10, IP 192.168.10.1/24, NAT inside
    - `Ethernet0/1.20` → VLAN 20, IP 192.168.20.1/24, NAT inside
    - `Ethernet0/2` → IP 192.168.255.100/24, NAT outside
  - **NAT/PAT** configurado para permitir que as redes internas acessem a Internet.
  - **Rota default**: `0.0.0.0/0` apontando para o gateway 192.168.255.1.
  - Serviços habilitados: HTTP, HTTPS, SSH.
  - ACL padrão permitindo redes internas (192.168.10.0/24 e 192.168.20.0/24).

- **Switch SW0**
  - IOS versão 17.16
  - Interfaces configuradas:
    - `Ethernet0/1` → Acesso VLAN 10
    - `Ethernet0/2` → Trunk dot1Q, VLANs permitidas 10 e 20
  - Spanning Tree configurado em modo Rapid-PVST.
  - Porta trunk conecta ao roteador para transporte das VLANs.

- **Switch SW1**
  - IOS versão 17.16
  - Interfaces configuradas:
    - `Ethernet0/1` → Acesso VLAN 20
    - `Ethernet0/2` → Trunk dot1Q, VLANs permitidas 10 e 20
  - Spanning Tree configurado em modo Rapid-PVST.
  - Porta trunk conecta ao roteador para transporte das VLANs.

---

##  Funcionamento da Topologia

1. **Separação de redes internas**  
   - VLAN 10 → Rede 192.168.10.0/24  
   - VLAN 20 → Rede 192.168.20.0/24  

2. **Switches**  
   - SW0 conecta dispositivos da VLAN 10.  
   - SW1 conecta dispositivos da VLAN 20.  
   - Ambos possuem trunk para o roteador, permitindo transporte das VLANs.

3. **Roteador**  
   - Atua como gateway para ambas as VLANs.  
   - Faz NAT/PAT para que os hosts internos acessem a Internet.  
   - Usa a rota default para encaminhar tráfego desconhecido ao gateway 192.168.255.1.

---

##  Diagrama Simplificado
                                                (Internet)
                                                    ^
                                                    |
                                                    |
                                                  E0/2
[PC VLAN 10]E0 <---> E0/1-[SW0]-E0/2 <---> E0/0-[Roteador]-E0/1 <---> E0/1-[SW1]-E0/1 <---> E0[PC VLAN 20]
Alphine Linux            Switch Cisco         Roteador Cisco              Switch Cisco      Alphine Linux


---

##  Objetivo

- Demonstrar configuração de VLANs em switches com trunk dot1Q.
- Implementar roteamento inter-VLAN no roteador.
- Configurar NAT/PAT para acesso externo.
- Garantir gerenciamento seguro via SSH e HTTPS.

---

##  Como usar

1. Suba os dispositivos no **Cisco Modeling Labs (CML)**.
2. Aplique as configurações conforme os arquivos de `running-config`.
3. Teste conectividade entre hosts das VLANs e acesso à Internet.
4. Verifique NAT com `ping` ou `traceroute` para endereços externos.

---

##  Observações

- O roteador usa **NAT overload (PAT)**, permitindo múltiplos hosts compartilharem o mesmo IP público.  
- O gateway externo (192.168.255.1) deve estar configurado corretamente para fornecer acesso à Internet.  
- Spanning Tree Rapid-PVST garante prevenção de loops na camada 2.  

