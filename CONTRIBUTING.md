# Contribuindo para o VoIP Capture Analyzer

Obrigado por contribuir com o VoIP Capture Analyzer. O objetivo deste documento é manter as contribuições técnicas, a documentação e o licenciamento consistentes.

## Licenciamento e direitos autorais

1. O projeto é distribuído sob a Licença MIT. Contribuições aceitas serão disponibilizadas sob a mesma licença do projeto.
2. O autor da contribuição mantém os direitos autorais que lhe pertencem, mas, ao enviar uma contribuição para incorporação ao projeto, concede ao projeto uma licença perpétua, mundial, não exclusiva, gratuita e irrevogável para usar, reproduzir, modificar, sublicenciar e distribuir a contribuição como parte do projeto.
3. Não envie código, documentação, imagens ou outros materiais de terceiros sem verificar os direitos de uso e a compatibilidade da licença.
4. Código GPL ou outro material com licença incompatível não deve ser incorporado ao código-fonte do projeto. Dependências externas podem possuir licenças próprias e devem continuar sendo tratadas como componentes independentes.

## Reportando bugs e sugerindo melhorias

Antes de abrir uma Issue:

- pesquise se o problema já foi reportado;
- informe a versão do VoIP Capture Analyzer;
- informe a versão do Python e do TShark;
- descreva o comportamento esperado e o observado;
- quando possível, forneça um exemplo mínimo reproduzível e logs relevantes, removendo dados sensíveis da captura.

## Pull Requests

1. Faça um fork do repositório.
2. Crie uma branch específica para a alteração:

```bash
git checkout -b feature/minha-nova-funcionalidade
# ou
 git checkout -b fix/nome-do-bug
```

3. Mantenha o escopo da alteração claro e pequeno sempre que possível.
4. Para novos arquivos de código-fonte, utilize o cabeçalho de licença padrão:

```text
# Copyright (c) 2026 Bruno Barreto
# This source code is licensed under the MIT license found in the
# LICENSE file in the root directory of this source tree.
```

5. Execute os testes antes de abrir o PR:

```bash
pytest
```

6. Atualize o `README.md` quando a alteração mudar instalação, configuração ou uso.
7. Descreva no PR o que foi alterado, como foi validado e eventuais limitações conhecidas.

## Padrões de código

- Siga a PEP 8 para Python.
- Prefira código simples, explícito e testável.
- Adicione testes `pytest` para novas regras de análise e correções de regressões.
- Não trate uma métrica como fato quando ela for apenas uma inferência.
- Preserve a separação entre evidência observada, métrica calculada e diagnóstico.
- Mantenha o TShark como ferramenta externa; o projeto não incorpora código-fonte do TShark.

## Segurança e dados de captura

Capturas SIP/RTP podem conter números, endereços IP, URIs, cabeçalhos, áudio e outras informações sensíveis. Não publique capturas reais em Issues ou Pull Requests sem autorização e sem aplicar a anonimização necessária.


### Preparação do ambiente

Use `python activate.py` para criar `.venv`, instalar as dependências e, em Linux/macOS, abrir um shell já ativado. Para sair do ambiente, use `exit` nesse subshell ou `python deactivate.py` para consultar o comando apropriado.
