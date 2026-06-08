# gamebar-pwa-selector

Widget customizado para **Xbox Game Bar** que permite selecionar e visualizar PWAs instaladas via Microsoft Edge diretamente no overlay (Win + G).

Evita Alt+Tab, mantém o fluxo natural do Game Bar e só aparece quando você quiser.

## 🚀 Status

**Inicial / Em desenvolvimento**

## 🎯 Objetivo

Criar um widget para o Game Bar que funcione como um "visualizador rápido" de PWAs. O usuário pressiona Win + G, escolhe ou carrega automaticamente a PWA desejada e interage sem sair do jogo/aplicativo em fullscreen.

## 📝 Especificação (Requirements)

### Visão Geral

Criar um widget para o Xbox Game Bar (ativado via `Win + G`) que permita ao usuário selecionar uma PWA instalada via Microsoft Edge e visualizar seu conteúdo diretamente dentro do overlay do Game Bar, sem precisar fazer Alt+Tab ou manter janelas sempre no topo.

O widget deve funcionar como um “visualizador rápido” de PWAs, aparecendo apenas quando o usuário abre o Game Bar, mantendo o fluxo de uso típico do Win+G (abre → usa → fecha).

### Requisitos Funcionais

| ID     | Requisito                                                                 | Prioridade |
|--------|---------------------------------------------------------------------------|------------|
| FR-01  | O widget deve aparecer na lista de widgets disponíveis do Xbox Game Bar   | Alta       |
| FR-02  | Na primeira execução (ou quando nenhuma PWA estiver selecionada), o widget deve exibir uma interface de seleção de PWAs instaladas no sistema | Alta       |
| FR-03  | O usuário deve conseguir selecionar **uma** PWA da lista de instaladas    | Alta       |
| FR-04  | Após a seleção, o widget deve carregar e exibir o conteúdo da PWA escolhida usando WebView2 | Alta       |
| FR-05  | O widget deve lembrar da última PWA selecionada entre sessões             | Alta       |
| FR-06  | O usuário deve poder trocar a PWA selecionada a qualquer momento             | Média     |
| FR-07  | O widget deve suportar redimensionamento e posicionamento dentro do Game Bar | Alta       |
| FR-08  | O widget deve funcionar corretamente quando o Game Bar é aberto em jogos em fullscreen | Alta       |
| FR-09  | Suporte básico a navegação dentro da PWA                                 | Alta       |
| FR-10  | O widget deve ter opção de “Fixar” (pin) como outros widgets do Game Bar  | Baixa      |

### Fluxo do Usuário

1. Usuário pressiona **Win + G**
2. Abre o menu de widgets e seleciona o widget "PWA Selector"
3. Primeira vez / sem PWA selecionada → mostra tela de seleção com lista das PWAs instaladas via Edge
4. Usuário escolhe uma PWA
5. Conteúdo da PWA é carregado dentro do widget via WebView2
6. Usuário interage normalmente
7. Fecha o Game Bar ou foca de volta no jogo → widget some
8. Na próxima abertura, já carrega a última PWA escolhida

### Critérios de Aceitação

- Widget aparece corretamente no menu do Game Bar
- Usuário consegue selecionar uma PWA instalada
- PWA escolhida carrega e funciona dentro do widget
- Seleção persiste após reiniciar o PC
- Não causa impacto perceptível de performance no jogo
- Funciona em fullscreen sem quebrar o jogo

## 🛠️ Tecnologias

- **UWP** (Universal Windows Platform)
- **Xbox Game Bar SDK**
- **WebView2** (Microsoft.Web.WebView2)
- C# + XAML

## 📁 Estrutura sugerida do projeto

```
gamebar-pwa-selector/
├── src/
│   ├── GameBarPwaWidget/
│       ├── App.xaml
│       ├── MainWidget.xaml          # UI do widget
│       ├── PwaSelectorPage.xaml     # Tela de seleção de PWAs
│       ├── Services/
│       ├── ViewModels/
├── docs/
├── .gitignore
├── README.md
└── LICENSE
```

## 🚀 Como começar (próximos passos)

1. Clone o repositório
2. Abra no Visual Studio 2022/2026 com workload **UWP** instalado
3. Adicione o NuGet `Microsoft.Gaming.XboxGameBar` (Game Bar SDK)
4. Adicione o NuGet `Microsoft.Web.WebView2`
5. Siga a documentação oficial:
   - [Xbox Game Bar SDK](https://learn.microsoft.com/en-us/gaming/game-bar/)
   - [Quickstart](https://learn.microsoft.com/en-us/gaming/game-bar/quickstart/introduction)

## 📖 Referências

- [Xbox Game Bar SDK Documentation](https://learn.microsoft.com/en-us/gaming/game-bar/)
- [WebView2 Documentation](https://learn.microsoft.com/en-us/microsoft-edge/webview2/)
- [Edge Game Assist](https://support.xbox.com/help/games-apps/game-setup-and-play/use-microsoft-edge-game-assist-with-game-bar) (exemplo oficial de web no Game Bar)

## 💬 Contribuição

Pull requests são bem-vindos! Abra uma issue para discutir features ou bugs.

---

**Criado por Gustavo** | Gustavo's Game Bar PWA Widget