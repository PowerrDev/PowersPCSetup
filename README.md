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
- RetroBar
- Open Shell
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

## Settings
Set Font -> Lexica Ultralegible<br />
Set Mouse -> Breeze Cursor (https://github.com/black7375/Breeze-Cursors-for-Windows) <br />
Set Mouse -> Speed (12)<br />
Set Mouse -> Disable pointer precision<br />
Set Mouse -> Scroll 5 lines<br />
Effects -> Disable shadow below desktop icon texts<br />
Color -> Light Blue

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
- Install all extensions required
- Set font to Inter (via CSS plugins)

## After installing OS:
- Setup MacType
- Setup browsers
- Setup login into Github, Discord, WhatsApp and Canva
- Apply the configurations in this repository!

## Retrobar
- Set theme to Windows Longhorn Aero
- Enable "Suavização da Fonte"
- Hide inactive icons
- Start automatically on logon

## Projects
Clone GitHub projects & setup environment keys, packages and everything needed. Self-host section will be described in each repository.
