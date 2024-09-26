# Proyecto de Base de Datos para una Universidad

Este proyecto consiste en una base de datos SQL que administra estudiantes, cursos, profesores y calificaciones.

## 1. Creación de la Base de Datos y Tablas

```sql
CREATE DATABASE Universidad;

USE Universidad;

-- Tabla de Estudiantes
CREATE TABLE Estudiantes (
    id_estudiante INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    fecha_nacimiento DATE NOT NULL
);

-- Tabla de Cursos
CREATE TABLE Cursos (
    id_curso INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    creditos INT NOT NULL
);

-- Tabla de Profesores
CREATE TABLE Profesores (
    id_profesor INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

-- Tabla de Calificaciones
CREATE TABLE Calificaciones (
    id_calificacion INT AUTO_INCREMENT PRIMARY KEY,
    id_estudiante INT,
    id_curso INT,
    id_profesor INT,
    nota DECIMAL(3, 2) NOT NULL,
    FOREIGN KEY (id_estudiante) REFERENCES Estudiantes(id_estudiante),
    FOREIGN KEY (id_curso) REFERENCES Cursos(id_curso),
    FOREIGN KEY (id_profesor) REFERENCES Profesores(id_profesor)
);
```

## 2. Inserción de Datos de Muestra

```sql
-- Insertar estudiantes
INSERT INTO Estudiantes (nombre, fecha_nacimiento) VALUES
('Mohamad', '2000-01-15'),
('Ali', '1999-02-20'),
('Ismael', '2001-03-05');

-- Insertar cursos
INSERT INTO Cursos (nombre, creditos) VALUES
('Matemáticas', 4),
('Física', 3),
('Química', 4);

-- Insertar profesores
INSERT INTO Profesores (nombre) VALUES
('Dr. Jawad'),
('Prof. Omar'),
('Dr. Habib');

-- Insertar calificaciones
INSERT INTO Calificaciones (id_estudiante, id_curso, id_profesor, nota) VALUES
(1, 1, 1, 8.5),
(1, 2, 1, 9.0),
(2, 1, 2, 7.5),
(2, 3, 2, 8.0),
(3, 2, 3, 9.5),
(3, 3, 3, 6.0);
```

## 3. Consultas SQL

### a) Nota media que otorga cada profesor

```sql
SELECT p.nombre AS profesor, AVG(c.nota) AS nota_media
FROM Calificaciones c
JOIN Profesores p ON c.id_profesor = p.id_profesor
GROUP BY p.id_profesor;
```

### b) Mejores notas de cada estudiante

```sql
SELECT e.nombre AS estudiante, MAX(c.nota) AS mejor_nota
FROM Calificaciones c
JOIN Estudiantes e ON c.id_estudiante = e.id_estudiante
GROUP BY e.id_estudiante;
```

### c) Ordenar a los estudiantes por los cursos en los que están inscritos

```sql
SELECT e.nombre AS estudiante, c.nombre AS curso
FROM Calificaciones cal
JOIN Estudiantes e ON cal.id_estudiante = e.id_estudiante
JOIN Cursos c ON cal.id_curso = c.id_curso
ORDER BY e.nombre, c.nombre;
```

### d) Informe resumido de los cursos y sus calificaciones promedio

```sql
SELECT c.nombre AS curso, AVG(cal.nota) AS calificacion_promedio
FROM Calificaciones cal
JOIN Cursos c ON cal.id_curso = c.id_curso
GROUP BY c.id_curso
ORDER BY AVG(cal.nota) ASC;
```

### e) Estudiante y profesor con más cursos en común

```sql
SELECT e.nombre AS estudiante, p.nombre AS profesor, COUNT(*) AS cursos_en_comun
FROM Calificaciones c
JOIN Estudiantes e ON c.id_estudiante = e.id_estudiante
JOIN Profesores p ON c.id_profesor = p.id_profesor
GROUP BY e.id_estudiante, p.id_profesor
ORDER BY cursos_en_comun DESC
LIMIT 1;
```

