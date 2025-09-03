devkitPro snapshot para Ubuntu no WSL2

TL;DR: este repositório disponibiliza um snapshot de /opt/devkitpro para ser usado exclusivamente no Ubuntu rodando no WSL2 (Windows 10/11).
Instale extraindo o arquivo da aba Releases diretamente na raiz do sistema (/). Depois, adicione as variáveis de ambiente ao seu ~/.bashrc.

⚠️ Suporte / Escopo

✅ Compatível: Ubuntu no WSL2 (shell do Ubuntu).

⚠️ Não testado/sem suporte: WSL1, outras distros no WSL, Linux nativo, macOS, Windows “puro”.

Se você quer algo portável para outras plataformas, use o instalador oficial do devkitPro/pacman. Este snapshot é apenas um atalho pragmático para WSL2 + Ubuntu.

📦 O que é este snapshot?

Um tarball com a árvore pronta do devkitPro em /opt/devkitpro para desenvolvimento homebrew (por ex. Nintendo Switch).
O conteúdo típico inclui:

Toolchains: devkitA64/ (AArch64) e devkitARM/ (ARM EABI)

libnx/ e tools/ (ex.: elf2nro)

Portlibs comuns (podem variar por release): switch-glfw, switch-glad, switch-mesa (EGL/GLES), switch-mbedtls, switch-glm, switch-libdrm_nouveau, switch-zlib, switch-libpng, switch-freetype, switch-bzip2, switch-pkg-config, switch-curl, etc.

Veja a descrição do Release para a lista exata.

✅ Pré-requisitos

Windows 10/11 com WSL2 habilitado

Ubuntu instalado no WSL2

Espaço livre em disco (o tar tem ~166 MB; instalado ocupa mais)

🚀 Instalação (Ubuntu no WSL2)

IMPORTANTE: execute no terminal do Ubuntu (WSL2), não no PowerShell.

Baixe o arquivo na aba Releases (ex.: devkitpro-wsl-YYYY-MM-DD.tar.zst).
Se baixou pelo navegador do Windows, ele estará em algo como:

/mnt/c/Users/SEU_USUARIO/Downloads/devkitpro-wsl-YYYY-MM-DD.tar.zst


Extraia na raiz / (o tar já contém opt/devkitpro/... por dentro):

# caminho de exemplo — ajuste para o nome/posição do seu arquivo
TAR="/mnt/c/Users/SEU_USUARIO/Downloads/devkitpro-wsl-YYYY-MM-DD.tar.zst"

# crie /opt/devkitpro (por garantia) e extraia o snapshot
sudo mkdir -p /opt/devkitpro
# Se a extensão for .tar.zst:
sudo tar -I zstd -xvf "$TAR" -C /
# Se a extensão for .tar.xz (alternativo):
# sudo tar -xvf "$TAR" -C /


Adicione as variáveis de ambiente ao seu ~/.bashrc:

grep -q "DEVKITPRO=/opt/devkitpro" ~/.bashrc || cat >>~/.bashrc <<'EOF'
# devkitPro (WSL2)
export DEVKITPRO=/opt/devkitpro
export DEVKITARM=${DEVKITPRO}/devkitARM
export DEVKITA64=${DEVKITPRO}/devkitA64
export PATH=$PATH:${DEVKITPRO}/tools/bin:${DEVKITARM}/bin
EOF

# Aplique na sessão atual
source ~/.bashrc


Verifique a instalação:

which aarch64-none-elf-gcc && aarch64-none-elf-gcc --version | head -n1
which arm-none-eabi-gcc     && arm-none-eabi-gcc --version | head -n1
which elf2nro
test -f /opt/devkitpro/libnx/switch_rules && echo "libnx OK"


Se os comandos acima aparecem, está tudo certo.

🧪 Dica de uso

Compile seus projetos sem sudo (ajuste permissões do diretório do projeto se necessário):

# exemplo
sudo chown -R "$USER":"$USER" /caminho/do/seu/projeto
cd /caminho/do/seu/projeto
make -j"$(nproc)"


Se precisar usar sudo e manter as variáveis do devkitPro:

sudo --preserve-env=PATH,DEVKITPRO,DEVKITARM,DEVKITA64 make -j"$(nproc)"

🧹 Desinstalar / Remover
sudo rm -rf /opt/devkitpro
# (opcional) remova as linhas do devkitPro do seu ~/.bashrc

🔐 Verificação (opcional)

Se o Release fornecer checksum:

# Exemplo para SHA256
sha256sum devkitpro-wsl-YYYY-MM-DD.tar.zst
# compare com o hash publicado na página do Release

❓ Perguntas frequentes

“Posso usar no Linux nativo / macOS?”
Este snapshot é pensado para Ubuntu no WSL2. Para outras plataformas, prefira o instalador oficial do devkitPro.

“Erro ‘cannot find -l…’ ao linkar”
Pode faltar alguma portlib específica para o seu projeto. Verifique se ela está listada no Release; caso não esteja, instale manualmente ou use o método oficial (pacman devkitPro).

“sudo não encontra as ferramentas do devkitPro”
Use sudo --preserve-env=PATH,DEVKITPRO,DEVKITARM,DEVKITA64 ... ou ajuste secure_path no visudo.

🙏 Créditos

devkitPro e mantenedores das portlibs.

Comunidade homebrew.

Este repositório apenas entrega um snapshot pronto para agilizar o setup no WSL2 + Ubuntu.

Boas compilações! 🛠️🎮
