---
title: "How to Easily Add VS Code Extensions in Antigravity When Not Available on Open VSX"
seoTitle: "Add VS Code Extensions Without Open VSX"
seoDescription: "Easily install unavailable VS Code extensions in Antigravity by downloading the VSIX file and manually adding it to the editor"
datePublished: Tue Nov 25 2025 18:44:24 GMT+0000 (Coordinated Universal Time)
cuid: cmiexd8yx000102ld1zb8gome
slug: how-to-easily-add-vs-code-extensions-in-antigravity-when-not-available-on-open-vsx
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764093791189/8c2a15c3-a13e-402a-975b-e5aca7bc67fb.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1764096189821/8d322c24-0031-4df5-924e-811120d90eeb.png
tags: chaicode

---

I began with a simple task: installing the Chai theme extension in Antigravity.  
My first thought was to search for it in the editor’s extension marketplace, but nothing appeared. I tried several different keywords, but still had no success.

It became clear that the theme wasn’t available in the Open VSX registry, which is the only marketplace Antigravity can use by default. Due to licensing rules, Antigravity can't access the official Microsoft marketplace, so it couldn't download the extension automatically.

This left me with only one option: to install it manually.

### Getting the VSIX File

Since the extension wasn't available in Open VSX, I obtained the .vsix package directly from VS Code. Once you have the VSIX file, Antigravity can easily install it locally. In my case, the file was:

`hiteshchoudharycode.chai-theme-0.2.0-web.vsix`

### Installing the Extension Manually

Antigravity supports the same local extension installation process as VS Code. All you need is this command:

```bash
antigravity --install-extension hiteshchoudharycode.chai-theme-0.2.0-web.vsix
```

Right after that, you receive the confirmation:

```bash
Installing extensions...
Extension 'hiteshchoudharycode.chai-theme-0.2.0-web.vsix' was successfully installed.
```

Theme applied.

### Final Notes

If an extension isn’t available in Open VSX, you can always:

1. Download the .vsix file
    
2. Install it manually
    

This method works and saves you from having to switch editors just for a theme or plugin.