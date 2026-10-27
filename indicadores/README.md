# Velas por Dentro PRO (TradingView)

Indicador basado en la idea del vídeo de *marrow_trading* "Cada vela trae adentro…":
cada vela de una temporalidad mayor (HTF) se dibuja como una **caja con mechas** que
envuelve las velas de tu gráfico. Así ves, por ejemplo, que una vela roja de 1H
que parece un retroceso está hecha por dentro de velas de 15m con compras, y que
los vendedores no tienen fuerza.

Archivo: [`velas_por_dentro_pro.pine`](velas_por_dentro_pro.pine) (Pine Script v6).

## Instalación

1. En TradingView abre **Editor de Pine** (parte inferior).
2. Borra lo que haya, pega el contenido de `velas_por_dentro_pro.pine`.
3. **Guardar** → **Añadir al gráfico**.

## Qué añade respecto al vídeo

| Mejora | Para qué sirve |
|---|---|
| TF superior automático | Eliges tu gráfico y el indicador elige la vela grande adecuada. |
| Mechas y nivel 50 % de la vela HTF | El 50 % es la zona típica donde termina un retroceso sano. |
| Presión compradora/vendedora interna | Reparte el volumen de cada vela pequeña según dónde cierra; dice quién manda *dentro* de la vela grande. |
| Detección de **retroceso débil** (R▲ / R▼) | Vela HTF en contra de la tendencia, pero con presión interna a favor de la tendencia. |
| Señal de **COMPRA / VENTA** | Solo cuando el precio rompe el extremo del retroceso débil (confirmación). |
| Línea de entrada (amarilla) y stop (gris) | Gestión del riesgo clara. |
| Panel y 4 alertas | Sin estar mirando la pantalla. |

La tendencia se mide con una EMA (20 por defecto) **en la temporalidad superior**
y solo con velas cerradas, por lo que las marcas R▲/R▼ no repintan.

## Cómo leerlo

- **Caja azul** = vela HTF alcista. **Caja roja** = vela HTF bajista.
- **R▲** debajo de una caja roja: en tendencia alcista, retroceso donde los compradores
  siguieron activos por dentro (≥ 45 % por defecto). Espera la ruptura del máximo de esa caja.
- **COMPRA**: la vela de tu gráfico cerró por encima de la línea amarilla. Stop bajo la línea gris.
- **R▼ / VENTA**: lo mismo en espejo para tendencia bajista.
- Si el precio cierra al otro lado del stop antes de romper, el setup se cancela.
  Si no rompe en 2 velas HTF (configurable), caduca.

## Uso por temporalidad

| Tu gráfico | Vela HTF (Auto) | Velas dentro | Perfil | Consejos |
|---|---|---|---|---|
| 1m | 5m | 5 | Scalping | Mucho ruido. Úsalo solo en horas de volumen (apertura Londres/NY). Sube el umbral a 50–55 %. |
| 3m / 5m | 15m | 5 / 3 | Scalping–intradía | Bueno para entradas finas de un plan hecho en 1H. Opera solo a favor del 1H. |
| 15m | 1H | 4 | **Intradía (recomendado para empezar)** | Mejor equilibrio señal/ruido. Usa R▲/R▼ cerca del 50 % de la vela HTF anterior o de soportes. |
| 30m | 2H | 4 | Intradía tranquilo | Menos señales, más fiables. |
| 1H | 4H | 4 | Swing corto | Muy útil en cripto. Revisa 2–3 veces al día; usa alertas. |
| 2H / 4H | Diario | 12 / 6 | Swing | Ver la vela diaria "por dentro". Stops más amplios: reduce el tamaño de la posición. |
| Diario | Semanal | 5–7 | Posición | Para decidir dirección general, no para entradas exactas. |
| Semanal | Mensual | 4–5 | Inversión | Solo contexto macro. |

**Método top-down sugerido (multi-temporalidad):**

1. **Dirección**: en 4H (con vela diaria dentro) mira si la tendencia del panel es Alcista/Bajista.
2. **Setup**: baja a 1H (vela 4H dentro) y espera un R▲ o R▼ a favor de esa dirección.
3. **Entrada**: baja a 15m y entra con la señal COMPRA/VENTA o con la ruptura de la línea amarilla.

Si quieres fijar tú la temporalidad (por ejemplo ver siempre la vela de 4H), pon **Modo = Manual**.

## Ajustes recomendados

| Parámetro | Por defecto | Cuándo cambiarlo |
|---|---|---|
| EMA de tendencia | 20 | 50 para filtrar más (menos señales, más calidad). |
| Umbral retroceso débil | 45 % | 50–55 % en 1m–5m o mercados muy ruidosos; 40 % en 4H+. |
| Velas HTF para confirmar | 2 | 1 si quieres solo rupturas rápidas. |

## Alertas

Clic derecho en el gráfico → **Añadir alerta** → Condición: *Velas por Dentro PRO* y elige:
`Retroceso débil alcista/bajista` (aviso previo) o `Entrada larga/corta confirmada`.
Usa la opción **"Una vez por cierre de barra"**.

## Gestión del riesgo (lo más importante para ser sostenible)

- Arriesga **máximo 1 % de la cuenta** por operación: tamaño = (1 % de la cuenta) / (entrada − stop).
- Objetivo mínimo **2R** (el doble de lo que arriesgas); el máximo de la vela HTF previa suele ser un buen primer objetivo.
- En pares sin volumen real (algunos forex/CFD) el indicador usa el conteo de velas en lugar del volumen; la presión es menos precisa.
- **Prueba primero en demo o con el Replay de TradingView** al menos 30–50 operaciones y apunta los resultados antes de usar dinero real.

> Ningún indicador garantiza ganancias. Esto es una herramienta de lectura del precio, no un consejo financiero.
