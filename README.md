# Obtención de la 1-FDP de un PVIA mediante RVT y Liouville-Gibbs

Código de la **Actividad 2** de la asignatura *Ecuaciones diferenciales estocásticas y aplicaciones* (Máster Universitario en Ingeniería Matemática y Computación, UNIR).

Se verifica numéricamente la primera función de densidad de probabilidad (1-FDP) del proceso solución del problema de valor inicial aleatorio

$$
X'(t) = a\,(b - X(t)), \qquad X(0) = X_0,
$$

obtenida analíticamente por el método de transformación de variables aleatorias (RVT) y por la ecuación de Liouville-Gibbs:

$$
f_1(x,t) = f_{X_0}\!\left(b + (x-b)\,e^{a t}\right) e^{a t}.
$$

## Caso de estudio

Enfriamiento de un cuerpo con temperatura inicial incierta:

| Parámetro | Valor |
|---|---|
| Tasa $a$ | $0.1$ min⁻¹ |
| Temperatura ambiente $b$ | $20$ |
| Condición inicial $X_0$ | $N(90, 5^2)$ |
| Intervalo temporal | $t \in [0, 20]$ min |
| Semilla | $2026$ (PCG64, NumPy) |

El notebook `Actividad2.ipynb` compara la 1-FDP obtenida por RVT con tres aproximaciones independientes:

1. **Normalización y momentos:** integra $f_1(x,t)$ y sus dos primeros momentos por cuadratura adaptativa y los compara con $\mu_X(t) = b + (\mu_0 - b)e^{-at}$ y $\sigma_X^2(t) = \sigma_0^2 e^{-2at}$.
2. **Liouville-Gibbs por características:** integra con Runge-Kutta de orden 4 las ecuaciones $dx/dt = a(b-x)$ y $d(\ln f_1)/dt = a$ desde 2001 puntos iniciales.
3. **Liouville-Gibbs como EDP:** resuelve $\partial_t f_1 + \partial_x[a(b-x)f_1] = 0$ con volúmenes finitos MUSCL (limitador de van Leer) y Runge-Kutta SSP de orden 2, y estudia la convergencia al refinar la malla.
4. **Montecarlo:** genera $M = 10^5$ realizaciones, compara media y varianza muestrales con las exactas y aplica el contraste de Kolmogorov-Smirnov.

## Resultados principales

- La 1-FDP integra 1 con diez cifras decimales y sus momentos coinciden con los exactos.
- Características con RK4: error relativo máximo de $5.4 \times 10^{-12}$ (nivel de redondeo).
- Volúmenes finitos: masa conservada y orden de convergencia $L^1$ que tiende a 2.
- Montecarlo: $D_M = 0.0016 < 0.0043$ (valor crítico al 5 %), p-valor $0.95$.

## Figuras generadas

| Archivo | Contenido |
|---|---|
| `fig1_densidades.png` | 1-FDP en varios instantes para $a = 0.1$ y $a = -0.1$ |
| `fig2_mapa_caracteristicas.png` | Mapa de $f_1(x,t)$ con curvas características |
| `fig3_comparacion.png` | RVT, Liouville-Gibbs (volúmenes finitos) e histograma de Montecarlo |
| `fig4_convergencia_vf.png` | Convergencia del esquema de volúmenes finitos |

## Ejecución

Requiere Python 3 con NumPy, SciPy y Matplotlib. La forma más sencilla es abrir
`Actividad2.ipynb` en Google Colab y ejecutar todas las celdas
(*Entorno de ejecución → Ejecutar todas*).

Para ejecutarlo localmente:

```bash
pip install numpy scipy matplotlib notebook
jupyter notebook Actividad2.ipynb
```

El tiempo de ejecución es de aproximadamente un minuto. Las tablas se muestran
en la salida de las celdas y las figuras se guardan en el directorio de trabajo.
El tiempo de ejecución es de aproximadamente un minuto. Las tablas se muestran por consola y las figuras se guardan en el directorio de trabajo.

## Autor

Gonzalo Duarte
