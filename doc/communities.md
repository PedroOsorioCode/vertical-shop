# Descripción community

> Que es una comunidad


## tabla
```
CREATE TABLE communities (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL,
    address VARCHAR(200),
    city VARCHAR(100),
    country VARCHAR(100),
    description TEXT,
    is_featured BOOLEAN DEFAULT FALSE,           -- destacado manualmente
    is_evaluated BOOLEAN DEFAULT FALSE,          -- evaluado por sistema automático
    evaluation_score NUMERIC(5,2),               -- valor numérico de la evaluación (0.00 a 100.00)
    last_evaluated_at TIMESTAMP,                -- última fecha de evaluación
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE
);
```


## atributos

## consideraciones de riesgo