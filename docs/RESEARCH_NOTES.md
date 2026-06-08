# Research Notes - Game Bar PWA Widget

**Data:** Junho 2026

Este documento resume pesquisa profunda sobre implementação de widgets no Xbox Game Bar, especialmente com conteúdo web (WebView2) e seleção de PWAs.

---

## 1. O SDK é a única maneira?

**Sim, para integração nativa dentro do Win+G.**

- O **Xbox Game Bar SDK** é a forma oficial e suportada pela Microsoft para criar widgets que aparecem no overlay (Win + G).
- Widgets devem ser apps **UWP com XAML**.
- Não existe alternativa oficial para adicionar conteúdo arbitrário (como uma PWA) diretamente no Game Bar sem usar o SDK.
- Outras soluções (PowerToys Always On Top, AutoHotkey, WebView2 standalone, Electron overlays, etc.) funcionam como janelas flutuantes independentes, mas **não integram** no comportamento do Win+G (só aparece quando aperta a tecla).

**Conclusão:** Se o objetivo é o fluxo exato que você quer (só aparece com Win+G), o SDK é o caminho correto.

---

## 2. WebView2 dentro do Game Bar Widget

**Viabilidade: Alta**

- Microsoft lançou o widget oficial **Edge Game Assist** (2024/2025) que é basicamente um mini navegador dentro do Game Bar. Isso prova que conteúdo web é suportado.
- Em issue antigo do repositório de samples (2021), um engenheiro da Microsoft comentou: "WebView2 support in Game Bar is coming, yes. For UWP WebView2, you need to create a UWP XAML widget and throw a WebView2 control into your XAML."
- WebView2 está disponível em UWP e deve funcionar no contexto hospedado do Game Bar.

**Cuidados:**
- Inicialização do WebView2 pode precisar de tratamento especial no ambiente do Game Bar.
- Foco e input precisam ser testados.
- Performance em jogos fullscreen deve ser validada.

---

## 3. Maior Desafio: Descobrir PWAs Instaladas

Não existe API pública simples e oficial para listar todas as PWAs instaladas pelo Edge.

**Abordagens comuns da comunidade:**

1. **Start Menu Shortcuts** (mais usado)
   - PWAs aparecem como atalhos em:
     `%APPDATA%\Microsoft\Windows\Start Menu\Programs`
   - Muitos têm nomes amigáveis e ícones.

2. **Pasta de dados do Edge**
   - `%LocalAppData%\Microsoft\Edge\User Data\Default\Web Applications`
   - Contém pastas por PWA.

3. **AppUserModelId / Protocol handlers**
   - Algumas PWAs registram handlers ou têm AppUserModelId específico.

**Recomendação para implementação:**
- Começar com scanning de atalhos do Start Menu (mais simples e confiável).
- Ter fallback ou método manual de adição de URL.
- Criar modelo `PwaInfo` com Name, Icon e Target URI.

---

## 4. Common Pitfalls (Armadilhas Comuns)

- **Setup do projeto**: Muita gente trava na instalação do workload UWP no Visual Studio e no NuGet do Game Bar SDK.
- **Manifest (Package.appxmanifest)**: Erros aqui são os mais comuns. A extensão do Game Bar precisa estar corretamente declarada.
- **WebView2 initialization**: Pode falhar silenciosamente se não configurado direito no contexto do widget.
- **Foco e input**: O Game Bar é um overlay hospedado. Alguns controles podem não receber input corretamente sem ajustes.
- **Fullscreen games**: Alguns widgets têm problemas de performance ou não aparecem corretamente.
- **Múltiplas instâncias**: Existe documentação de known issues com múltiplos widgets.
- **Descoberta de PWAs**: A parte mais instável. Depende de heurísticas.
- **Sideloading vs Store**: Testar localmente é fácil, publicar no Widget Store tem mais requisitos.

---

## 5. Status Atual do SDK (2026)

- O SDK continua ativo e documentado.
- Game Bar recebeu atualizações de UI em 2025 e o widget Edge Game Assist.
- Não há sinais de depreciação.
- Microsoft ainda investe no Game Bar (especialmente para handhelds e experiência console-like).

---

## 6. Recomendações Estratégicas

1. **Implemente em camadas**:
   - Primeiro: Widget básico aparecendo no Game Bar (Fase 1-2 do plano)
   - Depois: WebView2 simples carregando uma URL fixa
   - Por último: Sistema de seleção + descoberta de PWAs

2. **Comece testando com uma PWA conhecida** (ex: Grok) antes de implementar a descoberta automática.

3. **Use o Edge Game Assist como referência** de como a Microsoft fez web embedding.

4. **Documente bem** o PwaDiscoveryService, pois será a parte mais custom e frágil.

---

**Fontes principais:**
- Documentação oficial do Game Bar SDK
- GitHub XboxGameBarSamples (issue sobre WebView2)
- Anúncios do Edge Game Assist widget
- Comunidade (Reddit, StackOverflow, ElevenForum)