# Changelog — VoIP Capture Analyzer

## v2.27

### [Added]
- Checklist de validação de infraestrutura na interface e nos relatórios, derivado dos candidatos diagnósticos observados na captura.
- Recomendações estruturadas para validação de firewall, NAT, SIP ALG, SBC/PBX, QoS/DSCP e roteamento/ponto de captura.

### [Changed]
- Evoluído o diagnóstico para separar explicitamente ações de validação externa de hipóteses de causa raiz.
- Atualizada a versão da aplicação para `v2.27`.

### [Fixed]
- Removida importação duplicada de `tshark_media` no engine de análise.

### [To do]
- Evoluir o checklist para permitir registro de evidências externas e resultado da validação sem alterar retroativamente os dados observados da captura.

## v2.26

### [Changed]
- Separada visualmente a instrução “Como usar” da observação sobre REGISTER e OPTIONS na Metodologia dos relatórios e da interface.
- Padronizadas as orientações de Multi-PCAP com limite padrão de 10 arquivos por análise e 500 MB por arquivo nos pontos de uso.
- Ampliadas as recomendações diagnósticas para validação de firewall, portas, protocolos, serviços, NAT e eventual SIP ALG quando os sintomas observados forem compatíveis. A recomendação é apresentada como validação externa, sem atribuir causalidade sem evidência.
- Padronizada a versão do projeto para v2.26 nos elementos de interface e documentação.

### [Fixed]
- Corrigido o erro `NameError: name 'Inches' is not defined` que interrompia a geração do DOCX após a geração dos gráficos.
- Mantida a numeração de páginas nos relatórios PDF e DOCX, com campo de página atual/total no DOCX e contadores de página no PDF.

### [Added]
- Adicionados `NAT` e `SIP ALG` ao glossário compartilhado entre interface e relatórios, mantendo ordenação alfabética por Termo.
- Adicionada regressão automatizada para o helper de inserção de gráficos no DOCX e para as novas orientações diagnósticas.

### [To do]
- Continuar evoluindo a correlação entre evidências de captura e validações de infraestrutura externas, sem transformar hipóteses em causa raiz confirmada.

## v2.25

### [Fixed]
- Corrigido erro no exportador DOCX que interrompia a análise após a geração dos gráficos, causado pela resolução de `qn`/`OxmlElement` dentro do helper de sombreamento das células.
- Adicionada regressão automatizada para garantir que o helper de formatação DOCX permaneça funcional.
- Atualizada a versão exibida na interface e nos artefatos de teste para `v2.25`.



## v2.24

### [Added]
- Numeração de páginas nos relatórios PDF e DOCX, incluindo indicação de página atual e total quando suportado pelo formato.
- Script `activate.py` para criação do ambiente virtual e instalação das dependências do projeto.
- Orientações de Multi-PCAP, limite padrão de 10 arquivos por análise e limite de 500 MB por arquivo no fluxo de uso.

### [Changed]
- A observação sobre `REGISTER` e `OPTIONS` foi separada da instrução principal na aba Metodologia.
- A documentação de uso passou a explicitar o fluxo Multi-PCAP e seus limites.
- O rodapé do PDF e DOCX mantém a sequência padronizada de copyright, autoria, marca e versão, com links definidos pelo projeto.
- A importação de dependências de PDF/DOCX foi tornada tardia (*lazy import*), evitando que a simples coleta de testes falhe quando uma dependência de exportação não estiver instalada no ambiente.

### [Fixed]
- Corrigido o cenário em que `pytest` falhava durante a coleta por `ModuleNotFoundError: No module named 'docx'` ao importar o módulo de relatórios, quando a dependência de DOCX não estava instalada no ambiente de testes.

### [To do]
- Avaliar futuramente melhorias de UX para preparação automática do ambiente em diferentes sistemas operacionais, sem substituir o controle explícito do ambiente pelo usuário.

## v2.23

### [Added]
- Suporte a **multi-PCAP**: a interface permite selecionar/arrastar uma ou mais capturas na mesma análise.
- Consolidação automática de múltiplas capturas por meio do utilitário externo `mergecap` antes do pipeline de análise.
- Variáveis `MERGECAP_BIN` e `MAX_CAPTURE_FILES` para configuração do merge e do limite de arquivos por análise.
- Script `run.py` para inicialização simplificada, com seleção de `--host` e `--port`.
- Variáveis `HOST` e `PORT` para configuração da escuta HTTP.
- Testes automatizados para o fluxo de merge, limpeza e novas configurações.

### [Changed]
- O fluxo de upload passou a trabalhar com uma coleção de capturas, mantendo compatibilidade com clientes que ainda enviam o campo singular `file`.
- Quando existe somente uma captura, ela continua seguindo diretamente para a análise, sem execução desnecessária do `mergecap`.
- Quando existem várias capturas, o job registra os nomes originais e apresenta todos eles como referência no relatório.
- A limpeza temporária passou a remover todos os arquivos do job no diretório de uploads, incluindo capturas individuais e a captura consolidada.
- O Docker passou a utilizar a inicialização simplificada e permite parametrizar a porta publicada pelo `docker compose`.
- O README foi consolidado como documentação permanente do projeto, sem notas de versão e sem glossário duplicado.
- O rodapé dos relatórios permanece padronizado com copyright, autoria e marca/versão com os hyperlinks definidos pelo projeto.
- O glossário foi ampliado para documentar `Mergecap`, `Multi-PCAP`, `PCAP` e `PCAPNG`, além das métricas e siglas já utilizadas.

### [Fixed]
- Corrigida a possibilidade de deixar capturas individuais de um job multi-PCAP no diretório temporário.
- Evitada a criação de uma etapa `mergecap` quando a análise possui apenas uma captura.

### [To do]
- Evoluções futuras do pipeline poderão adicionar estratégias de normalização/validação de capturas de múltiplos pontos antes da correlação, sem alterar o modelo de análise já consolidado.

## Histórico consolidado das evoluções anteriores

### [Changed]
- Evolução do pipeline de análise para recuperação estruturada de SIP, SDP, RTP e RTCP utilizando TShark, com caminhos alternativos de recuperação quando a decodificação automática não é suficiente.
- Reconstrução e correlação de chamadas e pernas SIP, incluindo cenários B2BUA e múltiplos `Call-ID`s.
- Inclusão progressiva de métricas de sinalização e mídia, como ASR, PDD, duração, retransmissões SIP, jitter, perda RTP, bitrate e PPS.
- Separação entre dados observados, métricas calculadas e inferências diagnósticas, com uso de `N/A` quando a captura não oferece evidência suficiente.
- Inclusão de análise de estado da chamada e `Call Anomaly Score` como indicador próprio do Analyzer.
- Evolução dos gráficos para análise de response codes, PDD, perda/jitter, codecs, simultaneidade, qualidade, timelines SIP/RTP, RTT de transações SIP, estabelecimento de mídia, consistência SDP/RTP, saúde de OPTIONS e perfil de banda RTP.
- Inclusão de glossário compartilhado entre interface, análise e relatórios, com manutenção centralizada da terminologia técnica.
- Padronização da identidade visual com marca/logotipo nos dashboards e relatórios.
- Padronização dos relatórios PDF e DOCX para aproximar conteúdo, estrutura, KPIs, gráficos e identificação visual.
- Inclusão de exportação PDF/DOCX com opção de baixar ou abrir em nova janela quando suportado pelo navegador.
- Preservação do nome original da captura nos relatórios para reforçar rastreabilidade e auditoria.
- Proteção para que o ID interno do job/sessão não seja apresentado como `Call-ID` quando a correlação não fornecer um identificador SIP válido.
- Inclusão do menu Ajuda com conteúdo de uso, download da suíte atual, glossário, Licença MIT, CONTRIBUTING e contato.
- Centralização dos links de download da suíte no release mais recente do GitHub.
- Limpeza automática das capturas temporárias ao final do processamento e dos relatórios/artefatos ao iniciar uma nova análise.
- Correção do pipeline do glossário que podia interromper uma análise com `NameError` após a extração de SIP/SDP/RTP/RTCP.
