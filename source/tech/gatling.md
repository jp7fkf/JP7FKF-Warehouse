# Gatling install battle

```
[root@host ~]# yum install -y java
Loaded plugins: fastestmirror, priorities
Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
base                                                                                                                | 3.6 kB  00:00:00
epel/x86_64/metalink                                                                                                | 7.7 kB  00:00:00
epel                                                                                                                | 5.3 kB  00:00:00
extras                                                                                                              | 3.4 kB  00:00:00
updates                                                                                                             | 3.4 kB  00:00:00
(1/4): epel/x86_64/updateinfo                                                                                       | 788 kB  00:00:00
(2/4): extras/7/x86_64/primary_db                                                                                   | 200 kB  00:00:01
(3/4): updates/7/x86_64/primary_db                                                                                  | 5.0 MB  00:00:09
(4/4): epel/x86_64/primary_db                                                                                       | 6.0 MB  00:00:09
Determining fastest mirrors
 * base: ftp-srv2.kddilabs.jp
 * epel: ftp.riken.jp
 * extras: ftp-srv2.kddilabs.jp
 * updates: ftp-srv2.kddilabs.jp
Resolving Dependencies
--> Running transaction check
---> Package java-1.8.0-openjdk.x86_64 1:1.8.0.212.b04-0.el7_6 will be installed
--> Processing Dependency: java-1.8.0-openjdk-headless(x86-64) = 1:1.8.0.212.b04-0.el7_6 for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: xorg-x11-fonts-Type1 for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libpng15.so.15(PNG15_0)(64bit) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libjvm.so(SUNWprivate_1.1)(64bit) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libjava.so(SUNWprivate_1.1)(64bit) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libXcomposite(x86-64) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: gtk2(x86-64) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: fontconfig(x86-64) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libpng15.so.15()(64bit) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libjvm.so()(64bit) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libjava.so()(64bit) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libgif.so.4()(64bit) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libXtst.so.6()(64bit) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libXrender.so.1()(64bit) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libXi.so.6()(64bit) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libXext.so.6()(64bit) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: libX11.so.6()(64bit) for package: 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
--> Running transaction check
---> Package fontconfig.x86_64 0:2.13.0-4.3.el7 will be installed
--> Processing Dependency: freetype >= 2.8-7 for package: fontconfig-2.13.0-4.3.el7.x86_64
--> Processing Dependency: fontpackages-filesystem for package: fontconfig-2.13.0-4.3.el7.x86_64
--> Processing Dependency: dejavu-sans-fonts for package: fontconfig-2.13.0-4.3.el7.x86_64
---> Package giflib.x86_64 0:4.1.6-9.el7 will be installed
--> Processing Dependency: libSM.so.6()(64bit) for package: giflib-4.1.6-9.el7.x86_64
--> Processing Dependency: libICE.so.6()(64bit) for package: giflib-4.1.6-9.el7.x86_64
---> Package gtk2.x86_64 0:2.24.31-1.el7 will be installed
--> Processing Dependency: pango >= 1.20.0-1 for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libXrandr >= 1.2.99.4-2 for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: atk >= 1.29.4-2 for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: hicolor-icon-theme for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: gtk-update-icon-cache for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libpangoft2-1.0.so.0()(64bit) for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libpangocairo-1.0.so.0()(64bit) for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libpango-1.0.so.0()(64bit) for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libgdk_pixbuf-2.0.so.0()(64bit) for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libcups.so.2()(64bit) for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libcairo.so.2()(64bit) for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libatk-1.0.so.0()(64bit) for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libXrandr.so.2()(64bit) for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libXinerama.so.1()(64bit) for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libXfixes.so.3()(64bit) for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libXdamage.so.1()(64bit) for package: gtk2-2.24.31-1.el7.x86_64
--> Processing Dependency: libXcursor.so.1()(64bit) for package: gtk2-2.24.31-1.el7.x86_64
---> Package java-1.8.0-openjdk-headless.x86_64 1:1.8.0.212.b04-0.el7_6 will be installed
--> Processing Dependency: tzdata-java >= 2015d for package: 1:java-1.8.0-openjdk-headless-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: nss-softokn(x86-64) >= 3.36.0 for package: 1:java-1.8.0-openjdk-headless-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: nss(x86-64) >= 3.36.0 for package: 1:java-1.8.0-openjdk-headless-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: copy-jdk-configs >= 3.3 for package: 1:java-1.8.0-openjdk-headless-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: pcsc-lite-libs(x86-64) for package: 1:java-1.8.0-openjdk-headless-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: lksctp-tools(x86-64) for package: 1:java-1.8.0-openjdk-headless-1.8.0.212.b04-0.el7_6.x86_64
--> Processing Dependency: jpackage-utils for package: 1:java-1.8.0-openjdk-headless-1.8.0.212.b04-0.el7_6.x86_64
---> Package libX11.x86_64 0:1.6.5-2.el7 will be installed
--> Processing Dependency: libX11-common >= 1.6.5-2.el7 for package: libX11-1.6.5-2.el7.x86_64
--> Processing Dependency: libxcb.so.1()(64bit) for package: libX11-1.6.5-2.el7.x86_64
---> Package libXcomposite.x86_64 0:0.4.4-4.1.el7 will be installed
---> Package libXext.x86_64 0:1.3.3-3.el7 will be installed
---> Package libXi.x86_64 0:1.7.9-1.el7 will be installed
---> Package libXrender.x86_64 0:0.9.10-1.el7 will be installed
---> Package libXtst.x86_64 0:1.2.3-1.el7 will be installed
---> Package libpng.x86_64 2:1.5.13-7.el7_2 will be installed
---> Package xorg-x11-fonts-Type1.noarch 0:7.5-9.el7 will be installed
--> Processing Dependency: ttmkfdir for package: xorg-x11-fonts-Type1-7.5-9.el7.noarch
--> Processing Dependency: ttmkfdir for package: xorg-x11-fonts-Type1-7.5-9.el7.noarch
--> Processing Dependency: mkfontdir for package: xorg-x11-fonts-Type1-7.5-9.el7.noarch
--> Processing Dependency: mkfontdir for package: xorg-x11-fonts-Type1-7.5-9.el7.noarch
--> Running transaction check
---> Package atk.x86_64 0:2.28.1-1.el7 will be installed
---> Package cairo.x86_64 0:1.15.12-3.el7 will be installed
--> Processing Dependency: libpixman-1.so.0()(64bit) for package: cairo-1.15.12-3.el7.x86_64
--> Processing Dependency: libGL.so.1()(64bit) for package: cairo-1.15.12-3.el7.x86_64
--> Processing Dependency: libEGL.so.1()(64bit) for package: cairo-1.15.12-3.el7.x86_64
---> Package copy-jdk-configs.noarch 0:3.3-10.el7_5 will be installed
---> Package cups-libs.x86_64 1:1.6.3-35.el7 will be installed
--> Processing Dependency: libavahi-common.so.3()(64bit) for package: 1:cups-libs-1.6.3-35.el7.x86_64
--> Processing Dependency: libavahi-client.so.3()(64bit) for package: 1:cups-libs-1.6.3-35.el7.x86_64
---> Package dejavu-sans-fonts.noarch 0:2.33-6.el7 will be installed
--> Processing Dependency: dejavu-fonts-common = 2.33-6.el7 for package: dejavu-sans-fonts-2.33-6.el7.noarch
---> Package fontpackages-filesystem.noarch 0:1.44-8.el7 will be installed
---> Package freetype.x86_64 0:2.4.11-12.el7 will be updated
---> Package freetype.x86_64 0:2.8-12.el7_6.1 will be an update
---> Package gdk-pixbuf2.x86_64 0:2.36.12-3.el7 will be installed
--> Processing Dependency: glib2(x86-64) >= 2.48.0 for package: gdk-pixbuf2-2.36.12-3.el7.x86_64
--> Processing Dependency: libjasper.so.1()(64bit) for package: gdk-pixbuf2-2.36.12-3.el7.x86_64
---> Package gtk-update-icon-cache.x86_64 0:3.22.30-3.el7 will be installed
---> Package hicolor-icon-theme.noarch 0:0.12-7.el7 will be installed
---> Package javapackages-tools.noarch 0:3.4.1-11.el7 will be installed
--> Processing Dependency: python-javapackages = 3.4.1-11.el7 for package: javapackages-tools-3.4.1-11.el7.noarch
--> Processing Dependency: libxslt for package: javapackages-tools-3.4.1-11.el7.noarch
---> Package libICE.x86_64 0:1.0.9-9.el7 will be installed
---> Package libSM.x86_64 0:1.2.2-2.el7 will be installed
---> Package libX11-common.noarch 0:1.6.5-2.el7 will be installed
---> Package libXcursor.x86_64 0:1.1.15-1.el7 will be installed
---> Package libXdamage.x86_64 0:1.1.4-4.1.el7 will be installed
---> Package libXfixes.x86_64 0:5.0.3-1.el7 will be installed
---> Package libXinerama.x86_64 0:1.1.3-2.1.el7 will be installed
---> Package libXrandr.x86_64 0:1.5.1-2.el7 will be installed
---> Package libxcb.x86_64 0:1.13-1.el7 will be installed
--> Processing Dependency: libXau.so.6()(64bit) for package: libxcb-1.13-1.el7.x86_64
---> Package lksctp-tools.x86_64 0:1.0.17-2.el7 will be installed
---> Package nss.x86_64 0:3.21.0-17.el7 will be updated
--> Processing Dependency: nss = 3.21.0-17.el7 for package: nss-sysinit-3.21.0-17.el7.x86_64
--> Processing Dependency: nss(x86-64) = 3.21.0-17.el7 for package: nss-tools-3.21.0-17.el7.x86_64
---> Package nss.x86_64 0:3.36.0-7.1.el7_6 will be an update
--> Processing Dependency: nss-util >= 3.36.0-1.1 for package: nss-3.36.0-7.1.el7_6.x86_64
--> Processing Dependency: nspr >= 4.19.0 for package: nss-3.36.0-7.1.el7_6.x86_64
--> Processing Dependency: nss-pem(x86-64) for package: nss-3.36.0-7.1.el7_6.x86_64
--> Processing Dependency: libnssutil3.so(NSSUTIL_3.31)(64bit) for package: nss-3.36.0-7.1.el7_6.x86_64
--> Processing Dependency: libnssutil3.so(NSSUTIL_3.24)(64bit) for package: nss-3.36.0-7.1.el7_6.x86_64
---> Package nss-softokn.x86_64 0:3.16.2.3-14.4.el7 will be updated
---> Package nss-softokn.x86_64 0:3.36.0-5.el7_5 will be an update
--> Processing Dependency: nss-softokn-freebl(x86-64) >= 3.36.0-5.el7_5 for package: nss-softokn-3.36.0-5.el7_5.x86_64
---> Package pango.x86_64 0:1.42.4-2.el7_6 will be installed
--> Processing Dependency: libthai(x86-64) >= 0.1.9 for package: pango-1.42.4-2.el7_6.x86_64
--> Processing Dependency: libXft(x86-64) >= 2.0.0 for package: pango-1.42.4-2.el7_6.x86_64
--> Processing Dependency: harfbuzz(x86-64) >= 1.4.2 for package: pango-1.42.4-2.el7_6.x86_64
--> Processing Dependency: fribidi(x86-64) >= 1.0 for package: pango-1.42.4-2.el7_6.x86_64
--> Processing Dependency: libthai.so.0(LIBTHAI_0.1)(64bit) for package: pango-1.42.4-2.el7_6.x86_64
--> Processing Dependency: libthai.so.0()(64bit) for package: pango-1.42.4-2.el7_6.x86_64
--> Processing Dependency: libharfbuzz.so.0()(64bit) for package: pango-1.42.4-2.el7_6.x86_64
--> Processing Dependency: libfribidi.so.0()(64bit) for package: pango-1.42.4-2.el7_6.x86_64
--> Processing Dependency: libXft.so.2()(64bit) for package: pango-1.42.4-2.el7_6.x86_64
---> Package pcsc-lite-libs.x86_64 0:1.8.8-8.el7 will be installed
---> Package ttmkfdir.x86_64 0:3.0.9-42.el7 will be installed
---> Package tzdata-java.noarch 0:2019a-1.el7 will be installed
---> Package xorg-x11-font-utils.x86_64 1:7.5-21.el7 will be installed
--> Processing Dependency: libfontenc.so.1()(64bit) for package: 1:xorg-x11-font-utils-7.5-21.el7.x86_64
--> Running transaction check
---> Package avahi-libs.x86_64 0:0.6.31-19.el7 will be installed
---> Package dejavu-fonts-common.noarch 0:2.33-6.el7 will be installed
---> Package fribidi.x86_64 0:1.0.2-1.el7 will be installed
---> Package glib2.x86_64 0:2.46.2-4.el7 will be updated
---> Package glib2.x86_64 0:2.56.1-2.el7 will be an update
---> Package harfbuzz.x86_64 0:1.7.5-2.el7 will be installed
--> Processing Dependency: libgraphite2.so.3()(64bit) for package: harfbuzz-1.7.5-2.el7.x86_64
---> Package jasper-libs.x86_64 0:1.900.1-33.el7 will be installed
---> Package libXau.x86_64 0:1.0.8-2.1.el7 will be installed
---> Package libXft.x86_64 0:2.3.2-2.el7 will be installed
---> Package libfontenc.x86_64 0:1.1.3-3.el7 will be installed
---> Package libglvnd-egl.x86_64 1:1.0.1-0.8.git5baa1e5.el7 will be installed
--> Processing Dependency: libglvnd(x86-64) = 1:1.0.1-0.8.git5baa1e5.el7 for package: 1:libglvnd-egl-1.0.1-0.8.git5baa1e5.el7.x86_64
--> Processing Dependency: mesa-libEGL(x86-64) >= 13.0.4-1 for package: 1:libglvnd-egl-1.0.1-0.8.git5baa1e5.el7.x86_64
--> Processing Dependency: libGLdispatch.so.0()(64bit) for package: 1:libglvnd-egl-1.0.1-0.8.git5baa1e5.el7.x86_64
---> Package libglvnd-glx.x86_64 1:1.0.1-0.8.git5baa1e5.el7 will be installed
--> Processing Dependency: mesa-libGL(x86-64) >= 13.0.4-1 for package: 1:libglvnd-glx-1.0.1-0.8.git5baa1e5.el7.x86_64
---> Package libthai.x86_64 0:0.1.14-9.el7 will be installed
---> Package libxslt.x86_64 0:1.1.28-5.el7 will be installed
---> Package nspr.x86_64 0:4.11.0-1.el7_2 will be updated
---> Package nspr.x86_64 0:4.19.0-1.el7_5 will be an update
---> Package nss-pem.x86_64 0:1.0.3-5.el7_6.1 will be installed
---> Package nss-softokn-freebl.x86_64 0:3.16.2.3-14.4.el7 will be updated
---> Package nss-softokn-freebl.x86_64 0:3.36.0-5.el7_5 will be an update
---> Package nss-sysinit.x86_64 0:3.21.0-17.el7 will be updated
---> Package nss-sysinit.x86_64 0:3.36.0-7.1.el7_6 will be an update
---> Package nss-tools.x86_64 0:3.21.0-17.el7 will be updated
---> Package nss-tools.x86_64 0:3.36.0-7.1.el7_6 will be an update
---> Package nss-util.x86_64 0:3.21.0-2.2.el7_2 will be updated
---> Package nss-util.x86_64 0:3.36.0-1.1.el7_6 will be an update
---> Package pixman.x86_64 0:0.34.0-1.el7 will be installed
---> Package python-javapackages.noarch 0:3.4.1-11.el7 will be installed
--> Processing Dependency: python-lxml for package: python-javapackages-3.4.1-11.el7.noarch
--> Running transaction check
---> Package graphite2.x86_64 0:1.3.10-1.el7_3 will be installed
---> Package libglvnd.x86_64 1:1.0.1-0.8.git5baa1e5.el7 will be installed
---> Package mesa-libEGL.x86_64 0:18.0.5-4.el7_6 will be installed
--> Processing Dependency: mesa-libgbm = 18.0.5-4.el7_6 for package: mesa-libEGL-18.0.5-4.el7_6.x86_64
--> Processing Dependency: libxshmfence.so.1()(64bit) for package: mesa-libEGL-18.0.5-4.el7_6.x86_64
--> Processing Dependency: libwayland-server.so.0()(64bit) for package: mesa-libEGL-18.0.5-4.el7_6.x86_64
--> Processing Dependency: libwayland-client.so.0()(64bit) for package: mesa-libEGL-18.0.5-4.el7_6.x86_64
--> Processing Dependency: libglapi.so.0()(64bit) for package: mesa-libEGL-18.0.5-4.el7_6.x86_64
--> Processing Dependency: libgbm.so.1()(64bit) for package: mesa-libEGL-18.0.5-4.el7_6.x86_64
---> Package mesa-libGL.x86_64 0:18.0.5-4.el7_6 will be installed
--> Processing Dependency: libdrm >= 2.4.83 for package: mesa-libGL-18.0.5-4.el7_6.x86_64
--> Processing Dependency: libXxf86vm.so.1()(64bit) for package: mesa-libGL-18.0.5-4.el7_6.x86_64
---> Package python-lxml.x86_64 0:3.2.1-4.el7 will be installed
--> Running transaction check
---> Package libXxf86vm.x86_64 0:1.1.4-1.el7 will be installed
---> Package libdrm.x86_64 0:2.4.67-3.el7 will be updated
---> Package libdrm.x86_64 0:2.4.91-3.el7 will be an update
---> Package libwayland-client.x86_64 0:1.15.0-1.el7 will be installed
---> Package libwayland-server.x86_64 0:1.15.0-1.el7 will be installed
---> Package libxshmfence.x86_64 0:1.2-1.el7 will be installed
---> Package mesa-libgbm.x86_64 0:18.0.5-4.el7_6 will be installed
---> Package mesa-libglapi.x86_64 0:18.0.5-4.el7_6 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===========================================================================================================================================
 Package                                    Arch                  Version                                     Repository              Size
===========================================================================================================================================
Installing:
 java-1.8.0-openjdk                         x86_64                1:1.8.0.212.b04-0.el7_6                     updates                270 k
Installing for dependencies:
 atk                                        x86_64                2.28.1-1.el7                                base                   263 k
 avahi-libs                                 x86_64                0.6.31-19.el7                               base                    61 k
 cairo                                      x86_64                1.15.12-3.el7                               base                   741 k
 copy-jdk-configs                           noarch                3.3-10.el7_5                                base                    21 k
 cups-libs                                  x86_64                1:1.6.3-35.el7                              base                   357 k
 dejavu-fonts-common                        noarch                2.33-6.el7                                  base                    64 k
 dejavu-sans-fonts                          noarch                2.33-6.el7                                  base                   1.4 M
 fontconfig                                 x86_64                2.13.0-4.3.el7                              base                   254 k
 fontpackages-filesystem                    noarch                1.44-8.el7                                  base                   9.9 k
 fribidi                                    x86_64                1.0.2-1.el7                                 base                    79 k
 gdk-pixbuf2                                x86_64                2.36.12-3.el7                               base                   570 k
 giflib                                     x86_64                4.1.6-9.el7                                 base                    40 k
 graphite2                                  x86_64                1.3.10-1.el7_3                              base                   115 k
 gtk-update-icon-cache                      x86_64                3.22.30-3.el7                               base                    28 k
 gtk2                                       x86_64                2.24.31-1.el7                               base                   3.4 M
 harfbuzz                                   x86_64                1.7.5-2.el7                                 base                   267 k
 hicolor-icon-theme                         noarch                0.12-7.el7                                  base                    42 k
 jasper-libs                                x86_64                1.900.1-33.el7                              base                   150 k
 java-1.8.0-openjdk-headless                x86_64                1:1.8.0.212.b04-0.el7_6                     updates                 32 M
 javapackages-tools                         noarch                3.4.1-11.el7                                base                    73 k
 libICE                                     x86_64                1.0.9-9.el7                                 base                    66 k
 libSM                                      x86_64                1.2.2-2.el7                                 base                    39 k
 libX11                                     x86_64                1.6.5-2.el7                                 base                   606 k
 libX11-common                              noarch                1.6.5-2.el7                                 base                   164 k
 libXau                                     x86_64                1.0.8-2.1.el7                               base                    29 k
 libXcomposite                              x86_64                0.4.4-4.1.el7                               base                    22 k
 libXcursor                                 x86_64                1.1.15-1.el7                                base                    30 k
 libXdamage                                 x86_64                1.1.4-4.1.el7                               base                    20 k
 libXext                                    x86_64                1.3.3-3.el7                                 base                    39 k
 libXfixes                                  x86_64                5.0.3-1.el7                                 base                    18 k
 libXft                                     x86_64                2.3.2-2.el7                                 base                    58 k
 libXi                                      x86_64                1.7.9-1.el7                                 base                    40 k
 libXinerama                                x86_64                1.1.3-2.1.el7                               base                    14 k
 libXrandr                                  x86_64                1.5.1-2.el7                                 base                    27 k
 libXrender                                 x86_64                0.9.10-1.el7                                base                    26 k
 libXtst                                    x86_64                1.2.3-1.el7                                 base                    20 k
 libXxf86vm                                 x86_64                1.1.4-1.el7                                 base                    18 k
 libfontenc                                 x86_64                1.1.3-3.el7                                 base                    31 k
 libglvnd                                   x86_64                1:1.0.1-0.8.git5baa1e5.el7                  base                    89 k
 libglvnd-egl                               x86_64                1:1.0.1-0.8.git5baa1e5.el7                  base                    44 k
 libglvnd-glx                               x86_64                1:1.0.1-0.8.git5baa1e5.el7                  base                   125 k
 libpng                                     x86_64                2:1.5.13-7.el7_2                            base                   213 k
 libthai                                    x86_64                0.1.14-9.el7                                base                   187 k
 libwayland-client                          x86_64                1.15.0-1.el7                                base                    33 k
 libwayland-server                          x86_64                1.15.0-1.el7                                base                    39 k
 libxcb                                     x86_64                1.13-1.el7                                  base                   214 k
 libxshmfence                               x86_64                1.2-1.el7                                   base                   7.2 k
 libxslt                                    x86_64                1.1.28-5.el7                                base                   242 k
 lksctp-tools                               x86_64                1.0.17-2.el7                                base                    88 k
 mesa-libEGL                                x86_64                18.0.5-4.el7_6                              updates                102 k
 mesa-libGL                                 x86_64                18.0.5-4.el7_6                              updates                162 k
 mesa-libgbm                                x86_64                18.0.5-4.el7_6                              updates                 38 k
 mesa-libglapi                              x86_64                18.0.5-4.el7_6                              updates                 44 k
 nss-pem                                    x86_64                1.0.3-5.el7_6.1                             updates                 74 k
 pango                                      x86_64                1.42.4-2.el7_6                              updates                280 k
 pcsc-lite-libs                             x86_64                1.8.8-8.el7                                 base                    34 k
 pixman                                     x86_64                0.34.0-1.el7                                base                   248 k
 python-javapackages                        noarch                3.4.1-11.el7                                base                    31 k
 python-lxml                                x86_64                3.2.1-4.el7                                 base                   758 k
 ttmkfdir                                   x86_64                3.0.9-42.el7                                base                    48 k
 tzdata-java                                noarch                2019a-1.el7                                 updates                187 k
 xorg-x11-font-utils                        x86_64                1:7.5-21.el7                                base                   104 k
 xorg-x11-fonts-Type1                       noarch                7.5-9.el7                                   base                   521 k
Updating for dependencies:
 freetype                                   x86_64                2.8-12.el7_6.1                              updates                380 k
 glib2                                      x86_64                2.56.1-2.el7                                base                   2.5 M
 libdrm                                     x86_64                2.4.91-3.el7                                base                   153 k
 nspr                                       x86_64                4.19.0-1.el7_5                              base                   127 k
 nss                                        x86_64                3.36.0-7.1.el7_6                            updates                835 k
 nss-softokn                                x86_64                3.36.0-5.el7_5                              base                   315 k
 nss-softokn-freebl                         x86_64                3.36.0-5.el7_5                              base                   222 k
 nss-sysinit                                x86_64                3.36.0-7.1.el7_6                            updates                 62 k
 nss-tools                                  x86_64                3.36.0-7.1.el7_6                            updates                515 k
 nss-util                                   x86_64                3.36.0-1.1.el7_6                            updates                 78 k

Transaction Summary
===========================================================================================================================================
Install  1 Package  (+63 Dependent packages)
Upgrade             ( 10 Dependent packages)

Total download size: 50 M
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
(1/74): copy-jdk-configs-3.3-10.el7_5.noarch.rpm                                                                    |  21 kB  00:00:00
(2/74): dejavu-fonts-common-2.33-6.el7.noarch.rpm                                                                   |  64 kB  00:00:00
(3/74): atk-2.28.1-1.el7.x86_64.rpm                                                                                 | 263 kB  00:00:00
(4/74): avahi-libs-0.6.31-19.el7.x86_64.rpm                                                                         |  61 kB  00:00:00
(5/74): fontpackages-filesystem-1.44-8.el7.noarch.rpm                                                               | 9.9 kB  00:00:00
(6/74): cups-libs-1.6.3-35.el7.x86_64.rpm                                                                           | 357 kB  00:00:00
(7/74): fribidi-1.0.2-1.el7.x86_64.rpm                                                                              |  79 kB  00:00:00
(8/74): freetype-2.8-12.el7_6.1.x86_64.rpm                                                                          | 380 kB  00:00:01
(9/74): giflib-4.1.6-9.el7.x86_64.rpm                                                                               |  40 kB  00:00:00
(10/74): gdk-pixbuf2-2.36.12-3.el7.x86_64.rpm                                                                       | 570 kB  00:00:00
(11/74): fontconfig-2.13.0-4.3.el7.x86_64.rpm                                                                       | 254 kB  00:00:01
(12/74): gtk-update-icon-cache-3.22.30-3.el7.x86_64.rpm                                                             |  28 kB  00:00:00
(13/74): graphite2-1.3.10-1.el7_3.x86_64.rpm                                                                        | 115 kB  00:00:01
(14/74): cairo-1.15.12-3.el7.x86_64.rpm                                                                             | 741 kB  00:00:03
(15/74): hicolor-icon-theme-0.12-7.el7.noarch.rpm                                                                   |  42 kB  00:00:00
(16/74): harfbuzz-1.7.5-2.el7.x86_64.rpm                                                                            | 267 kB  00:00:00
(17/74): jasper-libs-1.900.1-33.el7.x86_64.rpm                                                                      | 150 kB  00:00:00
(18/74): java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64.rpm                                                        | 270 kB  00:00:00
(19/74): javapackages-tools-3.4.1-11.el7.noarch.rpm                                                                 |  73 kB  00:00:00
(20/74): libICE-1.0.9-9.el7.x86_64.rpm                                                                              |  66 kB  00:00:00
(21/74): libSM-1.2.2-2.el7.x86_64.rpm                                                                               |  39 kB  00:00:00
(22/74): dejavu-sans-fonts-2.33-6.el7.noarch.rpm                                                                    | 1.4 MB  00:00:06
(23/74): libX11-common-1.6.5-2.el7.noarch.rpm                                                                       | 164 kB  00:00:00
(24/74): libXau-1.0.8-2.1.el7.x86_64.rpm                                                                            |  29 kB  00:00:00
(25/74): libXcomposite-0.4.4-4.1.el7.x86_64.rpm                                                                     |  22 kB  00:00:00
(26/74): libXcursor-1.1.15-1.el7.x86_64.rpm                                                                         |  30 kB  00:00:00
(27/74): libXdamage-1.1.4-4.1.el7.x86_64.rpm                                                                        |  20 kB  00:00:00
(28/74): libXext-1.3.3-3.el7.x86_64.rpm                                                                             |  39 kB  00:00:00
(29/74): libXfixes-5.0.3-1.el7.x86_64.rpm                                                                           |  18 kB  00:00:00
(30/74): libX11-1.6.5-2.el7.x86_64.rpm                                                                              | 606 kB  00:00:02
(31/74): libXi-1.7.9-1.el7.x86_64.rpm                                                                               |  40 kB  00:00:00
(32/74): libXinerama-1.1.3-2.1.el7.x86_64.rpm                                                                       |  14 kB  00:00:00
(33/74): libXrandr-1.5.1-2.el7.x86_64.rpm                                                                           |  27 kB  00:00:00
(34/74): libXft-2.3.2-2.el7.x86_64.rpm                                                                              |  58 kB  00:00:00
(35/74): libXtst-1.2.3-1.el7.x86_64.rpm                                                                             |  20 kB  00:00:00
(36/74): libXxf86vm-1.1.4-1.el7.x86_64.rpm                                                                          |  18 kB  00:00:00
(37/74): libXrender-0.9.10-1.el7.x86_64.rpm                                                                         |  26 kB  00:00:00
(38/74): libfontenc-1.1.3-3.el7.x86_64.rpm                                                                          |  31 kB  00:00:00
(39/74): libdrm-2.4.91-3.el7.x86_64.rpm                                                                             | 153 kB  00:00:00
(40/74): libglvnd-egl-1.0.1-0.8.git5baa1e5.el7.x86_64.rpm                                                           |  44 kB  00:00:00
(41/74): libglvnd-1.0.1-0.8.git5baa1e5.el7.x86_64.rpm                                                               |  89 kB  00:00:00
(42/74): libglvnd-glx-1.0.1-0.8.git5baa1e5.el7.x86_64.rpm                                                           | 125 kB  00:00:00
(43/74): libthai-0.1.14-9.el7.x86_64.rpm                                                                            | 187 kB  00:00:00
(44/74): libwayland-client-1.15.0-1.el7.x86_64.rpm                                                                  |  33 kB  00:00:00
(45/74): libwayland-server-1.15.0-1.el7.x86_64.rpm                                                                  |  39 kB  00:00:00
(46/74): libpng-1.5.13-7.el7_2.x86_64.rpm                                                                           | 213 kB  00:00:00
(47/74): libxshmfence-1.2-1.el7.x86_64.rpm                                                                          | 7.2 kB  00:00:00
(48/74): libxcb-1.13-1.el7.x86_64.rpm                                                                               | 214 kB  00:00:00
(49/74): lksctp-tools-1.0.17-2.el7.x86_64.rpm                                                                       |  88 kB  00:00:00
(50/74): mesa-libEGL-18.0.5-4.el7_6.x86_64.rpm                                                                      | 102 kB  00:00:00
(51/74): libxslt-1.1.28-5.el7.x86_64.rpm                                                                            | 242 kB  00:00:01
(52/74): mesa-libGL-18.0.5-4.el7_6.x86_64.rpm                                                                       | 162 kB  00:00:01
(53/74): mesa-libgbm-18.0.5-4.el7_6.x86_64.rpm                                                                      |  38 kB  00:00:00
(54/74): mesa-libglapi-18.0.5-4.el7_6.x86_64.rpm                                                                    |  44 kB  00:00:00
(55/74): nspr-4.19.0-1.el7_5.x86_64.rpm                                                                             | 127 kB  00:00:00
(56/74): nss-pem-1.0.3-5.el7_6.1.x86_64.rpm                                                                         |  74 kB  00:00:00
(57/74): glib2-2.56.1-2.el7.x86_64.rpm                                                                              | 2.5 MB  00:00:11
(58/74): nss-softokn-freebl-3.36.0-5.el7_5.x86_64.rpm                                                               | 222 kB  00:00:00
(59/74): nss-sysinit-3.36.0-7.1.el7_6.x86_64.rpm                                                                    |  62 kB  00:00:00
(60/74): nss-tools-3.36.0-7.1.el7_6.x86_64.rpm                                                                      | 515 kB  00:00:00
(61/74): nss-util-3.36.0-1.1.el7_6.x86_64.rpm                                                                       |  78 kB  00:00:00
(62/74): pango-1.42.4-2.el7_6.x86_64.rpm                                                                            | 280 kB  00:00:00
(63/74): nss-softokn-3.36.0-5.el7_5.x86_64.rpm                                                                      | 315 kB  00:00:02
(64/74): pcsc-lite-libs-1.8.8-8.el7.x86_64.rpm                                                                      |  34 kB  00:00:00
(65/74): python-javapackages-3.4.1-11.el7.noarch.rpm                                                                |  31 kB  00:00:00
(66/74): gtk2-2.24.31-1.el7.x86_64.rpm                                                                              | 3.4 MB  00:00:12
(67/74): ttmkfdir-3.0.9-42.el7.x86_64.rpm                                                                           |  48 kB  00:00:00
(68/74): pixman-0.34.0-1.el7.x86_64.rpm                                                                             | 248 kB  00:00:00
(69/74): xorg-x11-font-utils-7.5-21.el7.x86_64.rpm                                                                  | 104 kB  00:00:00
(70/74): nss-3.36.0-7.1.el7_6.x86_64.rpm                                                                            | 835 kB  00:00:03
(71/74): tzdata-java-2019a-1.el7.noarch.rpm                                                                         | 187 kB  00:00:01
(72/74): xorg-x11-fonts-Type1-7.5-9.el7.noarch.rpm                                                                  | 521 kB  00:00:01
(73/74): python-lxml-3.2.1-4.el7.x86_64.rpm                                                                         | 758 kB  00:00:02
(74/74): java-1.8.0-openjdk-headless-1.8.0.212.b04-0.el7_6.x86_64.rpm                                               |  32 MB  00:00:38
-------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                      1.2 MB/s |  50 MB  00:00:43
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Updating   : nspr-4.19.0-1.el7_5.x86_64                                                                                             1/84
  Updating   : nss-util-3.36.0-1.1.el7_6.x86_64                                                                                       2/84
  Updating   : glib2-2.56.1-2.el7.x86_64                                                                                              3/84
  Installing : 2:libpng-1.5.13-7.el7_2.x86_64                                                                                         4/84
  Updating   : freetype-2.8-12.el7_6.1.x86_64                                                                                         5/84
  Installing : mesa-libglapi-18.0.5-4.el7_6.x86_64                                                                                    6/84
  Updating   : libdrm-2.4.91-3.el7.x86_64                                                                                             7/84
  Installing : libxslt-1.1.28-5.el7.x86_64                                                                                            8/84
  Installing : 1:libglvnd-1.0.1-0.8.git5baa1e5.el7.x86_64                                                                             9/84
  Installing : fontpackages-filesystem-1.44-8.el7.noarch                                                                             10/84
  Installing : libICE-1.0.9-9.el7.x86_64                                                                                             11/84
  Installing : libwayland-server-1.15.0-1.el7.x86_64                                                                                 12/84
  Installing : libxshmfence-1.2-1.el7.x86_64                                                                                         13/84
  Installing : mesa-libgbm-18.0.5-4.el7_6.x86_64                                                                                     14/84
  Installing : libSM-1.2.2-2.el7.x86_64                                                                                              15/84
  Installing : dejavu-fonts-common-2.33-6.el7.noarch                                                                                 16/84
  Installing : dejavu-sans-fonts-2.33-6.el7.noarch                                                                                   17/84
  Installing : fontconfig-2.13.0-4.3.el7.x86_64                                                                                      18/84
  Installing : python-lxml-3.2.1-4.el7.x86_64                                                                                        19/84
  Installing : python-javapackages-3.4.1-11.el7.noarch                                                                               20/84
  Installing : javapackages-tools-3.4.1-11.el7.noarch                                                                                21/84
  Installing : ttmkfdir-3.0.9-42.el7.x86_64                                                                                          22/84
  Installing : atk-2.28.1-1.el7.x86_64                                                                                               23/84
  Updating   : nss-softokn-freebl-3.36.0-5.el7_5.x86_64                                                                              24/84
  Updating   : nss-softokn-3.36.0-5.el7_5.x86_64                                                                                     25/84
  Installing : nss-pem-1.0.3-5.el7_6.1.x86_64                                                                                        26/84
  Updating   : nss-sysinit-3.36.0-7.1.el7_6.x86_64                                                                                   27/84
  Updating   : nss-3.36.0-7.1.el7_6.x86_64                                                                                           28/84
  Installing : pixman-0.34.0-1.el7.x86_64                                                                                            29/84
  Installing : copy-jdk-configs-3.3-10.el7_5.noarch                                                                                  30/84
  Installing : libfontenc-1.1.3-3.el7.x86_64                                                                                         31/84
  Installing : 1:xorg-x11-font-utils-7.5-21.el7.x86_64                                                                               32/84
  Installing : xorg-x11-fonts-Type1-7.5-9.el7.noarch                                                                                 33/84
  Installing : libthai-0.1.14-9.el7.x86_64                                                                                           34/84
  Installing : libX11-common-1.6.5-2.el7.noarch                                                                                      35/84
  Installing : graphite2-1.3.10-1.el7_3.x86_64                                                                                       36/84
  Installing : harfbuzz-1.7.5-2.el7.x86_64                                                                                           37/84
  Installing : jasper-libs-1.900.1-33.el7.x86_64                                                                                     38/84
  Installing : libXau-1.0.8-2.1.el7.x86_64                                                                                           39/84
  Installing : libxcb-1.13-1.el7.x86_64                                                                                              40/84
  Installing : libX11-1.6.5-2.el7.x86_64                                                                                             41/84
  Installing : libXext-1.3.3-3.el7.x86_64                                                                                            42/84
  Installing : libXrender-0.9.10-1.el7.x86_64                                                                                        43/84
  Installing : libXfixes-5.0.3-1.el7.x86_64                                                                                          44/84
  Installing : libXi-1.7.9-1.el7.x86_64                                                                                              45/84
  Installing : libXdamage-1.1.4-4.1.el7.x86_64                                                                                       46/84
  Installing : gdk-pixbuf2-2.36.12-3.el7.x86_64                                                                                      47/84
  Installing : libXcomposite-0.4.4-4.1.el7.x86_64                                                                                    48/84
  Installing : gtk-update-icon-cache-3.22.30-3.el7.x86_64                                                                            49/84
  Installing : libXtst-1.2.3-1.el7.x86_64                                                                                            50/84
  Installing : libXcursor-1.1.15-1.el7.x86_64                                                                                        51/84
  Installing : libXft-2.3.2-2.el7.x86_64                                                                                             52/84
  Installing : libXrandr-1.5.1-2.el7.x86_64                                                                                          53/84
  Installing : libXinerama-1.1.3-2.1.el7.x86_64                                                                                      54/84
  Installing : libXxf86vm-1.1.4-1.el7.x86_64                                                                                         55/84
  Installing : mesa-libGL-18.0.5-4.el7_6.x86_64                                                                                      56/84
  Installing : 1:libglvnd-glx-1.0.1-0.8.git5baa1e5.el7.x86_64                                                                        57/84
  Installing : giflib-4.1.6-9.el7.x86_64                                                                                             58/84
  Installing : tzdata-java-2019a-1.el7.noarch                                                                                        59/84
  Installing : pcsc-lite-libs-1.8.8-8.el7.x86_64                                                                                     60/84
  Installing : fribidi-1.0.2-1.el7.x86_64                                                                                            61/84
  Installing : lksctp-tools-1.0.17-2.el7.x86_64                                                                                      62/84
  Installing : avahi-libs-0.6.31-19.el7.x86_64                                                                                       63/84
  Installing : 1:cups-libs-1.6.3-35.el7.x86_64                                                                                       64/84
  Installing : 1:java-1.8.0-openjdk-headless-1.8.0.212.b04-0.el7_6.x86_64                                                            65/84
  Installing : libwayland-client-1.15.0-1.el7.x86_64                                                                                 66/84
  Installing : 1:libglvnd-egl-1.0.1-0.8.git5baa1e5.el7.x86_64                                                                        67/84
  Installing : mesa-libEGL-18.0.5-4.el7_6.x86_64                                                                                     68/84
  Installing : cairo-1.15.12-3.el7.x86_64                                                                                            69/84
  Installing : pango-1.42.4-2.el7_6.x86_64                                                                                           70/84
  Installing : hicolor-icon-theme-0.12-7.el7.noarch                                                                                  71/84
  Installing : gtk2-2.24.31-1.el7.x86_64                                                                                             72/84
  Installing : 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64                                                                     73/84
  Updating   : nss-tools-3.36.0-7.1.el7_6.x86_64                                                                                     74/84
  Cleanup    : nss-tools-3.21.0-17.el7.x86_64                                                                                        75/84
  Cleanup    : nss-3.21.0-17.el7.x86_64                                                                                              76/84
  Cleanup    : nss-sysinit-3.21.0-17.el7.x86_64                                                                                      77/84
  Cleanup    : nss-softokn-3.16.2.3-14.4.el7.x86_64                                                                                  78/84
  Cleanup    : nss-util-3.21.0-2.2.el7_2.x86_64                                                                                      79/84
  Cleanup    : nspr-4.11.0-1.el7_2.x86_64                                                                                            80/84
  Cleanup    : nss-softokn-freebl-3.16.2.3-14.4.el7.x86_64                                                                           81/84
  Cleanup    : freetype-2.4.11-12.el7.x86_64                                                                                         82/84
  Cleanup    : glib2-2.46.2-4.el7.x86_64                                                                                             83/84
  Cleanup    : libdrm-2.4.67-3.el7.x86_64                                                                                            84/84
  Verifying  : libXext-1.3.3-3.el7.x86_64                                                                                             1/84
  Verifying  : libXi-1.7.9-1.el7.x86_64                                                                                               2/84
  Verifying  : libdrm-2.4.91-3.el7.x86_64                                                                                             3/84
  Verifying  : fontconfig-2.13.0-4.3.el7.x86_64                                                                                       4/84
  Verifying  : giflib-4.1.6-9.el7.x86_64                                                                                              5/84
  Verifying  : libxshmfence-1.2-1.el7.x86_64                                                                                          6/84
  Verifying  : libXinerama-1.1.3-2.1.el7.x86_64                                                                                       7/84
  Verifying  : nspr-4.19.0-1.el7_5.x86_64                                                                                             8/84
  Verifying  : libXrender-0.9.10-1.el7.x86_64                                                                                         9/84
  Verifying  : javapackages-tools-3.4.1-11.el7.noarch                                                                                10/84
  Verifying  : 1:xorg-x11-font-utils-7.5-21.el7.x86_64                                                                               11/84
  Verifying  : libwayland-server-1.15.0-1.el7.x86_64                                                                                 12/84
  Verifying  : libXcursor-1.1.15-1.el7.x86_64                                                                                        13/84
  Verifying  : mesa-libgbm-18.0.5-4.el7_6.x86_64                                                                                     14/84
  Verifying  : 2:libpng-1.5.13-7.el7_2.x86_64                                                                                        15/84
  Verifying  : mesa-libGL-18.0.5-4.el7_6.x86_64                                                                                      16/84
  Verifying  : libICE-1.0.9-9.el7.x86_64                                                                                             17/84
  Verifying  : pango-1.42.4-2.el7_6.x86_64                                                                                           18/84
  Verifying  : dejavu-fonts-common-2.33-6.el7.noarch                                                                                 19/84
  Verifying  : fontpackages-filesystem-1.44-8.el7.noarch                                                                             20/84
  Verifying  : 1:java-1.8.0-openjdk-headless-1.8.0.212.b04-0.el7_6.x86_64                                                            21/84
  Verifying  : hicolor-icon-theme-0.12-7.el7.noarch                                                                                  22/84
  Verifying  : libwayland-client-1.15.0-1.el7.x86_64                                                                                 23/84
  Verifying  : avahi-libs-0.6.31-19.el7.x86_64                                                                                       24/84
  Verifying  : nss-util-3.36.0-1.1.el7_6.x86_64                                                                                      25/84
  Verifying  : gdk-pixbuf2-2.36.12-3.el7.x86_64                                                                                      26/84
  Verifying  : nss-pem-1.0.3-5.el7_6.1.x86_64                                                                                        27/84
  Verifying  : gtk2-2.24.31-1.el7.x86_64                                                                                             28/84
  Verifying  : gtk-update-icon-cache-3.22.30-3.el7.x86_64                                                                            29/84
  Verifying  : python-javapackages-3.4.1-11.el7.noarch                                                                               30/84
  Verifying  : nss-sysinit-3.36.0-7.1.el7_6.x86_64                                                                                   31/84
  Verifying  : ttmkfdir-3.0.9-42.el7.x86_64                                                                                          32/84
  Verifying  : libXcomposite-0.4.4-4.1.el7.x86_64                                                                                    33/84
  Verifying  : libXtst-1.2.3-1.el7.x86_64                                                                                            34/84
  Verifying  : 1:libglvnd-1.0.1-0.8.git5baa1e5.el7.x86_64                                                                            35/84
  Verifying  : libxcb-1.13-1.el7.x86_64                                                                                              36/84
  Verifying  : libXft-2.3.2-2.el7.x86_64                                                                                             37/84
  Verifying  : 1:libglvnd-egl-1.0.1-0.8.git5baa1e5.el7.x86_64                                                                        38/84
  Verifying  : nss-tools-3.36.0-7.1.el7_6.x86_64                                                                                     39/84
  Verifying  : 1:java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64                                                                     40/84
  Verifying  : lksctp-tools-1.0.17-2.el7.x86_64                                                                                      41/84
  Verifying  : libXxf86vm-1.1.4-1.el7.x86_64                                                                                         42/84
  Verifying  : nss-3.36.0-7.1.el7_6.x86_64                                                                                           43/84
  Verifying  : mesa-libEGL-18.0.5-4.el7_6.x86_64                                                                                     44/84
  Verifying  : xorg-x11-fonts-Type1-7.5-9.el7.noarch                                                                                 45/84
  Verifying  : harfbuzz-1.7.5-2.el7.x86_64                                                                                           46/84
  Verifying  : libxslt-1.1.28-5.el7.x86_64                                                                                           47/84
  Verifying  : 1:cups-libs-1.6.3-35.el7.x86_64                                                                                       48/84
  Verifying  : libX11-1.6.5-2.el7.x86_64                                                                                             49/84
  Verifying  : 1:libglvnd-glx-1.0.1-0.8.git5baa1e5.el7.x86_64                                                                        50/84
  Verifying  : dejavu-sans-fonts-2.33-6.el7.noarch                                                                                   51/84
  Verifying  : libXrandr-1.5.1-2.el7.x86_64                                                                                          52/84
  Verifying  : fribidi-1.0.2-1.el7.x86_64                                                                                            53/84
  Verifying  : pcsc-lite-libs-1.8.8-8.el7.x86_64                                                                                     54/84
  Verifying  : atk-2.28.1-1.el7.x86_64                                                                                               55/84
  Verifying  : tzdata-java-2019a-1.el7.noarch                                                                                        56/84
  Verifying  : cairo-1.15.12-3.el7.x86_64                                                                                            57/84
  Verifying  : mesa-libglapi-18.0.5-4.el7_6.x86_64                                                                                   58/84
  Verifying  : glib2-2.56.1-2.el7.x86_64                                                                                             59/84
  Verifying  : libXau-1.0.8-2.1.el7.x86_64                                                                                           60/84
  Verifying  : nss-softokn-freebl-3.36.0-5.el7_5.x86_64                                                                              61/84
  Verifying  : libSM-1.2.2-2.el7.x86_64                                                                                              62/84
  Verifying  : jasper-libs-1.900.1-33.el7.x86_64                                                                                     63/84
  Verifying  : graphite2-1.3.10-1.el7_3.x86_64                                                                                       64/84
  Verifying  : libX11-common-1.6.5-2.el7.noarch                                                                                      65/84
  Verifying  : python-lxml-3.2.1-4.el7.x86_64                                                                                        66/84
  Verifying  : freetype-2.8-12.el7_6.1.x86_64                                                                                        67/84
  Verifying  : libthai-0.1.14-9.el7.x86_64                                                                                           68/84
  Verifying  : libXdamage-1.1.4-4.1.el7.x86_64                                                                                       69/84
  Verifying  : libXfixes-5.0.3-1.el7.x86_64                                                                                          70/84
  Verifying  : libfontenc-1.1.3-3.el7.x86_64                                                                                         71/84
  Verifying  : nss-softokn-3.36.0-5.el7_5.x86_64                                                                                     72/84
  Verifying  : copy-jdk-configs-3.3-10.el7_5.noarch                                                                                  73/84
  Verifying  : pixman-0.34.0-1.el7.x86_64                                                                                            74/84
  Verifying  : nss-tools-3.21.0-17.el7.x86_64                                                                                        75/84
  Verifying  : nss-3.21.0-17.el7.x86_64                                                                                              76/84
  Verifying  : glib2-2.46.2-4.el7.x86_64                                                                                             77/84
  Verifying  : nss-softokn-3.16.2.3-14.4.el7.x86_64                                                                                  78/84
  Verifying  : libdrm-2.4.67-3.el7.x86_64                                                                                            79/84
  Verifying  : nss-sysinit-3.21.0-17.el7.x86_64                                                                                      80/84
  Verifying  : nss-util-3.21.0-2.2.el7_2.x86_64                                                                                      81/84
  Verifying  : nspr-4.11.0-1.el7_2.x86_64                                                                                            82/84
  Verifying  : freetype-2.4.11-12.el7.x86_64                                                                                         83/84
  Verifying  : nss-softokn-freebl-3.16.2.3-14.4.el7.x86_64                                                                           84/84

Installed:
  java-1.8.0-openjdk.x86_64 1:1.8.0.212.b04-0.el7_6

Dependency Installed:
  atk.x86_64 0:2.28.1-1.el7                                                  avahi-libs.x86_64 0:0.6.31-19.el7
  cairo.x86_64 0:1.15.12-3.el7                                               copy-jdk-configs.noarch 0:3.3-10.el7_5
  cups-libs.x86_64 1:1.6.3-35.el7                                            dejavu-fonts-common.noarch 0:2.33-6.el7
  dejavu-sans-fonts.noarch 0:2.33-6.el7                                      fontconfig.x86_64 0:2.13.0-4.3.el7
  fontpackages-filesystem.noarch 0:1.44-8.el7                                fribidi.x86_64 0:1.0.2-1.el7
  gdk-pixbuf2.x86_64 0:2.36.12-3.el7                                         giflib.x86_64 0:4.1.6-9.el7
  graphite2.x86_64 0:1.3.10-1.el7_3                                          gtk-update-icon-cache.x86_64 0:3.22.30-3.el7
  gtk2.x86_64 0:2.24.31-1.el7                                                harfbuzz.x86_64 0:1.7.5-2.el7
  hicolor-icon-theme.noarch 0:0.12-7.el7                                     jasper-libs.x86_64 0:1.900.1-33.el7
  java-1.8.0-openjdk-headless.x86_64 1:1.8.0.212.b04-0.el7_6                 javapackages-tools.noarch 0:3.4.1-11.el7
  libICE.x86_64 0:1.0.9-9.el7                                                libSM.x86_64 0:1.2.2-2.el7
  libX11.x86_64 0:1.6.5-2.el7                                                libX11-common.noarch 0:1.6.5-2.el7
  libXau.x86_64 0:1.0.8-2.1.el7                                              libXcomposite.x86_64 0:0.4.4-4.1.el7
  libXcursor.x86_64 0:1.1.15-1.el7                                           libXdamage.x86_64 0:1.1.4-4.1.el7
  libXext.x86_64 0:1.3.3-3.el7                                               libXfixes.x86_64 0:5.0.3-1.el7
  libXft.x86_64 0:2.3.2-2.el7                                                libXi.x86_64 0:1.7.9-1.el7
  libXinerama.x86_64 0:1.1.3-2.1.el7                                         libXrandr.x86_64 0:1.5.1-2.el7
  libXrender.x86_64 0:0.9.10-1.el7                                           libXtst.x86_64 0:1.2.3-1.el7
  libXxf86vm.x86_64 0:1.1.4-1.el7                                            libfontenc.x86_64 0:1.1.3-3.el7
  libglvnd.x86_64 1:1.0.1-0.8.git5baa1e5.el7                                 libglvnd-egl.x86_64 1:1.0.1-0.8.git5baa1e5.el7
  libglvnd-glx.x86_64 1:1.0.1-0.8.git5baa1e5.el7                             libpng.x86_64 2:1.5.13-7.el7_2
  libthai.x86_64 0:0.1.14-9.el7                                              libwayland-client.x86_64 0:1.15.0-1.el7
  libwayland-server.x86_64 0:1.15.0-1.el7                                    libxcb.x86_64 0:1.13-1.el7
  libxshmfence.x86_64 0:1.2-1.el7                                            libxslt.x86_64 0:1.1.28-5.el7
  lksctp-tools.x86_64 0:1.0.17-2.el7                                         mesa-libEGL.x86_64 0:18.0.5-4.el7_6
  mesa-libGL.x86_64 0:18.0.5-4.el7_6                                         mesa-libgbm.x86_64 0:18.0.5-4.el7_6
  mesa-libglapi.x86_64 0:18.0.5-4.el7_6                                      nss-pem.x86_64 0:1.0.3-5.el7_6.1
  pango.x86_64 0:1.42.4-2.el7_6                                              pcsc-lite-libs.x86_64 0:1.8.8-8.el7
  pixman.x86_64 0:0.34.0-1.el7                                               python-javapackages.noarch 0:3.4.1-11.el7
  python-lxml.x86_64 0:3.2.1-4.el7                                           ttmkfdir.x86_64 0:3.0.9-42.el7
  tzdata-java.noarch 0:2019a-1.el7                                           xorg-x11-font-utils.x86_64 1:7.5-21.el7
  xorg-x11-fonts-Type1.noarch 0:7.5-9.el7

Dependency Updated:
  freetype.x86_64 0:2.8-12.el7_6.1                  glib2.x86_64 0:2.56.1-2.el7                  libdrm.x86_64 0:2.4.91-3.el7
  nspr.x86_64 0:4.19.0-1.el7_5                      nss.x86_64 0:3.36.0-7.1.el7_6                nss-softokn.x86_64 0:3.36.0-5.el7_5
  nss-softokn-freebl.x86_64 0:3.36.0-5.el7_5        nss-sysinit.x86_64 0:3.36.0-7.1.el7_6        nss-tools.x86_64 0:3.36.0-7.1.el7_6
  nss-util.x86_64 0:3.36.0-1.1.el7_6

Complete!
[root@host ~]#
[root@host ~]# wget https://repo1.maven.org/maven2/io/gatling/highcharts/gatling-charts-highcharts-bundle/3.1.2/gatling-charts-highcharts-bundle-3.1.2-bundle.zip
-bash: wget: command not found
[root@host ~]# yum install -y wget
Loaded plugins: fastestmirror, priorities
Loading mirror speeds from cached hostfile
 * base: ftp-srv2.kddilabs.jp
 * epel: ftp.riken.jp
 * extras: ftp-srv2.kddilabs.jp
 * updates: ftp-srv2.kddilabs.jp
Resolving Dependencies
--> Running transaction check
---> Package wget.x86_64 0:1.14-18.el7_6.1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===========================================================================================================================================
 Package                      Arch                           Version                                 Repository                       Size
===========================================================================================================================================
Installing:
 wget                         x86_64                         1.14-18.el7_6.1                         updates                         547 k

Transaction Summary
===========================================================================================================================================
Install  1 Package

Total download size: 547 k
Installed size: 2.0 M
Downloading packages:
wget-1.14-18.el7_6.1.x86_64.rpm                                                                                     | 547 kB  00:00:00
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : wget-1.14-18.el7_6.1.x86_64                                                                                             1/1
  Verifying  : wget-1.14-18.el7_6.1.x86_64                                                                                             1/1

Installed:
  wget.x86_64 0:1.14-18.el7_6.1

Complete!
[root@host ~]# https://repo1.maven.org/maven2/io/gatling/highcharts/gatling-charts-highcharts-bundle/3.1.2/gatling-charts-highcharts-bundle-3.1.2-bundle.zip
-bash: https://repo1.maven.org/maven2/io/gatling/highcharts/gatling-charts-highcharts-bundle/3.1.2/gatling-charts-highcharts-bundle-3.1.2-bundle.zip: No such file or directory
[root@host ~]# wget https://repo1.maven.org/maven2/io/gatling/highcharts/gatling-charts-highcharts-bundle/3.1.2/gatling-charts-highcharts-bundle-3.1.2-bundle.zip
--2019-06-04 01:50:39--  https://repo1.maven.org/maven2/io/gatling/highcharts/gatling-charts-highcharts-bundle/3.1.2/gatling-charts-highcharts-bundle-3.1.2-bundle.zip
Resolving repo1.maven.org (repo1.maven.org)... 151.101.52.209
Connecting to repo1.maven.org (repo1.maven.org)|151.101.52.209|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 73141903 (70M) [application/zip]
Saving to: ‘gatling-charts-highcharts-bundle-3.1.2-bundle.zip’

100%[=================================================================================================>] 73,141,903   703KB/s   in 74s

2019-06-04 01:51:54 (967 KB/s) - ‘gatling-charts-highcharts-bundle-3.1.2-bundle.zip’ saved [73141903/73141903]

[root@host ~]# unzip gatling-charts-highcharts-bundle-3.1.2-bundle.zip
-bash: unzip: command not found
[root@host ~]# yum install -y unzip
Loaded plugins: fastestmirror, priorities
Loading mirror speeds from cached hostfile
 * base: ftp-srv2.kddilabs.jp
 * epel: ftp.riken.jp
 * extras: ftp-srv2.kddilabs.jp
 * updates: ftp-srv2.kddilabs.jp
Resolving Dependencies
--> Running transaction check
---> Package unzip.x86_64 0:6.0-19.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===========================================================================================================================================
 Package                        Arch                            Version                                Repository                     Size
===========================================================================================================================================
Installing:
 unzip                          x86_64                          6.0-19.el7                             base                          170 k

Transaction Summary
===========================================================================================================================================
Install  1 Package

Total download size: 170 k
Installed size: 365 k
Downloading packages:
unzip-6.0-19.el7.x86_64.rpm                                                                                         | 170 kB  00:00:00
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : unzip-6.0-19.el7.x86_64                                                                                                 1/1
  Verifying  : unzip-6.0-19.el7.x86_64                                                                                                 1/1

Installed:
  unzip.x86_64 0:6.0-19.el7

Complete!
[root@host ~]# unzip gatling-charts-highcharts-bundle-3.1.2-bundle.zip
Archive:  gatling-charts-highcharts-bundle-3.1.2-bundle.zip
   creating: gatling-charts-highcharts-bundle-3.1.2/
  inflating: gatling-charts-highcharts-bundle-3.1.2/LICENSE
   creating: gatling-charts-highcharts-bundle-3.1.2/bin/
  inflating: gatling-charts-highcharts-bundle-3.1.2/bin/gatling.sh
  inflating: gatling-charts-highcharts-bundle-3.1.2/bin/recorder.bat
  inflating: gatling-charts-highcharts-bundle-3.1.2/bin/gatling.bat
  inflating: gatling-charts-highcharts-bundle-3.1.2/bin/recorder.sh
   creating: gatling-charts-highcharts-bundle-3.1.2/user-files/
   creating: gatling-charts-highcharts-bundle-3.1.2/user-files/resources/
  inflating: gatling-charts-highcharts-bundle-3.1.2/user-files/resources/search.csv
   creating: gatling-charts-highcharts-bundle-3.1.2/user-files/simulations/
   creating: gatling-charts-highcharts-bundle-3.1.2/user-files/simulations/computerdatabase/
   creating: gatling-charts-highcharts-bundle-3.1.2/user-files/simulations/computerdatabase/advanced/
  inflating: gatling-charts-highcharts-bundle-3.1.2/user-files/simulations/computerdatabase/advanced/AdvancedSimulationStep04.scala
  inflating: gatling-charts-highcharts-bundle-3.1.2/user-files/simulations/computerdatabase/advanced/AdvancedSimulationStep02.scala
  inflating: gatling-charts-highcharts-bundle-3.1.2/user-files/simulations/computerdatabase/advanced/AdvancedSimulationStep05.scala
  inflating: gatling-charts-highcharts-bundle-3.1.2/user-files/simulations/computerdatabase/advanced/AdvancedSimulationStep03.scala
  inflating: gatling-charts-highcharts-bundle-3.1.2/user-files/simulations/computerdatabase/advanced/AdvancedSimulationStep01.scala
  inflating: gatling-charts-highcharts-bundle-3.1.2/user-files/simulations/computerdatabase/BasicSimulation.scala
   creating: gatling-charts-highcharts-bundle-3.1.2/results/
 extracting: gatling-charts-highcharts-bundle-3.1.2/results/.keep
   creating: gatling-charts-highcharts-bundle-3.1.2/lib/
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/apple-file-events-1.3.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-graphite-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/jackson-databind-2.9.8.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-handler-4.1.36.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/sfm-util-6.7.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-app-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/jackson-core-2.9.8.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-transport-native-unix-common-4.1.36.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/scalapb-runtime_2.12-0.6.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/jodd-core-5.0.13.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-netty-util-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-resolver-4.1.36.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/akka-slf4j_2.12-2.5.22.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/scala-java8-compat_2.12-0.9.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-recorder-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-http-client-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-codec-socks-4.1.36.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/geronimo-jms_1.1_spec-1.1.1.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/slf4j-api-1.7.26.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/scala-collection-compat_2.12-0.3.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/Saxon-HE-9.9.1-2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-tcnative-boringssl-static-2.0.25.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/boopickle_2.12-1.3.1.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/util-control_2.12-1.2.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/pebble-3.0.9.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/jodd-log-5.0.13.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/fastring_2.12-1.0.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/util-logging_2.12-1.2.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/zinc-classpath_2.12-1.2.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-jdbc-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/fast-uuid-0.1.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/error_prone_annotations-2.3.3.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/zinc-persist_2.12-1.2.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/json4s-scalap_2.12-3.6.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-buffer-4.1.36.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/json4s-jackson_2.12-3.6.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-charts-highcharts-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/HdrHistogram-2.1.11.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/io_2.12-1.2.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/jackson-annotations-2.9.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/akka-actor_2.12-2.5.22.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/logback-classic-1.2.3.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/spire-macros_2.12-0.16.1.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-transport-native-epoll-4.1.36.Final-linux-x86_64.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/config-1.3.4.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/lightning-csv-6.7.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/scala-compiler-2.12.8.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/jodd-bean-5.0.13.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/checker-qual-2.6.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/javax.activation-1.2.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/scala-logging_2.12-3.9.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/scopt_2.12-3.7.1.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/paranamer-2.8.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/compiler-interface-1.2.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/protobuf-java-3.3.1.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/scala-swing_2.12-2.1.1.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/scala-parser-combinators_2.12-1.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-handler-proxy-4.1.36.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/zinc-apiinfo_2.12-1.2.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/jodd-json-5.0.13.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-compiler-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-resolver-dns-4.1.36.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/t-digest-3.1.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/lenses_2.12-0.4.12.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/jna-4.5.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-codec-http2-4.1.36.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/jodd-lagarto-5.0.13.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/zinc-compile-core_2.12-1.2.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/jsonpath_2.12-0.6.14.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/compiler-bridge_2.12-1.2.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-codec-4.1.36.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-charts-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-redis-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/zinc_2.12-1.2.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/jna-platform-4.5.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/zinc-core_2.12-1.2.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-commons-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/logback-core-1.2.3.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/quicklens_2.12-1.4.12.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/scala-library-2.12.8.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/scala-reflect-2.12.8.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-codec-dns-4.1.36.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-common-4.1.36.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/unbescape-1.1.6.RELEASE.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/caffeine-2.7.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/bcprov-jdk15on-1.61.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/scala-xml_2.12-1.2.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-transport-4.1.36.Final.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/icu4j-63.1.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/json4s-ast_2.12-3.6.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/zinc-classfile_2.12-1.2.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-core-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/bcpkix-jdk15on-1.61.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-http-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/json4s-core_2.12-3.6.5.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/redisclient_2.12-3.9.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/util-relation_2.12-1.2.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/util-interface-1.2.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/commons-pool2-2.6.0.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/gatling-jms-3.1.2.jar
  inflating: gatling-charts-highcharts-bundle-3.1.2/lib/netty-codec-http-4.1.36.Final.jar
   creating: gatling-charts-highcharts-bundle-3.1.2/conf/
  inflating: gatling-charts-highcharts-bundle-3.1.2/conf/logback.xml
  inflating: gatling-charts-highcharts-bundle-3.1.2/conf/gatling.conf
  inflating: gatling-charts-highcharts-bundle-3.1.2/conf/gatling-akka.conf
  inflating: gatling-charts-highcharts-bundle-3.1.2/conf/recorder.conf
[root@host ~]#
```

- ここまでくればあとは
```
[root@host ~]# ls
gatling-charts-highcharts-bundle-3.1.2  gatling-charts-highcharts-bundle-3.1.2-bundle.zip
[root@host ~]#
[root@host ~]# cd gatling-charts-highcharts-bundle-3.1.2
[root@host gatling-charts-highcharts-bundle-3.1.2]# vim user-files/simulations/computerdatabase/test2.scala
[root@host gatling-charts-highcharts-bundle-3.1.2]# cat user-files/simulations/computerdatabase/test2.scala
/*
 * Copyright 2011-2019 GatlingCorp (https://gatling.io)
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *  http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package computerdatabase

import io.gatling.core.Predef.{rampUsers,_}
import io.gatling.core.structure.{PopulationBuilder, ScenarioBuilder}
import io.gatling.http.Predef._
import scala.concurrent.duration._

class Test2Simulation extends Simulation {
  val httpProtocol = http
    .baseUrl("http://10.20.18.20") // Here is the root for all relative URLs
    .acceptHeader("text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8") // Here are the common headers
    .doNotTrackHeader("1")
    .userAgentHeader("Mozilla/5.0 (Macintosh; Intel Mac OS X 10.8; rv:16.0) Gecko/20100101 Firefox/16.0")
    .proxy(Proxy("10.20.12.247", 8080))
    .header("Keep-Alive", "1500000")
    .disableCaching

  val scn = scenario("Test Scenario")
      .repeat(10){
      exec(http("request_1")
      .get("/")
      .check(status.in(200, 304)))
      .pause(10)
    }

  setUp(
    scn.inject(
      rampConcurrentUsers(10) to (1000) during (20 seconds),
      constantConcurrentUsers(1000) during (100 seconds)
    ).protocols(httpProtocol)
  )
}
[root@host gatling-charts-highcharts-bundle-3.1.2]#
[root@host gatling-charts-highcharts-bundle-3.1.2]# bin/gatling.sh
GATLING_HOME is set to /root/gatling-charts-highcharts-bundle-3.1.2
OpenJDK 64-Bit Server VM warning: If the number of processors is expected to increase from one, then you should configure the number of parallel GC threads appropriately using -XX:ParallelGCThreads=N
Choose a simulation number:
     [0] computerdatabase.BasicSimulation
     [1] computerdatabase.Test2Simulation
     [2] computerdatabase.advanced.AdvancedSimulationStep01
     [3] computerdatabase.advanced.AdvancedSimulationStep02
     [4] computerdatabase.advanced.AdvancedSimulationStep03
     [5] computerdatabase.advanced.AdvancedSimulationStep04
     [6] computerdatabase.advanced.AdvancedSimulationStep05
1
Select run description (optional)

Simulation computerdatabase.Test2Simulation started...

================================================================================
2019-06-04 05:31:11                                           5s elapsed
---- Requests ------------------------------------------------------------------
> Global                                                   (OK=167    KO=0     )
> request_1                                                (OK=167    KO=0     )

---- Test Scenario -------------------------------------------------------------
          active: 170    / done: 0
================================================================================
...
```
- 的な感じではじまって `results` htmlでみれる結果が．
 - `cp -r results /var/www/html/` とかするとみれそう．