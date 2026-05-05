# Restricciones generales

Esta página resume restricciones transversales de la API de Kommo. Las restricciones específicas de un endpoint se documentan en su propia ficha.

- **Rate limit**: ver [`rate-limits.md`](rate-limits.md).
- **Tamaños de lote**: la mayoría de operaciones masivas aceptan hasta 250 ítems por petición.
- **Subdominio en URL**: cada cuenta tiene su propio subdominio; las URLs no son intercambiables entre cuentas.
- **Campos personalizados**: el mismo nombre puede tener distinto `field_id` entre cuentas; nunca uses IDs hardcodeados de otra cuenta.
- **Zonas horarias**: las marcas de tiempo se manejan en UTC (epoch en segundos).
- **IDs**: los IDs son numéricos y específicos de la cuenta; no asumas estabilidad entre entornos.
