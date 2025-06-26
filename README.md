# Laboratorios-del-modulo-IV.
Practica 3: Instalar un servidor de Impresion (1pts)
Practica de Servidor de Impresión CUPS (CentOS)
===============================================

1. Verificar configuración de red y servicios
---------------------------------------------
ip addr show
    Muestra las interfaces de red y sus IPs configuradas en el servidor.

ip route show
    Muestra las rutas configuradas para la comunicación de red.

sudo firewall-cmd --list-all
    Muestra las reglas activas del firewall.

sudo ss -tuln | grep 631
    Verifica que el puerto 631 (CUPS) está escuchando para conexiones IPP.

2. Instalación de paquetes necesarios
-------------------------------------
sudo dnf install cups -y
    Instala el servidor CUPS.

sudo dnf install epel-release -y
    Instala el repositorio EPEL para paquetes extra.

sudo dnf install ghostscript cups-filters -y
    Instala Ghostscript y filtros CUPS necesarios para procesar trabajos de impresión y manejo de PDF.

3. Manejo y configuración del servicio CUPS
-------------------------------------------
sudo systemctl enable cups
    Configura CUPS para que se inicie automáticamente al arrancar el sistema.

sudo systemctl start cups
    Inicia el servicio CUPS.

sudo systemctl restart cups
    Reinicia el servicio CUPS para aplicar cambios.

4. Creación y permisos del backend personalizado para impresora PDF
-------------------------------------------------------------------
mkdir -p /home/reily20241198/impresiones
    Crea la carpeta donde se guardarán los archivos PDF generados por la impresora.

sudo nano /usr/lib/cups/backend/pdfprint
    Edita (o crea) el script backend personalizado para guardar impresiones como PDF.

sudo chmod +x /usr/lib/cups/backend/pdfprint
    Da permisos de ejecución al script backend.

sudo chmod 755 /usr/lib/cups/backend/pdfprint
    Asegura permisos correctos para que CUPS pueda ejecutar el backend.

5. Verificación y gestión de impresoras en CUPS
-----------------------------------------------
sudo /usr/lib/cups/backend/pdfprint
    Ejecuta el backend personalizado para comprobar que responde correctamente.

lpstat -p
    Lista las impresoras configuradas en CUPS y su estado.

lp -d PDF-Custom prueba.txt
    Envía un archivo o texto a la impresora llamada PDF-Custom.

echo "Texto de prueba" | lp -d PDF-Custom
    Envía texto directamente a la impresora para imprimir.

6. Comandos para administrar permisos y directorios
---------------------------------------------------
ls -l /usr/lib/cups/backend/pdfprint
    Verifica los permisos y existencia del backend personalizado.

ls -ld /home/reily20241198/impresiones/
    Muestra permisos y propiedades de la carpeta de salida PDF.

chmod 755 /home/reily20241198/impresiones/
    Ajusta permisos para que la carpeta sea accesible.

chown reily20241198:reily20241198 /home/reily20241198/impresiones/
    Cambia el propietario y grupo de la carpeta para el usuario.

7. Comandos para navegación y prueba
------------------------------------
ls /home/reily20241198/impresiones/
    Lista archivos PDF generados.

ls -ltr /home/reily20241198/impresiones/
    Lista los archivos ordenados por fecha, para ver los más recientes.

Bonus: URL para agregar impresora desde cliente
-----------------------------------------------
ipp://192.168.0.108:631/printers/PDF-Custom
    URL IPP para conectar desde PC cliente a la impresora compartida.
