# Guía de colaboración

## Flujo de trabajo

1. Antes de comenzar, actualizar la copia local:

   ```bash
   git pull origin main
   ```

2. Trabajar únicamente en la carpeta o tarea asignada.
3. Guardar cambios con un commit descriptivo:

   ```bash
   git add .
   git commit -m "Describir el cambio realizado"
   ```

4. Sincronizar los cambios:

   ```bash
   git push origin main
   ```

5. Antes de editar nuevamente, ejecutar `git pull origin main`.

## Reglas para el archivo de Packet Tracer

- Evitar que dos personas editen el mismo archivo `.pkt` al mismo tiempo.
- Crear una copia de respaldo antes de realizar cambios importantes.
- Usar nombres de versión claros, por ejemplo:
  `red-corporativa-v01.pkt`, `red-corporativa-v02.pkt`.
- Documentar en el `README.md` o en la bitácora qué se modificó en cada versión.

## Reglas de commits

Los mensajes deben indicar el resultado del cambio. Ejemplos:

- `Definir departamentos y VLAN`
- `Agregar direccionamiento IP de la casa matriz`
- `Configurar VPN IPsec entre sedes`
- `Actualizar presupuesto de equipos`
- `Incluir evidencias de pruebas`

No subir credenciales reales, claves privadas, tokens ni información personal innecesaria.
