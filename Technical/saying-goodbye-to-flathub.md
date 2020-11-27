# Saying Goodbye to Flathub

[Flatpak](http://docs.flatpak.org/en/latest/getting-started.html) format is being touted to be a robust way of doing package management (list available, install, update, remove etc.) in Linux systems for quite some time. I tried [Snap packages](https://itsfoss.com/use-snap-packages-ubuntu-16-04/) from Canonical (the makers of Ubuntu) a while ago but didn't use it after that one attempt (I don't even remember what I installed using it)

Just as all packages one installs using [*apt*](https://mvogt.wordpress.com/2014/04/04/apt-1-0/) (It is what you can use in place of the long known *apt-get*!) come from APT software repositories stored in a remote server, Flatpak packages come from Flathub (the official repository). I thought of giving the Flathub/Flatpak ecosystem a try by using it to obtain FOSS (Free and Open Source Software) build of [Microsoft's VSCode](https://github.com/Microsoft/vscode)

The installation was smooth and I really liked writing content for this blog ina Markdown supported editor (with Live preview provided by an add-on) as well as using Bash terminal right from the bottom. However I found out that I can't execute certain command line tools due to the [*Sandboxing* model of Flatpak installations](https://github.com/flathub/com.visualstudio.code.oss/issues/22#issuecomment-433643938). In particular, my roadblock is similar to that of the reporter for this [issue](https://github.com/flathub/com.visualstudio.code/issues/44). So I resorted to a workaround of [this form](https://github.com/flathub/com.visualstudio.code.oss/issues/22#issuecomment-436932639)

In my Ubuntu system, the application launcher uses *.desktop* files to create clickable shortcuts for applications. I found that the relevant file to edit in my current situation is **com.visualstudio.code.oss.desktop**. This is what I made it look like:

````conf
[Desktop Entry]
Name=Visual Studio Code - OSS
Comment=Code Editing. Redefined.
GenericName=Text Editor
Exec=/var/lib/flatpak/app/com.visualstudio.code.oss/x86_64/stable/active/files/share/code-oss/code-oss --unity-launch @@ %F @@
Icon=com.visualstudio.code.oss
Type=Application
StartupNotify=false
StartupWMClass=Code - OSS
Categories=Utility;TextEditor;Development;IDE;
MimeType=text/plain;inode/directory;
Actions=new-empty-window;
Keywords=vscode;
X-Flatpak=com.visualstudio.code.oss

[Desktop Action new-empty-window]
Name=New Empty Window
Exec=/var/lib/flatpak/app/com.visualstudio.code.oss/x86_64/stable/active/files/share/code-oss/code-oss --new-window @@ %F @@
Icon=com.visualstudio.code.oss
````

I only edited the 2 *Exec* lines and saved it. Doing this, I could overcome the sandboxing model and just use the system binaries directly from the integrated teminal in VSCode thereafter.

However, with every update to VScode, provided via Flatpak system, my modifications got replaced and I had to re-apply the same fix over and over. This became a pain as I had to keep updating this script for Gnome Application launcher shortcuts to work.

I finally gave up and started considering building from source myself. However the notes and comments [here](https://github.com/VSCodium/vscodium#why-does-this-exist) and [here](https://github.com/Microsoft/vscode/issues/45978) took me to consider following this [barebones project](https://github.com/VSCodium/vscodium) instead.

As pointed to from the README.md of the above project, I proceeded to embrace [this](https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/tree/repos/debs) project.

By the time this post was done, I finished removing the existing Flathub provided VSCode and installed the new **VSCodium**! I'm writing this paragraph from the new installation and I'm very happy with what I received. The process of installing VSCodium has been frictionless and now I can just use the integrated terminal! No more sandbox non-sense to deal with.

Head over to the [official README](https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo) for installation instructions. Thank the developer [Pavlo Rudyi](https://gitlab.com/paulcarroty) for the efforts!