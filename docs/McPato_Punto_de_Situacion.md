# McPato --- Punto de situación

> Documento de trabajo temporal.
>
> **Objetivo:** capturar el estado actual del diseño, las decisiones
> tomadas y los temas pendientes para poder retomar el proyecto
> exactamente donde se dejó.
>
> **No es documentación funcional definitiva.**

------------------------------------------------------------------------

# Estado del proyecto

## Completado

-   Definida la arquitectura inicial del libro.
-   Diseñadas las hojas:
    -   Activos
    -   Maestros
    -   Operaciones
    -   Dividendos (pendiente de revisión)
    -   Foto (modelo simplificado)
-   Diseñados los controles de calidad de Operaciones, Dividendos y
    Foto.
-   Decidido utilizar Google Sheets como fuente de verdad.
-   Decidido construir posteriormente un motor FIFO independiente.

------------------------------------------------------------------------

# Decisiones congeladas

## Operaciones

-   Una fila representa un único movimiento.
-   No existe contrapartida.
-   No existen columnas de coste de adquisición ni valor de transmisión.
-   El nombre del activo se obtiene automáticamente desde Activos.
-   El usuario únicamente introduce datos de entrada; los importes se
    calculan mediante fórmulas.
-   Los gastos se registran en una única columna **Gastos** (comisión,
    cánones, IVA, etc.).
-   Las retenciones se registran siempre separadas.
-   El signo económico se obtiene mediante `Factor_Importe` definido en
    el maestro `TipoMovimiento`.
-   La lógica nunca se codifica en las fórmulas; reside en los maestros.

## Activos

-   La identificación operativa es Mercado + Ticker.
-   El Activo_Fiscal únicamente existe en la hoja Activos.
-   El FIFO trabajará con Activo_Fiscal, no con Mercado + Ticker.

## Foto

-   Snapshot manual.
-   Todos los importes en EUR.
-   Incluye inversiones, efectivo y cuentas.
-   No depende de Google Finance.

------------------------------------------------------------------------

# Criterios adoptados

## Gastos

-   Si un gasto pertenece a un único activo, se registra íntegramente
    sobre ese activo.
-   Si un gasto afecta a varios activos, se registra una fila por
    activo.
-   El IVA u otros gastos comunes se reparten proporcionalmente entre
    los activos.
-   El redondeo se ajusta en el último activo para que el total coincida
    exactamente con el extracto.

## Cambios de divisa

-   Se representan mediante dos movimientos independientes:
    -   venta de la divisa origen;
    -   compra de la divisa destino.

## Custodia

Las comisiones de custodia se registran como:

-   Tipo = Comisión
-   Unidades = 0
-   Precio = 0
-   Gastos = importe total

------------------------------------------------------------------------

# Pendientes

## Alta prioridad

-   Revisar completamente la hoja Dividendos.
-   Simplificar su estructura siguiendo la filosofía de Operaciones.
-   Revisar si es necesario incorporar Gastos.
-   Definir los controles definitivos.

## Media prioridad

-   Revisar la hoja Foto tras cargar datos históricos.
-   Validar que todas las casuísticas reales pueden registrarse sin
    excepciones.

## Posterior

-   Diseñar el motor FIFO.
-   Diseñar la hoja de resultados fiscales.
-   Documentación funcional definitiva.

------------------------------------------------------------------------

# Riesgos conocidos

-   No introducir lógica de negocio en las fórmulas.
-   Evitar duplicar información entre hojas.
-   No registrar cálculos fiscales en Operaciones.
-   Mantener los maestros como único punto de configuración.

------------------------------------------------------------------------

# Próximos pasos

1.  Revisar y cerrar Dividendos.
2.  Cargar histórico real.
3.  Detectar casuísticas no cubiertas.
4.  Ajustar únicamente si aparecen casos reales.
5.  Congelar definitivamente Operaciones y Dividendos.
6.  Diseñar el motor FIFO.

------------------------------------------------------------------------

# Observaciones

Este documento debe mantenerse actualizado durante el diseño.

Cuando el módulo quede estabilizado, servirá de base para redactar la
documentación funcional definitiva.
