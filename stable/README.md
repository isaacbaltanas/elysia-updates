# Canal estable

El archivo operativo de esta carpeta será `channel.json`. Debe ser un sobre JSON
con exactamente dos campos:

```json
{"payload":"<base64 canónico>","signature":"<firma Ed25519 base64>"}
```

El `payload`, una vez decodificado, debe cumplir el contrato `StableRelease` de
`workshop-agent/os-image/channel.ts`: formato 1, canal `stable`, versión semántica,
SHA-256 del manifiesto firmado, época de datos y fechas UTC de emisión/caducidad.

Orden obligatorio para publicar:

1. construir y probar la release completa;
2. firmar y verificar el bundle fuera de este directorio;
3. crear la GitHub Release borrador `agent-vX.Y.Z` con el bundle exacto;
4. revisar y publicar esa release sin sobrescribir artefactos;
5. generar el sobre con la clave Ed25519 custodiada externamente;
6. verificar localmente el sobre con la clave pública distribuida;
7. incorporar `channel.json` mediante revisión protegida;
8. probar su lectura anónima y directa desde un Debian limpio.

No reutilizar una versión, mover una etiqueta publicada ni anunciar un borrador.
Ante cualquier identidad ambigua, firma inválida, canal vencido o artefacto
ausente, no modificar el último canal válido y detener la publicación.
