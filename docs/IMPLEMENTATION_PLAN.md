# Implementation Plan - gamebar-pwa-selector

**Objetivo:** Criar um widget completo para Xbox Game Bar que permite ao usuário selecionar e visualizar qualquer PWA instalada via Microsoft Edge.

Este documento serve como guia detalhado para um **AI Coding Agent** (Codex, Cursor, Claude, GitHub Copilot Agent, etc.) implementar o projeto do zero ao fim.

---

## 1. Arquitetura Geral

### Componentes Principais

- **GameBarPwaWidget** (UWP App)
  - Registra como Widget do Xbox Game Bar
  - Contém a UI principal (XAML)
  - Hospeda o WebView2

- **PwaSelectorPage** (ou UserControl)
  - Lista as PWAs instaladas
  - Permite seleção

- **Services**
  - `PwaDiscoveryService` → Descobre PWAs instaladas no sistema
  - `SettingsService` → Persiste a última PWA escolhida
  - `GameBarService` → Helpers para integração com o Game Bar SDK

- **ViewModels** (MVVM recomendado)
  - `MainWidgetViewModel`
  - `PwaSelectorViewModel`

### Tecnologias

- UWP + XAML
- Xbox Game Bar SDK (`Microsoft.Gaming.XboxGameBar`)
- WebView2 (`Microsoft.Web.WebView2`)
- C# .NET (recomendado .NET 8/9 com UWP)

---

## 2. Desafios Técnicos Importantes

1. **Descoberta de PWAs instaladas**
   - PWAs do Edge não são fáceis de listar via API oficial simples.
   - Possíveis abordagens:
     - Ler atalhos da pasta `Start Menu > Programs`
     - Usar `Windows.Management.Deployment.PackageManager` (para alguns casos)
     - Procurar na pasta de dados do Edge (`%LocalAppData%\Microsoft\Edge\User Data\Default\Web Applications`)
   - **Tarefa crítica:** Criar um `PwaDiscoveryService` robusto.

2. **WebView2 dentro do Game Bar Widget**
   - O Game Bar roda em um ambiente hospedado.
   - Precisa garantir que o WebView2 inicialize corretamente.
   - Tratar foco, input e ciclo de vida do widget.

3. **Registro do Widget no Game Bar**
   - Modificar o `Package.appxmanifest` com as extensões corretas do Game Bar SDK.
   - Usar `GameBarWidget` activation.

4. **Persistência da seleção**
   - Usar `ApplicationData.LocalSettings` ou arquivo JSON simples.

5. **Performance e Fullscreen**
   - O widget deve carregar rápido e não impactar o jogo.

---

## 3. Plano de Implementação (Passo a Passo para o Agent)

### Fase 1: Setup do Projeto (Obrigatório)

1. Criar solução UWP em `src/GameBarPwaWidget/`
2. Adicionar referências NuGet:
   - `Microsoft.Gaming.XboxGameBar`
   - `Microsoft.Web.WebView2`
3. Configurar `Package.appxmanifest`:
   - Capabilities necessárias
   - Extensões do Game Bar (ver documentação oficial)
4. Criar estrutura de pastas:
   - `Services/`
   - `ViewModels/`
   - `Views/` ou páginas XAML
   - `Models/`

### Fase 2: Integração Básica com Game Bar

1. Implementar ativação do widget (`OnActivated` ou `GameBarWidget`)
2. Criar a página principal `MainWidget.xaml`
3. Fazer o widget aparecer no menu do Game Bar
4. Testar abertura/fechamento via Win + G

### Fase 3: Descoberta de PWAs (PwaDiscoveryService)

1. Criar serviço que lista PWAs instaladas via Edge
2. Representar cada PWA como um modelo simples (`PwaInfo` com Name, Icon, LaunchUri ou AppUserModelId)
3. Exibir a lista em uma ListView ou GridView
4. Permitir seleção

### Fase 4: Integração com WebView2

1. Adicionar controle `WebView2` na UI principal
2. Navegar para a URL da PWA selecionada
3. Tratar eventos de navegação, loading, erros
4. Garantir que funcione dentro do contexto do Game Bar

### Fase 5: Persistência e UX

1. Salvar a última PWA escolhida
2. Na abertura do widget, carregar automaticamente a última PWA (se existir)
3. Adicionar botão "Trocar PWA" que volta para a tela de seleção
4. Melhorar UI (loading indicators, empty state, etc.)

### Fase 6: Polimento e Testes

1. Testar em jogos fullscreen
2. Testar redimensionamento e pinning do widget
3. Verificar performance
4. Adicionar tratamento de erros
5. (Opcional) Publicar no Widget Store

---

## 4. Tarefas Prioritárias (para Issues)

- [ ] Setup inicial do projeto UWP + Game Bar SDK
- [ ] Implementar registro correto do widget no manifest
- [ ] Criar PwaDiscoveryService (descoberta de PWAs)
- [ ] Implementar seleção de PWA + navegação com WebView2
- [ ] Persistência da escolha do usuário
- [ ] UI/UX básica (tela de seleção + visualizador)
- [ ] Testes em ambiente real (jogos)

---

## 5. Referências Críticas

- [Xbox Game Bar SDK Quickstart](https://learn.microsoft.com/en-us/gaming/game-bar/quickstart/introduction)
- [Game Bar Widget Samples](https://github.com/microsoft/XboxGameBarSamples)
- [WebView2 Getting Started (UWP)](https://learn.microsoft.com/en-us/microsoft-edge/webview2/get-started/winui)
- [Edge Game Assist Widget](https://support.xbox.com/help/games-apps/game-setup-and-play/use-microsoft-edge-game-assist-with-game-bar) (exemplo oficial)

---

**Dica para o Agent:** Comece pela Fase 1 e Fase 2. Só avance para descoberta de PWAs depois que o widget básico já estiver aparecendo no Game Bar.