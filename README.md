# Legacy
Modulo6_Eejercicio_1
## 1. Orden y Secuencia de Transformaciones Realizadas
1. **Extracción y Carga Inicial:** Conexión desde la fuente legacy e ingreso directo a Power Query.
2. **Depuración de Duplicados:** Eliminación de registros repetidos utilizando la clave primaria de transacción.
3. **Tratamiento de Nulos:** Filtrado de registros incompletos en importes de ventas y reemplazo de valores faltantes en campos descriptivos.
4. **Estandarización de Encabezados:** Renombrado de nombres técnicos a estándar `snake_case`.
5. **Tipificación de Variables:** Asignación explícita de tipos de datos por columna.
6. **Normalización (Modelo Estrella):** Separación del tablón único en una tabla de hechos (`FactVentas`) y una tabla de dimensión (`DimCliente`).

## 2. Justificación Técnica de Tipos de Datos
* **`id_transaccion` / `id_cliente` (Texto):** Aunque contengan números, los identificadores no representan cantidades operables matemáticamente. Definirlos como texto previene agregaciones accidentales (sumas, promedios) y optimiza el filtrado.
* **`fecha_venta` (Fecha):** Esencial para habilitar inteligencia de tiempo (Time Intelligence en DAX) y la conexión futura con una tabla calendario dedicada.
* **`La columna TOT_VENT original presentaba inconsistencias debido a la presencia de valores nulos provenientes del sistema legacy. Para resolverlo, se generó una nueva columna calculada multiplicando la cantidad vendida por el precio unitario (aplicando los descuentos correspondientes), garantizando que el 100% de los registros cuente con un importe correcto y sin vacíos. Se le asignó el tipo de dato Número decimal fijo (Moneda) para evitar errores de redondeo por coma flotante y asegurar precisión matemática exacta en la consolidación financiera del reporte.
---

## 3. Estrategia de Gestión de Nulos y Duplicados
* **Duplicados:** Se aplicó eliminación de duplicados en la clave única de transacción. Un registro duplicado altera métricas de volumen transaccional e ingresos reales.
* **Nulos en Montos:** Se duplico la columna TOT_VENT, se cambiò el nombre por TOTAL_VENTA, se genero una columna nueva calculada, cantidad_vendida * precio_unitario - descuento_porcentual. 
* **Nulos en Atributos Secundarios:** En campos como la email_cliente y telefono_cleinte, los valores nulos se imputaron con el texto `"Sin telefono"`"Sin eamil", preservando la integridad conceptual sin perder el registro de la transacción.

---

## 4. Criterio de Normalización
Se aplicaron los principios de normalización de modelos relacionales (1NF / 2NF):
* **`DimCliente`:** Contiene la información propia de la entidad cliente.
  CODIGO_CLIENTE	NOMBRE_CLIENTE	MAIL_CLIENTE	TELEFONO_CLIENTE	CIUDAD_CLIENTE	PROVINCIA_CLIENTE	SEGURO_CLIENTE	ESTADO_CLIENTE	FECHA_ALTA_CLIENTE
COD_CLI_005	Lucas Ortiz	lucas.ortiz5@mail.com	11-6261-5803	Mendoza	Mendoza	CORPORATIVO	S	10/1/2021
COD_CLI_082	Santiago Pereyra  	santiago.pereyra82@mail.com	sin telefono	Salta	Salta	MINORISTA	S	25/6/2020
COD_CLI_091	Franco Ruiz	sin email	sin telefono	Posadas	Misiones	MAYORISTA	S	2/5/2023
COD_CLI_051	Ignacio Gimenez	ignacio.gimenez51@mail.com	11-6357-4185	Posadas	Misiones	CORPORATIVO	S	9/10/2021
COD_CLI_095	Sofia Sanchez	sofia.sanchez95@mail.com	sin telefono	Posadas	Misiones	MINORISTA	S	7/1/2023
COD_CLI_094	Ignacio Lopez	ignacio.lopez94@mail.com	11-4648-6091	Buenos Aires	CABA	CORPORATIVO	S	15/4/2021
COD_CLI_077	Pilar Perez	pilar.perez77@mail.com	sin telefono	Santa Fe	Santa Fe	CORPORATIVO	S	4/4/2019
COD_CLI_103	Ramiro Silva	ramiro.silva103@mail.com	11-5998-9864	Posadas	Misiones	MINORISTA	S	25/4/2020
COD_CLI_058	Delfina Ferrari	delfina.ferrari58@mail.com	11-6358-7232	Bariloche	Rio Negro	MAYORISTA	S	21/12/2020
COD_CLI_017	Josefina Pereyra  	josefina.pereyra17@mail.com	sin telefono	La Plata	Buenos Aires	CORPORATIVO	S	20/9/2019
COD_CLI_114	Benjamin Perez	benjamin.perez114@mail.com	11-6055-5465	Mar del Plata	Buenos Aires	MINORISTA	S	16/8/2023
COD_CLI_013	Renata Romero	sin email	11-6792-9797	San Rafael	Mendoza	MAYORISTA	S	27/11/2020
COD_CLI_032	Mateo Rodriguez	mateo.rodriguez32@mail.com	11-4281-5002	San Rafael	Mendoza	MAYORISTA	S	16/6/2021
COD_CLI_101	Renata Pereyra	renata.pereyra101@mail.com	11-6154-8486	Cordoba	Cordoba	MINORISTA	S	19/4/2021
COD_CLI_034	Gonzalo Silva	gonzalo.silva34@mail.com	sin telefono	La Plata	Buenos Aires	MINORISTA	S	15/11/2019
COD_CLI_012	Ramiro Lopez	ramiro.lopez12@mail.com	11-4260-7304	Mar del Plata	Buenos Aires	MAYORISTA	S	16/8/2021
COD_CLI_031	Agustina Sosa	agustina.sosa31@mail.com	sin telefono	Mendoza	Mendoza	MAYORISTA	S	27/2/2022
COD_CLI_046	Gonzalo Torres	gonzalo.torres46@mail.com	11-5230-9308	Salta	Salta	MAYORISTA	S	30/10/2020
COD_CLI_052	Delfina Sanchez	sin email	11-4436-7965	Mar del Plata	Buenos Aires	MINORISTA	S	26/11/2022
COD_CLI_057	Lucia Martinez	lucia.martinez57@mail.com	11-5908-7812	Neuquen	Neuquen	MINORISTA	S	10/5/2021
COD_CLI_043	Pilar Aguirre	pilar.aguirre43@mail.com	11-4731-5349	Cordoba	Cordoba	MINORISTA	S	8/6/2021
COD_CLI_086	Lucas Diaz	lucas.diaz86@mail.com	11-5182-8195	Bariloche	Rio Negro	MINORISTA	S	14/9/2020
COD_CLI_097	Gonzalo Gimenez	gonzalo.gimenez97@mail.com	sin telefono	San Rafael	Mendoza	MAYORISTA	S	2/6/2023
COD_CLI_028	Lucas Pereyra	lucas.pereyra28@mail.com	11-6080-2312	Mendoza	Mendoza	MINORISTA	S	20/5/2019
COD_CLI_076	Malena Castro	malena.castro76@mail.com	11-6414-9595	Mar del Plata	Buenos Aires	CORPORATIVO	S	25/7/2019
COD_CLI_002	Sofia Pereyra  	sin email	11-6465-1434	Villa Maria	Cordoba	CORPORATIVO	S	23/8/2022
COD_CLI_079	Ignacio Acosta	sin email	11-5956-2869	Cordoba	Cordoba	MINORISTA	S	1/6/2019
COD_CLI_066	Isabella Gomez	isabella.gomez66@mail.com	11-5624-4249	Parana	Entre Rios	MINORISTA	S	8/7/2022
COD_CLI_115	Ignacio Nunez	ignacio.nunez115@mail.com	sin telefono	Santa Fe	Santa Fe	MINORISTA	S	7/4/2020
COD_CLI_118	Franco Sanchez  	franco.sanchez118@mail.com	11-6498-4626	Mendoza	Mendoza	CORPORATIVO	S	15/8/2023
COD_CLI_049	Camila Herrera	camila.herrera49@mail.com	11-6698-2389	Mendoza	Mendoza	MAYORISTA	S	20/7/2022
COD_CLI_026	Benjamin Ruiz	benjamin.ruiz26@mail.com	11-5863-5673	Buenos Aires	CABA	MAYORISTA	S	5/2/2023
COD_CLI_021	Sofia Herrera	sofia.herrera21@mail.com	11-6962-8744	Rosario	Santa Fe	MINORISTA	S	12/7/2023
COD_CLI_089	Lucas Rojas	lucas.rojas89@mail.com	11-5139-3973	Bariloche	Rio Negro	CORPORATIVO	S	26/7/2023
COD_CLI_087	Lucia Ruiz	lucia.ruiz87@mail.com	sin telefono	Santa Fe	Santa Fe	MINORISTA	N	13/2/2023
COD_CLI_112	Agustina Perez	agustina.perez112@mail.com	11-6788-3240	Salta	Salta	CORPORATIVO	S	9/7/2022
COD_CLI_039	Mateo Alvarez	mateo.alvarez39@mail.com	11-4633-4878	Bariloche	Rio Negro	MINORISTA	N	19/7/2023
COD_CLI_072	Joaquin Nunez	joaquin.nunez72@mail.com	11-5131-1166	Santa Fe	Santa Fe	CORPORATIVO	N	25/6/2019
COD_CLI_007	Agustina Cabrera	agustina.cabrera7@mail.com	11-6874-2169	Cordoba	Cordoba	CORPORATIVO	S	29/12/2021
COD_CLI_011	Julieta Ortiz	julieta.ortiz11@mail.com	11-4566-9348	Salta	Salta	MAYORISTA	S	7/4/2019
COD_CLI_038	Tomas Martinez	tomas.martinez38@mail.com	11-4163-6862	Villa Maria	Cordoba	MINORISTA	S	27/9/2022
COD_CLI_067	Thiago Lopez	thiago.lopez67@mail.com	11-4495-1672	Neuquen	Neuquen	MAYORISTA	S	16/9/2022
COD_CLI_084	Lucas Sanchez	lucas.sanchez84@mail.com	11-6815-4824	Santa Fe	Santa Fe	MAYORISTA	S	4/7/2022
COD_CLI_107	Nicolas Silva	nicolas.silva107@mail.com	11-6612-5161	Rosario	Santa Fe	MAYORISTA	S	20/1/2019
COD_CLI_050	Nicolas Torres	nicolas.torres50@mail.com	11-4920-4262	La Plata	Buenos Aires	MINORISTA	S	16/5/2020
COD_CLI_071	Malena Acosta	malena.acosta71@mail.com	sin telefono	Salta	Salta	MAYORISTA	S	10/3/2020
COD_CLI_099	Joaquin Torres	joaquin.torres99@mail.com	11-4772-4912	San Rafael	Mendoza	CORPORATIVO	S	8/5/2023
COD_CLI_016	Malena Peralta	malena.peralta16@mail.com	11-4458-6947	San Rafael	Mendoza	MAYORISTA	S	8/5/2020
COD_CLI_073	Thiago Ortiz	thiago.ortiz73@mail.com	11-6645-9041	Villa Maria	Cordoba	MAYORISTA	S	10/7/2019
COD_CLI_024	Valentina Benitez	valentina.benitez24@mail.com	11-5815-2604	Mar del Plata	Buenos Aires	MINORISTA	S	12/1/2022
COD_CLI_090	Ignacio Rodriguez	ignacio.rodriguez90@mail.com	11-4658-6403	Mendoza	Mendoza	MAYORISTA	S	13/8/2020
COD_CLI_001	Agustina Lopez	agustina.lopez1@mail.com	sin telefono	Buenos Aires	CABA	MINORISTA	S	21/1/2022
COD_CLI_063	Ignacio Gomez	ignacio.gomez63@mail.com	11-6662-2124	Neuquen	Neuquen	CORPORATIVO	S	5/3/2019
COD_CLI_085	Isabella Silva	isabella.silva85@mail.com	11-6652-3441	Bariloche	Rio Negro	MAYORISTA	S	27/5/2019
COD_CLI_029	Franco Vega	franco.vega29@mail.com	11-6371-1651	Rosario	Santa Fe	CORPORATIVO	S	8/9/2022
COD_CLI_004	Valentina Perez	sin email	sin telefono	Bariloche	Rio Negro	MAYORISTA	S	11/7/2023
COD_CLI_036	Sofia Molina	sin email	11-4535-5291	Villa Maria	Cordoba	MINORISTA	S	3/2/2022
COD_CLI_030	Isabella Pereyra	isabella.pereyra30@mail.com	11-5286-4910	San Rafael	Mendoza	MAYORISTA	S	7/10/2022
COD_CLI_116	Ramiro Silva	ramiro.silva116@mail.com	11-6282-6992	Neuquen	Neuquen	MINORISTA	S	29/1/2019
COD_CLI_027	Federico Aguirre	federico.aguirre27@mail.com	11-4239-9883	Tucuman	Tucuman	MINORISTA	S	28/4/2019
COD_CLI_033	Pilar Aguirre	pilar.aguirre33@mail.com	11-6166-1128	Neuquen	Neuquen	CORPORATIVO	S	5/9/2020
COD_CLI_108	Josefina Nunez	josefina.nunez108@mail.com	11-5809-6661	Mendoza	Mendoza	CORPORATIVO	S	18/5/2021
COD_CLI_041	Mateo Ruiz  	mateo.ruiz41@mail.com	11-5885-6728	Posadas	Misiones	MAYORISTA	S	11/4/2020
COD_CLI_056	Julieta Acosta	julieta.acosta56@mail.com	11-6212-2320	La Plata	Buenos Aires	MINORISTA	S	23/2/2021
COD_CLI_018	Franco Medina	franco.medina18@mail.com	11-4867-9835	San Rafael	Mendoza	CORPORATIVO	S	31/12/2022
COD_CLI_003	Lucia Aguirre	lucia.aguirre3@mail.com	11-4026-3615	Villa Maria	Cordoba	CORPORATIVO	S	23/7/2020
COD_CLI_092	Delfina Vega	sin email	11-5892-7770	San Rafael	Mendoza	MINORISTA	S	10/1/2021
COD_CLI_110	Isabella Sosa	isabella.sosa110@mail.com	11-5124-1132	Neuquen	Neuquen	CORPORATIVO	N	13/5/2023
COD_CLI_060	Ignacio Gomez	ignacio.gomez60@mail.com	11-4676-8657	Villa Maria	Cordoba	MINORISTA	N	29/12/2021
COD_CLI_109	Lucia Romero	lucia.romero109@mail.com	11-5979-2747	Mendoza	Mendoza	MINORISTA	S	5/1/2021
COD_CLI_100	Bautista Fernandez	bautista.fernandez100@mail.com	11-5581-3492	Cordoba	Cordoba	MAYORISTA	N	16/9/2019
COD_CLI_074	Santiago Sanchez	santiago.sanchez74@mail.com	11-5938-9698	Villa Maria	Cordoba	MAYORISTA	S	8/3/2023
COD_CLI_015	Gonzalo Lopez	gonzalo.lopez15@mail.com	11-4814-3504	Posadas	Misiones	MAYORISTA	S	9/1/2022
COD_CLI_040	Benjamin Ferrari  	benjamin.ferrari40@mail.com	11-5686-5065	Villa Maria	Cordoba	MAYORISTA	S	7/12/2022
COD_CLI_023	Mateo Fernandez	mateo.fernandez23@mail.com	sin telefono	Villa Maria	Cordoba	MINORISTA	S	7/7/2021
COD_CLI_083	Franco Rodriguez	franco.rodriguez83@mail.com	11-5673-8381	Mar del Plata	Buenos Aires	CORPORATIVO	S	19/8/2020
COD_CLI_054	Renata Aguirre	renata.aguirre54@mail.com	11-6946-9270	Salta	Salta	MAYORISTA	S	27/1/2022
COD_CLI_068	Joaquin Rodriguez	sin email	11-6007-2729	San Rafael	Mendoza	MAYORISTA	N	25/7/2022
COD_CLI_080	Isabella Molina  	isabella.molina80@mail.com	11-5244-2395	Tucuman	Tucuman	MINORISTA	S	15/4/2023
COD_CLI_061	Martina Peralta	martina.peralta61@mail.com	sin telefono	Villa Maria	Cordoba	MAYORISTA	S	3/8/2021
COD_CLI_037	Lucia Benitez  	sin email	11-4610-9938	San Rafael	Mendoza	MINORISTA	S	7/4/2022
COD_CLI_111	Bruno Fernandez	bruno.fernandez111@mail.com	11-5172-4770	Parana	Entre Rios	CORPORATIVO	S	24/3/2020
COD_CLI_104	Agustina Silva	agustina.silva104@mail.com	sin telefono	Parana	Entre Rios	MINORISTA	S	12/12/2022
COD_CLI_106	Joaquin Alvarez	joaquin.alvarez106@mail.com	11-4737-3785	Villa Maria	Cordoba	MINORISTA	S	22/2/2021
COD_CLI_120	Mateo Lopez	mateo.lopez120@mail.com	11-6746-9850	Rosario	Santa Fe	MAYORISTA	S	28/1/2023
COD_CLI_045	Martina Herrera	martina.herrera45@mail.com	11-6746-4228	Parana	Entre Rios	MAYORISTA	S	22/9/2022
COD_CLI_069	Josefina Cabrera	josefina.cabrera69@mail.com	11-6664-5425	Mendoza	Mendoza	CORPORATIVO	S	7/1/2022
COD_CLI_022	Bautista Perez  	bautista.perez22@mail.com	11-5912-1887	La Plata	Buenos Aires	CORPORATIVO	S	16/8/2022
COD_CLI_020	Isabella Aguirre	sin email	11-4241-4750	Rosario	Santa Fe	MINORISTA	N	7/11/2020
COD_CLI_081	Bautista Gimenez	bautista.gimenez81@mail.com	11-5845-8253	Salta	Salta	MAYORISTA	N	28/5/2021
COD_CLI_119	Catalina Peralta	catalina.peralta119@mail.com	sin telefono	Neuquen	Neuquen	MINORISTA	S	5/10/2021
COD_CLI_098	Renata Romero  	renata.romero98@mail.com	11-6375-9022	Posadas	Misiones	MINORISTA	S	18/9/2021
COD_CLI_006	Isabella Perez  	isabella.perez6@mail.com	11-4326-4814	Neuquen	Neuquen	MINORISTA	S	17/7/2021
COD_CLI_065	Catalina Castro	catalina.castro65@mail.com	11-6942-2876	Santa Fe	Santa Fe	MINORISTA	N	10/8/2019
COD_CLI_047	Delfina Castro	delfina.castro47@mail.com	11-5552-3851	Santa Fe	Santa Fe	CORPORATIVO	S	11/4/2021
COD_CLI_010	Camila Molina	camila.molina10@mail.com	11-5084-3287	Mendoza	Mendoza	MINORISTA	S	8/1/2022
* **`FactVentas`:** Registra las métricas numéricas y claves de relación (`codigo_operacion`, `fecha_venta`, `codigo_cliente`, `total_venta`).
CODIGO_OPERATIVO	CODIGO_CLIENTE	FECHA_VENTA	CODIGO_PROCUTO	CANTIDAD_VENDIDA	PRECIO_UNITARIO	DESCUENTO_PORCENTUAL	CODIGO_MONEDA	CANAL_VENTA	TOTAL_VENTA
OP-100820	COD_CLI_005	15/10/2024	848	2	45,91	0	ARS	Sucursal	91,82
OP-100296	COD_CLI_082	25/7/2024	948	6	108,33	0	ARS	ONLINE	649,98
OP-100349	COD_CLI_091	29/10/2023	893	4	1008,59	0,05	ARS	Sucursal	3832,642
OP-100155	COD_CLI_051	7/7/2024	744	1	1358,23	0,05	ARS	online	1290,3185
OP-100416	COD_CLI_095	25/9/2024	807	1	121,51	0	ARS	online	121,51
OP-100323	COD_CLI_094	1/11/2024	771	2	3395,95	0,15	ARS	online	5773,115
OP-100830	COD_CLI_077	4/2/2023	929	4	30,37	0,05	ARS	Sucursal	115,406
OP-100754	COD_CLI_103	12/7/2023	859	1	23,6	0	ARS	TELEFONICO	23,6
OP-100573	COD_CLI_058	3/2/2023	894	5	118,56	0,05	ARS	online	563,16
OP-100717	COD_CLI_017	4/12/2024	915	4	38,43	0	ARS	SUCURSAL	153,72
OP-100475	COD_CLI_114	21/3/2023	832	1	359,61	0	ARS	Telefonico	359,61
OP-100371	COD_CLI_013	10/11/2023	850	3	62,39	0	ARS	Telefonico	187,17
OP-100101	COD_CLI_032	1/8/2023	847	2	34,03	0,2	ARS	Telefonico	54,448
OP-100268	COD_CLI_101	15/1/2024	908	2	28,19	0	ARS	Sucursal	56,38
OP-100814	COD_CLI_034	3/12/2023	936	1	63,1	0	ARS	Online	63,1
OP-100694	COD_CLI_012	22/12/2023	753	3	3497,17	0,05	ARS	TELEFONICO	9966,9345
OP-100854	COD_CLI_031	18/9/2023	907	1	105,7	0	ARS	SUCURSAL	105,7
OP-100837	COD_CLI_046	26/10/2023	864	3	63,76	0	ARS	online	191,28
OP-100778	COD_CLI_052	24/6/2024	988	6	577,51	0	ARS	SUCURSAL	3465,06
OP-100385	COD_CLI_057	8/9/2024	789	1	2446,9	0	ARS	Telefonico	2446,9
OP-100521	COD_CLI_043	9/12/2024	749	1	3548,55	0	ARS	Telefonico	3548,55
OP-100344	COD_CLI_086	5/3/2023	733	1	609,71	0	ARS	Telefonico	609,71
OP-100058	COD_CLI_097	2/12/2024	875	1	8,86	0	ARS	Sucursal	8,86
OP-100153	COD_CLI_028	31/7/2024	995	2	96,65	0	ARS	Telefonico	193,3
OP-100023	COD_CLI_076	29/7/2023	954	2	2338,4	0	ARS	online	4676,8
OP-100669	COD_CLI_002	12/4/2023	975	1	1648,52	0	ARS	ONLINE	1648,52
OP-100599	COD_CLI_079	22/9/2024	792	5	2336,81	0,1	ARS	ONLINE	10515,645
OP-100378	COD_CLI_066	20/9/2024	751	2	3579,05	0,05	ARS	Telefonico	6800,195
OP-100613	COD_CLI_115	16/9/2024	817	1	302,77	0	ARS	Sucursal	302,77
OP-100779	COD_CLI_118	29/10/2024	780	3	2307,51	0	ARS	Sucursal	6922,53
OP-100577	COD_CLI_049	4/4/2024	904	2	351,17	0	ARS	Sucursal	702,34
OP-100047	COD_CLI_026	29/12/2024	775	6	3223,85	0	ARS	TELEFONICO	19343,1
OP-100611	COD_CLI_021	30/11/2023	719	2	1376,54	0,15	ARS	Sucursal	2340,118
OP-100594	COD_CLI_089	26/1/2023	888	2	1042,58	0	ARS	Telefonico	2085,16
OP-100127	COD_CLI_087	13/7/2023	983	2	760,2	0	ARS	Telefonico	1520,4
OP-100125	COD_CLI_112	23/12/2023	898	2	339,51	0,05	ARS	Online	645,069
OP-100272	COD_CLI_039	8/8/2024	745	2	1397,37	0	ARS	Telefonico	2794,74
OP-100561	COD_CLI_072	9/5/2023	901	5	328,76	0	ARS	ONLINE	1643,8
OP-100085	COD_CLI_007	2/12/2023	853	10	77,72	0	ARS	SUCURSAL	777,2
OP-100434	COD_CLI_011	27/1/2023	812	1	59,46	0	ARS	TELEFONICO	59,46
OP-100358	COD_CLI_038	30/6/2024	803	1	168,16	0	ARS	Online	168,16
OP-100132	COD_CLI_067	29/6/2023	818	2	83,64	0,05	ARS	Telefonico	158,916
OP-100288	COD_CLI_084	16/5/2023	914	2	28,11	0,2	ARS	Telefonico	44,976
OP-100661	COD_CLI_107	20/3/2023	974	5	1737,14	0,05	ARS	Online	8251,415
OP-100212	COD_CLI_050	5/1/2024	762	1	793,8	0	ARS	online	793,8
OP-100422	COD_CLI_071	4/8/2023	839	1	1427,4	0,05	ARS	Sucursal	1356,03
OP-100818	COD_CLI_099	15/10/2023	709	1	9,38	0	ARS	Telefonico	9,38
OP-100139	COD_CLI_016	26/11/2023	950	6	261,51	0,05	ARS	TELEFONICO	1490,607
OP-100744	COD_CLI_073	6/4/2023	921	1	4,95	0	ARS	Online	4,95
OP-100460	COD_CLI_024	5/7/2024	714	3	49,31	0,05	ARS	TELEFONICO	140,5335
OP-100720	COD_CLI_090	24/6/2023	791	1	2472,18	0,05	ARS	SUCURSAL	2348,571
OP-100260	COD_CLI_001	23/1/2024	729	2	345,06	0,2	ARS	online	552,096
OP-100426	COD_CLI_063	9/8/2023	858	1	25,37	0	ARS	TELEFONICO	25,37
OP-100575	COD_CLI_085	28/9/2023	930	4	33,84	0,05	ARS	TELEFONICO	128,592
OP-100248	COD_CLI_029	30/6/2023	999	5	556,05	0	ARS	ONLINE	2780,25
OP-100246	COD_CLI_004	28/4/2024	868	3	66,7	0,1	ARS	SUCURSAL	180,09
OP-100798	COD_CLI_036	28/7/2024	750	5	3652,41	0,05	ARS	Telefonico	17348,9475
OP-100436	COD_CLI_030	5/2/2023	831	4	346,78	0	ARS	Telefonico	1387,12
OP-100457	COD_CLI_116	11/7/2023	725	2	335,11	0	ARS	SUCURSAL	670,22
OP-100099	COD_CLI_027	29/6/2024	992	1	548,75	0	ARS	online	548,75
OP-100513	COD_CLI_033	27/4/2023	870	3	5,19	0,1	ARS	Telefonico	14,013
OP-100054	COD_CLI_108	15/3/2024	960	3	706,45	0	ARS	Online	2119,35
OP-100178	COD_CLI_041	24/11/2024	739	1	1406,82	0	ARS	TELEFONICO	1406,82
OP-100733	COD_CLI_056	11/5/2024	764	12	816,44	0	ARS	Sucursal	9797,28
OP-100563	COD_CLI_018	10/4/2024	917	5	274,44	0,05	ARS	SUCURSAL	1303,59
OP-100026	COD_CLI_003	3/3/2024	940	10	83,92	0,05	ARS	online	797,24
OP-100215	COD_CLI_092	6/6/2023	961	4	736,68	0	ARS	online	2946,72
OP-100135	COD_CLI_110	18/5/2023	778	5	3514,96	0,05	ARS	Sucursal	16696,06
OP-100672	COD_CLI_060	26/3/2023	761	2	782,66	0	ARS	online	1565,32
OP-100693	COD_CLI_109	1/10/2024	979	1	724,92	0	ARS	Telefonico	724,92
OP-100502	COD_CLI_100	3/8/2024	801	1	1085,17	0,1	ARS	SUCURSAL	976,653
OP-100081	COD_CLI_074	4/10/2024	840	3	1469,72	0	ARS	SUCURSAL	4409,16
OP-100462	COD_CLI_015	26/7/2023	754	2	1394,81	0,1	ARS	Online	2510,658
OP-100709	COD_CLI_040	25/2/2023	715	3	52,02	0	ARS	Telefonico	156,06
OP-100031	COD_CLI_023	20/10/2024	957	2	2399,26	0,05	ARS	Telefonico	4558,594
OP-100144	COD_CLI_083	8/7/2023	956	2	2439,56	0	ARS	SUCURSAL	4879,12
OP-100750	COD_CLI_054	16/12/2023	857	1	92,01	0,1	ARS	TELEFONICO	82,809
OP-100870	COD_CLI_068	5/2/2024	740	2	1346,58	0	ARS	ONLINE	2693,16
OP-100637	COD_CLI_080	13/11/2024	787	4	1056,8	0,2	ARS	TELEFONICO	3381,76
OP-100300	COD_CLI_061	4/6/2023	804	2	237,92	0	ARS	ONLINE	475,84
OP-100108	COD_CLI_037	8/2/2023	735	6	570,41	0,05	ARS	ONLINE	3251,337
OP-100767	COD_CLI_111	18/8/2023	962	1	709,96	0	ARS	Sucursal	709,96
OP-100495	COD_CLI_104	22/7/2024	736	3	349,99	0,15	ARS	online	892,4745
OP-100700	COD_CLI_106	16/7/2023	851	3	58,53	0,05	ARS	Online	166,8105
OP-100147	COD_CLI_120	9/10/2023	952	3	19,26	0	ARS	SUCURSAL	57,78
OP-100839	COD_CLI_045	2/5/2024	805	12	34,68	0	ARS	Telefonico	416,16
OP-100439	COD_CLI_069	27/2/2024	863	6	37,92	0	ARS	online	227,52
OP-100080	COD_CLI_022	18/10/2024	877	5	8	0	ARS	TELEFONICO	40
OP-100417	COD_CLI_020	8/7/2024	759	1	749,99	0,15	ARS	Online	637,4915
OP-100038	COD_CLI_081	3/11/2024	876	1	115,13	0,05	ARS	SUCURSAL	109,3735
OP-100486	COD_CLI_119	25/2/2023	824	1	233,05	0,1	ARS	ONLINE	209,745
OP-100678	COD_CLI_098	19/2/2024	844	4	20,59	0,15	ARS	Sucursal	70,006
OP-100223	COD_CLI_006	2/11/2024	946	2	48,03	0	ARS	Sucursal	96,06
OP-100172	COD_CLI_065	15/10/2024	919	2	271,73	0	ARS	Online	543,46
OP-100730	COD_CLI_047	17/11/2023	925	1	259,21	0,05	ARS	Sucursal	246,2495
OP-100262	COD_CLI_010	20/1/2024	722	1	331,11	0,15	ARS	ONLINE	281,4435
* **Justificación:** Mantiene una única fuente de verdad para los datos del cliente, reduce la redundancia de almacenamiento y facilita el modelado dimensional bajo esquema en estrella en Power BI.
Adjunto capturas de pantalla
dimcliente
<img width="1359" height="703" alt="image" src="https://github.com/user-attachments/assets/b379ec83-c43b-44bb-acde-95bf14242daf" />
facventas
<img width="1357" height="675" alt="image" src="https://github.com/user-attachments/assets/46cbdfcf-2afc-4169-b0f0-4ea243a9533f" />

