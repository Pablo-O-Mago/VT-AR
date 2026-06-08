# Vitrine AR — Contexto do Projeto

## O que é esse projeto

App mobile de realidade aumentada para lojas de calçados. O vitrinista aponta a câmera para a vitrine e vê as informações de cada sapato (referência, cor, quantidade) sobrepostas em tempo real. Ao clicar num item, abre a ficha completa do produto ("Pokédex"). Também há um modo de busca: digita o código do item e ele fica destacado em verde quando a câmera passa por cima.

**Problema real que resolve:** vitrinista precisa localizar um sapato específico numa vitrine extensa, ou fazer a vistoria do que está faltando para montar a lista de reposição antes de subir ao estoque.

---

## Status atual

Protótipo inicial criado: `vitrine-ar-prototipo.html` — arquivo HTML único com AR.js + A-Frame.
- Roda no browser, sem app nativo
- 2 itens mapeados (Fone e Teclado) usando marcadores Hiro e Kanji
- Busca por código com destaque verde funcionando
- Modal "Pokédex" com ficha do produto ao clicar

**Próximos passos definidos:**
1. Adicionar mouse e carregador (requer marcadores ArUco numerados)
2. Corrigir evento de toque em mobile
3. Evoluir para Expo + ViroReact quando validar a ideia

---

## Stack tecnológica

### Protótipo atual (fase 0)
- **AR.js** — biblioteca AR que roda no browser via WebXR/WebRTC
- **A-Frame** — framework 3D declarativo (HTML-like) que o AR.js usa por baixo
- **HTML/JS puro** — zero build tools, zero npm, abre direto no browser
- **Marcadores físicos** — Hiro e Kanji impressos em papel funcionam como âncoras AR

### Stack alvo (fase 1 — app real)
- **React Native + Expo** — cross-platform iOS e Android, base de código única
- **ViroReact** — engine AR para React Native, abstrai ARKit (iOS) e ARCore (Android)
- **ARKit / ARCore** — engines nativas de AR da Apple e Google
- **AsyncStorage** — armazenamento local do mapa no celular (JSON)
- **Node.js + PostgreSQL** — backend futuro para sincronizar mapa e estoque

---

## Como a tecnologia AR funciona (para referência)

1. **Âncora espacial** — QR code ou marcador físico define a origem (0,0,0) do espaço 3D
2. **Mapeamento** — posição de cada objeto salva como `{ id, x, y, z }` relativa à âncora
3. **Tracking** — ARKit/ARCore rastreiam posição e orientação da câmera 60x/segundo via Visual Inertial Odometry (câmera + giroscópio + acelerômetro)
4. **Projeção** — matriz de projeção perspectiva converte coordenadas 3D em pixels na tela
5. **Overlay** — label renderizado na posição calculada, ancorado no espaço real

---

## Estrutura de dados

### Produto (JSON local por enquanto)
```json
{
  "id": "001",
  "nome": "Fone de Ouvido",
  "ref": "Ref: 001",
  "categoria": "Eletrônico",
  "cor": "Preto",
  "estoque": "3 unidades",
  "localizacao": "Prateleira A1",
  "posicao": { "x": 0, "y": 0.2, "z": -0.5 },
  "marcador": "hiro"
}
```

### Mapa da vitrine
```json
{
  "anchoraId": "qr-vitrine-principal",
  "criadoEm": "2026-06-08",
  "items": [ ...produtos com posições ]
}
```

---

## Itens do protótipo de escritório

| Ref | Nome | Marcador | Status |
|-----|------|----------|--------|
| 001 | Fone de Ouvido | Hiro | ✅ implementado |
| 002 | Teclado Mecânico | Kanji | ✅ implementado |
| 003 | Mouse | ArUco #3 | ⏳ próximo |
| 004 | Carregador | ArUco #4 | ⏳ próximo |

---

## Como rodar o protótipo

```bash
# na pasta do projeto
python3 -m http.server 8080

# browser no computador
http://localhost:8080/vitrine-ar-prototipo.html

# celular na mesma rede WiFi
http://SEU-IP-LOCAL:8080/vitrine-ar-prototipo.html
```

**Marcadores para imprimir:**
- Hiro: https://raw.githack.com/AR-js-org/AR.js/master/data/images/hiro.png
- Kanji: https://raw.githack.com/AR-js-org/AR.js/master/data/images/kanji.png
- Imprimir em pelo menos 10×10 cm

---

## Limitações conhecidas do protótipo atual

- Apenas 2 marcadores padrão disponíveis (Hiro e Kanji) — para mais itens precisa de marcadores ArUco numerados
- Evento de toque nos marcadores funciona melhor em desktop — em mobile AR o clique precisa ser tratado diferente
- AR.js tem drift em ambientes com pouca textura ou muito reflexo (vidro da vitrine real será um desafio)
- Sem persistência: o mapa é hardcoded no HTML, não salva entre sessões

---

## Visão de futuro (roadmap)

**Fase 0 — atual:** protótipo web com objetos do escritório, validar conceito  
**Fase 1:** app Expo com ViroReact, 4 itens, marcadores ArUco  
**Fase 2:** Pokédex completa + lista de reposição exportável  
**Fase 3:** integração com ERP/estoque para quantidades em tempo real  
**Fase 4 (aberto):** visão computacional para detecção automática de produtos (YOLO)

---

## Decisões de design

- **Solo developer** — manter stack o mais simples possível, evitar over-engineering
- **Sem backend no protótipo** — tudo local em JSON, migrar para cloud só quando necessário
- **Mobile first** — o usuário final (vitrinista) usa celular no chão de loja
- **UX não é prioridade no protótipo** — qualquer melhoria vs. busca manual já é ganho real