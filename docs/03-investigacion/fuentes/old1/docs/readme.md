# Consultar los planos de SANIA

Ejecuta en PowerShell:

```powershell
python "C:\Projects\ingenieria-requisitos\visor\servir.py" --datos "C:\Projects\SANIA\docs\planos.json"
```

El visor abrirá automáticamente el navegador en `http://127.0.0.1:8765/`. Si ese puerto estuviera ocupado, la consola mostrará la dirección alternativa.

Los cambios de `planos.json` se ven al recargar la página. El visor se apaga después de 15 minutos sin visitas; vuelve a ejecutar el comando para levantarlo otra vez.

## Evidencias de negocio

- [Preguntas pendientes de SANIA](preguntas-pendientes.md): lista consolidada de decisiones, casos reales e investigaciones que todavía faltan.
- [Catálogo de correos transaccionales de Wallapop y Vinted](evidencias/correos-transaccionales-wallapop-vinted.md): punto de entrada para reconocer las seis plantillas reales aportadas, extraer sus campos y saber qué estados respaldan.
- [Informe del flujo de correos de Wallapop](informe_flujo_correos_wallapop.md): análisis ampliado del corpus histórico de Wallapop, sus estados, enlaces y limitaciones.
