<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mapeo Animal — La Florida</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/leaflet@1.9.4/dist/leaflet.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary: #15803d;
            --primary-light: #dcfce7;
            --secondary: #0f766e;
            --accent: #f59e0b;
            --text: #334155;
            --light: #f8fafc;
            --white: #ffffff;
            --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            --shadow-strong: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            --radius: 12px;
            --transition: all 0.3s ease;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            line-height: 1.6;
            color: var(--text);
            background-color: #fefefe;
            overflow-x: hidden;
        }
        
        /* 1. NAVEGACIÓN Y ESTRUCTURA MEJORADA */
        header {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            color: var(--white);
            padding: 2rem 1rem;
            text-align: center;
            box-shadow: var(--shadow-strong);
            position: relative;
            overflow: hidden;
        }
        
        header::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url("data:image/svg+xml,%3Csvg width='100' height='100' viewBox='0 0 100 100' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M11 18c3.866 0 7-3.134 7-7s-3.134-7-7-7-7 3.134-7 7 3.134 7 7 7zm48 25c3.866 0 7-3.134 7-7s-3.134-7-7-7-7 3.134-7 7 3.134 7 7 7zm-43-7c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zm63 31c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zM34 90c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zm56-76c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zM12 86c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm28-65c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm23-11c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm-6 60c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm29 22c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zM32 63c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm57-13c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm-9-21c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM60 91c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM35 41c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM12 60c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2z' fill='%23ffffff' fill-opacity='0.05' fill-rule='evenodd'/%3E%3C/svg%3E");
            opacity: 0.1;
        }
        
        .logo {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin-bottom: 1rem;
        }
        
        .logo-icon {
            font-size: 2.5rem;
            color: var(--accent);
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem 1rem;
        }
        
        .tabs {
            display: flex;
            justify-content: center;
            margin: 3rem 0 1rem;
            border-bottom: 1px solid #e2e8f0;
            flex-wrap: wrap;
            position: relative;
        }
        
        .tab {
            padding: 1rem 2rem;
            background: none;
            border: none;
            font-size: 1rem;
            cursor: pointer;
            color: var(--text);
            border-bottom: 3px solid transparent;
            transition: var(--transition);
            position: relative;
            overflow: hidden;
            border-radius: var(--radius) var(--radius) 0 0;
        }
        
        .tab::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(21, 128, 61, 0.1), transparent);
            transition: left 0.5s;
        }
        
        .tab:hover::before {
            left: 100%;
        }
        
        .tab.active {
            color: var(--primary);
            border-bottom: 3px solid var(--primary);
            font-weight: 600;
            background: rgba(21, 128, 61, 0.05);
        }
        
        .tab-content {
            display: none;
            animation: fadeIn 0.5s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .tab-content.active {
            display: block;
        }
        
        .floating-action-btn {
            position: fixed;
            bottom: 2rem;
            right: 2rem;
            width: 60px;
            height: 60px;
            background: var(--primary);
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: var(--shadow-strong);
            cursor: pointer;
            z-index: 100;
            transition: var(--transition);
            animation: pulse 2s infinite;
        }
        
        .floating-action-btn:hover {
            transform: scale(1.1);
            background: #0f7229;
        }
        
        /* 2. EXPERIENCIA VISUAL MEJORADA */
        .hero {
            display: flex;
            flex-direction: column;
            gap: 2rem;
            margin: 2rem 0;
            position: relative;
        }
        
        @media (min-width: 768px) {
            .hero {
                flex-direction: row;
            }
        }
        
        .hero-text {
            flex: 1;
            padding: 2.5rem;
            background: var(--white);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            transition: var(--transition);
            position: relative;
            overflow: hidden;
        }
        
        .hero-text:hover {
            transform: translateY(-5px);
            box-shadow: var(--shadow-strong);
        }
        
        .hero-text::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 5px;
            height: 100%;
            background: linear-gradient(to bottom, var(--primary), var(--secondary));
        }
        
        .hero-image {
            flex: 1;
            background: linear-gradient(135deg, var(--primary-light) 0%, #e0f2fe 100%);
            border-radius: var(--radius);
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 2.5rem;
            box-shadow: var(--shadow);
            transition: var(--transition);
        }
        
        .hero-image:hover {
            transform: translateY(-5px);
            box-shadow: var(--shadow-strong);
        }
        
        .section-title {
            text-align: center;
            margin: 3rem 0 1.5rem;
            color: var(--primary);
            font-size: 2rem;
            position: relative;
            padding-bottom: 1rem;
        }
        
        .section-title::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 80px;
            height: 4px;
            background: linear-gradient(to right, var(--primary), var(--secondary));
            border-radius: 2px;
        }
        
        .panel {
            background: var(--white);
            border-radius: var(--radius);
            padding: 2rem;
            box-shadow: var(--shadow);
            margin: 1.5rem 0;
            transition: var(--transition);
            position: relative;
            overflow: hidden;
        }
        
        .panel::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(to right, var(--primary), var(--secondary));
        }
        
        .panel:hover {
            transform: translateY(-5px);
            box-shadow: var(--shadow-strong);
        }
        
        .stats {
            display: flex;
            justify-content: space-around;
            margin: 3rem 0;
            text-align: center;
            flex-wrap: wrap;
            gap: 1rem;
        }
        
        .stat-item {
            background: var(--white);
            padding: 2rem;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            flex: 1;
            min-width: 150px;
            transition: var(--transition);
            position: relative;
            overflow: hidden;
        }
        
        .stat-item:hover {
            transform: translateY(-5px);
            box-shadow: var(--shadow-strong);
        }
        
        .stat-item::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(to right, var(--primary), var(--secondary));
        }
        
        .animal-cards, .event-cards {
            display: grid;
            grid-template-columns: 1fr;
            gap: 1.5rem;
            margin: 2rem 0;
        }
        
        @media (min-width: 768px) {
            .animal-cards {
                grid-template-columns: repeat(3, 1fr);
            }
            .event-cards {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        
        .animal-card, .event-card {
            background: var(--white);
            border-radius: var(--radius);
            overflow: hidden;
            box-shadow: var(--shadow);
            transition: var(--transition);
            position: relative;
        }
        
        .animal-card:hover, .event-card:hover {
            transform: translateY(-10px);
            box-shadow: var(--shadow-strong);
        }
        
        .animal-img, .event-img {
            height: 200px;
            background-color: var(--primary-light);
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--primary);
            font-size: 3rem;
            position: relative;
            overflow: hidden;
        }
        
        .animal-img::after, .event-img::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(to right, var(--primary), var(--secondary));
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background: var(--primary);
            color: white;
            padding: 1rem 1.5rem;
            border-radius: 8px;
            box-shadow: var(--shadow-strong);
            z-index: 1000;
            transform: translateX(150%);
            transition: transform 0.3s ease-in-out;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        
        .notification.show {
            transform: translateX(0);
        }
        
        /* 3. MAPA Y GEOLOCALIZACIÓN MEJORADA */
        #map {
            height: 500px;
            width: 100%;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            margin: 1.5rem 0;
            transition: var(--transition);
        }
        
        #map:hover {
            box-shadow: var(--shadow-strong);
        }
        
        #map-form {
            height: 300px;
            margin-top: 1rem;
            border-radius: 8px;
            box-shadow: var(--shadow);
        }
        
        .map-controls {
            position: absolute;
            top: 10px;
            right: 10px;
            z-index: 1000;
            background: white;
            border-radius: 8px;
            box-shadow: var(--shadow);
            padding: 0.5rem;
        }
        
        .map-legend {
            position: absolute;
            bottom: 10px;
            left: 10px;
            z-index: 1000;
            background: white;
            border-radius: 8px;
            box-shadow: var(--shadow);
            padding: 1rem;
        }
        
        .cluster-marker {
            background: var(--primary);
            border-radius: 50%;
            color: white;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 0 0 5px rgba(21, 128, 61, 0.3);
            animation: pulse 2s infinite;
        }
        
        /* 4. FORMULARIOS E INTERACCIÓN MEJORADA */
        .form-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 1.5rem;
            margin-top: 1.5rem;
        }
        
        @media (min-width: 768px) {
            .form-grid {
                grid-template-columns: 1fr 1fr;
            }
        }
        
        .form-group {
            margin-bottom: 1.5rem;
            position: relative;
        }
        
        .form-control {
            width: 100%;
            padding: 1rem;
            border: 1px solid #e2e8f0;
            border-radius: 8px;
            font-size: 1rem;
            transition: var(--transition);
            background: #fafafa;
        }
        
        .form-control:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(21, 128, 61, 0.1);
            background: var(--white);
        }
        
        .form-section {
            margin-bottom: 2.5rem;
            padding-bottom: 1.5rem;
            border-bottom: 1px solid #e2e8f0;
            position: relative;
        }
        
        .radio-group {
            display: flex;
            gap: 1.5rem;
            margin-top: 0.5rem;
            flex-wrap: wrap;
        }
        
        .radio-option {
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        
        .radio-option input[type="radio"] {
            accent-color: var(--primary);
        }
        
        .btn {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
            padding: 1rem 2rem;
            background: var(--primary);
            color: var(--white);
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
            text-align: center;
            position: relative;
            overflow: hidden;
        }
        
        .btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            transition: left 0.5s;
        }
        
        .btn:hover::before {
            left: 100%;
        }
        
        .btn:hover {
            background: #0f7229;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        
        .btn-block {
            display: block;
            width: 100%;
        }
        
        .photo-upload {
            border: 2px dashed #cbd5e0;
            border-radius: 8px;
            padding: 2rem;
            text-align: center;
            cursor: pointer;
            transition: var(--transition);
            background: #fafafa;
        }
        
        .photo-upload:hover {
            border-color: var(--primary);
            background: var(--white);
        }
        
        .form-progress {
            display: flex;
            margin-bottom: 2rem;
            position: relative;
        }
        
        .form-step {
            flex: 1;
            text-align: center;
            padding: 1rem;
            position: relative;
            color: #94a3b8;
            font-weight: 600;
        }
        
        .form-step.active {
            color: var(--primary);
        }
        
        .form-step::after {
            content: '';
            position: absolute;
            top: 50%;
            right: 0;
            width: 100%;
            height: 2px;
            background: #e2e8f0;
            z-index: -1;
        }
        
        .form-step:last-child::after {
            display: none;
        }
        
        .form-step-number {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: #e2e8f0;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 0.5rem;
            transition: var(--transition);
        }
        
        .form-step.active .form-step-number {
            background: var(--primary);
            color: white;
        }
        
        /* 6. ELEMENTOS DE MARCA MEJORADOS */
        .brand-elements {
            display: flex;
            justify-content: center;
            gap: 2rem;
            margin: 2rem 0;
            flex-wrap: wrap;
        }
        
        .brand-card {
            background: var(--white);
            border-radius: var(--radius);
            padding: 2rem;
            text-align: center;
            box-shadow: var(--shadow);
            transition: var(--transition);
            flex: 1;
            min-width: 200px;
            max-width: 300px;
        }
        
        .brand-card:hover {
            transform: translateY(-5px);
            box-shadow: var(--shadow-strong);
        }
        
        .brand-icon {
            font-size: 3rem;
            color: var(--primary);
            margin-bottom: 1rem;
        }
        
        .trust-badges {
            display: flex;
            justify-content: center;
            gap: 1rem;
            margin: 2rem 0;
            flex-wrap: wrap;
        }
        
        .trust-badge {
            background: var(--white);
            border-radius: var(--radius);
            padding: 1rem 1.5rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            box-shadow: var(--shadow);
            transition: var(--transition);
        }
        
        .trust-badge:hover {
            transform: translateY(-2px);
            box-shadow: var(--shadow-strong);
        }
        
        /* Elementos de estado (burbujas de colores) */
        .health-indicator {
            display: inline-block;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            margin-right: 5px;
        }
        
        .health-good {
            background-color: #10b981;
        }
        
        .health-moderate {
            background-color: #f59e0b;
        }
        
        .health-critical {
            background-color: #ef4444;
        }
        
        .animal-tag {
            display: inline-block;
            padding: 0.5rem 1rem;
            background: var(--primary-light);
            color: var(--primary);
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            margin-top: 0.5rem;
        }
        
        .event-date {
            display: inline-block;
            padding: 0.5rem 1rem;
            background: var(--accent);
            color: white;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            margin-bottom: 0.5rem;
        }
        
        footer {
            text-align: center;
            color: var(--text);
            padding: 3rem 1rem;
            margin-top: 3rem;
            background: var(--white);
            border-top: 1px solid #e2e8f0;
            position: relative;
        }
        
        footer::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(to right, var(--primary), var(--secondary));
        }
        
        .loading-spinner {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 1s ease infinite;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }
    </style>
</head>
<body>
    <header>
        <div class="logo">
            <i class="fas fa-paw logo-icon"></i>
            <h1>Mapeo Animal — La Florida</h1>
        </div>
        <p>Localiza y reporta animales en situación de calle o desaparecidos.</p>
    </header>
    
    <div class="container">
        <div class="hero">
            <div class="hero-text">
                <h2>Ayudemos a los animales callejeros</h2>
                <p>Esta plataforma permite a los ciudadanos de La Florida reportar animales en situación de calle o desaparecidos, facilitando la colaboración entre vecinos y organizaciones de rescate.</p>
                <p>Tu reporte puede marcar la diferencia en la vida de un animal necesitado.</p>
                <div style="margin-top: 1.5rem;">
                    <button class="btn" onclick="showTab('reporte')">
                        <i class="fas fa-plus-circle"></i> Reportar Animal
                    </button>
                </div>
            </div>
            <div class="hero-image">
                <div style="text-align: center;">
                    <div style="font-size: 5rem; color: var(--primary); margin-bottom: 1rem; animation: bounce 2s infinite;">🐕</div>
                    <h3 style="color: var(--primary);">Comunidad unida por los animales</h3>
                </div>
            </div>
        </div>
        
        <!-- 6. ELEMENTOS DE MARCA -->
        <div class="brand-elements">
            <div class="brand-card">
                <div class="brand-icon">
                    <i class="fas fa-shield-alt"></i>
                </div>
                <h3>Seguro y Confiable</h3>
                <p>Tu información está protegida y solo se comparte con fines de rescate animal</p>
            </div>
            <div class="brand-card">
                <div class="brand-icon">
                    <i class="fas fa-users"></i>
                </div>
                <h3>Comunidad Activa</h3>
                <p>Vecinos comprometidos trabajando juntos por el bienestar animal</p>
            </div>
            <div class="brand-card">
                <div class="brand-icon">
                    <i class="fas fa-heart"></i>
                </div>
                <h3>Comprometidos</h3>
                <p>Trabajamos incansablemente por el bienestar de todos los animales</p>
            </div>
        </div>
        
        <div class="trust-badges">
            <div class="trust-badge">
                <i class="fas fa-check-circle" style="color: var(--primary);"></i>
                <span>Verificado por Profesionales</span>
            </div>
            <div class="trust-badge">
                <i class="fas fa-lock" style="color: var(--primary);"></i>
                <span>Datos Protegidos</span>
            </div>
            <div class="trust-badge">
                <i class="fas fa-star" style="color: var(--accent);"></i>
                <span>4.9/5 Evaluación</span>
            </div>
        </div>
        
        <div class="stats">
            <div class="stat-item">
                <div class="stat-number" id="total-animales">7</div>
                <div class="stat-label">Animales Reportados</div>
            </div>
            <div class="stat-item">
                <div class="stat-number">28</div>
                <div class="stat-label">Animales Rescatados</div>
            </div>
            <div class="stat-item">
                <div class="stat-number">15</div>
                <div class="stat-label">Familias de Acogida</div>
            </div>
            <div class="stat-item">
                <div class="stat-number">6</div>
                <div class="stat-label">Eventos Activos</div>
            </div>
        </div>
        
        <div class="tabs">
            <button class="tab active" onclick="showTab('mapa')">
                <i class="fas fa-map-marked-alt"></i> Mapa de Animales
            </button>
            <button class="tab" onclick="showTab('reporte')">
                <i class="fas fa-edit"></i> Reporte Animal Callejero
            </button>
            <button class="tab" onclick="showTab('eventos')">
                <i class="fas fa-calendar-alt"></i> Eventos por Fundaciones
            </button>
        </div>
        
        <div id="mapa-tab" class="tab-content active">
            <div class="panel">
                <h2><i class="fas fa-map"></i> Mapa de Animales Reportados</h2>
                <p>Este es un mapa comunitario de ayuda animal. Los puntos muestran reportes de animales en la comuna de La Florida. Haz clic en un marcador para ver su descripción.</p>
                <p><span class="health-indicator health-good"></span> Estado bueno | 
                   <span class="health-indicator health-moderate"></span> Estado moderado | 
                   <span class="health-indicator health-critical"></span> Estado crítico</p>
            </div>
            
            <div id="map">
                <div class="map-controls">
                    <button class="btn" onclick="locateUser()" style="padding: 0.5rem;">
                        <i class="fas fa-location-arrow"></i>
                    </button>
                </div>
                <div class="map-legend">
                    <p><strong>Leyenda</strong></p>
                    <p><span class="health-indicator health-good"></span> Estado bueno</p>
                    <p><span class="health-indicator health-moderate"></span> Estado moderado</p>
                    <p><span class="health-indicator health-critical"></span> Estado crítico</p>
                </div>
            </div>
            
            <h2 class="section-title">Animales que Necesitan Ayuda</h2>
            
            <div class="animal-cards" id="animal-cards-container">
                <!-- Los animales se cargarán dinámicamente aquí -->
            </div>
        </div>
        
        <div id="reporte-tab" class="tab-content">
            <div class="panel">
                <h2><i class="fas fa-edit"></i> Reporte Animal Callejero</h2>
                <p>Si encuentras animales callejeros, reporta su información y ubicación en el siguiente formulario.</p>
                
                <!-- 4. FORMULARIO MEJORADO CON PROGRESO -->
                <div class="form-progress">
                    <div class="form-step active">
                        <div class="form-step-number">1</div>
                        <div>Animal</div>
                    </div>
                    <div class="form-step">
                        <div class="form-step-number">2</div>
                        <div>Ubicación</div>
                    </div>
                    <div class="form-step">
                        <div class="form-step-number">3</div>
                        <div>Contacto</div>
                    </div>
                </div>
                
                <form id="reporte-form">
                    <div class="form-section">
                        <h3><i class="fas fa-paw"></i> Información del Animal</h3>
                        <div class="form-grid">
                            <div class="form-group">
                                <label for="nombre">Nombre (si se conoce)</label>
                                <input type="text" id="nombre" class="form-control" placeholder="Ej: Max, Luna...">
                            </div>
                            
                            <div class="form-group">
                                <label for="especie">Especie</label>
                                <select id="especie" class="form-control">
                                    <option value="">Seleccione una especie</option>
                                    <option value="perro">Perro</option>
                                    <option value="gato">Gato</option>
                                    <option value="otro">Otro</option>
                                </select>
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label for="estado-salud">Estado de Salud</label>
                            <select id="estado-salud" class="form-control">
                                <option value="good">Bueno - Sin signos de enfermedad</option>
                                <option value="moderate">Moderado - Algunos signos de enfermedad o desnutrición</option>
                                <option value="critical">Crítico - Herido o con enfermedad evidente</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="descripcion">Descripción del Animal</label>
                            <textarea id="descripcion" class="form-control" rows="3" placeholder="Describa el animal: color, tamaño, condición, comportamiento..."></textarea>
                        </div>
                        
                        <div class="form-group">
                            <label>Fotografía (opcional)</label>
                            <div class="photo-upload" onclick="document.getElementById('foto-input').click()">
                                <i class="fas fa-cloud-upload-alt"></i>
                                <p>Haga clic para subir una foto del animal</p>
                                <input type="file" id="foto-input" accept="image/*" style="display: none;" onchange="previewPhoto(event)">
                                <img id="foto-preview" class="uploaded-photo" alt="Vista previa">
                            </div>
                        </div>
                    </div>
                    
                    <div class="form-section">
                        <h3><i class="fas fa-map-marker-alt"></i> Ubicación</h3>
                        <div class="form-group">
                            <label for="direccion">Dirección Aproximada</label>
                            <input type="text" id="direccion" class="form-control" placeholder="Ej: Av. Vicuña Mackenna 7500">
                        </div>
                        
                        <div class="form-group">
                            <label>Seleccione la ubicación en el mapa</label>
                            <div id="map-form"></div>
                            <p style="margin-top: 0.5rem; font-size: 0.9rem; color: #64748b;">Haga clic en el mapa para marcar la ubicación exacta</p>
                        </div>
                    </div>
                    
                    <div class="form-section">
                        <h3><i class="fas fa-user"></i> Información de Contacto</h3>
                        <div class="form-grid">
                            <div class="form-group">
                                <label for="nombre-contacto">Su Nombre</label>
                                <input type="text" id="nombre-contacto" class="form-control">
                            </div>
                            
                            <div class="form-group">
                                <label for="telefono">Teléfono</label>
                                <input type="tel" id="telefono" class="form-control">
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label for="email">Email</label>
                            <input type="email" id="email" class="form-control">
                        </div>
                    </div>
                    
                    <div class="form-section">
                        <h3><i class="fas fa-clipboard-list"></i> Tipo de Reporte</h3>
                        <div class="radio-group">
                            <div class="radio-option">
                                <input type="radio" id="tipo-callejero" name="tipo-reporte" value="callejero" checked>
                                <label for="tipo-callejero">Animal Callejero</label>
                            </div>
                            <div class="radio-option">
                                <input type="radio" id="tipo-perdido" name="tipo-reporte" value="perdido">
                                <label for="tipo-perdido">Animal Perdido</label>
                            </div>
                            <div class="radio-option">
                                <input type="radio" id="tipo-herido" name="tipo-reporte" value="herido">
                                <label for="tipo-herido">Animal Herido</label>
                            </div>
                        </div>
                    </div>
                    
                    <button type="submit" class="btn btn-block">
                        <i class="fas fa-paper-plane"></i> Enviar Reporte
                    </button>
                </form>
            </div>
        </div>
        
        <div id="eventos-tab" class="tab-content">
            <div class="panel">
                <h2><i class="fas fa-calendar-alt"></i> Eventos por Fundaciones</h2>
                <p>Campañas de organizaciones de la comuna para que el público pueda inscribirse y participar.</p>
                
                <div class="event-cards" id="eventos-container">
                    <!-- Los eventos se cargarán dinámicamente aquí -->
                </div>
            </div>
        </div>
    </div>
    
    <div class="floating-action-btn" onclick="showTab('reporte')">
        <i class="fas fa-plus"></i>
    </div>
    
    <div class="notification" id="notification">
        <i class="fas fa-check-circle"></i> <span id="notification-text">¡Reporte enviado con éxito!</span>
    </div>
    
    <footer>
        <p>Proyecto comunitario — La Florida • Hecho para facilitar la colaboración entre ciudadanos y organizaciones</p>
        <p style="margin-top: 0.5rem; font-size: 0.9rem;">
            <i class="fas fa-phone"></i> Contacto: +56 9 1234 5678 | 
            <i class="fas fa-envelope"></i> info@mapeoanimal.cl
        </p>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        // Datos iniciales de animales
        let animales = [
            {
                id: 1,
                nombre: "Max", 
                especie: "perro", 
                desc: "Callejero cerca de Av. Vicuña Mackenna, cojea de la pata trasera.", 
                lat: -33.485, 
                lng: -70.599,
                tipo: "callejero",
                estadoSalud: "moderate",
                fecha: "2023-10-15"
            },
            {
                id: 2,
                nombre: "Luna", 
                especie: "gato", 
                desc: "Gata blanca y gris vista cerca de Mall Florida Center.", 
                lat: -33.493, 
                lng: -70.580,
                tipo: "callejero",
                estadoSalud: "good",
                fecha: "2023-10-18"
            },
            {
                id: 3,
                nombre: "Toby", 
                especie: "perro", 
                desc: "Perro grande y amistoso en la plaza El Llano.", 
                lat: -33.479, 
                lng: -70.590,
                tipo: "callejero",
                estadoSalud: "good",
                fecha: "2023-10-20"
            },
            {
                id: 4,
                nombre: "Rocky", 
                especie: "perro", 
                desc: "Perro pequeño con collar azul, parece perdido.", 
                lat: -33.500, 
                lng: -70.575,
                tipo: "perdido",
                estadoSalud: "good",
                fecha: "2023-10-22"
            },
            {
                id: 5,
                nombre: "Mimi", 
                especie: "gato", 
                desc: "Gata negra con manchas blancas, muy delgada.", 
                lat: -33.470, 
                lng: -70.600,
                tipo: "callejero",
                estadoSalud: "moderate",
                fecha: "2023-10-25"
            },
            {
                id: 6,
                nombre: "Rex", 
                especie: "perro", 
                desc: "Perro pastor alemán, parece desorientado.", 
                lat: -33.490, 
                lng: -70.595,
                tipo: "perdido",
                estadoSalud: "good",
                fecha: "2023-10-26"
            },
            {
                id: 7,
                nombre: "Nala", 
                especie: "gato", 
                desc: "Gata siamesa con ojos azules, muy asustadiza.", 
                lat: -33.475, 
                lng: -70.585,
                tipo: "callejero",
                estadoSalud: "good",
                fecha: "2023-10-28"
            }
        ];

        // Variables globales para los mapas
        let map, mapForm, markerForm, userLocation;

        // Inicializar la aplicación
        function initApp() {
            // Cargar datos del localStorage si existen
            const savedAnimales = localStorage.getItem('animales');
            if (savedAnimales) {
                animales = JSON.parse(savedAnimales);
            }
            
            // Inicializar mapas
            initMaps();
            
            // Cargar animales y eventos
            renderAnimalCards();
            renderEventos();
            
            // Actualizar estadísticas
            updateStats();
            
            // Inicializar formulario multipaso
            initMultiStepForm();
        }

        // Inicializar mapas
        function initMaps() {
            // Mapa principal
            map = L.map('map').setView([-33.485, -70.582], 14);
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                maxZoom: 19,
                attribution: '&copy; OpenStreetMap contributors'
            }).addTo(map);

            // Añadir marcadores al mapa principal
            addMarkersToMap();

            // Mapa del formulario
            mapForm = L.map('map-form').setView([-33.485, -70.582], 14);
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                maxZoom: 19,
                attribution: '&copy; OpenStreetMap contributors'
            }).addTo(mapForm);
            
            // Añadir marcador al hacer clic en el mapa del formulario
            mapForm.on('click', function(e) {
                if (markerForm) {
                    mapForm.removeLayer(markerForm);
                }
                markerForm = L.marker(e.latlng).addTo(mapForm);
                
                // Actualizar dirección aproximada
                updateAddressFromCoords(e.latlng.lat, e.latlng.lng);
            });
        }

        // 3. FUNCIONALIDAD MEJORADA DE MAPA - Localizar usuario
        function locateUser() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(function(position) {
                    userLocation = [position.coords.latitude, position.coords.longitude];
                    map.setView(userLocation, 16);
                    
                    // Añadir marcador de ubicación del usuario
                    L.marker(userLocation, {
                        icon: L.divIcon({
                            className: 'user-location-marker',
                            html: '<div style="background-color: #3b82f6; width: 20px; height: 20px; border-radius: 50%; border: 3px solid white; box-shadow: 0 0 5px rgba(0,0,0,0.5);"></div>',
                            iconSize: [20, 20],
                            iconAnchor: [10, 10]
                        })
                    }).addTo(map).bindPopup('Tu ubicación actual').openPopup();
                    
                    showNotification('Ubicación encontrada correctamente');
                }, function(error) {
                    showNotification('No se pudo obtener tu ubicación. Asegúrate de haber permitido el acceso.');
                });
            } else {
                showNotification('La geolocalización no es compatible con tu navegador.');
            }
        }

        // Actualizar dirección desde coordenadas
        function updateAddressFromCoords(lat, lng) {
            // Simulación de geocodificación inversa
            const addresses = [
                "Av. Vicuña Mackenna 7500",
                "Plaza El Llano",
                "Mall Florida Center", 
                "Centro de La Florida",
                "Av. Walker Martínez"
            ];
            
            const randomAddress = addresses[Math.floor(Math.random() * addresses.length)];
            document.getElementById('direccion').value = randomAddress;
        }

        // Función para añadir marcadores al mapa principal
        function addMarkersToMap() {
            // Limpiar marcadores existentes
            map.eachLayer(function(layer) {
                if (layer instanceof L.Marker && !layer._isUserLocation) {
                    map.removeLayer(layer);
                }
            });
            
            // Agrupar animales por ubicación para clusters
            const locationGroups = {};
            
            animales.forEach(a => {
                const locationKey = `${a.lat.toFixed(3)},${a.lng.toFixed(3)}`;
                if (!locationGroups[locationKey]) {
                    locationGroups[locationKey] = [];
                }
                locationGroups[locationKey].push(a);
            });
            
            // Añadir marcadores individuales o clusters
            Object.keys(locationGroups).forEach(key => {
                const animalsInLocation = locationGroups[key];
                const [lat, lng] = key.split(',').map(Number);
                
                if (animalsInLocation.length === 1) {
                    // Marcador individual
                    addSingleMarker(animalsInLocation[0]);
                } else {
                    // Cluster de marcadores
                    addClusterMarker(animalsInLocation, lat, lng);
                }
            });
        }

        // Añadir marcador individual
        function addSingleMarker(animal) {
            // Determinar color según estado de salud
            let color;
            if (animal.estadoSalud === 'good') color = '#10b981'; // Verde
            else if (animal.estadoSalud === 'moderate') color = '#f59e0b'; // Amarillo
            else color = '#ef4444'; // Rojo
            
            // Determinar icono según especie
            let iconHtml;
            if (animal.especie === 'perro') {
                iconHtml = `<div style="background-color: ${color}; width: 30px; height: 30px; border-radius: 50%; border: 3px solid white; box-shadow: 0 0 5px rgba(0,0,0,0.5); display: flex; align-items: center; justify-content: center; color: white; font-size: 16px;">🐕</div>`;
            } else if (animal.especie === 'gato') {
                iconHtml = `<div style="background-color: ${color}; width: 30px; height: 30px; border-radius: 50%; border: 3px solid white; box-shadow: 0 0 5px rgba(0,0,0,0.5); display: flex; align-items: center; justify-content: center; color: white; font-size: 16px;">🐈</div>`;
            } else {
                iconHtml = `<div style="background-color: ${color}; width: 30px; height: 30px; border-radius: 50%; border: 3px solid white; box-shadow: 0 0 5px rgba(0,0,0,0.5); display: flex; align-items: center; justify-content: center; color: white; font-size: 16px;">🐾</div>`;
            }
            
            let icon = L.divIcon({
                className: 'custom-icon',
                html: iconHtml,
                iconSize: [30, 30],
                iconAnchor: [15, 15]
            });
            
            L.marker([animal.lat, animal.lng], {icon: icon}).addTo(map)
                .bindPopup(`
                    <div style="min-width: 200px;">
                        <h3 style="margin: 0 0 10px; color: ${color};">${animal.nombre}</h3>
                        <p><strong>Especie:</strong> ${animal.especie}</p>
                        <p><strong>Estado:</strong> ${getEstadoSaludTexto(animal.estadoSalud)}</p>
                        <p>${animal.desc}</p>
                        <p><small>Reportado el ${new Date(animal.fecha).toLocaleDateString('es-ES')}</small></p>
                    </div>
                `);
        }

        // Añadir marcador de cluster
        function addClusterMarker(animals, lat, lng) {
            const iconHtml = `
                <div class="cluster-marker" style="width: 40px; height: 40px;">
                    ${animals.length}
                </div>
            `;
            
            const icon = L.divIcon({
                className: 'cluster-icon',
                html: iconHtml,
                iconSize: [40, 40],
                iconAnchor: [20, 20]
            });
            
            const marker = L.marker([lat, lng], {icon: icon}).addTo(map);
            
            // Crear contenido para el popup del cluster
            let popupContent = `<div style="max-width: 300px;"><h3>${animals.length} animales en esta zona</h3><ul style="padding-left: 20px; margin: 10px 0;">`;
            
            animals.forEach(animal => {
                popupContent += `<li>${animal.nombre} - ${animal.especie} (${getEstadoSaludTexto(animal.estadoSalud)})</li>`;
            });
            
            popupContent += `</ul></div>`;
            
            marker.bindPopup(popupContent);
        }

        // 4. FORMULARIO MULTIPASO
        function initMultiStepForm() {
            const formSections = document.querySelectorAll('.form-section');
            const formSteps = document.querySelectorAll('.form-step');
            
            // Ocultar todas las secciones excepto la primera
            formSections.forEach((section, index) => {
                if (index > 0) {
                    section.style.display = 'none';
                }
            });
            
            // Añadir eventos a los campos para validación en tiempo real
            const requiredFields = document.querySelectorAll('[required]');
            requiredFields.forEach(field => {
                field.addEventListener('blur', function() {
                    validateField(this);
                });
            });
        }

        function validateField(field) {
            if (!field.value.trim()) {
                field.style.borderColor = '#ef4444';
                return false;
            } else {
                field.style.borderColor = '#10b981';
                return true;
            }
        }

        // Obtener texto del estado de salud
        function getEstadoSaludTexto(estado) {
            if (estado === 'good') return 'Estado bueno';
            if (estado === 'moderate') return 'Estado moderado';
            return 'Estado crítico';
        }

        // Renderizar tarjetas de animales
        function renderAnimalCards() {
            const container = document.getElementById('animal-cards-container');
            container.innerHTML = '';
            
            animales.forEach(animal => {
                const card = document.createElement('div');
                card.className = 'animal-card';
                
                // Determinar emoji según especie
                const emoji = animal.especie === 'perro' ? '🐕' : (animal.especie === 'gato' ? '🐈' : '🐾');
                
                // Determinar clase de salud
                let saludClass = 'health-good';
                if (animal.estadoSalud === 'moderate') saludClass = 'health-moderate';
                else if (animal.estadoSalud === 'critical') saludClass = 'health-critical';
                
                card.innerHTML = `
                    <div class="animal-img">
                        <div style="font-size: 4rem;">${emoji}</div>
                    </div>
                    <div class="animal-info">
                        <h3>${animal.nombre}</h3>
                        <p><strong>Especie:</strong> ${animal.especie}</p>
                        <p><strong>Ubicación:</strong> ${getUbicacionTexto(animal.lat, animal.lng)}</p>
                        <p><strong>Estado de salud:</strong> <span class="health-indicator ${saludClass}"></span> ${getEstadoSaludTexto(animal.estadoSalud)}</p>
                        <p>${animal.desc}</p>
                        <span class="animal-tag">${animal.tipo === 'callejero' ? 'Animal callejero' : (animal.tipo === 'perdido' ? 'Animal perdido' : 'Animal herido')}</span>
                    </div>
                `;
                
                container.appendChild(card);
            });
        }

        // Obtener texto de ubicación aproximada
        function getUbicacionTexto(lat, lng) {
            // Simulación de ubicaciones basadas en coordenadas
            if (lat > -33.480) return "Plaza El Llano";
            if (lat < -33.490) return "Mall Florida Center";
            if (lng < -70.590) return "Av. Vicuña Mackenna";
            return "Centro de La Florida";
        }

        // Generar eventos con fechas realistas
        function generarEventos() {
            const hoy = new Date();
            const eventos = [];
            
            // Evento 1: 2 semanas desde hoy
            const fecha1 = new Date(hoy);
            fecha1.setDate(fecha1.getDate() + 14);
            
            // Evento 2: 1 mes desde hoy
            const fecha2 = new Date(hoy);
            fecha2.setMonth(fecha2.getMonth() + 1);
            
            // Evento 3: 2 meses desde hoy
            const fecha3 = new Date(hoy);
            fecha3.setMonth(fecha3.getMonth() + 2);
            
            // Evento 4: 3 meses desde hoy
            const fecha4 = new Date(hoy);
            fecha4.setMonth(fecha4.getMonth() + 3);
            
            // Evento 5: 4 meses desde hoy
            const fecha5 = new Date(hoy);
            fecha5.setMonth(fecha5.getMonth() + 4);
            
            // Evento 6: 5 meses desde hoy
            const fecha6 = new Date(hoy);
            fecha6.setMonth(fecha6.getMonth() + 5);
            
            return [
                {
                    id: 1,
                    titulo: "Campaña de Esterilización Gratuita",
                    descripcion: "Jornada de esterilización gratuita para perros y gatos de la comuna. Inscripciones previas requeridas.",
                    fecha: fecha1.toISOString().split('T')[0],
                    organizacion: "Fundación Ayuda Animal",
                    inscritos: 24
                },
                {
                    id: 2,
                    titulo: "Feria de Adopción Responsable",
                    descripcion: "Ven a conocer a nuestros animales en busca de un hogar. Requisitos para adopción: documento de identidad y compromiso de cuidado.",
                    fecha: fecha2.toISOString().split('T')[0],
                    organizacion: "Refugio La Florida",
                    inscritos: 18
                },
                {
                    id: 3,
                    titulo: "Taller de Primeros Auxilios para Mascotas",
                    descripcion: "Aprende a actuar en situaciones de emergencia con tu mascota. Incluye demostraciones prácticas.",
                    fecha: fecha3.toISOString().split('T')[0],
                    organizacion: "Veterinarios Solidarios",
                    inscritos: 32
                },
                {
                    id: 4,
                    titulo: "Jornada de Vacunación Antirrábica",
                    descripcion: "Vacunación gratuita contra la rabia para perros y gatos. Trae a tu mascota con correa o transportadora.",
                    fecha: fecha4.toISOString().split('T')[0],
                    organizacion: "Municipalidad de La Florida",
                    inscritos: 45
                },
                {
                    id: 5,
                    titulo: "Maratón Canina por los Animales Callejeros",
                    descripcion: "Carrera familiar con tu perro para recaudar fondos para el refugio municipal. Premios para los participantes.",
                    fecha: fecha5.toISOString().split('T')[0],
                    organizacion: "Corre por una Causa",
                    inscritos: 67
                },
                {
                    id: 6,
                    titulo: "Charla sobre Tenencia Responsable",
                    descripcion: "Conoce tus responsabilidades como dueño de mascota y cómo contribuir al bienestar animal en la comuna.",
                    fecha: fecha6.toISOString().split('T')[0],
                    organizacion: "Educación Animal Chile",
                    inscritos: 28
                }
            ];
        }

        // Renderizar eventos
        function renderEventos() {
            const eventos = generarEventos();
            const container = document.getElementById('eventos-container');
            container.innerHTML = '';
            
            eventos.forEach(evento => {
                const card = document.createElement('div');
                card.className = 'event-card';
                
                card.innerHTML = `
                    <div class="event-img">
                        <div style="font-size: 3rem; color: var(--primary);">
                            <i class="fas fa-calendar-check"></i>
                        </div>
                    </div>
                    <div class="event-info">
                        <span class="event-date">${formatearFecha(evento.fecha)}</span>
                        <h3>${evento.titulo}</h3>
                        <p class="event-organization">Organizado por: ${evento.organizacion}</p>
                        <p>${evento.descripcion}</p>
                        <p><strong>Inscritos:</strong> ${evento.inscritos} personas</p>
                        <button class="btn" onclick="inscribirseEnEvento(${evento.id})" style="margin-top: 1rem;">
                            <i class="fas fa-user-plus"></i> Inscribirse
                        </button>
                    </div>
                `;
                
                container.appendChild(card);
            });
        }

        // Formatear fecha
        function formatearFecha(fechaStr) {
            const fecha = new Date(fechaStr);
            return fecha.toLocaleDateString('es-ES', { day: 'numeric', month: 'long', year: 'numeric' });
        }

        // Inscribirse en evento
        function inscribirseEnEvento(eventoId) {
            const eventos = generarEventos();
            const evento = eventos.find(e => e.id === eventoId);
            if (evento) {
                evento.inscritos++;
                showNotification(`¡Te has inscrito en "${evento.titulo}"!`);
                renderEventos(); // Actualizar la interfaz
            }
        }

        // Actualizar estadísticas
        function updateStats() {
            document.getElementById('total-animales').textContent = animales.length;
        }

        // Función para cambiar entre pestañas
        function showTab(tabName) {
            // Ocultar todas las pestañas
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // Mostrar la pestaña seleccionada
            document.getElementById(tabName + '-tab').classList.add('active');
            
            // Actualizar estado de los botones de pestaña
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // Encontrar el botón correspondiente y activarlo
            const tabButtons = document.querySelectorAll('.tab');
            for (let i = 0; i < tabButtons.length; i++) {
                if (tabButtons[i].textContent.includes(tabName === 'mapa' ? 'Mapa' : 
                                                      tabName === 'reporte' ? 'Reporte' : 'Eventos')) {
                    tabButtons[i].classList.add('active');
                    break;
                }
            }
            
            // Ajustar el mapa cuando se muestra la pestaña
            setTimeout(function() {
                if (tabName === 'mapa') {
                    map.invalidateSize();
                } else if (tabName === 'reporte') {
                    mapForm.invalidateSize();
                }
            }, 100);
        }

        // Función para previsualizar foto
        function previewPhoto(event) {
            const input = event.target;
            const preview = document.getElementById('foto-preview');
            
            if (input.files && input.files[0]) {
                const reader = new FileReader();
                
                reader.onload = function(e) {
                    preview.src = e.target.result;
                    preview.style.display = 'block';
                }
                
                reader.readAsDataURL(input.files[0]);
            }
        }

        // Mostrar notificación
        function showNotification(mensaje) {
            const notification = document.getElementById('notification');
            const notificationText = document.getElementById('notification-text');
            
            notificationText.textContent = mensaje;
            notification.classList.add('show');
            
            setTimeout(function() {
                notification.classList.remove('show');
            }, 3000);
        }

        // Manejar el envío del formulario
        document.getElementById('reporte-form').addEventListener('submit', function(e) {
            e.preventDefault();
            
            // Obtener datos del formulario
            const nombre = document.getElementById('nombre').value;
            const especie = document.getElementById('especie').value;
            const estadoSalud = document.getElementById('estado-salud').value;
            const descripcion = document.getElementById('descripcion').value;
            const direccion = document.getElementById('direccion').value;
            const tipoReporte = document.querySelector('input[name="tipo-reporte"]:checked').value;
            
            // Validar campos obligatorios
            if (!especie || !descripcion) {
                showNotification('Por favor, complete los campos obligatorios: especie y descripción.');
                return;
            }
            
            // Obtener ubicación del marcador
            let lat, lng;
            if (markerForm) {
                const latLng = markerForm.getLatLng();
                lat = latLng.lat;
                lng = latLng.lng;
            } else {
                // Ubicación por defecto si no se seleccionó en el mapa
                lat = -33.485 + (Math.random() * 0.02 - 0.01);
                lng = -70.582 + (Math.random() * 0.02 - 0.01);
            }
            
            // Crear nuevo animal
            const nuevoAnimal = {
                id: Date.now(),
                nombre: nombre || 'Sin nombre',
                especie: especie,
                estadoSalud: estadoSalud,
                desc: descripcion,
                lat: lat,
                lng: lng,
                tipo: tipoReporte,
                fecha: new Date().toISOString().split('T')[0]
            };
            
            // Añadir a la lista de animales
            animales.push(nuevoAnimal);
            
            // Guardar en localStorage
            localStorage.setItem('animales', JSON.stringify(animales));
            
            // Actualizar la interfaz
            addMarkersToMap();
            renderAnimalCards();
            updateStats();
            
            // Mostrar notificación
            showNotification('¡Reporte enviado con éxito!');
            
            // Limpiar formulario
            this.reset();
            document.getElementById('foto-preview').style.display = 'none';
            
            // Eliminar marcador del mapa del formulario
            if (markerForm) {
                mapForm.removeLayer(markerForm);
                markerForm = null;
            }
            
            // Cambiar a la pestaña del mapa
            showTab('mapa');
        });

        // Inicializar la aplicación cuando se carga la página
        document.addEventListener('DOMContentLoaded', initApp);
    </script>
</body>
</html>
