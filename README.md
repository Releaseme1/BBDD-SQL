
--Creamos el esquema
create schema practica_elisabetsoriano authorization rkylqudi;

-- - MODELOS de mesa ---
CREAR  TABLA  practica_elisabetsoriano .modelos(
				id_model entero  NO NULO , -- PK
				id_brand entero  NOT NULL , -- FK -> marcas
				modelo varchar ( 30 ) NO NULO
				);

ALTER  TABLE  practica_elisabetsoriano . modelos
	AGREGAR RESTRICCIÓN pk_model CLAVE PRINCIPAL (id_model);

-- - mesa MARCAS ---
CREAR  TABLA  practica_elisabetsoriano .marcas(
				id_brand entero  NO NULO , -- PK
				id_company entero  NOT NULL , -- FK -> empresas
				marca varchar ( 30 ) NO NULO
				);

ALTER  TABLE  practica_elisabetsoriano . marcas
	AÑADIR RESTRICCIÓN pk_brand CLAVE PRINCIPAL (id_brand);

-- - tabla EMPRESAS ---
CREAR  TABLA  practica_elisabetsoriano .empresas(
				id_company entero  NO NULO , -- PK
				empresa varchar ( 30 ) NO NULO
				);

ALTER  TABLE  practica_elisabetsoriano . compañías
	AÑADIR RESTRICCIÓN pk_company PRIMARY KEY (id_company);

-- tabla SEGUROS ---
CREAR  TABLA  practica_elisabetsoriano .seguros(
				id_insurance entero  NO NULO , -- PK
				id_car entero  NO NULO , -- FK -> coche
				id_insurance_company entero  NO NULO , -- FK -> compañías_de_seguros
				número_seguro entero  NO NULO
				);

ALTER  TABLE  practica_elisabetsoriano. seguros
	AÑADIR RESTRICCIÓN pk_insurance CLAVE PRINCIPAL (id_insurance);

-- - tabla SEGUROS_EMPRESAS ---
CREAR  TABLA  practica_elisabetsoriano .insurance_companies(
				id_insurance_company entero  NO NULO , -- PK
				insurance_company varchar ( 50 ) NO NULO
				);

ALTER  TABLE  practica_elisabetsoriano. las compañías de seguros
	AGREGAR RESTRICCIÓN pk_insurance_company CLAVE PRINCIPAL (id_insurance_company);

-- - tabla MONEDAS ---
CREAR  TABLA  practica_elisabetsoriano .monedas(
				id_currency entero  NO NULO , -- PK
				moneda varchar ( 30 ) NO NULO ,
				región varchar ( 30 ) NULL
				);

ALTER  TABLE  practica_elisabetsoriano . monedas
	AÑADIR RESTRICCIÓN pk_currency CLAVE PRINCIPAL (id_currency);

-- - mesa INSPECCIONES ---
CREAR  TABLA  practica_elisabetsoriano .inspecciones(
				km entero  NO NULO ,
				id_car entero  NOT NULL , -- PK, FK --> coches
				id_currency entero  NO NULO predeterminado ' 001 ' , -- FK --> monedas
				inspección_fecha marca de tiempo  NOT NULL , -- PK
				precio entero  NO NULO
				);

ALTER  TABLE  practica_elisabetsoriano . inspecciones
	AGREGAR RESTRICCIÓN pk_inspection PRIMARY KEY (inspection_date, id_car);

-- - mesa COLORES ---
CREAR  TABLA  practica_elisabetsoriano .colores(
				id_color entero  NO NULO , -- PK
				color varchar ( 20 ) NO NULO
				);

ALTER  TABLE  practica_elisabetsoriano. colores
	AÑADIR RESTRICCIÓN pk_color CLAVE PRINCIPAL (id_color);

-- - mesa COCHES ---
CREAR  TABLA  practica_elisabetsoriano .cars(
				id_car entero  NO NULO , -- PK
				id_model entero  NOT NULL , -- FK --> modelos
				id_color entero  NOT NULL , -- FK --> colores
				date_of_purchase fecha  NOT NULL ,
				licencia_placa varchar ( 10 ) NO NULO ,
				km_total entero  NO NULO
				);
			
ALTER  TABLE  practica_elisabetsoriano. coches
	AÑADIR RESTRICCIÓN pk_car CLAVE PRIMARIA (id_car);

--- FK---
ALTER  TABLE  practica_elisabetsoriano . marcas
	AGREGAR RESTRICCIÓN brand_company CLAVE EXTERNA (id_company)
	REFERENCIAS  practica_elisabetsoriano . empresas (id_empresa);

ALTER  TABLE  practica_elisabetsoriano . modelos
	AGREGAR RESTRICCIÓN modelo_marca CLAVE EXTERNA (id_marca)
	REFERENCIAS  practica_elisabetsoriano . marcas (id_marca);

ALTER  TABLE  practica_elisabetsoriano. seguros
	AGREGAR RESTRICCIÓN which_company FOREIGN KEY (id_insurance_company)
	REFERENCIAS  practica_elisabetsoriano . compañías_de_seguros (id_compañía_de_seguros);

ALTER  TABLE  practica_elisabetsoriano . seguros
	AÑADIR RESTRICCIÓN coche CLAVE EXTRANJERA (id_car)
	REFERENCIAS  practica_elisabetsoriano. coches (id_coche);

ALTER  TABLE  practica_elisabetsoriano . coches
	AGREGAR RESTRICCIÓN type_model CLAVE EXTERNA (id_model)
	REFERENCIAS  practica_elisabetsoriano. modelos (id_modelo);

ALTER  TABLE  practica_elisabetsoriano . coches
	AGREGAR RESTRICCIÓN car_color CLAVE EXTRANJERA (id_color)
	REFERENCIAS  practica_elisabetsoriano. colores (id_color);

ALTER  TABLE practica_elisabetsoriano . inspecciones
	AÑADIR RESTRICCIÓN inspección_coche CLAVE EXTRANJERA (id_coche)
	REFERENCIAS  practica_elisabetsoriano. coches (id_coche);

ALTER  TABLE  practica_elisabetsoriano . inspecciones
	AGREGAR RESTRICCIÓN tipo_moneda CLAVE EXTRANJERA (id_moneda)
	REFERENCIAS  practica_elisabetsoriano . monedas (id_currency);

-- ---------
-- - LMD ---
-- ---------

-- - monedas ---
INSERTAR EN  practica_elisabetsoriano . monedas
	(id_moneda, moneda, región)
	VALORES ( ' 001 ' , ' euro ' , ' UE ' );

INSERTAR EN  practica_elisabetsoriano. monedas
	(id_moneda, moneda, región)
	VALORES ( ' 002 ' , ' dolar ' , ' USA ' );

INSERTAR EN  practica_elisabetsoriano. monedas
	(id_moneda, moneda, región)
	VALORES ( ' 003 ' , ' libra ' , ' Reino Unido ' );

-- - empresas ---
INSERTAR EN  practica_elisabetsoriano . compañías
	(id_empresa, empresa)
	VALORES ( ' 001 ' , ' RNMA ' );

INSERTAR EN  practica_elisabetsoriano. compañías
	(id_empresa, empresa)
	VALORES ( ' 002 ' , ' FCA ' );

-- - marcas ---
INSERTAR EN  practica_elisabetsoriano . marcas
	(id_marca, id_empresa, marca)
	VALORES ( ' 001 ' , ' 001 ' , ' Renault ' );

INSERTAR EN  practica_elisabetsoriano . marcas
	(id_marca, id_empresa, marca)
	VALORES ( ' 002 ' , ' 001 ' , ' Nissan ' );

INSERTAR EN  practica_elisabetsoriano . marcas
	(id_marca, id_empresa, marca)
	VALORES ( ' 003 ' , ' 002 ' , ' Fiat ' );

-- - modelos ---
INSERTAR EN  practica_elisabetsoriano. modelos
	(id_modelo, id_marca, modelo)
	VALORES ( ' 001 ' , ' 001 ' , ' Escénico ' );

INSERTAR EN practica_elisabetsoriano . modelos
	(id_modelo, id_marca, modelo)
	VALORES ( ' 002 ' , ' 002 ' , ' NOTA ' );

INSERTAR EN  practica_elisabetsoriano . modelos
	(id_modelo, id_marca, modelo)
	VALORES ( ' 003 ' , ' 002 ' , ' X-TRAIL ' );

INSERTAR EN  practica_elisabetsoriano. modelos
	(id_modelo, id_marca, modelo)
	VALORES ( ' 004 ' , ' 003 ' , ' Panda ' );

INSERTAR EN  practica_elisabetsoriano . modelos
	(id_modelo, id_marca, modelo)
	VALORES ( ' 005 ' , ' 003 ' , ' 500L ' );

INSERTAR EN  practica_elisabetsoriano. modelos
	(id_modelo, id_marca, modelo)
	VALORES ( ' 006 ' , ' 003 ' , ' 500X ' );

-- - compañías_de_seguros ---
INSERTAR EN  practica_elisabetsoriano . las compañías de seguros
	(id_compañía_seguros, compañía_seguros)
	VALORES ( ' 001 ' , ' Allianz ' );

INSERTAR EN  practica_elisabetsoriano. las compañías de seguros
	(id_compañía_seguros, compañía_seguros)
	VALORES ( ' 002 ' , ' Verti ' );

INSERTAR EN  practica_elisabetsoriano . las compañías de seguros
	(id_compañía_seguros, compañía_seguros)
	VALORES ( ' 003 ' , ' AXA ' );

-- - colores ---
INSERTAR EN  practica_elisabetsoriano . colores
	(id_color, color)
	VALORES ( ' 001 ' , ' blanco ' );

INSERTAR EN  practica_elisabetsoriano . colores
	(id_color, color)
	VALORES ( ' 002 ' , ' negro ' );

INSERTAR EN  practica_elisabetsoriano . colores
	(id_color, color)
	VALORES ( ' 003 ' , ' gris ' );

INSERTAR EN  practica_elisabetsoriano. colores
	(id_color, color)
	VALORES ( ' 004 ' , ' rojo ' );

-- - coches ---
INSERTAR EN  practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 001 ' , ' 001 ' , ' 003 ' , ' 2018-01-22 ' , ' AAA0101 ' , 12000 );

INSERTAR EN  practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 002 ' , ' 001 ' , ' 004 ' , ' 2018-04-22 ' , ' AA0131 ' , 13000 );

INSERTAR EN  practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 003 ' , ' 001 ' , ' 002 ' , ' 2018-01-22 ' , ' ABA0101 ' , 500 );

INSERTAR EN practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 004 ' , ' 001 ' , ' 001 ' , ' 2019-01-22 ' , ' ACA0101 ' , 8000 );

INSERTAR EN  practica_elisabetsoriano. coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 005 ' , ' 002 ' , ' 001 ' , ' 2018-06-22 ' , ' AAG0401 ' , 11500 );

INSERTAR EN  practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 006 ' , ' 002 ' , ' 002 ' , ' 2018-11-22 ' , ' BBA0101 ' , 5500 );

INSERTAR EN  practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 007 ' , ' 003 ' , ' 001 ' , ' 2019-01-22 ' , ' ATU0101 ' , 7800 );

INSERTAR EN  practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 008 ' , ' 003 ' , ' 002 ' , ' 2020-11-22 ' , ' AAD3101 ' , 12000 );

INSERTAR EN  practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 009 ' , ' 003 ' , ' 002 ' , ' 2020-01-22 ' , ' BHT0101 ' , 13000 );

INSERTAR EN  practica_elisabetsoriano. coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 010 ' , ' 004 ' , ' 001 ' , ' 2019-01-22 ' , ' AAA7101 ' , 2000 );

INSERTAR EN  practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 011 ' , ' 005 ' , ' 004 ' , ' 2019-04-22 ' , ' ACA7101 ' , 12000 );

INSERTAR EN  practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 012 ' , ' 005 ' , ' 001 ' , ' 2019-03-22 ' , ' FCA7101 ' , 10000 );

INSERTAR EN  practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 013 ' , ' 005 ' , ' 001 ' , ' 2019-05-22 ' , ' ADT7101 ' , 2000 );

INSERTAR EN  practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 014 ' , ' 006 ' , ' 003 ' , ' 2019-05-22 ' , ' AWX7101 ' , 12000 );

INSERTAR EN  practica_elisabetsoriano . coches
	(id_coche, id_modelo, id_color, fecha_de_compra, matrícula_matrícula, km_total)
	VALORES ( ' 015 ' , ' 006 ' , ' 001 ' , ' 2020-05-22 ' , ' QWX7101 ' , 7600 );

-- - seguros ---
INSERTAR EN  practica_elisabetsoriano. seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 201 ' , ' 001 ' , ' 001 ' , ' 90141501 ' );

INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 202 ' , ' 002 ' , ' 001 ' , ' 90141502 ' );

INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 203 ' , ' 003 ' , ' 001 ' , ' 90141503 ' );

INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 204 ' , ' 004 ' , ' 001 ' , ' 90141504 ' );

INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 205 ' , ' 005 ' , ' 002 ' , ' 90141505 ' );

INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 206 ' , ' 006 ' , ' 002 ' , ' 90141506 ' );

INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 207 ' , ' 007 ' , ' 002 ' , ' 90141507 ' );

INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 208 ' , ' 008 ' , ' 002 ' , ' 90141508 ' );
	
INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 209 ' , ' 009 ' , ' 002 ' , ' 90141509 ' );
	
INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 210 ' , ' 010 ' , ' 002 ' , ' 90141510 ' );
	
INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 211 ' , ' 011 ' , ' 002 ' , ' 90141511 ' );
	
INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 212 ' , ' 012 ' , ' 003 ' , ' 90141512 ' );
	
INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 213 ' , ' 013 ' , ' 003 ' , ' 90141513 ' );
	
INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 214 ' , ' 014 ' , ' 003 ' , ' 90141514 ' );
	
INSERTAR EN  practica_elisabetsoriano . seguros
	(id_seguro, id_coche, id_compañía_seguro, número_seguro)
	VALORES ( ' 215 ' , ' 015 ' , ' 003 ' , ' 90141515 ' );

-- - inspecciones ---

INSERTAR EN  practica_elisabetsoriano. inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 001 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );

INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 3000 , ' 001 ' , ' 001 ' , ' 2021-08-22 13:00:00 ' , 23 );
	
INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 002 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );

INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 4000 , ' 002 ' , ' 001 ' , ' 2022-01-22 13:00:00 ' , 27 );
	
INSERTAR EN practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 003 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );
	
INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 004 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );
	
INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 005 ' , ' 002 ' , ' 2021-01-22 13:00:00 ' , 25 );
	
INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 006 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );
	
INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 007 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );
	
INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 008 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );
	
INSERTAR EN practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 009 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );
	
INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 4000 , ' 009 ' , ' 001 ' , ' 2021-08-22 13:00:00 ' , 30 );

INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 7000 , ' 009 ' , ' 001 ' , ' 2022-02-22 13:00:00 ' , 28 );

INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 010 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );
	
INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 011 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );
	
INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 012 ' , ' 003 ' , ' 2021-01-22 13:00:00 ' , 15 );
	
INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 013 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );
	
INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 6500 , ' 013 ' , ' 001 ' , ' 2021-09-22 13:00:00 ' , 28 );

INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 014 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );
	
INSERTAR EN  practica_elisabetsoriano . inspecciones
	(km, id_coche, id_moneda, fecha_de_inspección, precio)
	VALORES ( 1000 , ' 015 ' , ' 001 ' , ' 2021-01-22 13:00:00 ' , 20 );
