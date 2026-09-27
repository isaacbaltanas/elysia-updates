# Canal de actualizaciones de ElysiaOS

Esta carpeta es la superficie pública mínima para descubrir actualizaciones de
ElysiaOS. No es un almacén de software y no concede confianza por estar en GitHub.

La copia presente en el repositorio privado de código define y revisa la
estructura. Su contenido se publica como raíz del repositorio público
`isaacbaltanas/elysia-updates`, que ElysiaOS puede leer sin credenciales. No se
incorpora ningún token GitHub a la imagen.

## Separación de responsabilidades

- `stable/channel.json` anuncia una única release estable mediante un sobre
  Ed25519 pequeño, versionado y con caducidad.
- GitHub Releases almacena el bundle completo e inmutable bajo el tag
  `agent-vX.Y.Z`.
- `workshop-agent/updates/source.ts` contiene exclusivamente las direcciones de
  confianza compiladas.
- `workshop-agent/os-image/channel.ts` valida formato, firma, reloj, caducidad,
  versión, identidad y época de datos.
- `workshop-agent/updates/download.ts` descarga el bundle con límites y orígenes
  permitidos.
- `workshop-agent/updates/store.ts` verifica y prepara releases inmutables.
- `workshop-agent/updates/coordinator.ts` posee activación, evidencia de salud y
  rollback conservador.

## Contenido permitido

Solo documentación y sobres de canal firmados. Nunca deben entrar aquí:

- bundles `.bundle`, ISO, tar o binarios;
- claves privadas, contraseñas, tokens o credenciales GitHub;
- datos de taller, identidades Cloud o copias de SQLite;
- scripts con privilegios ni lógica de instalación o rollback.

La ausencia de `stable/channel.json` significa que no hay ninguna actualización
publicable. No se añade un archivo vacío, de ejemplo o sin firma: el cliente debe
fallar cerrado hasta que exista una release aprobada.

El procedimiento de construcción, firma, publicación y recuperación está en
`docs/98-AGENT-COMPLETE-UPDATES.md`.
