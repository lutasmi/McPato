# McPato --- Documentación de la pestaña Operaciones

**Versión:** 1.0

## Objetivo

La pestaña **Operaciones** es el registro único de todos los movimientos
financieros que afectan a la cartera. Registra hechos, no cálculos.

No calcula FIFO, plusvalías, posiciones ni rentabilidades. Es la entrada
del futuro motor fiscal.

## Principios

-   Una fila = un movimiento.
-   Registrar hechos, no interpretaciones.
-   Mantener la hoja simple.
-   Toda la lógica de negocio vive en los maestros.
-   Las fórmulas nunca se modifican.

## Estructura

  -----------------------------------------------------------------------
  Campo                   Responsable             Descripción
  ----------------------- ----------------------- -----------------------
  ID_Operacion            Fórmula                 Identificador único.

  Fecha                   Usuario                 Fecha efectiva.

  Titular                 Usuario                 Propietario.

  Cuenta                  Usuario                 Broker, banco o
                                                  plataforma.

  Tipo_Movimiento         Usuario                 Tipo de movimiento.

  Mercado                 Usuario                 Mercado operativo.

  Ticker                  Usuario                 Ticker o ISIN.

  Activo                  Fórmula                 Nombre normalizado
                                                  desde Activos.

  Divisa                  Usuario                 Divisa original.

  Unidades                Usuario                 Positivas para
                                                  entradas, negativas
                                                  para salidas. Gastos
                                                  puros = 0.

  Precio_Unitario         Usuario                 Precio por unidad.
                                                  Gastos puros = 0.

  Gastos                  Usuario                 Comisión, cánones, IVA
                                                  y demás gastos
                                                  asociados.

  Retención               Usuario                 Retención fiscal.

  TC_EUR                  Usuario                 Tipo de cambio a EUR (1
                                                  si EUR).

  Importe_Bruto           Fórmula                 ABS(Unidades × Precio).

  Importe_Neto            Fórmula                 (Unidades × Precio ×
                                                  Factor_Importe) −
                                                  Gastos − Retención.

  Importe_Neto_EUR        Fórmula                 Importe_Neto en EUR.

  Flag_Revisar            Fórmula                 Marca incidencias.

  Motivo_Revision         Fórmula                 Explica la incidencia.

  Comentario              Usuario                 Observaciones.
  -----------------------------------------------------------------------

## Fórmulas

### Activo

Se obtiene automáticamente desde la hoja **Activos** mediante la
combinación Ticker + Mercado.

### Importe_Bruto

`ABS(Unidades × Precio_Unitario)`

### Importe_Neto

`(Unidades × Precio_Unitario × Factor_Importe) − Gastos − Retención`

`Factor_Importe` se obtiene desde el maestro **TipoMovimiento**. La
fórmula no contiene reglas específicas por tipo de movimiento.

### Importe_Neto_EUR

-   EUR → Importe_Neto.
-   Resto → Importe_Neto × TC_EUR.

## Reglas de registro

### Compra

-   Unidades positivas.
-   Gastos en la columna Gastos.

### Venta

-   Unidades negativas.

### Comisión independiente

-   Tipo = Comisión.
-   Unidades = 0.
-   Precio = 0.
-   Gastos = importe cargado.

### Venta de derechos

-   Tipo = Venta Derechos.
-   Retención en su columna.

### Cambio de divisa

-   Dos filas: venta de divisa origen y compra de divisa destino.

### Gastos comunes de varios activos

Registrar una fila por activo. Si existe IVA común, repartirlo
proporcionalmente.

## Qué NO hace

-   FIFO
-   Posiciones
-   Precio medio
-   Plusvalías
-   Minusvalías

## Futuro

El motor FIFO leerá esta hoja, resolverá Activo_Fiscal desde Activos y
calculará los resultados fiscales sin modificar el registro original.
