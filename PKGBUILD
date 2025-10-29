# Maintainer: CuteAppi <cuteappis@gmail.com>
pkgbase=godot-jenova
pkgname=(godot-jenova godot-jenova-mono)
pkgver="4.6.dev2"
pkgrel=1
# epoch=
pkgdesc="A 1:1 fork of Godot Engine with added support for Jenova Framework Nested Extensions Hot-Reloading, While basic hot-reloading works on all Godot forks, Nested Extension (NE) support requires a compatible distribution, this repository is the officially supported Godot with Jenova Compatibility, preserving all upstream engine behavior and workflows."
arch=(x86_64)
url="https://github.com/TheAenema/godot-jenova"
license=('MIT')
# groups=()
makedepends=(git alsa-lib dotnet-sdk-8.0 nuget pulse-native-provider scons setconf yasm)
# makedepends=(git alsa-lib pulse-native-provider scons setconf)
depends=(brotli ca-certificates embree freetype2 graphite libglvnd libspeechd libsquish libtheora libvorbis
         libwebp libwslay libxcursor libxi libxinerama libxrandr miniupnpc openxr pcre2)
optdepends=('pipewire-alsa: for audio support' 'pulse-native-provider: for audio support')

source=("$pkgname-$pkgver::git+https://github.com/TheAenema/godot-jenova.git")
md5sums=("SKIP")

prepare() {
  cd $pkgname-$pkgver

  # Patch for miniupnpc
  sed -i 's/addr, 16/addr, 16, nullptr, 0/g' modules/upnp/upnp.cpp

  cd misc/dist/linux


  # Prepare the Godot Jnova desktop file
  cp -f org.godotengine.Godot.desktop org.godotengine.Godot-Jenova.desktop
  setconf org.godotengine.Godot-Jenova.desktop Exec 'env LD_PRELOAD=/usr/lib/libnghttp2.so:/usr/lib/libacl.so:/usr/lib/libssh2.so:/usr/lib/liblz4.so:/usr/lib/libb2.so:/usr/lib/libidn2.so:/usr/lib/libldap.so godot-jenova --display-driver wayland %f'
  setconf org.godotengine.Godot-Jenova.desktop Icon icon.svg
  setconf org.godotengine.Godot-Jenova.desktop Name 'Godot Engine Jenova'

  # Fix the MIME info, ref FS#77810
  sed -i 's,xmlns="https://specifications.freedesktop.org/shared-mime-info-spec",xmlns="http://www.freedesktop.org/standards/shared-mime-info",g' \
    org.godotengine.Godot.xml

  # Prepare the Godot Jnova MIME file as well
  cp -f org.godotengine.Godot.xml org.godotengine.Godot-Jenova.xml

  #************************#
  #********* Mono *********#
  #************************#

  # Prepare the Godot Jnova Mono desktop file
  cp -f org.godotengine.Godot.desktop org.godotengine.Godot-Jenova-mono.desktop
  setconf org.godotengine.Godot-Jenova-mono.desktop Exec 'env LD_PRELOAD=/usr/lib/libnghttp2.so:/usr/lib/libacl.so:/usr/lib/libssh2.so:/usr/lib/liblz4.so:/usr/lib/libb2.so:/usr/lib/libidn2.so:/usr/lib/libldap.so godot-jenova-mono --display-driver wayland %f'
  setconf org.godotengine.Godot-Jenova-mono.desktop Icon icon.svg
  setconf org.godotengine.Godot-Jenova-mono.desktop Name 'Godot Engine Jenova Mono'

  # Fix the MIME info, ref FS#77810
  sed -i 's,xmlns="https://specifications.freedesktop.org/shared-mime-info-spec",xmlns="http://www.freedesktop.org/standards/shared-mime-info",g' \
    org.godotengine.Godot.xml

  # Prepare the Godot Jnova Mono MIME file as well
  cp -f org.godotengine.Godot.xml org.godotengine.Godot-Jenova-mono.xml

}


build() {
	cd "$pkgname-$pkgver"

  export BUILD_NAME=arch_linux

  # Not unbundled (yet):
  #  mbedtls
  #  enet (contains no upstreamed IPv6 support)
  #  AUR: libwebm, rvo2
  #  recastnavigation, xatlas

  _args=(
    -j$(nproc --all)
    # verbose=yes
    cflags="$CFLAGS -fPIC -Wl,-z,relro,-z,now -w"
    cxxflags="$CXXFLAGS -fPIC -Wl,-z,relro,-z,now -w"
    linkflags="$LDFLAGS"
    extra_libs=atomic
    builtin_brotli=no
    builtin_certs=no
    builtin_clipper2=yes
    builtin_embree=no
    builtin_enet=yes
    builtin_freetype=no
    builtin_glslang=yes
    builtin_graphite=no
    builtin_harfbuzz=yes
    builtin_icu4c=yes
    builtin_libogg=no
    builtin_libpng=no
    builtin_libtheora=no
    builtin_libvorbis=no
    builtin_libwebp=no
    builtin_mbedtls=yes
    builtin_miniupnpc=no
    builtin_msdfgen=yes
    builtin_openxr=no
    builtin_pcre2=no
    builtin_pcre2_with_jit=no
    builtin_recastnavigation=yes
    builtin_rvo2_2d=yes
    builtin_rvo2_3d=yes
    builtin_squish=no
    builtin_wslay=yes
    builtin_xatlas=yes
    builtin_zlib=no
    builtin_zstd=no
    colored=yes
    debug_symbols=yes
    disable_exceptions=false
    platform=linuxbsd
    production=yes
    pulseaudio=yes
    system_certs_path=/etc/ssl/certs/ca-certificates.crt
    target=editor
    use_llvm=yes
    use_static_cpp=no
    lto=full
    werror=no
  )

  # Regular build
  scons "${_args[@]}"

  # Mono build
  _args+=(module_mono_enabled=yes mono_glue=no)
  scons "${_args[@]}"

  bin/godot.linuxbsd.editor.$_CARCH.mono --headless --generate-mono-glue modules/mono/glue
  modules/mono/build_scripts/build_assemblies.py --godot-output-dir=./bin --godot-platform=linuxbsd

}

package_godot-jenova() {
	cd "$pkgname-$pkgver"

  install -Dm755 bin/godot.linuxbsd.editor.$CARCH "$pkgdir/usr/bin/godot-jenova"

  install -Dm644 icon.svg "$pkgdir/usr/share/pixmaps/$pkgname.svg"
  install -Dm644 misc/dist/linux/org.godotengine.Godot-Jenova.desktop "$pkgdir/usr/share/applications/org.godotengine.Godot-Jenova.desktop"
  install -Dm644 misc/dist/linux/org.godotengine.Godot-Jenova.xml "$pkgdir/usr/share/mime/packages/org.godotengine.Godot-Jenova.xml"

  install -Dm644 misc/dist/linux/godot.6 "$pkgdir/usr/share/man/man6/$pkgname.6"
  install -Dm644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

}

package_godot-jenova-mono(){
  depends+=(dotnet-sdk-8.0)

  cd $pkgbase-$pkgver

  install -Dm755 bin/godot.linuxbsd.editor.$CARCH.mono "$pkgdir/usr/lib/$pkgname/godot.linuxbsd.editor.$CARCH.mono"

  cp -a bin/GodotSharp "$pkgdir/usr/lib/$pkgname/"
  install -d "$pkgdir/usr/bin"
  ln -s /usr/lib/$pkgname/godot.linuxbsd.editor.$CARCH.mono "$pkgdir/usr/bin/$pkgname"

  install -Dm644 icon.svg "$pkgdir/usr/share/pixmaps/$pkgname.svg"
  install -Dm644 misc/dist/linux/org.godotengine.Godot-Jenova-mono.desktop "$pkgdir/usr/share/applications/org.godotengine.Godot-Jenova-mono.desktop"
  install -Dm644 misc/dist/linux/org.godotengine.Godot-Jenova-mono.xml "$pkgdir/usr/share/mime/packages/org.godotengine.Godot-Jenova-mono.xml"

  install -Dm644 misc/dist/linux/godot.6 "$pkgdir/usr/share/man/man6/$pkgname.6"
  install -Dm644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

