# Despliegue de agenda NODE

La landing existente permanece en `nodeq.com.mx`. Esta aplicación se desplegará por separado en `agenda.nodeq.com.mx`.

## 1. Neon

1. Crear un proyecto PostgreSQL en Neon, preferentemente en una región de Norteamérica.
2. Copiar su cadena de conexión **pooled** y guardarla como `DATABASE_URL` en Vercel. No pegarla en el repositorio.
3. Con la misma variable en local, ejecutar `pnpm db:migrate` y `pnpm db:seed`.

## 2. Vercel

1. Crear un proyecto importando la carpeta `node-booking` desde el repositorio Git que elijas.
2. Definir `Root Directory` como `node-booking` si el repositorio contiene también la landing.
3. Añadir las variables de `.env.example` en Vercel; los secretos deben estar disponibles únicamente en servidor.
4. Usar `pnpm build` como comando de build. Vercel detectará Next.js automáticamente.
5. Desplegar primero en una URL de vista previa. La producción se activará cuando estén terminadas las integraciones de Google y correo.

## 3. Dominio y Cloudflare

1. En Vercel, añadir `agenda.nodeq.com.mx` al proyecto.
2. Vercel mostrará el registro DNS exacto requerido (normalmente un CNAME).
3. En Cloudflare DNS, crear ese CNAME apuntando al destino que indique Vercel y dejarlo inicialmente como **DNS only**.
4. Esperar la verificación del dominio en Vercel y después establecer `NEXT_PUBLIC_APP_URL=https://agenda.nodeq.com.mx`.

## 4. Antes de producción

- Configurar credenciales OAuth de Google con las URL finales.
- Añadir Turnstile, Resend y el secreto de cron.
- Aplicar migraciones a Neon y probar una reserva completa.
- Cambiar los CTA de la landing a `https://agenda.nodeq.com.mx/reservar`.

No se debe publicar el subdominio como agenda funcional hasta completar las fases de interfaz y Calendar.
