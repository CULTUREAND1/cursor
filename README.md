Objetivo: Actualizar la edad de todos los usuarios en la base de datos
funciona con esta tabla

![image](https://github.com/user-attachments/assets/afb7ec99-432c-4743-8302-299ca084e382)





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

![image](https://github.com/user-attachments/assets/6e37b163-07f4-43b6-83d4-c64e6985f61c)
