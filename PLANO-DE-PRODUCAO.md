# Fumō (不毛) — Escopo Técnico e Plano de Produção

**Versão:** 1.0 · **Engine-alvo:** Godot 4.x · **Plataforma da demo:** PC (Windows/Linux), teclado e mouse; controle opcional após validação do teclado.  
**Meta:** Vertical Slice de 5–10 minutos, finalizável até 31 de dezembro de 2026.  
**Premissa de planejamento:** equipe pequena (3–5 pessoas, todas iniciantes) trabalhando em ritmo parcial. Reduzir equipe ou disponibilidade exige cortar polimento, não aumentar o prazo de prototipagem.

## 1. Decisões de escopo e pilares

1. **Legibilidade antes de complexidade:** um único ataque letal por personagem; a direção e a janela de perigo precisam ser imediatamente compreensíveis.
2. **Conteúdo curto, completo e autoral:** uma trilha de combate com três momentos (aldeia indefesa, resistência, elite) e uma conclusão; não construir campanha ou sistema de progressão.
3. **Arte modular e reutilizável:** uma base de personagem com variações de paleta/equipamento; animações em oito direções se o cronograma permitir, quatro direções como fallback.
4. **Violência com intenção narrativa:** sangue é feedback diegético e persistente só até o fim da fase; evitar gore detalhado que desvie o foco da consequência humana.
5. **Escopo fechado:** 1 cenário compacto, 2 arquétipos de inimigo (camponês e elite), 1 personagem jogável, 1 sequência de abertura e 1 encerramento. Sem inventário, loot, crafting, árvore de habilidades, multiplayer ou geração procedural.

## 2. Critérios de aceite da Vertical Slice

- É possível iniciar, pausar, retomar, morrer e reiniciar sem recarregar a aplicação.
- O jogador se move, mira, ataca, guarda, executa parry e esquiva com controles explicados na tela.
- Um ataque válido mata jogador e inimigos com um golpe; um bloqueio/parry só funciona na direção e janela corretas.
- Cada inimigo apresenta um aviso de ataque visível/sonoro antes do impacto; o jogador consegue reagir sem depender de reflexos impossíveis.
- A câmera mantém jogador e ameaça no enquadramento e suaviza o zoom entre duelo e cerco.
- Elementos de foreground não escondem totalmente o jogador; efeitos de sangue não prejudicam a leitura de ataques.
- A sequência pode ser concluída em 5–10 minutos por alguém que acabou de aprender os controles.
- Introdução, transição narrativa e finalização estão presentes; o final mostra a contradição entre “missão cumprida” e custo humano.
- Build exportada e testada em ao menos dois computadores/perfis de resolução; não há erros bloqueantes no debugger.

## 3. Work Breakdown Structure (WBS)

### 0. Produção e fundamentos
- 0.1 Criar repositório Git, `.gitignore` para Godot/Aseprite, README e convenções de nomes.
- 0.2 Criar projeto Godot 4, configurar resolução, viewport, stretch, filtro de textura e renderer 2D.
- 0.3 Criar quadro de tarefas com estados simples: A fazer → Em andamento → Revisão → Feito.
- 0.4 Fazer uma cena de teste para validar velocidade, escala de pixels, paleta e legibilidade antes de produzir conteúdo.
- 0.5 Definir “feito”: merge revisado, teste manual realizado, sem erro no debugger e instruções atualizadas.
- 0.6 Fazer builds semanais de checkpoint; manter uma build anterior exportada como fallback.

### 1. Programação / Godot

**Fundação e fluxo**
- 1.1 Definir `InputMap`: `move_up/down/left/right`, `attack`, `guard`, `dodge`, `pause` e `interact`.
- 1.2 Implementar troca de cenas/estados de jogo: menu/tela de abertura → fase → pausa → vitória/derrota.
- 1.3 Criar dados de balanceamento em Resources ou constantes únicas; evitar números mágicos repetidos em scripts.
- 1.4 Implementar câmera com `Camera2D`, limites da fase, smoothing e zoom por pressão/ameaça.

**Movimentação e combate**
- 1.5 Jogador em `CharacterBody2D`: aceleração simples, velocidade normal, velocidade reduzida em guarda e colisão com cenário.
- 1.6 Leitura de mira pelo mouse e, futuramente, analógico direito; converter vetor de mira para espaço global.
- 1.7 Classificador de ataque: vertical, horizontal ou estocada usando limiares angulares documentados.
- 1.8 Implementar FSM (máquina de estados finita) para movimento/ação, com transições explícitas e sem múltiplos `if` independentes competindo.
- 1.9 Criar hitbox de ataque temporária e direcional; aplicar dano uma vez por ataque e desativar ao encerrar a janela ativa.
- 1.10 Implementar guarda direcional, janela curta de parry, reação de quebra de postura e invulnerabilidade estritamente durante esquiva.
- 1.11 Criar feedback de acerto, bloqueio, parry, morte e cooldown sem depender apenas de texto.

**Inimigos e encontro**
- 1.12 Inimigo base reutilizável com estados: espera/patrulha, aproximação, preparação do golpe, ataque, recuperação, atordoado e morto.
- 1.13 Camponês: movimentação/ataque simples e previsível, com leitura visual clara; não exigir comportamento complexo de grupo.
- 1.14 Elite: aproximação mais deliberada, estocada telegrafada e recuperação maior após erro/parry.
- 1.15 Controlador de encontros por gatilhos ou zonas; garantir que a próxima leva só apareça quando a anterior terminar.
- 1.16 Separar percepção/decisão do inimigo de animação e dano; validar distância e direção no momento do impacto.
- 1.17 Condição de vitória, morte do jogador, reinício e limpeza de inimigos/efeitos ao reiniciar a fase.

**Qualidade e suporte**
- 1.18 Interface de controles, objetivo atual, pausa, contagem/feedback de estágio e legenda narrativa.
- 1.19 Opções mínimas: volume geral, volume de efeitos, tela cheia e remapeamento apenas se houver folga.
- 1.20 Testar tamanhos de janela, foco de mouse, reinício, colisões nas bordas e comportamento com frame rate variável.
- 1.21 Exportar presets e validar executáveis em máquina sem o editor instalado.

### 2. Arte e animação

- 2.1 Definir resolução-base dos sprites (ex.: 48×48 ou 64×64), escala na tela, paleta e regras de contorno/contraste.
- 2.2 Criar guia visual de silhueta: jogador imperial, camponês e elite distinguíveis em tamanho pequeno e em fundo de bambu.
- 2.3 Produzir tiles de chão, trilha, limites, vegetação e elementos de cobertura em tileset modular.
- 2.4 Produzir um conjunto compacto de props: casas/ruínas, cercas, lanternas, bandeiras e detalhes narrativos.
- 2.5 Animar personagem: idle, corrida, guarda, ataque vertical/horizontal/estocada, esquiva, dano e morte.
- 2.6 Para caber no prazo, priorizar oito direções para idle/movimento e usar espelhamento ou orientação simplificada nos ataques menos visíveis; validar o resultado antes de duplicar todos os quadros.
- 2.7 Criar variações de camponês e elite reaproveitando corpo/base, com diferenças de roupa, arma, postura e cor.
- 2.8 Criar retratos/silhuetas e cartões simples para abertura/encerramento (sem cenas cinematográficas longas).
- 2.9 Organizar arquivos-fonte Aseprite e exportações; documentar pivô/origem e nomes de animações.

### 3. Shaders e VFX

**Obrigatório / baixo risco**
- 3.1 Silhueta/contorno ou sombra simples para separar personagens do chão.
- 3.2 Bambus e árvores de foreground com transparência por proximidade/oclusão; manter uma solução sem shader como fallback.
- 3.3 Partículas leves para poeira, impacto, faísca de lâmina, rastro de esquiva e golpe no ar.
- 3.4 Sangue direcional como decal/partícula persistente até trocar ou reiniciar a fase; limitar a quantidade e oferecer redução/desativação.
- 3.5 Sinalização de ataque com cor, forma e movimento; não comunicar perigo só com cor (acessibilidade).

**Opcional, só depois do combate validado**
- 3.6 Balanço de capim/bambu por shader ou deformação de vértices com ruído de vento.
- 3.7 Reação local à passagem/golpe por impulso com duração curta.
- 3.8 Capa com bones/ossos 2D ou deformação simples. Não simular tecido fisicamente nesta demo.
- 3.9 Vinheta, granulação discreta, flash de impacto e pequenas trepidações configuráveis.

### 4. Áudio

- 4.1 Definir três grupos: ambiente, música e efeitos; criar buses no Godot e opção de volume.
- 4.2 Ambiência contínua de floresta/vento, com pausas para valorizar o silêncio no encerramento.
- 4.3 Sons distintos para passos em terra/grama, preparo de golpe, lâmina, bloqueio, parry, esquiva e morte.
- 4.4 Dois motivos musicais curtos: tensão na aproximação e queda/ausência no desfecho; evitar trilha longa em loop para uma demo breve.
- 4.5 Reproduzir sinais de ataque com timing consistente com o hitbox, sem antecipar ou atrasar o impacto.
- 4.6 Normalizar níveis; checar mix em fones e alto-falantes de notebook; manter opção de silenciar.
- 4.7 Registrar licença e autoria de cada efeito/música; priorizar produção própria ou fontes licenciadas e arquivadas.

### 5. Level Design e narrativa

- 5.1 Desenhar mapa compacto em papel/planta antes de montar tiles; eixo principal legível e atalhos sem becos injustos.
- 5.2 Criar um greybox com três zonas, área de aprendizagem segura e terreno suficientemente aberto para ler direções.
- 5.3 Colocar cobertura/vegetação sem bloquear completamente leitura de inimigos, ataques e cursor.
- 5.4 Primeiro encontro ensina ataque e letalidade; o segundo apresenta pressão de vários alvos; o último isola o confronto de elite.
- 5.5 Distribuir pausas narrativas visuais (objetos abandonados, marcas da vila, bandeiras) sem interromper o controle por textos extensos.
- 5.6 Roteirizar abertura em 2–3 telas/cartões curtos e final em 1–2 momentos, sem dublagem obrigatória.
- 5.7 Fazer playtests de fluxo: identificar para onde ir, duração, justiça dos encontros e compreensão do parry.
- 5.8 Revisar linguagem e ritmo para comunicar a subversão sem depender de uma explicação final longa.

### 6. QA, acessibilidade e lançamento
- 6.1 Checklist de smoke test em cada build: iniciar, pausar, morrer, reiniciar, vencer e fechar.
- 6.2 Testar leitor visual: contraste de inimigo/ambiente, sinais redundantes (forma + som + animação), texto legível.
- 6.3 Opções de redução de flashes/tremor e limite de partículas/sangue.
- 6.4 Fazer pelo menos cinco testes externos, incluindo quem não conhece o projeto; observar sem explicar os controles.
- 6.5 Corrigir bloqueios e problemas de entendimento antes de polish, exportar build final e registrar versão/hash.

## 4. MVP: protótipo greybox versus versão final

| Sistema | MVP obrigatório no greybox | Juice/polimento para a demo final |
|---|---|---|
| Movimento | WASD, normalização diagonal, colisão básica, guarda lenta e esquiva com cooldown | Animação em oito direções, aceleração refinada, after-image e poeira |
| Ataque | Mira, três famílias direcionais, alcance/cone visível, hit único e letal | Animações dedicadas, trails, hit stop curto, som/impacto e partículas |
| Guarda/parry | Direção correta bloqueia; janela curta de parry; erro tem consequência clara | Pose de guarda, faísca, som em camadas, stagger legível e feedback de timing |
| Inimigos | Dois arquétipos, FSM pequena, telegraph antes do impacto, morte em um golpe | Variações visuais, animações exclusivas e comportamento ajustado por playtest |
| Câmera | Follow estável, limites do mapa e dois níveis de zoom conforme número/proximidade de ameaças | Smoothing bem calibrado, look-ahead, composição e trepidação configurável |
| Cenário | Uma arena/trilha navegável com chão, obstáculos e zonas de encontro | Tiles finais, bambu translúcido, animação de vento e resposta a golpes |
| Sangue/impacto | Um sinal simples de acerto persistente durante a sessão | Direcional, paleta contida, decal/roupa, opção de redução e limpeza eficiente |
| Narrativa | Cartão de abertura, três encontros e tela final | Composição visual, objetos ambientais e mistura/queda de áudio narrativa |
| UI/áudio | Objetivo, aviso de perigo, pausa/reinício; sons placeholder são suficientes no protótipo | Mix final, legendas, menus e opções de áudio/acessibilidade |

**Regra de corte:** se o combate ainda não for divertido/justo no greybox, não fazer shaders, animação de capa ou VFX elaborados. Se o prazo apertar, cortar reação ambiental de vento, quantidade de quadros e variedade de props; não cortar telegraphs, controles, pausa/reinício ou finalização.

## 5. Arquitetura sugerida em Godot 4

### Cena do jogador (`Player.tscn`)

```text
Player (CharacterBody2D)
├── VisualRoot (Node2D)                 # permite girar/squash sem girar colisão
│   ├── Sprite (AnimatedSprite2D)        # ou Sprite2D + AnimationPlayer
│   ├── WeaponPivot (Node2D)
│   │   ├── WeaponSprite (Sprite2D)
│   │   └── AttackArea (Area2D)          # monitorable desligado fora da janela ativa
│   │       └── AttackShape (CollisionShape2D)
│   ├── GuardVisual (Node2D)
│   └── Shadow (Sprite2D)
├── BodyShape (CollisionShape2D)
├── HurtArea (Area2D)
│   └── HurtShape (CollisionShape2D)
├── StateMachine (Node)
│   ├── Idle (Node)
│   ├── Move (Node)
│   ├── Attack (Node)
│   ├── Guard (Node)
│   ├── Dodge (Node)
│   ├── Stagger (Node)
│   └── Dead (Node)
├── AnimationPlayer (AnimationPlayer)
├── Audio (Node2D)
└── Camera2D
```

A hitbox pode ser substituída por um teste geométrico (`ShapeCast2D` ou verificação de distância/ângulo) caso as `Area2D` compliquem a primeira implementação. Escolher **uma** abordagem para o MVP. Não habilitar hitbox pelo tempo inteiro da animação: usar timestamps/Animation Call Method Track ou um timer de janela ativa.

### Personagem inimigo (`Enemy.tscn`)

```text
Enemy (CharacterBody2D)
├── VisualRoot (Node2D)
│   ├── Sprite (AnimatedSprite2D)
│   ├── WeaponPivot (Node2D)
│   ├── Telegraph (Node2D)              # ícone/forma de direção de ataque
│   └── Shadow (Sprite2D)
├── BodyShape (CollisionShape2D)
├── HurtArea (Area2D)
│   └── HurtShape (CollisionShape2D)
├── StateMachine (Node)
├── AnimationPlayer (AnimationPlayer)
└── Audio (Node2D)
```

### Responsabilidades dos scripts

- `Player.gd`: entrada, `velocity`, `move_and_slide()`, vetor de mira e chamadas à FSM. Não contém a lógica completa de todos os estados.
- `StateMachine.gd`/estados: `enter()`, `physics_update(delta)`, `update(delta)`, `exit()`. Um estado ativo por vez; transições pedidas explicitamente pelo estado.
- `CombatComponent.gd`: inicia ataque, classifica direção, controla janela ativa e emite sinais `attack_started`, `hit_confirmed`, `parried`.
- `Health/HitReceiver.gd`: para o MVP pode ser apenas `die()` sem pontos de vida; centraliza equipe, invulnerabilidade e regra de golpe letal.
- `EnemyBrain.gd`: percepção simples (distância/linha de visão opcional), escolhe aproximação ou ataque e emite um pedido; não aplica dano diretamente antes do frame de impacto.
- `EncounterDirector.gd`: inicia encontros por `Area2D`/marcadores, aguarda conclusão e atualiza objetivo.
- `CameraRig.gd`: calcula alvo de posição/zoom usando jogador e inimigos ativos; suavização e limites ficam num só lugar.
- `GameFlow.gd`: abertura, pausa, vitória, derrota e reinício.

### FSM de combate recomendada

```text
IDLE ↔ MOVE
  ├─ ataque pressionado → ATTACK_START → ATTACK_ACTIVE → ATTACK_RECOVERY → IDLE/MOVE
  ├─ guarda pressionada → GUARD
  │    ├─ direção correta + impacto na janela → PARRY → ENEMY_STAGGER
  │    ├─ direção correta fora da janela → BLOCK → GUARD
  │    └─ direção errada / guarda solta → HIT → DEAD
  └─ esquiva pressionada → DODGE (invulnerável apenas por janela curta) → IDLE/MOVE
Qualquer estado → DEAD em golpe letal
```

**Direção e legibilidade:** converter a mira para um vetor normalizado no espaço global; quantizar em oito setores para animação. Como MVP, mapear ataque predominantemente vertical/horizontal e diagonal para estocada (ou usar limiares explícitos, por exemplo ±30° em torno dos eixos). Não inferir direção do ataque do ângulo da animação depois de iniciado. Guardar a direção de ataque no começo da ação para que mouse tremido não altere o golpe no meio.

**Janela de parry:** usar o instante de início da guarda (`parry_timer`) e a hora prevista do impacto; não exigir “um frame exato”. Começar com 150–220 ms, telegraph de pelo menos 600 ms e testar em 30/60/120 FPS. Parry e esquiva não podem dar invulnerabilidade permanente por segurar botão.

### Organização de pastas

```text
res://
├── scenes/{player,enemies,levels,ui,effects}/
├── scripts/{player,enemies,combat,flow,ui}/
├── assets/{sprites,tiles,audio,fonts}/
├── resources/{animations,palettes}/
└── docs/{controls,build-notes}/
```

## 6. Plano de produção (12 de novembro a 31 de dezembro de 2026)

| Período | Entrega observável | Trabalho principal / gate |
|---|---|---|
| 12–18 nov · Semana 1 | Protótipo jogável com quadrados | Repositório, projeto Godot, InputMap, movimento, mira, hit único, arena greybox. **Gate:** atacar e morrer já funciona. |
| 19–25 nov · Semana 2 | Loop de combate | FSM player, guarda direcional, janela de parry, esquiva, telegraph de inimigo. Testes curtos diários. **Gate:** 1v1 compreensível sem arte. |
| 26 nov–2 dez · Semana 3 | Dois inimigos e encontro fechado | Camponês e elite, sequência de encontros, fluxo vitória/derrota/reinício, primeira sessão de playtest. **Gate:** jogo completo em blocos, 5–10 min. |
| 3–9 dez · Semana 4 | Greybox validado + direção de arte | Ajustar ritmo/alcances, bloquear cenário definitivo, guia de pixel art, sprites-base e tiles principais. **Gate:** congelamento de mecânicas; só corrigir bugs de combate depois. |
| 10–16 dez · Semana 5 | Conteúdo visual integrado | Personagens, animações essenciais, cenário modular, UI, câmera e som temporário/final prioritário. **Gate:** conteúdo completo, ainda que alguns efeitos sejam placeholders. |
| 17–23 dez · Semana 6 | Polimento com corte de escopo | Transparência de bambu, sangue/VFX simples, áudio, narrativa e acessibilidade. Playtests externos; remover funcionalidades instáveis. **Gate:** release candidate até 23/12. |
| 24–31 dez · Semana 7 | Estabilização e entrega | Congelamento de conteúdo, correções bloqueantes, exportação limpa, validação em computadores, zip/build e instruções. **Gate:** build candidata final em 28/12; 29–31 apenas correções críticas. |

**Ritmo recomendado:** cada tarefa deve caber em 0,5–2 dias para iniciantes. Reservar 20–25% do tempo de cada semana para integração, bugs e aprendizagem. Fazer commit pequeno ao terminar uma tarefa; não deixar integração para o fim. Uma pessoa deve ser responsável por aprovar merges e gerar builds, mesmo em equipe de três.

### Definition of Done por sprint
- A feature está integrada na branch principal e testada em build jogável, não só no editor.
- Os controles e comportamento estão documentados; os valores que precisam de ajuste são fáceis de localizar.
- Não há regressão no loop de iniciar → jogar → perder/vencer → reiniciar.
- Toda arte/áudio está no repositório ou acompanhada de fonte/licença.

## 7. Riscos técnicos e mitigação

| Risco/armadilha | Por que ameaça iniciantes | Mitigação prática |
|---|---|---|
| FSM grande ou condicionais misturadas | Estados de ataque/guarda continuam ativos em paralelo; bugs são difíceis de reproduzir. | Implementar estados pequenos com uma responsabilidade; transições explícitas; registrar o estado atual na tela de debug. |
| Hitbox ativa por muitos frames | Um golpe acerta várias vezes ou mata fora da direção pretendida. | Uma janela ativa curta, um hit por ataque, vetor congelado no início e teste visual de alcance. |
| Parry depende de frame perfeito | Latência, FPS e teclado tornam a defesa injusta. | Janela de 150–220 ms, telegraph longo, feedback claro; testar em taxas diferentes e ajustar pelo playtest. |
| Analógico/mouse e animação discordam | O sprite aponta para um lado e o dano sai em outro. | Uma única fonte de verdade: vetor global normalizado armazenado no início da ação; desenho e hitbox leem o mesmo vetor. |
| Física/colisão instável em 360° | Sprites pequenos se sobrepõem, obstáculos prendem e movimento diagonal fica mais rápido. | Usar `CharacterBody2D`, normalizar vetor, colisores simples, limitar obstáculos no caminho crítico; testar ângulos e cantos cedo. |
| IA persegue em círculo ou vários inimigos atacam juntos | O jogador é cercado sem leitura e não tem espaço de reação. | Controlar distância mínima, cooldown e cadência de ataque; limitar quantos inimigos podem iniciar windup ao mesmo tempo. |
| Câmera dinâmica causa enjoo ou esconde ameaça | Zoom reage a cada frame e compete com a mira. | Alvo de zoom por faixas com histerese/smoothing, limites da câmera e teste em tela pequena; não mudar zoom durante o golpe. |
| Shader/transparência não funciona em todos os fundos | Transparência e ordem de desenho podem esconder o jogador ou ter custo inesperado. | Solução alfa simples como base, shader apenas se validado; desenhar oclusores por camada e manter contorno/silhueta do jogador. |
| Arte detalhada antes do gameplay | Assets levam muito tempo e ficam incompatíveis com escala/câmera finais. | Trabalhar em blocos até semana 3; congelar resolução, pivô e paleta antes de produzir quadros finais. |
| Escopo cresce com “só mais um efeito” | A equipe aprende engine, arte e Git ao mesmo tempo; tarefas desconhecidas atrasam. | Lista explícita de não-objetivos, responsável por escopo e corte automático dos opcionais a partir de 17/12. |
| Merge/arquivos binários em conflito | Aseprite, cenas e recursos geram conflitos difíceis de resolver. | Cada pessoa trabalha em cenas/arquivos distintos, commits pequenos e coordenação antes de editar arquivo compartilhado; manter `.aseprite` fonte e exports organizados. |
| Sangue/flash reduz acessibilidade | Efeito persistente pode encobrir ataques e incomodar jogadores. | Limite de decal/partícula, opção de reduzir efeitos, sem flash branco intenso e sinal de perigo também por forma/som. |

## 8. Fora de escopo para proteger a data

- Campanha completa, árvore de habilidades, equipamentos, inventário, crafting, diálogos ramificados ou cutscenes longas.
- Física de tecido, simulação de cabelo/roupa, destruição sistêmica, corpos persistentes com ragdoll ou IA de grupo avançada.
- Shader procedural complexo para vento, iluminação dinâmica por pixel, multiplayer, suporte a consoles e localização ampla.
- Mais de uma fase, mais arquétipos de inimigo ou chefes com várias fases.

## 9. Plano de playtest e balanceamento

1. **Teste funcional (equipe):** todas as transições e reinício; nenhuma ação pode travar o jogador.
2. **Teste de leitura (pessoa nova):** sem explicar os controles, observar se entende ataque, direção de guarda, parry e objetivo. Anotar onde hesita; não defender a implementação durante o teste.
3. **Teste de justiça:** registrar mortes por ataque não visto, alcance injusto ou entrada simultânea de inimigos. Se a pessoa não identifica a causa, ajustar telegraph/câmera antes de reduzir dificuldade.
4. **Teste de duração:** medir conclusão de pelo menos cinco jogadores; alvo mediano de 6–8 minutos e extremos de 5–10.
5. **Teste de qualidade:** resolution/windowed/fullscreen, pausa, foco da janela, baixa taxa de frames, áudio desligado e reduzir efeitos.

**Regra de balanceamento:** primeiro melhorar clareza e janela de reação, depois ajustar dificuldade. Não compensar um telegraph ilegível tornando o jogador invulnerável por tempo excessivo; isso apaga o núcleo tático.
