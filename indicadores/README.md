# Velas por Dentro PRO (TradingView)

Basado en el vídeo de *marrow_trading* "Ver un retroceso por dentro": **cada vela te cuenta
quién mandó dentro de ella**. Cada vela se dibuja hueca (borde azul = alcista, borde rojo = bajista)
y por dentro se rellena con el **volumen comprador (verde)** y el **volumen vendedor (rojo)**.

Archivo: [`velas_por_dentro_pro.pine`](velas_por_dentro_pro.pine) (Pine Script v6).

## De dónde sale la información de compradores y vendedores

TradingView no da directamente "compras" y "ventas" de cada vela, así que el indicador
**abre la vela por dentro**: pide las velas de una temporalidad inferior con
`request.security_lower_tf` (por ejemplo, las 15 velas de 1 minuto que forman una vela de 15m) y:

- si una vela interna cierra **arriba** de su apertura, su volumen cuenta como **compra**;
- si cierra **abajo**, cuenta como **venta**;
- si cierra igual, se compara con el cierre anterior (regla del tick) o se reparte 50/50.

Es el mismo método que usa el indicador oficial *Volume Delta* de TradingView. Cuando no hay datos
internos (velas muy antiguas), se estima por la posición del cierre y el relleno sale **más transparente**.

> Diferencia con el vídeo: TradingView no permite dibujar dos barritas una al lado de la otra
> dentro de una vela. Por eso el reparto se muestra apilado: **verde abajo = % de compradores,
> rojo arriba = % de vendedores**. Más verde = mandaron los compradores.

## Instalación (paso a paso)

1. En el ordenador, abre el archivo `.pine` en GitHub y copia todo el código (botón de los dos cuadraditos).
2. En tradingview.com abre un gráfico → pestaña inferior **Editor de Pine**.
3. Selecciona todo (Ctrl+A), bórralo y pega el código (Ctrl+V).
4. **Guardar** → **Añadir al gráfico**.
5. **Oculta las velas normales** para que no se tapen: clic en el engranaje del gráfico
   (**Configuración**) → pestaña **Símbolo** → desmarca **Cuerpo**, **Bordes** y **Mecha** → Aceptar.
6. En el móvil lo encontrarás en **Indicadores → Mis scripts**.

## Cómo leerlo

| Lo que ves | Qué significa |
|---|---|
| Borde azul, casi todo verde dentro | Subida con compradores de verdad. Fuerte. |
| Borde rojo, casi todo rojo dentro | Bajada con vendedores de verdad. Fuerte. |
| **Borde amarillo** en vela roja con mucho verde dentro | Bajó, pero dentro mandaron los compradores → **retroceso débil**. |
| **Borde amarillo** en vela verde con mucho rojo dentro | Subió, pero dentro mandaron los vendedores → **subida débil**. |
| **Línea azul horizontal** | Máximo de la primera vela de giro tras un retroceso débil (como en el vídeo). |
| Línea gris punteada | Stop: el mínimo del retroceso. |
| **COMPRA / VENTA** | El precio cerró rompiendo la línea → confirmación. |

**Ejemplo del vídeo:** en una subida aparecen 3–4 velas rojas (el retroceso), pero por dentro
tienen más verde que rojo. Los vendedores no tienen fuerza. Cuando sale una vela azul con compradores
dentro, se marca la línea en su máximo; si el precio la rompe → **COMPRA**, stop bajo el mínimo del retroceso.

El panel (arriba a la derecha) muestra para la vela actual: temporalidad interna usada,
% compradores, % vendedores, delta (compras − ventas) y si hay un nivel activo.

## Uso por temporalidad

| Tu gráfico | TF interna (Auto) | Velas dentro | Perfil | Consejos |
|---|---|---|---|---|
| 1m | 1m ⚠️ | 1 | Scalping | No hay nada que abrir: pon Modo **Manual** y **1S** (requiere plan con segundos). Si no, usa 3m o 5m. |
| 5m | 1m | 5 | Scalping | Mucho ruido: sube "Velas mínimas del retroceso" a 3. Solo en horas de volumen. |
| **15m** | 1m | 15 | **Intradía (para empezar)** | El mejor equilibrio. Opera a favor de la tendencia de 1H. |
| 1H | 1m | 60 | Intradía / swing corto | Lectura interna muy precisa. Ideal en cripto. |
| 4H | 5m | 48 | Swing | Muy fiable; pocas señales. Usa alertas. |
| Diario | 15m | 96 (cripto) | Swing / posición | Para la dirección general. Stops amplios → posición más pequeña. |
| Semanal | 1H | 168 | Inversión | Solo contexto. |

> Las temporalidades internas bajas tienen menos historia: en gráficos lentos verás las velas
> más antiguas con relleno transparente (estimado). Es normal.

**Método top-down sugerido:**

1. **Dirección** en 4H: ¿las velas buenas tienen relleno del color de la vela y el precio está sobre la EMA 50?
2. **Retroceso** en 1H o 15m: busca velas en contra con **borde amarillo** (débiles por dentro).
3. **Entrada** cuando salga **COMPRA/VENTA** al romper la línea.

## Ajustes

| Parámetro | Por defecto | Cuándo cambiarlo |
|---|---|---|
| Temporalidad interna | Auto | Manual si quieres más detalle (p. ej. 1S) o más historia (p. ej. 5m). |
| Velas mínimas del retroceso | 2 | 3 en 1m–5m para filtrar ruido. |
| % velas con dominio contrario | 50 | 60–70 para menos señales pero más limpias. |
| Filtrar con EMA | Sí (50) | Desactívalo solo si sabes leer la tendencia tú mismo. |
| Velas máximas para romper | 10 | Menos si quieres solo rupturas rápidas. |
| Mostrar % bajo cada vela | No | Actívalo para ver el número exacto de compradores. |

## Alertas

Clic derecho en el gráfico → **Añadir alerta** → Condición: *Velas por Dentro PRO* → elige
`COMPRA confirmada`, `VENTA confirmada`, `Nivel de compra listo`, `Vela roja con compradores dentro`…
Marca **"Una vez por cierre de barra"**.

## Gestión del riesgo

- Arriesga **máximo 1 %** de la cuenta por operación: tamaño = (1 % de la cuenta) / (entrada − stop).
- Busca al menos **2R** (ganar el doble de lo que arriesgas).
- En forex/CFD el volumen es "de ticks" (número de cambios de precio): sirve, pero es menos preciso que en cripto.
- Prueba **30–50 operaciones en demo o con Replay** y apunta resultados antes de usar dinero real.

> Ningún indicador garantiza ganancias. Es una herramienta de lectura del precio, no un consejo financiero.
