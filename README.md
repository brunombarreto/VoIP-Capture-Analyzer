# VoIP-Capture-Analyzer
Framework para análise técnica de capturas VoIP, com foco em **SIP, SDP, RTP e RTCP**, reconstrução de chamadas, correlação de pernas B2BUA, métricas de mídia e diagnóstico baseado em evidências.

## Principais recursos

- Upload de `.cap`, `.pcap` e `.pcapng` pela interface web.
- TShark como decodificador e leitor primário da captura.
- Recuperação de SIP/SDP/RTP/RTCP por caminhos alternativos baseados em dados extraídos pelo TShark.
- Correlação de chamadas com múltiplos `Call-ID`s, incluindo cenários B2BUA.
- Métricas de ASR, PDD, duração, RTP, perda, jitter, bitrate e PPS quando os dados permitem cálculo confiável.
- Identificação de RTP unilateral, interrupções assimétricas e gaps de mídia a partir de 1 segundo.
- Linha do tempo correlacionada SIP + RTP, com início/fim de mídia e interrupções posicionadas no mesmo eixo temporal.
- Gráficos técnicos e relatório em HTML, PDF e DOCX.
- Glossário técnico incorporado ao dashboard e aos relatórios.
- Nome original da captura preservado nos relatórios para facilitar auditoria e rastreabilidade.
- Interface sem dependência de Alpine.js; o frontend utiliza JavaScript vanilla.
- Menu de ajuda com conteúdo de uso, download da versão atual, glossário, licença MIT, diretrizes de contribuição e contato.

## Execução local

```bash
python3.11 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
tshark --version
IP=$(hostname -I | awk '{print $1}')
uvicorn app.main:app --host "$IP" --port 8000
```

Acesse `http://IP:8000`.

### Dependências do sistema

O funcionamento completo depende do TShark e das bibliotecas necessárias pelo WeasyPrint no sistema operacional. Consulte a seção de requisitos de terceiros abaixo.

## Estrutura

```text
app/
  analyzers/       Motor de análise e modelos
  services/        TShark, gráficos, relatórios e jobs
  static/          JavaScript da interface
  templates/       Dashboard e template do relatório
  main.py          API FastAPI

tests/             Testes automatizados
LICENSE             Licença MIT
CONTRIBUTING.md     Diretrizes para contribuições
README.md           Documentação principal
```
## Interpretação das métricas

O Analyzer separa três níveis de informação:

1. **Observado** — diretamente identificado na captura/TShark.
2. **Calculado** — derivado matematicamente de dados observados.
3. **Diagnóstico** — interpretação técnica baseada nas evidências disponíveis.

A ausência de informação suficiente resulta em `N/A`, em vez de uma estimativa artificial.

REGISTER e OPTIONS são apresentados como tráfego de controle/saúde SIP e **não são contabilizados como tentativas de chamada**.

## Licença

Este projeto é distribuído sob a **Licença MIT**. Você pode usar, copiar, modificar, mesclar, publicar, distribuir, sublicenciar e vender cópias do software, observando as condições da licença, especialmente a preservação do aviso de copyright e da própria licença.

Consulte o arquivo LICENSE para o texto completo e as condições de isenção de responsabilidade.

---

## Requisitos de sistema e licenciamento de terceiros

O código-fonte deste projeto é MIT, mas o funcionamento depende de componentes externos com licenças próprias. Essas licenças continuam aplicáveis aos respectivos componentes.

### 1. Dependências do sistema operacional

- **TShark / Wireshark**: projeto distribuído sob a GNU GPL e outras licenças aplicáveis aos componentes do Wireshark. O VoIP Capture Analyzer não incorpora o código-fonte do TShark; ele executa o utilitário externamente por CLI.
- **Shared MIME Info**: componente externo do sistema, sujeito à sua própria licença.
- **Pango / GDK Pixbuf e bibliotecas relacionadas**: componentes externos utilizados pelo ambiente de renderização do WeasyPrint, sujeitos às respectivas licenças, incluindo LGPL quando aplicável.

### 2. Dependências Python

O projeto utiliza bibliotecas como FastAPI, Pydantic, Jinja2, Matplotlib, WeasyPrint e python-docx. Cada dependência permanece sujeita à licença indicada pelo seu próprio projeto e distribuição. Antes de redistribuir uma instalação completa, consulte as licenças efetivamente instaladas no ambiente.

O uso de uma dependência externa pelo projeto não altera a licença do código-fonte próprio do VoIP Capture Analyzer.

---

## Desenvolvimento

```bash
pytest
```

Consulte "CONTRIBUTING.md" para as regras de contribuição.

## Contato

Para contato com o mantenedor do projeto: <a href="https://linktr.ee/brunombarreto" target="_blank" rel="noopener">Bruno Barreto</a>.

## Download

A suíte atual está disponível no release mais recente do GitHub: https://github.com/brunombarreto/VoIP-Capture-Analyzer/releases/latest/download/VoIP_Capture_Analyzer.zip
