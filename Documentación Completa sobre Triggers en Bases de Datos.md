# Documentación Completa sobre Triggers en Bases de Datos

## 1. ¿Qué es un Trigger?

Un **trigger** (o **disparador**) es un conjunto de instrucciones que se almacenan y asocian a una tabla en una base de datos. Se ejecuta automáticamente cuando ocurre un evento específico en dicha tabla, como una operación de **INSERT**, **UPDATE** o **DELETE**.

Los triggers permiten automatizar tareas como la auditoría, validaciones, sincronización de datos y más.

---

## 2. ¿Para qué sirve un Trigger?

Los triggers se utilizan para:

- Ejecutar acciones automáticas ante cambios en la base de datos.
- Registrar operaciones en tablas de auditoría (logs).
- Mantener la integridad de los datos.
- Aplicar reglas de negocio sin depender del código de aplicación.
- Sincronizar tablas o mantener datos derivados actualizados.

---

## 3. ¿Cuándo se puede usar un Trigger?

Puedes usar triggers en casos como:

- Guardar un historial de cambios de datos.
- Validar condiciones antes de insertar o modificar registros.
- Calcular valores automáticamente al insertar datos.
- Notificar mediante correos o logs cuando algo cambia.
- Prevenir la eliminación de registros importantes.

---

## 4. Estructura de un Trigger

La estructura puede variar según el motor de base de datos, pero generalmente sigue esta forma:

```sql
CREATE TRIGGER nombre_del_trigger
{ BEFORE | AFTER | INSTEAD OF } { INSERT | UPDATE | DELETE }
ON nombre_de_tabla
[FOR EACH ROW]
BEGIN
   -- Instrucciones SQL
END;
```
## Ejemplo Basico En MysSQL
```sql
CREATE TRIGGER trg_audit AFTER INSERT
ON empleados FOR EACH ROW
BEGIN
  INSERT INTO auditoria (accion, fecha)
  VALUES ('Nuevo empleado insertado', NOW());
END;
```

### Tipos de Trigger
**BEFORE:**  Se ejecuta antes de la operación en la tabla. Útil para validaciones.

**AFTER:** Se ejecuta después de la operación. Común para auditorías.

**INSTEAD OF:** Sustituye una operación (solo en vistas en algunos sistemas como SQL Server).

## 6. ¿Cuándo ejecuta su acción un Trigger?
Los triggers se ejecutan automáticamente cuando ocurre el evento asociado:

**BEFORE INSERT:** Antes de insertar un registro.

**AFTER UPDATE:** Después de actualizar un registro.

**INSTEAD OF DELETE:** En lugar de eliminar el registro (en vistas).

## 7. Ejemplo de un Trigger

Escenario:
Queremos registrar cada vez que se actualiza el salario de un empleado.

```sql
CREATE TRIGGER trg_salario_update
AFTER UPDATE ON empleados
FOR EACH ROW
BEGIN
  IF OLD.salario <> NEW.salario THEN
    INSERT INTO historial_salario(id_empleado, salario_anterior, nuevo_salario, fecha_cambio)
    VALUES (OLD.id, OLD.salario, NEW.salario, NOW());
  END IF;
END;
```

## 8. Trigger Marketing
En el contexto de marketing, un "trigger" se refiere a un evento automático que activa una acción de marketing (email, notificación, etc.)

**Ejemplo:**

Enviar un correo automático cuando un usuario abandona el carrito de compras.

Enviar una oferta de cumpleaños.

Activar una secuencia de onboarding cuando un usuario se registra.

Este tipo de triggers se gestiona en herramientas como Mailchimp, HubSpot o ActiveCampaign.

## 9. ¿Cómo hacer un Trigger?

Pasos generales:

Identificar el evento: **INSERT, UPDATE o DELETE.**

Elegir el momento: **BEFORE, AFTER, INSTEAD OF.**

Escribir el bloque de código **SQL** con las instrucciones deseadas.

Crear el trigger en el SGBD **(MySQL, PostgreSQL, SQL Server, etc.)**

## 10. Características de un Trigger

Automático: Se ejecuta sin intervención manual.

Asociado a tablas o vistas.

Puede acceder a los valores anteriores (OLD) y nuevos (NEW).

Se ejecuta por fila o por operación (según el sistema).

Transaccional: Forma parte de la transacción SQL.

## 11. Desventajas de un Trigger
Difícil de depurar: El código no es tan visible como en las aplicaciones.

Puede impactar el rendimiento si mal implementado.

Puede provocar errores silenciosos.

Incrementa la complejidad del sistema.

Dependencia fuerte con la base de datos.

## 12. Sustituto de los Trigger en otras bases de datos

Algunas alternativas o sustitutos a los triggers en otros entornos:

SGBD / Sistema                            Alternativas                                                   

**MySQL / PostgreSQL**                        Procedimientos almacenados, lógica en el backend.       

**MongoDB (NoSQL)**                           Middleware en la aplicación o *Change Streams* (replica sets). 

**Firebase**                                  Cloud Functions que reaccionan a eventos.                      
**ORMs (Entity Framework, Sequelize, etc.)** 
Hooks y eventos (beforeSave, afterUpdate).                 

**ETL Tools**                                 Automatización mediante flujos programados.                    
