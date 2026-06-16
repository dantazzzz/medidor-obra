# 📐 Medidor de Obra — Belas Artes

Um **medidor digital de obra** que roda numa telinha redonda sensível ao toque
(ESP32-S3). Junta numa só ferramenta de bolso: **nível, prumo, declividade (%),
transferidor e decibelímetro** — e ainda cria um **WiFi próprio** onde você abre
uma página no celular pra ver/baixar as medições e calcular **insolação solar**
(carta solar) de qualquer local, **100% offline**.

Projeto desenvolvido por **Murillo Vinícius** · Centro Universitário **Belas Artes**
de São Paulo (FEBASP).

> 👉 Não é da área de eletrônica/programação? Leia o **[GUIA.md](GUIA.md)** —
> explica tudo em linguagem simples.

---

## ✨ O que ele faz

| Ferramenta | Pra que serve |
|---|---|
| **Nível** | bolha 2D pra nivelar piso, laje, móvel |
| **Prumo** | conferir se parede/pilar está na vertical |
| **Declividade (%)** | caimento de rampa, telhado, tubo, calha — com **checagem de norma** (rampa NBR 9050, esgoto, ralo…) que fica **verde/vermelho** |
| **Transferidor** | medir ângulo entre duas superfícies |
| **Ruído (dB)** | decibelímetro usando o microfone da placa (faixas da NR-15) |

E mais:

- **Salvar medições** (com data/hora do relógio interno) e ver tudo no celular.
- **Portal WiFi** (`Medidor-Obra`): página no navegador com leitura **ao vivo**,
  **tabela** das medições, **download CSV** e botão pra **acertar o relógio** com a
  hora do celular.
- **Sol / Insolação** (`/sol`): digita a coordenada e a data e a página desenha a
  **carta solar**, calcula **nascer/pôr do sol**, **horas de sol direto numa
  fachada** e o **comprimento da sombra** — tudo por algoritmo de astronomia,
  **sem internet**.

---

## 🧰 Hardware

**Placa: Waveshare ESP32-S3-Touch-LCD-1.46B**

- Tela **redonda AMOLED 412×412** com toque (controlador SPD2010, barramento QSPI)
- **IMU QMI8658** (acelerômetro + giroscópio) — é o sensor que vira nível/prumo
- **Microfone MEMS** (I2S) — vira o decibelímetro
- **Relógio de tempo real** PCF85063 — carimba as medições com data/hora
- WiFi/Bluetooth, PSRAM 8 MB, bateria LiPo, cartão SD

Nenhum sensor extra é necessário — tudo usa o que já vem na placa. O que a placa
**não** tem (bússola, GPS) é fornecido pelo **celular** através da página web.

---

## 🚀 Como gravar na placa

O passo a passo completo (Arduino IDE, configuração da placa, biblioteca) está em
**[LEIA-ME.md](LEIA-ME.md)**. Resumo:

1. **Arduino IDE 2.x** + pacote **`esp32` da Espressif (versão 3.x)**.
2. Biblioteca **LVGL 8.3.x** + o `lv_conf.h` deste projeto.
3. Abrir `NivelDigital.ino`, selecionar **ESP32S3 Dev Module** com **OPI PSRAM /
   16MB / USB CDC On Boot: Enabled**, e **Upload**.

---

## 🏗️ Como funciona (arquitetura)

```
NivelDigital.ino     -> liga a placa, sobe as tarefas e o loop principal
 ├─ AppUi.*          -> telas: SPLASH (logo) -> MENU (arrasta/swipe) -> ferramenta
 ├─ LevelApp.*       -> as 5 ferramentas (matemática do ângulo + UI da medição)
 ├─ Mic_dB.*         -> decibelímetro (lê o microfone I2S e calcula o nível em dB)
 ├─ DataLog.*        -> guarda as medições salvas (com hora do relógio)
 ├─ WebPortal.*      -> cria o WiFi e serve as páginas (dados + sol)
 ├─ logo_belasartes.c-> o brasão exibido no splash
 └─ drivers Waveshare-> tela, toque, IMU, relógio, I2C, etc.
```

**A medição do ângulo** vem do acelerômetro: parado, ele "sente" a gravidade, e a
direção dessa força revela a inclinação (`roll = atan2(ay, az)`,
`pitch = atan2(-ax, √(ay²+az²))`). A declividade em % é `tan(ângulo) × 100`.

**O decibelímetro** lê blocos de áudio do microfone, tira a média (nível RMS) e
converte para dB. É aproximado e calibrável (constante `SPL_REF` em `Mic_dB.cpp`).

**O cálculo solar** é feito em JavaScript, **no navegador do celular**: a partir da
latitude/longitude e da data, calcula a declinação solar, a equação do tempo, e a
posição do sol (altura e azimute) ao longo do dia — astronomia clássica, offline.

> Detalhe técnico: a tela é **redonda**, então toda a interface fica dentro de um
> círculo seguro (cantos e bordas são cortados pelo vidro).

---

## 📁 Estrutura

- `NivelDigital.ino` — setup/loop
- `AppUi.*`, `LevelApp.*`, `Mic_dB.*`, `DataLog.*`, `WebPortal.*` — o aplicativo
- `logo_belasartes.c`, `_logo/` — o brasão (e o script Python que o gerou)
- `lv_conf.h` — configuração do LVGL
- Drivers da Waveshare (`Display_SPD2010.*`, `Gyro_QMI8658.*`, etc.)
- `LEIA-ME.md` — guia de gravação · `GUIA.md` — guia para leigos

---

## 🙏 Créditos

- **Desenvolvimento:** Murillo Vinícius — GitHub [@dantazzzz](https://github.com/dantazzzz) · murillovinicius18@gmail.com
- **Instituição:** Centro Universitário Belas Artes de São Paulo (FEBASP)
- **Drivers da placa:** demos oficiais da [Waveshare](https://www.waveshare.com/wiki/ESP32-S3-Touch-LCD-1.46B)
- **Interface gráfica:** [LVGL](https://lvgl.io) 8.3 (licença MIT)

## 📜 Licença

**CC BY-NC 4.0** — uso livre para fins **não comerciais**, desde que **citada a
autoria** (Murillo Vinícius). Uso comercial requer autorização. Ver [LICENSE](LICENSE).
