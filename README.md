# Minecraft Cleaner

Limpador profundo de ficheiros temporários do Minecraft e Windows. Remove logs, cache, crash reports e ficheiros desnecessários sem tocar em saves, mods, configurações ou resource packs.

## Download

Descarregue o executável mais recente em [Releases](../../releases).

**Requisitos:** Windows 10/11 64-bit

## Uso Rápido

```powershell
# Limpeza padrão (Minecraft + %TEMP% + Prefetch)
.\MinecraftCleaner.exe

# Modo profundo (padrões extra, pastas debug/tmpcache)
.\MinecraftCleaner.exe --deep

# Modo ultra (cache browsers, GPU, Discord, logs Windows, telemetria)
.\MinecraftCleaner.exe --ultra

# Máximo: ultra + reciclagem + event logs + DNS
.\MinecraftCleaner.exe --ultra --recycle --event-logs

# Sem confirmação (para scripts)
.\MinecraftCleaner.exe --ultra --yes

# Só ficheiros com mais de 30 dias
.\MinecraftCleaner.exe --days=30
```

## O que limpa

### Minecraft (deteta automaticamente)
- **Modrinth App:** `%APPDATA%\ModrinthApp\profiles\*` ou `instances\*`
- **Launcher oficial:** `%APPDATA%\.minecraft`

| Pasta | Descrição |
|-------|-----------|
| `logs/` | Logs do jogo |
| `crash-reports/` | Relatórios de crash |
| `cache/`, `tmp/`, `webcache2/` | Caches temporários |
| `debug/`, `tmpcache/`, `reports/` | Modo `--deep` |
| `screenshots/` | Só com `--screenshots` |
| `backups/` | Só com `--backups` |

### Windows

| Alvo | Requer Admin |
|------|--------------|
| `%TEMP%`, `AppData\Local\Temp` | Não |
| Cache IE/Edge, WebCache, thumbnails | Não |
| CrashDumps, D3DSCache | Não |
| Relatórios de erro (WER) | Não |
| `C:\Windows\Prefetch` | Sim |
| `C:\Windows\Temp` | Sim |
| `C:\Windows\SoftwareDistribution\Download` | Sim |
| `C:\Windows\Logs`, `Minidump` | Sim |
| Cache DNS (`--dns`) | Não |
| Event Logs (`--event-logs`) | Sim |
| Reciclagem (`--recycle`) | Não |

### Modo Ultra (`--ultra`)
Adiciona cache de: Chrome, Edge, Firefox, Discord, Teams, NVIDIA/AMD shaders, Office, Delivery Optimization, telemetria antiga.

## Flags

| Flag | Descrição |
|------|-----------|
| `--deep` | Padrões extra (`.old`, `.dmp`, `.part`) + pastas debug/tmpcache |
| `--ultra` | Máximo seguro (inclui `--deep`) |
| `--days=N` | Só apaga ficheiros com N+ dias |
| `--yes` | Sem confirmação |
| `--screenshots` | Inclui screenshots do Minecraft |
| `--backups` | Inclui backups automáticos de mundos |
| `--recycle` | Esvazia a Reciclagem |
| `--event-logs` | Limpa Event Logs (requer admin) |
| `--dns` | Limpa cache DNS |

## Segurança

- **Nunca apaga:** `saves/`, `mods/`, `config/`, `resourcepacks/`, `shaderpacks/`, `options.txt`
- Pedido de confirmação antes de eliminar (exceto com `--yes`)
- Resumo detalhado do que será eliminado
- Ignora automaticamente ficheiros em uso

## Executar como Administrador

Para limpeza completa (Prefetch, Windows\Temp, logs CBS, Event Logs):

```powershell
# PowerShell como Administrador
.\MinecraftCleaner.exe --ultra
```

Ou crie um atalho com "Executar como administrador" ativado.

## Compilar do Código-Fonte

Requer [.NET 10 SDK](https://dotnet.microsoft.com/download).

```powershell
# Clone
git clone https://github.com/SEU-USUARIO/MinecraftCleaner.git
cd MinecraftCleaner

# Build e publish
.\publish.ps1

# Ou manual:
dotnet publish -c Release -r win-x64 --self-contained -o dist
```

O executável estará em `dist\MinecraftCleaner.exe`.

## Estrutura do Projeto

```
MinecraftCleaner/
├── 13/
│   ├── Program.cs          # Código principal
│   └── 13.csproj           # Projeto .NET
├── publish.ps1             # Script de build/publish
├── README.md
├── LICENSE
└── .gitignore
```

## Licença

MIT — veja [LICENSE](LICENSE).

## Disclaimer

Este software é fornecido "tal como está". Embora testado, use por sua conta e risco. Faça backup de dados importantes antes de executar ferramentas de limpeza.
