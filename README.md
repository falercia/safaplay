# Safe Play — Motor de Detecção em Camadas (POC)

Protótipo acadêmico de um sistema de proteção a crianças e adolescentes em jogos online. Projeto de Impacto do Certificado Profissional em Transformação Digital na Era da IA — MIT Professional Education.

A maioria das soluções de moderação age depois do dano. Esta POC demonstra uma arquitetura que age antes — detectando padrões acumulados de risco ao longo de uma conversa, não eventos isolados.

## O que esta demo é

Uma simulação interativa que mostra **como a decisão de detecção fluiria**, não um detector treinado. É a planta da casa, não a casa habitada.

O motor é 100% determinístico: cada alerta tem uma regra explícita e rastreável por trás. Não há machine learning, modelo treinado ou caixa-preta. Essa foi uma escolha de engenharia, não uma limitação — num sistema que protege menor e pode gerar escalonamento jurídico, toda decisão precisa ser auditável. Caixa-preta não sobrevive a uma auditoria de LGPD.

Todo o vocabulário de risco é **sintético e fictício**, criado apenas para demonstrar o mecanismo.

## Como funciona — as 3 camadas

A demo processa uma conversa de jogo mensagem a mensagem. Cada mensagem passa por três camadas em sequência.

**Camada 1 — Assinatura.** Match exato contra uma base versionada de padrões conhecidos (emojis, gírias, expressões). É o conceito de antivírus: rápido, explicável, mas só pega o que já está catalogado.

**Camada 2 — Heurística de candidatos.** A resposta ao "zero-day". Quando um token novo aparece no contexto de assinaturas conhecidas, ele herda suspeita e vira candidato a catalogar. É o que permite reagir ao que ainda não está na base.

**Camada 3 — Score acumulado por relação.** O risco sobe ao longo do tempo, combinando assimetria de idade, migração para canal privado, pedido de sigilo e escalada. Um evento isolado não dispara nada. O padrão acumulado dispara. Acima do limiar, o caso escala para revisão humana.

## O loop de aprendizado

O detalhe que diferencia esta POC: o ciclo fecha entre a Camada 2 e a Camada 1.

Token desconhecido é detectado → entra na fila de candidatos → um humano valida (na demo, um clique) → vira assinatura → a base incrementa de versão. Da próxima vez, aquele token já dá match imediato.

É o mesmo princípio de um antivírus moderno: o comportamento suspeito de hoje vira a assinatura de amanhã. Em produção, esse "clique" seria uma fila de revisão de especialistas em Trust & Safety.

## Como usar

Abra o link da demo e processe as mensagens uma a uma, ou use o modo automático.

Sugestão de sequência para entender o sistema:

1. **Aproximação progressiva** — o padrão clássico, risco subindo devagar até escalar.
2. **Conversa saudável** — controle. Prova que o sistema não acusa todo mundo: zero alerta numa conversa limpa.
3. **Falso-positivo controlado** — dois adolescentes de idade próxima usando emojis da base em contexto inocente. O risco sobe e recua, porque nunca aparece a assimetria de idade nem o pedido de sigilo.
4. **Evasão por fragmentação** — o predador quebra o código em pedaços para driblar o match exato. Mostra ao vivo o limite da Camada 1 e por que as outras camadas existem.
5. **Código novo (zero-day)** → promova o token novo a assinatura (a base vira v1.1.0).
6. **Reincidência** → rode em seguida, sem reiniciar. O token que era desconhecido há um minuto agora é reconhecido na hora.

> O combo zero-day → promover → reincidência é o ciclo de aprendizado acontecendo na frente do usuário. Rode nessa ordem: o botão Reiniciar limpa o score e os candidatos, mas não desfaz a base já aprendida.

## Limites assumidos

Esta POC entrega a arquitetura de decisão. Não entrega um detector validado. Três pontos estão fora do escopo de propósito:

A heurística de candidatos não foi medida contra dataset rotulado — precisão e recall reais exigiriam parceria com plataforma e dados anonimizados sob convênio de pesquisa.

O vocabulário é fictício. A base operacional teria de ser curada por especialistas, não por catálogo automático sem revisão.

A análise lê apenas texto e emoji. Aliciamento real também usa áudio, imagem e voz — detecção completa demanda pipeline multimodal.

## Do protótipo ao produto

Para virar produto, o núcleo determinístico permanece — é ele que dá rastreabilidade ao alerta — reforçado por três camadas de inteligência: embeddings semânticos para capturar variação de linguagem que o match exato perde, um classificador de risco sobre features comportamentais acumuladas, e um pipeline multimodal.

A governança é inegociável: human-in-the-loop para todo caso crítico, trilha de auditoria completa, e gestão de falso-positivo como métrica de primeira classe. Os dados sob LGPD: processamento no edge sempre que possível, retenção mínima, pseudonimização até o ponto de necessidade legal.

A assinatura dá o porquê do alerta. A IA dá o alcance. As duas juntas — não uma no lugar da outra.

## Stack

HTML, CSS e JavaScript puro. Sem dependências, sem build, sem backend. Roda em qualquer navegador, inclusive mobile.

## Projeto de Impacto — Grupo 1

Anderson Lopes · Fabio Garcia · Polyana Resende Brant · Rubiana Enz

---

*Vocabulário e cenários são sintéticos, criados exclusivamente para fins de demonstração acadêmica. Esta POC não processa nem armazena dados reais.*
