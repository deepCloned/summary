## firefox 安装

+ 下载压缩文件
+ 在压缩文件目录下，打开终端，执行 tar -xvf  Firefox-latest-x86_64.tar.bz2  解压缩
+ sudo mv firefox /opt/firefox 写入文件
+ 新增桌面链接
  - cd /usr/share/applications
  - sudo touch firefox.desktop
  - sudo vim firefox.desktop -- 编辑内容

```
[Desktop Entry]
Name=Firefox
Name[bg]=Firefox
Name[ca]=Firefox
Name[cs]=Firefox
Name[el]=Firefox
Name[es]=Firefox
Name[fa]=Firefox
Name[fi]=Firefox
Name[fr]=Firefox
Name[hu]=Firefox
Name[it]=Firefox
Name[ja]=Firefox
Name[ko]=Firefox
Name[nb]=Firefox
Name[nl]=Firefox
Name[nn]=Firefox
Name[no]=Firefox
Name[pl]=Firefox
Name[pt]=Firefox
Name[pt_BR]=Firefox
Name[ru]=Firefox
Name[sk]=Firefox
Name[sv]=Firefox
Comment=Browse the World Wide Web
Comment[bg]=Сърфиране в Мрежата
Comment[ca]=Navegueu per el web
Comment[cs]=Prohlížení stránek World Wide Webu
Comment[de]=Im Internet surfen
Comment[el]=Περιηγηθείτε στον παγκόσμιο ιστό
Comment[es]=Navegue por la web
Comment[fa]=صفحات شبکه جهانی اینترنت را مرور نمایید
Comment[fi]=Selaa Internetin WWW-sivuja
Comment[fr]=Navigue sur Internet
Comment[hu]=A világháló böngészése
Comment[it]=Esplora il web
Comment[ja]=ウェブを閲覧します
Comment[ko]=웹을 돌아 다닙니다
Comment[nb]=Surf på nettet
Comment[nl]=Verken het internet
Comment[nn]=Surf på nettet
Comment[no]=Surf på nettet
Comment[pl]=Przeglądanie stron WWW 
Comment[pt]=Navegue na Internet
Comment[pt_BR]=Navegue na Internet
Comment[ru]=Обозреватель Всемирной Паутины
Comment[sk]=Prehliadanie internetu
Comment[sv]=Surfa på webben
GenericName=Web Browser
GenericName[bg]=Интернет браузър
GenericName[ca]=Navegador web
GenericName[cs]=Webový prohlížeč
GenericName[de]=Webbrowser
GenericName[el]=Περιηγητής ιστού
GenericName[es]=Navegador web
GenericName[fa]=مرورگر اینترنتی
GenericName[fi]=WWW-selain
GenericName[fr]=Navigateur Web
GenericName[hu]=Webböngésző
GenericName[it]=Browser Web
GenericName[ja]=ウェブ・ブラウザ
GenericName[ko]=웹 브라우저
GenericName[nb]=Nettleser
GenericName[nl]=Webbrowser
GenericName[nn]=Nettlesar
GenericName[no]=Nettleser
GenericName[pl]=Przeglądarka WWW
GenericName[pt]=Navegador Web
GenericName[pt_BR]=Navegador Web
GenericName[ru]=Интернет-браузер
GenericName[sk]=Internetový prehliadač
GenericName[sv]=Webbläsare
X-GNOME-FullName=Firefox Web Browser
X-GNOME-FullName[bg]=Интернет браузър (Firefox)
X-GNOME-FullName[ca]=Navegador web Firefox
X-GNOME-FullName[cs]=Firefox Webový prohlížeč
X-GNOME-FullName[el]=Περιηγήτης Ιστού Firefox
X-GNOME-FullName[es]=Navegador web Firefox
X-GNOME-FullName[fa]=مرورگر اینترنتی Firefox
X-GNOME-FullName[fi]=Firefox-selain
X-GNOME-FullName[fr]=Navigateur Web Firefox
X-GNOME-FullName[hu]=Firefox webböngésző
X-GNOME-FullName[it]=Firefox Browser Web
X-GNOME-FullName[ja]=Firefox ウェブ・ブラウザ
X-GNOME-FullName[ko]=Firefox 웹 브라우저
X-GNOME-FullName[nb]=Firefox Nettleser
X-GNOME-FullName[nl]=Firefox webbrowser
X-GNOME-FullName[nn]=Firefox Nettlesar
X-GNOME-FullName[no]=Firefox Nettleser
X-GNOME-FullName[pl]=Przeglądarka WWW Firefox
X-GNOME-FullName[pt]=Firefox Navegador Web
X-GNOME-FullName[pt_BR]=Navegador Web Firefox
X-GNOME-FullName[ru]=Интернет-браузер Firefox
X-GNOME-FullName[sk]=Internetový prehliadač Firefox
X-GNOME-FullName[sv]=Webbläsaren Firefox
Exec=/opt/firefox/firefox %u
Terminal=false
X-MultipleArgs=false
Type=Application
Icon=firefox
Categories=Network;WebBrowser;
MimeType=text/html;text/xml;application/xhtml+xml;application/xml;application/vnd.mozilla.xul+xml;application/rss+xml;application/rdf+xml;image/gif;image/jpeg;image/png;x-scheme-handler/http;x-scheme-handler/https;
StartupWMClass=Firefox
StartupNotify=true
```

复制 ctrl+c 粘贴 ctrl+shift+v 退出编辑 esc 保存并退出 ZZ