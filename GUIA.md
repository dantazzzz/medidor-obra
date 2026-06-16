# 📖 Guia para todos — como o Medidor de Obra foi feito e como funciona

Este guia é para **quem não é da área** de eletrônica ou programação. Sem termos
complicados — e quando aparecer um, ele vem explicado. 🙂

---

## 🤔 O que é, em uma frase

É uma **telinha redonda** que você segura na mão e que mede coisas de obra —
**se algo está reto, inclinado, no prumo, a inclinação de uma rampa, e até o
barulho do ambiente** — e que ainda **conversa com o seu celular pelo WiFi**.

Pense num **nível de pedreiro** (aquele com a bolhinha), só que **digital**, com
tela, e fazendo o trabalho de várias ferramentas ao mesmo tempo.

---

## 👀 Como usar (passo a passo)

1. **Ligue** a placa (cabo USB-C). Aparece o **brasão da Belas Artes** (a tela de
   abertura) e depois o **menu**.
2. No menu, **arraste para o lado** (como trocar de foto no celular) para passar
   pelas ferramentas e **toque** na que quiser:
   - **Nível** — deite a placa numa superfície; a bolinha verde mostra se está reto.
   - **Prumo** — encoste numa parede; mostra se ela está "em pé" certinho.
   - **Declividade** — mostra a inclinação em **porcentagem** (rampa, telhado, cano).
     Tem um aviso que fica **verde** se está dentro da norma, **vermelho** se não.
   - **Transferidor** — mede ângulo (canto, abertura).
   - **Ruído** — mede o **barulho** em decibéis (dB), usando o microfone da placa.
3. Dentro de cada ferramenta há 4 botões:
   - **MENU** — volta pro menu · **ZERAR** — define o ponto atual como "zero" ·
     **HOLD** — congela o número · **SALVAR** — guarda a medição.

---

## 📱 A parte do celular (WiFi)

A placa cria uma **rede WiFi própria**, como se fosse um mini-roteador. Você:

1. No celular, conecta na rede **`Medidor-Obra`** (senha **`belasartes`**).
2. Abre o **navegador** (Chrome, Safari) e digita **`192.168.4.1`**.
3. Pronto: aparece uma página com a **leitura ao vivo**, a **lista das medições que
   você salvou** (com horário), um botão pra **baixar tudo em planilha (CSV)** e
   outro pra **acertar o relógio** da placa com a hora do seu celular.

E tem uma página extra, **"Sol / Insolação"**: você informa **onde** (a coordenada
do lugar) e **quando** (a data), e ela desenha o **caminho do sol no céu** naquele
dia, diz a hora do **nascer e pôr do sol**, **quantas horas de sol** uma parede vai
receber e o **tamanho da sombra**. Isso é ouro para arquitetura (saber a melhor
orientação de uma casa, onde bate sol, etc.).

---

## 🧠 Como funciona "por dentro" (sem susto)

- **Como ela sabe a inclinação?** Dentro da placa há um sensorzinho chamado
  *acelerômetro*. Ele sente a **gravidade** (aquela força que puxa tudo pra baixo).
  Quando você inclina a placa, a direção dessa força muda — e a partir disso o
  programa calcula o ângulo. É a mesma ideia da bolha do nível de pedreiro, só que
  com matemática no lugar do líquido.

- **Como ela mede o barulho?** Tem um **microfone** minúsculo na placa. O programa
  "escuta" e mede o quão forte é o som, transformando em decibéis (dB).

- **Como ela fala com o celular?** A placa tem **WiFi**, igual ao do seu roteador.
  Ela cria uma rede e serve uma **página de internet** — mas que mora **dentro da
  própria placa** (não precisa de internet de verdade).

- **Como calcula o sol sem internet?** A posição do sol no céu é **previsível por
  matemática** (astronomia). Sabendo o lugar e a data, dá pra calcular tudo com
  fórmulas — então funciona até no meio do mato, sem sinal.

---

## 🛠️ Como foi feito (a história resumida)

1. **A placa.** Foi usada uma plaquinha pronta da marca *Waveshare*, com uma tela
   redonda colorida sensível ao toque e vários sensores já embutidos
   (a "ESP32-S3" — um pequeno computador do tamanho de uma moeda).

2. **O programa.** Foi escrito no **Arduino IDE** (um programa gratuito de
   computador onde se escreve o código e se "grava" ele na placa pela USB). A
   linguagem é o **C/C++**.

3. **A tela.** A interface (botões, bolinha, números, menu) foi montada com uma
   biblioteca gráfica chamada **LVGL**, feita pra telas pequenas.

4. **As ferramentas.** Cada modo (nível, prumo, etc.) é um pedaço do programa que
   pega a leitura do sensor e desenha o resultado na tela.

5. **O WiFi e o sol.** A placa virou um mini-servidor de internet; a página que
   aparece no celular foi feita com **HTML e JavaScript** (a "linguagem dos sites").
   O cálculo do sol roda **no celular**, dentro dessa página.

6. **O acabamento.** Logo da faculdade na abertura, menu que se arrasta, e os
   botões posicionados pra caber na tela **redonda** (os cantos de uma tela redonda
   não existem, então tudo fica dentro de um círculo).

---

## 📚 Mini-glossário

- **ESP32 / placa:** o "computadorzinho" que faz tudo funcionar.
- **Acelerômetro:** sensor que percebe inclinação/movimento (sente a gravidade).
- **Firmware:** o programa que fica gravado dentro da placa.
- **LVGL:** a "caixa de ferramentas" pra desenhar a tela.
- **dB (decibel):** unidade que mede o volume do som.
- **CSV:** arquivo de planilha (abre no Excel/Google Sheets).
- **Carta solar:** desenho do caminho que o sol faz no céu ao longo do dia.

---

Feito com 💙 por **Murillo Vinícius** — Belas Artes (FEBASP).
