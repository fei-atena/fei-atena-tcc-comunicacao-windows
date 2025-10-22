# Sistema de Comunicação MQTT Windows

## Visão Geral
Este sistema é responsável por enviar dados da visão computacional do Windows para o sistema ROS no Linux. Ele lê um arquivo JSON local contendo informações de pose e estado das mãos, e envia esses dados via MQTT para um broker rodando no Linux.

## Pré-requisitos
- Python 3.x instalado no Windows
- Biblioteca paho-mqtt
- Arquivo `data.json` com os dados da visão computacional
- VM Linux ou máquina Linux com broker MQTT configurado

## Instalação

1. Instale o Python no Windows (se ainda não estiver instalado):
   - Baixe do [site oficial do Python](https://www.python.org/downloads/)
   - Durante a instalação, marque a opção "Add Python to PATH"

2. Instale a biblioteca MQTT via pip:
```bash
pip install paho-mqtt
```

## Configuração

Ajuste as seguintes variáveis no código:
- `BROKER`: IP da máquina Linux (exemplo: "192.168.15.3")
- `PORT`: 1883 (porta padrão do MQTT)
- `TOPIC`: "pingpong/ros" (tópico para comunicação)

### Estrutura do Arquivo data.json
O arquivo `data.json` deve estar no mesmo diretório do script e conter:
```json
{
    "current_frame": {
        "hands": {
            "right_hand_open": true/false,
            "left_hand_open": true/false
        },
        "body": {
            "body_list": [
                {
                    "local_orientation_euler_deg": {
                        "ShoulderLeft": {
                            "pitch": valor,
                            "roll": valor,
                            "yaw": valor
                        },
                        // ... outros ângulos
                    }
                }
            ]
        }
    }
}
```

## Executando o Sistema

1. Certifique-se de que o broker MQTT está rodando no Linux

2. Verifique se o arquivo `data.json` está presente no diretório

3. Execute o script:
```bash
python mqtt_publisher.py
```

## Depuração

### Erros Comuns

1. Conexão Recusada:
   - Verifique se o IP do Linux está correto
   - Confirme se o broker MQTT está rodando no Linux
   - Verifique se as portas estão liberadas no firewall

2. Erro de Arquivo:
   - Confirme se `data.json` existe no diretório
   - Verifique se o arquivo tem permissão de leitura

3. Erro de JSON:
   - Verifique se o arquivo `data.json` está bem formatado

### Monitoramento

Para verificar se os dados estão sendo enviados corretamente:

1. No Linux, use o comando:
```bash
mosquitto_sub -t "pingpong/ros" -v
```

## Observações

- O sistema envia dados a cada 10ms (100Hz)
- Em caso de erro, o sistema continuará tentando enviar
- O arquivo `data.json` deve estar sempre acessível
- Mantenha uma conexão de rede estável entre Windows e Linux
