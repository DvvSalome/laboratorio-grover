# Laboratorio Grover

Web para celular que funciona como la vista en vivo del IBM Quantum Composer: circuito de 2 o 3 qubits, Grover con 1 a 3 iteraciones, Q-sphere, Bloch, disco de fase, statevector, Inspect paso a paso, medición con ruido y código Qiskit/OpenQASM sincronizado. Además corre Python de verdad con Pyodide.
Un solo archivo (`index.html`), sin dependencias ni build. Solo carga las fuentes IBM Plex desde Google Fonts.

## Recorrido guiado, paso a paso
La pestaña **Probar** ya no muestra todo de una vez: abre en un recorrido con una tarjeta por paso (Inicio, Superposición, Oráculo, Difusión, Medir — o más vueltas con 3 qubits), con **Atrás / Siguiente / Reproducir** (autoplay), una franja de progreso arriba y un texto de "Qué mirar" que dice qué fijarse en el resultado o el circuito. El circuito se resalta solo en la columna del paso actual, con una animación suave entre estados. Tocar el circuito o pegar/editar código a mano sale del recorrido a "Exploración libre". El selector de qubits/caja/iteraciones quedó en un panel plegable aparte para no estorbar.
La pestaña **Recursos** incluye ahora también el enlace a las diapositivas de la charla.

## Publicarla (GitHub Pages)
1. Crea un repositorio nuevo en GitHub (por ejemplo `laboratorio-grover`).
2. Sube `index.html` a la raíz.
3. Settings > Pages > Branch: `main` / carpeta `/ (root)` > Save.
4. En un minuto queda en `https://DvvSalome.github.io/laboratorio-grover/`. Con esa URL generas el QR de la diapositiva final.

## Probarla en local
Abre `index.html` en el navegador, o `python3 -m http.server` en esta carpeta.

## Python de verdad (Pyodide)
El botón **Ejecutar Python** descarga Pyodide (Python compilado a WebAssembly, unos 10 MB la primera vez) desde jsDelivr y corre el código completo en el celular: variables, bucles, `print`, errores con número de línea.

El Qiskit oficial no funciona en el navegador, así que la página trae un **mini-Qiskit** escrito en Python (`qiskit` y `qiskit_aer` dentro del mismo `index.html`) con:
- `QuantumCircuit`: h, x, y, z, s, sdg, t, tdg, sx, rx, ry, rz, p, cx, cz, cp, swap, ccx, measure, measure_all, barrier, draw, depth, count_ops. Hasta 12 qubits.
- `AerSimulator().run(qc, shots=...).result().get_counts()`, `Aer.get_backend(...)`, `transpile` (no cambia el circuito).
- `qiskit.quantum_info.Statevector` y `qiskit.visualization.plot_histogram` (en texto).

Simula el vector de estado exacto. Lo comparé con Qiskit 2.6 en 40 circuitos aleatorios y dio igual en todos. No simula ruido de un chip real.

Si el circuito es de 2 o 3 qubits y usa h, x, y, z, s, sdg, t, tdg, cx, cz, swap, ccx o ccz, se dibuja en el lienzo y los conteos aparecen en las barras. El lienzo también lee OpenQASM 2.

## Verificación
- Simulador de la página (JavaScript, amplitudes complejas): igual a Qiskit 2.6 en 60 circuitos aleatorios de 2 y 3 qubits.
- Mini-Qiskit (Python): igual a Qiskit 2.6 en 80 circuitos aleatorios de 1 a 4 qubits.
- Grover 3 qubits: 78,1 % (1 iteración), 94,5 % (2), 33,0 % (3).
El ruido de chip es un modelo simple, no una medida de un procesador real.
