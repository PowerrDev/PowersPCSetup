<img width="1920" height="1200" alt="WinXP" src="https://github.com/user-attachments/assets/b15bcf88-f610-4ab3-bb00-907abe9fd41b" />

<h1 align="center"> Windows 11 Configuration </h1>

<p>This setup is tailored to my personal workflow and performance preferences. Don't use this blindly without knowing what it does, and make sure you understand what each modification does.</p>

##  Why create this repository?

I reinstall Windows every few months to keep my OS clean, remove accumulated junk, and start from a fresh environment! Occasionally, I also perform a complete system wipe when experimenting with new configurations or troubleshooting major issues (such as installing Linux and nuking my Windows Boot Manager...)

This repository serves as a centralized place to document my Windows 11 setup, making it easy to reproduce my preferred configuration after a reinstall :D

## Apps to Install
- Intellij IDEA
- Visual Studio Code
- Chrome
- Firefox
- Discord
- Min Browser
- Claude
- Windhawk
- Open Shell
- HideVolumeOSD
- Flow Launcher
- Steam
- MacType
- Bun
- Node.js
- Scoop
- Oh-My-Posh

## Fonts
- Install Lexica Ultralegible
- Install DM Sans
- Install Inter
- Install JetBrains Mono Nerd Font
- Install BlexMono Nerd Font
- Install Hack Nerd Font

# Cursors
- Install KDE Plasma Cursor
- Install macOS Cursor
- Install Windows Small Cursor

## Settings
Set Font -> Lexica Ultralegible<br />
Set Mouse -> Breeze Cursor (https://github.com/black7375/Breeze-Cursors-for-Windows) <br />
Set Mouse -> Speed (12)<br />
Set Mouse -> Disable pointer precision<br />
Set Mouse -> Scroll 5 lines<br />
Effects -> Disable shadow below desktop icon texts<br />
Color -> Light Blue

### Set Cursor Size (Optional)
Run the following command in PowerShell:
```sh
$size=24; New-ItemProperty -Path 'HKCU:\Control Panel\Cursors' -Name 'CursorBaseSize' -Value $size -PropertyType DWord -Force; Add-Type -TypeDefinition 'using System; using System.Runtime.InteropServices; public class Custom { [DllImport("user32.dll")] public static extern bool SystemParametersInfo(uint uiAction, uint uiParam, uint pvParam, uint fWinIni); }'; [Custom]::SystemParametersInfo(0x2029, 0, $size, 0x01)
```

The following command will set your mouse cursor to 24px, it *might* look blurry on certain cursors.

## Firefox
- Set UI font to "DM Sans"
- Install Tampermonkey extension
- Install Adaptive Tab Colour
- Install AdBlock
- Install Twemojify
- - Create Tampermonkey script


### Firefox: Tampermonkey
```js
// ==UserScript==
// @name         Change system-font to Lexica Ultralegible
// @match        *://*/*
// @grant        none
// ==/UserScript==

(function () {
    ""use strict"";

    const LEXICA = '"Lexica Ultralegible"';

    const systemFontKeywords = [
        '"system-ui"',
        '"-apple-system"',
        '"blinkmacsystemfont"',
        '"segoe ui"',
        '"roboto"',
        '"arial"',
        '"sans-serif"',
        '"ui-sans-serif"'
    ];

    function isSystemFont(font) {
        if (!font) return false;

        const f = font.toLowerCase();
        const parts = font.split("","").map(s => s.trim().toLowerCase());
        const first = parts[0];

        if (!systemFontKeywords.some(k => first.includes(k))) {
            return false;
        }

        return systemFontKeywords.some(k => f.includes(k));
    }

    function patchStylesheet(sheet) {
        let rules;

        try {
            rules = sheet.cssRules;
        } catch {
            return;
        }

        if (!rules) return;

        for (const rule of rules) {
            if (rule.type !== CSSRule.STYLE_RULE) continue;

            const style = rule.style;
            const ff = style.getPropertyValue(""font-family"");

            if (isSystemFont(ff)) {
                style.setProperty(""font-family"", LEXICA, ""important"");
            }
        }
    }

    function run() {
        for (const sheet of document.styleSheets) {
            patchStylesheet(sheet);
        }
    }

    run();

    const observer = new MutationObserver(run);
    observer.observe(document.documentElement, {
        childList: true,
        subtree: true
    });

    const style = document.createElement(""style"");
    style.textContent = `
        html, body {
            font-family: ${LEXICA} !important;
        }

        h1, h2, h3, h4, h5, h6, h7, button, p, input, textarea, select, article {
            font-family: ${LEXICA} !important;
        }

        pre, code {
            font-family: "BlexMono Nerd Font" !important;
            }
    `;
    document.head.appendChild(style);
})();
```

## Intellij IDEA
- Set font to Lexica Ultralegible
- Set theme to Darcula Darker
- Setup everything

## Visual Studio Code
### Install all extensions required:
- TailwindCSS Intellisense
- Prisma
- Prettier
- Monokai Pro
- Material Icons Theme
- Discord Rich Presence
- Custom UI Style
- Set font to Inter (via CSS plugins)

User Settings:
```json
{
    "workbench.iconTheme": "material-icon-theme",
    "oneDarkPro.vivid": true,
    "editor.tabSize": 2,
    "custom-ui-style.font.sansSerif": "Inter",
    "editor.fontFamily": "Hack Nerd Font",
    "editor.lineHeight": 1.5,
    "window.menuBarVisibility": "toggle",
    "git.enableSmartCommit": true,
    "git.confirmSync": false,
    "git.autofetch": true,
    "js/ts.updateImportsOnFileMove.enabled": "always",
    "explorer.compactFolders": false,
    "workbench.colorTheme": "Monokai Pro",
    "diffEditor.ignoreTrimWhitespace": false,
    "workbench.sideBar.location": "right",
    "editor.fontSize": 12,
    "[typescriptreact]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
}
```

## After installing Windows:
- Setup MacType
- Setup browsers
- Setup login into Github, Discord, WhatsApp and Canva
- Apply the configurations in this repository!

## Retrobar
- Set theme to Darkness (https://www.deviantart.com/bikehard2ride/art/Darkness-1166365058)

# Windhawk
After some careful consideration, I decided to use Windhawk instead of Retrobar.
Here's my KDE Plasma theme:
```yaml
theme: ''
styleConstants:
  - taskbandInactiveNormal=<SolidColorBrush Color="#000000" Opacity="0.04" />
  - taskbandPointerOver=<SolidColorBrush Color="#000000" Opacity="0.08" />
  - taskbandActive=<SolidColorBrush Color="#000000" Opacity="0.10" />
  - indicatorActive=<SolidColorBrush Color="#3DAEE9" Opacity="0.9" />
  - indicatorInactive=<SolidColorBrush Color="#000000" Opacity="0.25" />
  - indicatorPointerOver=<SolidColorBrush Color="#3DAEE9" Opacity="0.7" />
  - taskbandAttention=<SolidColorBrush Color="#E67E22" Opacity="0.55" />
  - indicatorAttention=<SolidColorBrush Color="#E67E22" Opacity="0.9" />
  - selectionBorder=<LinearGradientBrush StartPoint="0,0" EndPoint="1,0"><GradientStop Color="Transparent" Offset="0.0" /><GradientStop Color="Transparent" Offset="0.2" /><GradientStop Color="#3DAEE9" Offset="0.2" /><GradientStop Color="#3DAEE9" Offset="0.8" /><GradientStop Color="Transparent" Offset="0.8" /><GradientStop Color="Transparent" Offset="1.0" /></LinearGradientBrush>
  - selectionBorderExtended=<SolidColorBrush Color="#3DAEE9" />
  - desktopButton=https://raw.githubusercontent.com/ramensoftware/windows-11-taskbar-styling-guide/refs/heads/main/Themes/Plasma/ThemeResources/desktop.png
  - plusIndicator=https://raw.githubusercontent.com/ramensoftware/windows-11-taskbar-styling-guide/refs/heads/main/Themes/Plasma/ThemeResources/plus.png
  - StartButton=kubuntu
  - WindhawkBlur=<WindhawkBlur BlurAmount="2" TintColor="#ccffffff" />
  - Acrylic=<AcrylicBrush TintColor="#ffffff" TintOpacity="0.70" FallbackColor="#ffffff" />
controlStyles:
  - target: Taskbar.TaskbarFrame > Grid#RootGrid > Taskbar.TaskbarBackground > Grid > Rectangle#BackgroundFill
    styles:
      - Fill:=$Acrylic
  - target: Rectangle#BackgroundStroke
    styles:
      - Visibility=Collapsed
  - target: Taskbar.TaskListButton > Taskbar.TaskListLabeledButtonPanel
    styles:
      - Padding=0
  - target: Taskbar.TaskListButton > Taskbar.TaskListLabeledButtonPanel@CommonStates > Windows.UI.Xaml.Controls.Border#BackgroundElement
    styles:
      - CornerRadius=0
      - BorderThickness=0
      - BorderBrush=Transparent
      - Margin=0
      - Background@ActiveNormal:=$taskbandActive
      - Background@InactiveNormal:=$taskbandInactiveNormal
      - Background@ActivePointerOver:=$taskbandPointerOver
      - Background@InactivePointerOver:=$taskbandPointerOver
      - Background@ActivePressed:=$taskbandPointerOver
      - Background@InactivePressed:=$taskbandPointerOver
      - Background@MultiWindowNormal:=$taskbandInactiveNormal
      - Background@MultiWindowPointerOver:=$taskbandPointerOver
      - Background@MultiWindowPressed:=$taskbandPointerOver
      - Background@MultiWindowActive:=$taskbandActive
      - Background@RequestingAttention:=$taskbandAttention
      - Background@RequestingAttentionPointerOver:=$taskbandPointerOver
      - Background@RequestingAttentionPressed:=$taskbandPointerOver
      - Background@RequestingAttentionMulti:=$taskbandAttention
      - Background@RequestingAttentionMultiPointerOver:=$taskbandPointerOver
      - Background@RequestingAttentionMultiPressed:=$taskbandPointerOver
  - target: Taskbar.TaskListButton > Taskbar.TaskListLabeledButtonPanel@RunningIndicatorStates > Windows.UI.Xaml.Controls.Border#BackgroundElement
    styles:
      - Opacity@NoRunningIndicator=0
  - target: Taskbar.TaskListLabeledButtonPanel#IconPanel > Windows.UI.Xaml.Controls.Image#Icon
    styles:
      - Padding=6,0,6,0
  - target: Taskbar.TaskListLabeledButtonPanel@CommonStates > Windows.UI.Xaml.Shapes.Rectangle#RunningIndicator
    styles:
      - Width=50
      - RadiusX=0
      - RadiusY=0
      - Height=3
      - VerticalAlignment=Top
      - RenderTransform:=<TranslateTransform X="0" />
      - Margin=0
      - Fill@ActiveNormal:=$indicatorActive
      - Fill@ActivePointerOver:=$indicatorPointerOver
      - Fill@InactiveNormal:=$indicatorInactive
      - Fill@InactivePointerOver:=$indicatorPointerOver
      - Fill@ActivePressed:=$indicatorPointerOver
      - Fill@InactivePressed:=$indicatorPointerOver
      - Fill@MultiWindowNormal:=$indicatorInactive
      - Fill@MultiWindowPointerOver:=$indicatorPointerOver
      - Fill@MultiWindowPressed:=$indicatorPointerOver
      - Fill@MultiWindowActive:=$indicatorActive
      - Fill@RequestingAttention:=$indicatorAttention
      - Fill@RequestingAttentionPointerOver:=$indicatorPointerOver
      - Fill@RequestingAttentionPressed:=$indicatorPointerOver
      - Fill@RequestingAttentionMulti:=$indicatorAttention
      - Fill@RequestingAttentionMultiPointerOver:=$indicatorPointerOver
      - Fill@RequestingAttentionMultiPressed:=$indicatorPointerOver
  - target: Windows.UI.Xaml.Controls.Border#MultiWindowElement
    styles:
      - Visibility=Collapsed
  - target: Taskbar.TaskListLabeledButtonPanel@CommonStates > Windows.UI.Xaml.Shapes.Rectangle#DefaultIcon
    styles:
      - Fill:=<ImageBrush Stretch="Uniform" ImageSource="$plusIndicator" />
      - Width=11
      - Height=11
      - RadiusX=0
      - RadiusY=0
      - VerticalAlignment=Bottom
      - HorizontalAlignment=Center
      - RenderTransform:=<TranslateTransform X="0" />
      - Visibility=Collapsed
      - Visibility@MultiWindowNormal=Visible
      - Visibility@MultiWindowActive=Visible
      - Visibility@MultiWindowPointerOver=Visible
      - Visibility@MultiWindowPressed=Visible
  - target: Taskbar.TaskListButtonPanel#ExperienceToggleButtonRootPanel
    styles:
      - Padding=0
      - Width=50
  - target: Taskbar.TaskListButtonPanel#ExperienceToggleButtonRootPanel@CommonStates > Windows.UI.Xaml.Controls.Border#BackgroundElement
    styles:
      - CornerRadius=0
      - BorderThickness=0
      - Width=32
      - Background=Transparent
  - target: Taskbar.ExperienceToggleButton#LaunchListButton[AutomationProperties.AutomationId=StartButton] > Taskbar.TaskListButtonPanel > Microsoft.UI.Xaml.Controls.AnimatedVisualPlayer#Icon
    styles:
      - Visibility=Collapsed
  - target: Taskbar.TaskListButtonPanel#ExperienceToggleButtonRootPanel@CommonStates
    styles:
      - Width=50
      - BorderBrush:=$selectionBorder
      - BorderThickness=0
      - BorderThickness@ActiveNormal=0,3,0,0
      - BorderThickness@ActivePointerOver=0,3,0,0
      - BorderThickness@ActivePressed=0,3,0,0
  - target: Taskbar.ExperienceToggleButton#LaunchListButton[AutomationProperties.AutomationId=StartButton] > Taskbar.TaskListButtonPanel > Border#BackgroundElement
    styles:
      - Background:=<ImageBrush Stretch="Uniform" ImageSource="https://raw.githubusercontent.com/ramensoftware/windows-11-taskbar-styling-guide/refs/heads/main/Themes/Plasma/ThemeResources/$StartButton.png" />
  - target: Taskbar.AugmentedEntryPointButton[AutomationProperties.AutomationId=WidgetsButton] > Taskbar.TaskListButtonPanel#ExperienceToggleButtonRootPanel
    styles:
      - Width=Auto
  - target: SystemTray.SystemTrayFrame > Grid
    styles:
      - VerticalAlignment=Stretch
      - Height=44
  - target: SystemTray.SystemTrayFrame
    styles:
      - VerticalAlignment=Stretch
      - Height=44
  - target: SystemTray.Stack#MainStack
    styles:
      - VerticalAlignment=Center
      - Height=44
  - target: SystemTray.Stack#NonActivatableStack
    styles:
      - Grid.Column=2
      - VerticalAlignment=Center
      - Height=44
  - target: SystemTray.Stack#NotifyIconStack
    styles:
      - Grid.Column=0
      - VerticalAlignment=Center
      - Height=44
  - target: SystemTray.Stack#ShowDesktopStack
    styles:
      - Width=48
      - VerticalAlignment=Stretch
      - Height=44
  - target: SystemTray.NotificationAreaIcons#NotificationAreaIcons
    styles:
      - Grid.Column=1
      - VerticalAlignment=Center
  - target: SystemTray.OmniButton#ControlCenterButton
    styles:
      - Grid.Column=3
      - VerticalAlignment=Center
  - target: SystemTray.OmniButton#ControlCenterButton
    styles:
      - Visibility=Collapsed

  - target: SystemTray.Stack#MainStack
    styles:
      - Margin=0
      - Padding=0

  - target: SystemTray.Stack#NotifyIconStack
    styles:
      - Margin=0
  - target: SystemTray.OmniButton > Windows.UI.Xaml.Controls.Grid > Windows.UI.Xaml.Controls.Border#BackgroundBorder
    styles:
      - Background=Transparent
      - Margin=1,0,1,0
      - CornerRadius=0
      - VerticalAlignment=Center
  - target: SystemTray.OmniButton > Windows.UI.Xaml.Controls.Grid@CommonStates > Windows.UI.Xaml.Controls.Border#BackgroundBorder
    styles:
      - BorderThickness=0
      - BorderBrush@Checked:=$selectionBorderExtended
      - BorderThickness@Checked=0,3,0,0
      - BorderBrush@CheckedPointerOver:=$selectionBorderExtended
      - BorderThickness@CheckedPointerOver=0,3,0,0
      - BorderBrush@CheckedPressed:=$selectionBorderExtended
      - BorderThickness@CheckedPressed=0,3,0,0
  - target: Windows.UI.Xaml.Controls.Grid#ContainerGrid@ > Windows.UI.Xaml.Controls.Border#BackgroundBorder
    styles:
      - BorderThickness=0
      - BorderBrush@CheckedNormal:=$selectionBorderExtended
      - BorderThickness@CheckedNormal=0,3,0,0
      - BorderBrush@CheckedPointerOver:=$selectionBorderExtended
      - BorderThickness@CheckedPointerOver=0,3,0,0
      - BorderBrush@CheckedPressed:=$selectionBorderExtended
      - BorderThickness@CheckedPressed=0,3,0,0
  - target: SystemTray.NotifyIconView#NotifyItemIcon
    styles:
      - MinWidth=24
      - VerticalAlignment=Center
  - target: SystemTray.NotifyIconView > Windows.UI.Xaml.Controls.Grid > Windows.UI.Xaml.Controls.Border#BackgroundBorder
    styles:
      - Background=Transparent
      - Margin=1,0,1,0
      - CornerRadius=0
      - VerticalAlignment=Center
  - target: SystemTray.IconView#SystemTrayIcon > Windows.UI.Xaml.Controls.Grid > Windows.UI.Xaml.Controls.Border#BackgroundBorder
    styles:
      - Background=Transparent
      - Margin=1,0,1,0
      - CornerRadius=0
      - VerticalAlignment=Center
  - target: SystemTray.ChevronIconView
    styles:
      - Margin=-5,0
      - VerticalAlignment=Center
  - target: SystemTray.ChevronIconView > Windows.UI.Xaml.Controls.Grid > Windows.UI.Xaml.Controls.Border#BackgroundBorder
    styles:
      - Background=Transparent
      - Margin=1,0,1,0
      - CornerRadius=0
      - VerticalAlignment=Center
  - target: SystemTray.ChevronIconView > Windows.UI.Xaml.Controls.Grid#ContainerGrid > Windows.UI.Xaml.Controls.ContentPresenter#ContentPresenter > Windows.UI.Xaml.Controls.Grid#ContentGrid > SystemTray.TextIconContent > Windows.UI.Xaml.Controls.Grid#ContainerGrid > SystemTray.AdaptiveTextBlock#Base > Windows.UI.Xaml.Controls.TextBlock#InnerTextBlock
    styles:
      - FontSize=17.33
      - VerticalAlignment=Center
      - Foreground=#2B2B2B
  - target: Windows.UI.Xaml.Controls.TextBlock#TimeInnerTextBlock
    styles:
      - FontSize=17.33
      - TextAlignment=Center
      - VerticalAlignment=Center
      - FontFamily=Lexica Ultralegible, Segoe UI
      - Foreground=#2B2B2B
  - target: Windows.UI.Xaml.Controls.TextBlock#DateInnerTextBlock
    styles:
      - FontSize=13.33
      - TextAlignment=Center
      - VerticalAlignment=Center
      - FontFamily=Lexica Ultralegible, Segoe UI
      - Margin=0,-5,0,0
      - Foreground=#2B2B2B
  - target: SystemTray.DateTimeIconContent > Windows.UI.Xaml.Controls.Grid#ContainerGrid
    styles:
      - Padding=0
      - VerticalAlignment=Center
  - target: SystemTray.IconView[AutomationProperties.Name=Show Desktop]
    styles:
      - Width=48
      - VerticalAlignment=Center
  - target: Windows.UI.Xaml.Shapes.Rectangle#ShowDesktopPipe
    styles:
      - Width=48
      - Height=44
      - Fill:=<ImageBrush Stretch="None" ImageSource="$desktopButton" />
  - target: SystemTray.StackListView[AutomationProperties.AutomationId=Main]
    styles:
      - Foreground=#2B2B2B
      - Margin=-8,0,0,0
      - VerticalAlignment=Center
  - target: SystemTray.AdaptiveTextBlock#LanguageInnerTextBlock > Windows.UI.Xaml.Controls.TextBlock
    styles:
      - FontFamily=Lexica Ultralegible, Segoe UI
      - VerticalAlignment=Center
      - Foreground=#2B2B2B
themeResourceVariables:
  - ''
xamlDiagnosticsHandling: ''
```
- Enable "Suavização da Fonte"
- Hide inactive icons
- Start automatically on logon

## Projects
Clone GitHub projects & setup environment keys, packages and everything needed. Self-host section will be described in each repository.
