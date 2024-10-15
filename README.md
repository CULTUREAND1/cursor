Objetivo: Actualizar la edad de todos los usuarios en la base de datos 


DELIMITER $$

CREATE PROCEDURE actualizarColumnaEdad()
BEGIN
    DECLARE done INT DEFAULT 0;
    DECLARE id INT;
    DECLARE fecha_nacimiento DATE;
    DECLARE cur CURSOR FOR SELECT idUsuario, fechaNacimiento FROM Usuarios;
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;
    
    OPEN cur;
    
    read_loop: LOOP
        FETCH cur INTO id, fecha_nacimiento;
        IF done THEN
            LEAVE read_loop;
        END IF;
        
        -- Calcular la edad usando la función calcularEdad
        UPDATE Usuarios
        SET edad = calcularEdad(fecha_nacimiento)
        WHERE idUsuario = id;
    END LOOP;
    
    CLOSE cur;
END $$

DELIMITER ;
