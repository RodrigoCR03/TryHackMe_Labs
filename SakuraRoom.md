# Lab: Sakura Room (Easy Mode) - OSINT

## 1) Análise de Metadados de Imagem

- Descarregar uma imagem no formato `.svg` e extrair os metadados usando a ferramenta `exiftool`:
  ```sh
  exiftool sakurapwnedletter.svg
  ```
- Resultado relevante:
  ```
  Export-filename: /home/SakuraSnowAngelAiko/Desktop/pwnedletter.png
  ```
  - **Informação encontrada**: `SakuraSnowAngelAiko` (INFO:Flag!)

## 2) Pesquisa no Google pelo Nome "SakuraSnowAngelAiko"

### Identificação de Repositório no GitHub

- Identificado um perfil no **GitHub** com um repositório chamado `PGP`.
- O repositório contém uma **chave pública PGP**, que ao ser decodificada revela o seguinte e-mail:
  ```
  Website: "GPG/PGP Decoder" -> "SakuraSnowAngel83@rotonmail.com"
  ```
  - **Informação encontrada**: (INFO:Flag!)
- **Nota**: Pode-se usar a ferramenta online **OSINT Industries** para identificar outras plataformas associadas ao e-mail.

### Identificação no Twitter

- Identificado um **Twitter** associado ao nome `Aiko Abe`.
  - **Informação encontrada**: (INFO:Flag!)
- Username alternativo no Twitter: `SakuraLoverAiko`.
  - **Informação encontrada**: (INFO:Flag!)

### Identificação de Carteira Ethereum

- No **GitHub**, há um repositório chamado `ETH` que contém indícios de posse de uma carteira Ethereum.
  - **Informação encontrada**: (INFO:Flag!)
- Foi identificado um commit recente que apagou um endereço de carteira ETH:
  ```
  0x102397dbeeBeFD8cD2F73A89122fCdB53abB6ef
  ```
  - **Informação encontrada**: (INFO:Flag!)
- Usando o **Etherscan**, é possível rastrear as transações associadas a essa carteira.
  - **Transação identificada**: Pool de mineração **Ethermine**, em **23 de janeiro de 2021**.
  - **Moeda identificada**: **Tether (USDT)**
  - **Informação encontrada**: (INFO:Flag!)

### Identificação de Informações no DeepPaste

- No **Twitter**, o usuário menciona armazenar informações no **DeepPaste**.
- Ao acessar o DeepPaste, encontramos os seguintes dados:
  ```
  Home Wifi: DK1F-G
  ```
  - **SSID encontrado**: (INFO:Flag!)
- Utilizando **Wigle.net (Advanced Search)**, foi possível localizar o exato local no mapa.
  ```
  BSSID: 84:AF:EC:34:FC:F8
  ```
  - **Informação encontrada**: (INFO:Flag!)

## 3) Análise de Imagens com Google Lens

- Utilizando **Google Lens**, encontramos imagens semelhantes e identificamos informações sobre localizações:
  - **Aeroporto mais próximo**: `DCA` (INFO:Flag!)
  - **Última escala do invasor**: `HND` (INFO:Flag!)
  - **Lago identificado**: `Lake Inawashiro` (INFO:Flag!)
  - **Local de residência do invasor**: `Hirosaki, Japão` (INFO:Flag!)
