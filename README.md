# Salmones Analytics – Power BI + SQL Server + Python (DirectQuery)

> Análisis operativo de una salmonera con
> **ingesta Python → SQL Server** y visualización en **Power BI** usando **DirectQuery**
> para reflejar datos **al instante**.

[🎥 Demo corta (2–3 min)](TODO-link-video)

---

## 1) Elevator pitch

Este proyecto transforma datos crudos de producción y exportaciones en **decisiones accionables**:
- Monitoreo de **Kilos/HH y Kilos por empleado** por **turno y planta**.
- Seguimiento de **ventas por país/cliente** y **precio por kg**.
- **Match** entre stock y exportaciones, con detección de **brechas** por producto.
- **DirectQuery** + **Python** para ingesta incremental: los nuevos registros quedan visibles en minutos.

---

## 2) Arquitectura

Python (pandas) ──► SQL Server (tablas + vistas) ──► Power BI (DirectQuery)
▲ 
└────────── validaciones y carga ───────────┘

- **Fuente**: SQL Server (vistas normalizadas por tema).
- **Ingesta**: script/notebook Python para carga incremental.
- **Modelo**: star-like con dimensiones (`DimMes`, `DimCalendario`, `DimPlanta`, `DimCliente`, `DimProductos`).
- **Conexión**: Power BI en **DirectQuery** (near real-time).
- **Despliegue**: PBIX/PBIP publicado en Power BI Service (opcional).

---

## 3) Dashboards (qué preguntas responden)

### 3.1 Resumen operativo
- KPIs rápidos: **Total kilos**, **Costo promedio por kg**, **Kilos exportados**, **Empleados activos**.
- **Kilos por turno** (torta) y **kilos por planta** (barra).
- **Kilos y costo/kg por mes** (combo).
> **Insights ejemplo:**  
> • El **turno Noche** concentra el mayor % de producción.  
> • La planta **TODO-planta** aporta ~TODO% del total.  
> • Estacionalidad con pico en **TODO-mes**.

### 3.2 Exportaciones (País & Cliente)
- Ventas CLP y kilos exportados por país.
- **Top 25 clientes** por ventas y por kilos.
- Línea temporal **kilos exportados** + **precio/kg**.
> **Insights ejemplo:** concentración en **Top 5 países** (~TODO%), variación de **$ por kg** en Q3.

### 3.3 Productos: stock / exportaciones
- Tabla de **brecha** (stock que no “matchea” exportación).
- **% de productos con match** y **stock asociado a exportaciones**.
- **Mapa**: exportaciones por país.
> **Insights ejemplo:** la brecha se concentra en *TODO_producto* (priorizar clearing / estrategia).

### 3.4 Productividad RR.HH.
- **Kilos/HH**, **Kilos por empleado**, **Turno ganador**.
- Matriz **kilos por turno vs día de semana**.
- **Top 15 empleados** y **resumen** con % de contribución.
- Serie mensual con **Kilos sin mes (Indefinido)** monitoreada.
> **Insights ejemplo:** 15 empleados aportan ~TODO% del total; oportunidad de rotación/mentoring.

---

## 4) KPIs clave

- **Kilos/HH**: `DIVIDE([Kilos], [HH (horas)])`
- **Kilos por empleado**: `DIVIDE([Kilos], DISTINCTCOUNT('v_empleado_kilos'[ID_empleado]))`
- **Turno ganador (etiqueta)**: `SELECTEDVALUE( TOPN(1, VALUES('v_empleado_kilos'[Turno]), [Kilos], DESC), "—" )`
- **% productos con match**: `DIVIDE([Productos con match], [Productos totales])`
- **Precio por kg (exportaciones)**: `DIVIDE([Ventas CLP], [Kilos exportados])`

> Nota: los **nulos** de fecha/mes se normalizan a **“Indefinido”** para trazabilidad.

---

## 5) Stack usado en el proyecto

Power BI (DirectQuery, DAX, drill-through, tooltips, bookmarks).

SQL Server (vistas, joins, normalización).

Python (pandas, pyodbc/msal/sqlalchemy según setup).

ODBC Driver 18 for SQL Server (sirve para insertar datos de Jupyter a Sql server)

GitHub (código, documentación, versionado).

---

<img width="1990" height="1123" alt="Image" src="https://github.com/user-attachments/assets/09b5d912-ea19-49ea-908d-4a9fd042e2a5" />
<img width="1560" height="1004" alt="Image" src="https://github.com/user-attachments/assets/4a293b85-a1bc-42e6-b70e-eaa123d424b1" />
<img width="2400" height="1454" alt="Image" src="https://github.com/user-attachments/assets/5d50d051-8f02-416c-b5c8-d4d080dddd4e" />
