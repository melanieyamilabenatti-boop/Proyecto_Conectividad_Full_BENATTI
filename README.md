Proyecto: Base de Datos para Empresa de Telecomunicaciones "CONECTIVIDAD FULL"

Introducción:

El objetivo de este proyecto es diseñar y desarrollar una base de datos para una empresa de telecomunicaciones que permita gestionar la información de los clientes, servicios, pagos y bajas. La base de datos debe ser capaz de almacenar y procesar grandes cantidades de datos de manera eficiente y segura.
Objetivo:

El objetivo de este proyecto es crear una base de datos que permita a la empresa de telecomunicaciones gestionar de manera efectiva la información de sus clientes, servicios y pagos, y tomar decisiones informadas sobre la base de los datos almacenados.

Situación Problemática:

La empresa de telecomunicaciones actualmente utiliza un sistema manual para gestionar la información de sus clientes, lo que ha llevado a errores y pérdidas de datos. La implementación de una base de datos permitirá a la empresa mejorar la eficiencia y la precisión en la gestión de la información.

Modelo de Negocio:

La empresa de telecomunicaciones ofrece servicios de internet, TV y telefonía a sus clientes. La base de datos debe ser capaz de almacenar información sobre los clientes, servicios, pagos y bajas.

Diagrama E-R:
              

Listado de Tablas:

1. Clientes –Datos del cliente, clave única del mismo, datos de contacto y ubicación del servicio
- DNI (VARCHAR(10))
- Numero_Celular (VARCHAR(15))
- Nombre (VARCHAR(50))
- Apellidos (VARCHAR(50))
- Mail (VARCHAR(100))
- Region (VARCHAR(50))
- Servicio_Contratado (VARCHAR(50))
- Numero_de_Servicio (VARCHAR(15) PRIMARY KEY)
- Monto_Ultimo_Pago (DECIMAL(10,2))

2. Servicios_de_Internet –Servicios que provee la empresa -tipo-costos
- ID_Servicio (SERIAL PRIMARY KEY)
- Tipo_de_Servicio (VARCHAR(50))
- Costo (DECIMAL(10,2))

3. Servicios_Extras -Ademas de fibra óptica se ofrecen servicios adicionales, televisión, canales premium, packs
- ID_Servicio_Extra (SERIAL PRIMARY KEY)
- Numero_de_Cliente (VARCHAR(15))
- Servicio_Contratado (VARCHAR(50))
- FOREIGN KEY (Numero_de_Cliente) REFERENCES Clientes(Numero_de_Servicio)

4. Migraciones_Servicios- up selling o downselling de los servicios ya contratados
- ID_Migracion (SERIAL PRIMARY KEY)
- DNI (VARCHAR(10))
- Numero_de_Cliente (VARCHAR(15))
- Servicio_Actual (VARCHAR(50))
- Servicio_a_Migrar (VARCHAR(50))
- Acepta_TV_Cable (BOOLEAN)
- Acepta_Pack_Futbol (BOOLEAN)
- Acepta_Pack_Canales_Premium (BOOLEAN)
- FOREIGN KEY (Numero_de_Cliente) REFERENCES Clientes(Numero_de_Servicio)

5. Pagos- registro de pagos de los servicios, método , ciclo
- ID_Pago (SERIAL PRIMARY KEY)
- DNI (VARCHAR(10))
- Numero_de_Cliente (VARCHAR(15))
- Fecha_Pago (DATE)
- Monto_Pago (DECIMAL(10,2))
- Metodo_Pago (VARCHAR(50))
- Estado_Pago (VARCHAR(50))
- FOREIGN KEY (Numero_de_Cliente) REFERENCES Clientes(Numero_de_Servicio)

6. Bajas_Clientes – solicitudes de bajas, retención de clientes
- ID_Baja (SERIAL PRIMARY KEY)
- DNI (VARCHAR(10))
- Numero_de_Cliente (VARCHAR(15))
- Fecha_Solicitud_Baja (DATE)
- Fecha_Efectiva_Baja (DATE)
- Motivo_Baja (VARCHAR(100))
- Descuento_Aplicado (DECIMAL(10,2))
- FOREIGN KEY (Numero_de_Cliente) REFERENCES Clientes(Numero_de_Servicio)
 TECNICO: 
A continuación, les comparto el script de creación de tablas del proyecto:

-- Crear el schema
CREATE SCHEMA conectividad_full;

-- Usar el schema
USE conectividad_full;

CREATE TABLE Clientes (
  DNI VARCHAR(10),
  Numero_Celular VARCHAR(15),
  Nombre VARCHAR(50),
  Apellidos VARCHAR(50),
  Mail VARCHAR(100),
  Region VARCHAR(50),
  Servicio_Contratado VARCHAR(50),
  Numero_de_Servicio VARCHAR(15) PRIMARY KEY,
  Monto_Ultimo_Pago DECIMAL(10,2)
);

-- Tipos de Servicios que ofrece la compañia ,id del servicio y costo
CREATE TABLE Servicios_de_Internet (
  ID_Servicio SERIAL PRIMARY KEY,
  Tipo_de_Servicio VARCHAR(50),
  Costo DECIMAL(10,2)
);

-- packs extras de television por cable y canales premium
CREATE TABLE Servicios_Extras (
  ID_Servicio_Extra SERIAL PRIMARY KEY,
  Numero_de_Cliente VARCHAR(15),
  Servicio_Contratado VARCHAR(50),
  FOREIGN KEY (Numero_de_Cliente) REFERENCES Clientes(Numero_de_Servicio)
);

-- clientes que aceptaron up-selling en sus servicios contratados
CREATE TABLE Migraciones_Servicios (
  ID_Migracion SERIAL PRIMARY KEY,
  DNI VARCHAR(10),
  Numero_de_Cliente VARCHAR(15),
  Servicio_Actual VARCHAR(50),
  Servicio_a_Migrar VARCHAR(50),
  Acepta_TV_Cable BOOLEAN,
  Acepta_Pack_Futbol BOOLEAN,
  Acepta_Pack_Canales_Premium BOOLEAN,
  FOREIGN KEY (Numero_de_Cliente) REFERENCES Clientes(Numero_de_Servicio)
);

-- 
CREATE TABLE Pagos (
  ID_Pago SERIAL PRIMARY KEY,
  DNI VARCHAR(10),
  Numero_de_Cliente VARCHAR(15),
  Fecha_Pago DATE,
  Monto_Pago DECIMAL(10,2),
  Metodo_Pago VARCHAR(50),
  Estado_Pago VARCHAR(50),
  FOREIGN KEY (Numero_de_Cliente) REFERENCES Clientes(Numero_de_Servicio)
);

-- informacion de cliente que se comunicaron para solicitar la baja y se les otorgo un descuento para mantener el servicio
CREATE TABLE Bajas_Clientes (
  ID_Baja SERIAL PRIMARY KEY,
  DNI VARCHAR(10),
  Numero_de_Cliente VARCHAR(15),
  Fecha_Solicitud_Baja DATE,
  Fecha_Efectiva_Baja DATE,
  Motivo_Baja VARCHAR(100),
  Descuento_Aplicado DECIMAL(10,2),
  FOREIGN KEY (Numero_de_Cliente) REFERENCES Clientes(Numero_de_Servicio)
);

