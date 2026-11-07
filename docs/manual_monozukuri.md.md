<?php

declare(strict_types=1);

// ============================================================================
// 1. DOMINIO / LÓGICA (El Chef)
// ============================================================================
final class Pelicula
{
    public function __construct(
        public readonly string $titulo,
        public readonly int $anio,
        public readonly string $genero
    ) {}

    public function obtenerDescripcion(): string
    {
        return sprintf(
            "La película %s se estrenó en el año %d y es de género %s.",
            $this->titulo,
            $this->anio,
            $this->genero
        );
    }
}

// ============================================================================
// 2. CASOS DE USO (Datos)
// ============================================================================
function obtenerCartelera(): array
{
    return [
        new Pelicula("Matrix", 1999, "Ciencia Ficción"),
        new Pelicula("Titanic", 1997, "Romance"),
        new Pelicula("El Padrino", 1972, "Drama"),
        new Pelicula("Interstellar", 2014, "Sci-Fi"),
    ];
}

$cartelera = obtenerCartelera();

// ============================================================================
// 3. VISTA (El Mesero)
// ============================================================================
?>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cartelera Universitaria | Monozukuri</title>
    <style>
        :root {
            --primary-color: #2563eb;
            --bg-color: #f8fafc;
            --card-bg: #ffffff;
            --text-main: #1e293b;
            --text-muted: #64748b;
            --danger-color: #ef4444;
            --border-radius: 8px;
            --shadow-sm: 0 1px 3px rgba(0,0,0,0.1);
            --shadow-md: 0 4px 6px -1px rgba(0,0,0,0.1);
        }
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { 
            font-family: system-ui, -apple-system, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            padding: 2rem;
            line-height: 1.5;
        }
        .container { max-width: 1000px; margin: 0 auto; }
        h1 { 
            font-size: 2rem; 
            margin-bottom: 1.5rem; 
            border-bottom: 2px solid var(--primary-color);
            padding-bottom: 0.5rem;
            display: inline-block;
        }
        .grid-cartelera {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 1.5rem;
        }
        .card {
            background-color: var(--card-bg);
            border-radius: var(--border-radius);
            padding: 1.5rem;
            box-shadow: var(--shadow-sm);
            transition: transform 0.2s ease, box-shadow 0.2s ease;
            border: 1px solid #e2e8f0;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        .card:hover { transform: translateY(-2px); box-shadow: var(--shadow-md); }
        .card-title { font-size: 1.25rem; color: var(--primary-color); margin-bottom: 0.5rem; }
        .card-meta { font-size: 0.875rem; color: var(--text-muted); margin-bottom: 1rem; }
        .card-description { font-size: 1rem; margin-bottom: 1.5rem; flex-grow: 1; }
        .btn-group { display: flex; gap: 0.5rem; }
        .btn {
            padding: 0.5rem 1rem; border-radius: 6px; text-decoration: none;
            font-size: 0.875rem; font-weight: 500; text-align: center;
            transition: background-color 0.2s; flex: 1;
        }
        .btn-primary { background-color: var(--primary-color); color: white; }
        .btn-primary:hover { background-color: #1d4ed8; }
        .btn-danger { background-color: transparent; color: var(--danger-color); border: 1px solid var(--danger-color); }
        .btn-danger:hover { background-color: var(--danger-color); color: white; }
    </style>
</head>
<body>
    <main class="container">
        <h1>Cartelera Universitaria</h1>
        <section class="grid-cartelera">
            <?php foreach ($cartelera as $pelicula): ?>
                <article class="card">
                    <div>
                        <h2 class="card-title"><?= htmlspecialchars($pelicula->titulo, ENT_QUOTES, 'UTF-8') ?></h2>
                        <div class="card-meta">
                            <span><?= htmlspecialchars((string)$pelicula->anio, ENT_QUOTES, 'UTF-8') ?></span> • 
                            <span><?= htmlspecialchars($pelicula->genero, ENT_QUOTES, 'UTF-8') ?></span>
                        </div>
                        <p class="card-description">
                            <?= htmlspecialchars($pelicula->obtenerDescripcion(), ENT_QUOTES, 'UTF-8') ?>
                        </p>
                    </div>
                    <div class="btn-group">
                        <a href="#" class="btn btn-primary">Ver Detalles</a>
                        <a href="#" class="btn btn-danger">Eliminar</a>
                    </div>
                </article>
            <?php endforeach; ?>
        </section>
    </main>
</body>
</html>