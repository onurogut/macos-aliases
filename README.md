# 🚀 Spike - Port Killer with Smart Process Detection

Portları tek komutla sonlandıran ve hangi dosyayı çalıştırdığını gösteren akıllı terminal alias'ı.

## 📸 Örnek Kullanım
```
spike 8080
```

**Çıktı:**
```
--------------------------------------------------
✅ Port 8080 başarıyla sonlandırıldı.
🚀 Komut: Python app.py
📍 Dosya Yolu: /Users/onurogut/ornek/app.py
--------------------------------------------------
```
<img width="736" height="314" alt="image" src="https://github.com/user-attachments/assets/6d822c57-ebac-43f6-a9c8-143bf7ce720f" />


## 🔧 Kurulum

### macOS / Linux (Zsh)

1. `.zshrc` dosyanızı açın:
```bash
nano ~/.zshrc
```

2. Aşağıdaki alias'ı dosyanın sonuna ekleyin:
```bash
alias spike='f(){ pid=$(lsof -t -i:"$1"); if [ -n "$pid" ]; then process_info=$(ps -p $pid -o args=); script_name=$(echo "$process_info" | awk "{for(i=2;i<=NF;i++){if(\$i !~ /^-/ && \$i ~ /\// || \$i ~ /^\./){print \$i; exit}}}"); if [ -z "$script_name" ]; then script_name=$(echo "$process_info" | awk "{for(i=2;i<=NF;i++){if(\$i !~ /^-/){print \$i; exit}}}"); fi; cwd=$(lsof -p $pid 2>/dev/null | grep " cwd " | awk "{print \$NF}"); if [ -n "$script_name" ]; then if [[ "$script_name" = /* ]]; then script_path="$script_name"; elif [ -n "$cwd" ]; then script_path="${cwd}/${script_name}"; else script_path="$script_name"; fi; else script_path="Bulunamadı"; fi; kill -9 $pid; echo "--------------------------------------------------"; echo "✅ Port $1 başarıyla sonlandırıldı."; echo "🚀 Komut: ${process_info##*/}"; echo "📍 Dosya Yolu: ${script_path}"; echo "--------------------------------------------------"; else echo "❌ Port $1 üzerinde çalışan bir işlem bulunamadı."; fi; unset -f f; }; f'
```

3. Kaydedin (Ctrl+O, Enter, Ctrl+X) ve aktif edin:
```bash
source ~/.zshrc
```
