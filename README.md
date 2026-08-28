# VoIP Capture Analyzer

Framework web para análise técnica de capturas VoIP, com foco em **SIP, SDP, RTP e RTCP**, reconstrução de chamadas, correlação de pernas B2BUA, métricas de mídia e diagnóstico baseado em evidências.

## Principais recursos

- Upload de uma ou várias capturas `.cap`, `.pcap` ou `.pcapng`.
- Consolidação automática de múltiplas capturas com `mergecap` antes da análise.
- TShark como decodificador e leitor primário da captura consolidada.
- Recuperação de SIP/SDP/RTP/RTCP por caminhos alternativos baseados em dados extraídos pelo TShark.
- Correlação de chamadas com múltiplos `Call-ID`s, incluindo cenários B2BUA.
- Métricas de ASR, PDD, duração, RTP, perda, jitter, bitrate e PPS quando os dados permitem cálculo confiável.
- Identificação de RTP unilateral, interrupções assimétricas e gaps de mídia.
- Linha do tempo correlacionada SIP + RTP.
- Gráficos técnicos e relatório em HTML, PDF e DOCX.
- Glossário técnico incorporado ao dashboard e aos relatórios.
- Nome original das capturas preservado nos relatórios para facilitar auditoria e rastreabilidade.
- Limpeza automática dos arquivos de captura após o processamento.
- Limpeza dos relatórios e artefatos anteriores ao iniciar uma nova análise.
- Interface web com atualização de progresso em tempo real via WebSocket.
- Menu de ajuda com instruções de uso, download da suíte atual, glossário, licença, contribuição e contato.
- Checklist de validação de infraestrutura orientado pelos achados da captura, sem afirmar causa raiz sem evidência externa.

## Requisitos

- Python 3.11 ou superior.
- TShark instalado e disponível no `PATH`, ou informado pela variável `TSHARK_BIN`.
- `mergecap` instalado e disponível no `PATH`, ou informado pela variável `MERGECAP_BIN`. Ele é necessário quando duas ou mais capturas são enviadas para a mesma análise.
- Dependências Python do arquivo `requirements.txt`.
- Bibliotecas de sistema necessárias para o WeasyPrint quando PDF é utilizado.

## Execução local

### Instalação

A forma recomendada é usar o script de preparação do ambiente:

```bash
python activate.py
source .venv/bin/activate
```

O script cria `.venv` quando necessário e instala as dependências de `requirements.txt`. Também é possível executar manualmente:

```bash
python3.11 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
tshark --version
mergecap --version
```

### Inicialização simplificada

```bash
python run.py
```

Por padrão, a aplicação escuta em `0.0.0.0:8000`.

A porta e o endereço podem ser escolhidos na inicialização:

```bash
python run.py --port 8080
```

```bash
python run.py --host 127.0.0.1 --port 8080
```

Também é possível utilizar as variáveis de ambiente:

```bash
HOST=0.0.0.0 PORT=8080 python run.py
```

A inicialização direta pelo Uvicorn continua disponível:

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

Acesse `http://IP:PORTA` pelo navegador.

## Upload de capturas

A interface permite selecionar ou arrastar uma ou mais capturas.

- **Uma captura:** o arquivo é analisado diretamente pelo pipeline existente.
- **Duas ou mais capturas:** os arquivos são enviados para um diretório temporário do job e consolidados pelo `mergecap`. A captura consolidada é então analisada pelo mesmo pipeline TShark/SIP/SDP/RTP/RTCP.
- Os nomes originais das capturas são preservados como metadados do job e apresentados no relatório.
- Os arquivos de captura temporários, inclusive o arquivo consolidado, são removidos após a conclusão ou falha do job.
- Ao iniciar **Nova análise**, os relatórios e gráficos da análise anterior também são removidos.

O limite padrão é de **500 MB por arquivo** e **10 arquivos por análise**. Esses valores podem ser alterados por variáveis de ambiente.

## Configuração

| Variável | Padrão | Finalidade |
|---|---|---|
| `HOST` | `0.0.0.0` | Endereço de escuta HTTP |
| `PORT` | `8000` | Porta HTTP |
| `TSHARK_BIN` | `tshark` | Executável do TShark |
| `MERGECAP_BIN` | `mergecap` | Executável do mergecap |
| `MAX_UPLOAD_MB` | `500` | Limite por arquivo de captura |
| `MAX_CAPTURE_FILES` | `10` | Quantidade máxima de capturas por análise |
| `ANALYSIS_TIMEOUT_SECONDS` | `1800` | Tempo limite de operações de análise/merge |

Exemplo:

```bash
export PORT=8080
export MAX_UPLOAD_MB=1024
export MAX_CAPTURE_FILES=20
python run.py
```

## Docker

O `docker-compose.yml` permite parametrizar a porta publicada:

```bash
PORT=8080 docker compose up -d --build
```

A aplicação ficará disponível na porta escolhida no host.

Em diagnósticos de mídia/sinalização, valide também firewall, portas/protocolos/serviços, NAT e eventual SIP ALG quando os achados forem compatíveis.

## Estrutura

```text
app/
  analyzers/       Motor de análise e modelos
  services/        TShark, mergecap, gráficos, relatórios, glossário e jobs
  static/          JavaScript e identidade visual da interface
  templates/       Dashboard e template do relatório
data/              Arquivos temporários e relatórios gerados em runtime
tests/             Testes automatizados
run.py             Inicialização simplificada com escolha de host/porta
LICENSE            Licença MIT
CONTRIBUTING.md    Diretrizes para contribuições
CHANGELOG.md       Histórico de alterações
```

## Licenciamento

O código-fonte deste projeto é distribuído sob a **Licença MIT**. Consulte o arquivo [LICENSE](LICENSE) para os termos completos.

As ferramentas e bibliotecas externas utilizadas pelo ambiente de execução permanecem sujeitas às suas próprias licenças. O Analyzer executa TShark e mergecap como utilitários externos por linha de comando e não incorpora o código-fonte dessas ferramentas.

## Download

A suíte atual está disponível no release mais recente do GitHub:

[Download da suíte atual](https://github.com/brunombarreto/VoIP-Capture-Analyzer/releases/latest/download/VoIP_Capture_Analyzer.zip)

## Desenvolvimento

Execute os testes com:

```bash
pytest
```

Para regras de contribuição, padrões de código e processo de Pull Request, consulte [CONTRIBUTING.md](CONTRIBUTING.md).

## Contato

[Bruno Barreto](https://linktr.ee/brunombarreto)
