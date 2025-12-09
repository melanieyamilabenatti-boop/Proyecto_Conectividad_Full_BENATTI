# Proyecto_Conectividad_Full_BENATTI
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

