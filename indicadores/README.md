# Velas por Dentro PRO (TradingView)

Réplica mejorada del indicador del vídeo de *marrow_trading* "Ver un retroceso por dentro":
**cada vela te cuenta quién mandó**.

Archivo: [`velas_por_dentro_pro.pine`](velas_por_dentro_pro.pine) (Pine Script v6).

## Cómo se ve cada vela

```
      │            ← mecha (color del borde)
  ┌───┬───┐        ← borde AZUL = la vela subió · borde ROJO = la vela bajó
  │███│   │
  │███│███│        barra VERDE (izquierda) = compradores
  │███│███│        barra ROJA  (derecha)   = vendedores
  └───┴───┘        la barra MÁS ALTA es el bando que mandó
      │
```

- **Borde azul + verde más alta** → subió y compraron de verdad. Fuerza alcista.
- **Borde rojo + roja más alta** → bajó y vendieron de verdad. Fuerza bajista.
- **Borde rojo + verde más alta** → bajó, pero dentro mandaron los compradores: **retroceso débil** (lo del vídeo).
  Si el bando contrario gana con claridad (≥ 60 %), el borde se pone **amarillo**.
- **Borde azul + roja más alta** → subió sin compradores: **subida débil**, cuidado.

## Cómo funciona (importante)

Para que quepan **dos barras dentro de una vela**, cada vela se dibuja con varias velas del gráfico.
Por eso **el gráfico va en una temporalidad menor** que las velas que quieres ver:

| Pon el gráfico en | Verás velas de | Compras/ventas medidas con |
|---|---|---|
| 1m | 5m | 1m |
| 3m | 15m | 1m |
| 5m | 30m | 1m |
| **15m** | **1H** | 1m |
| 30m | 2H | 1m |
| 1H | 4H | 1m |
| 4H | Diario | 5m |
| Diario | Semanal | 15m |

(Puedes elegir otra combinación con *Temporalidad de las velas → Manual*.)

Compras y ventas: el indicador abre cada vela con `request.security_lower_tf`; el volumen de las
velas internas que cierran arriba cuenta como compra y el de las que cierran abajo, como venta
(mismo método que el *Volume Delta* oficial de TradingView). En velas muy antiguas sin datos internos
se estima y las barras salen más transparentes.

## Instalación

1. Copia todo el código de `velas_por_dentro_pro.pine` (botón de los dos cuadraditos en GitHub).
2. TradingView (ordenador) → **Editor de Pine** → Ctrl+A, pegar, **Guardar** → **Añadir al gráfico**.
3. **Oculta las velas normales**: engranaje del gráfico → **Símbolo** → desmarca **Cuerpo**, **Bordes** y **Mecha**.
4. Pon el gráfico en **15m** para ver velas de **1H** (o la combinación que prefieras de la tabla).

## La señal del vídeo: retroceso débil → línea → ruptura

1. En una subida aparecen **2 o más velas rojas** (retroceso), pero dentro **la barra verde es más alta**.
2. Sale una **vela azul** con compradores al mando → se dibuja una **línea azul** en lo alto de su cuerpo,
   hacia la derecha (como al final del vídeo). La línea gris punteada es el stop (mínimo del retroceso).
3. Si el precio **cierra por encima de la línea** → **COMPRA**. Si cae bajo el stop o pasan 3 velas, se cancela.
4. Lo mismo al revés para **VENTA** (rebote débil en bajada).

## Ajustes

| Parámetro | Por defecto | Para qué |
|---|---|---|
| Altura de las barras | Comparar | La barra ganadora llena la vela y la otra su proporción. "% del cuerpo" = cada barra mide su %. |
| Divergencia si el bando contrario ≥ % | 60 | Sube a 65–70 en monedas pequeñas si ves demasiado amarillo. |
| Velas mínimas del retroceso | 2 | 3 para menos señales y más limpias. |
| Filtrar con EMA | Sí (20 velas dibujadas) | Solo compras en subida y ventas en bajada. |
| Mostrar % de compradores | No | Número exacto bajo cada vela. |

## Alertas

Clic derecho → **Añadir alerta** → *Velas por Dentro PRO* → `COMPRA confirmada`, `VENTA confirmada`,
`Nivel de compra listo`, `Nivel de venta listo`. Opción **"Una vez por cierre de barra"**.

## Gestión del riesgo

- Máximo **1 %** de la cuenta por operación: tamaño = (1 % de la cuenta) / (entrada − stop).
- Objetivo mínimo **2R**.
- Prueba **30–50 operaciones en demo o Replay** antes de usar dinero real.

> Ningún indicador garantiza ganancias. Es una herramienta de lectura del precio, no un consejo financiero.
