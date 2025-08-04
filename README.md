<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Horario Universitario</title>
    <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Quicksand', sans-serif;
            background: linear-gradient(135deg, #fce4ec 0%, #f8bbd9 50%, #f48fb1 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(233, 30, 99, 0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #e91e63, #ad1457);
            color: white;
            text-align: center;
            padding: 30px;
        }

        .header h1 {
            font-size: 2.5rem;
            font-weight: 600;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.1rem;
            opacity: 0.9;
            font-weight: 400;
        }

        .instructions {
            background: rgba(252, 228, 236, 0.7);
            padding: 15px;
            margin: 20px;
            border-radius: 15px;
            text-align: center;
            font-size: 0.9rem;
            color: #880e4f;
        }

        .mobile-instructions {
            display: none;
        }

        .desktop-instructions {
            display: inline;
        }

        .schedule-grid {
            display: grid;
            grid-template-columns: 120px repeat(7, 1fr);
            gap: 1px;
            background: #f8bbd9;
            margin: 20px;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 25px rgba(233, 30, 99, 0.15);
        }

        .time-header, .day-header {
            background: linear-gradient(135deg, #e91e63, #c2185b);
            color: white;
            padding: 15px 10px;
            text-align: center;
            font-weight: 600;
            font-size: 0.95rem;
        }

        .time-slot {
            background: #fce4ec;
            padding: 15px 8px;
            text-align: center;
            font-weight: 500;
            font-size: 0.85rem;
            color: #880e4f;
            border-right: 1px solid #f8bbd9;
        }

        .class-slot {
            background: white;
            padding: 8px;
            text-align: center;
            font-size: 0.8rem;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
        }

        .class-slot:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.2);
        }

        .class-slot.empty {
            background: #fce4ec;
            opacity: 0.5;
        }

        .class-slot.editing {
            background: #fff3e0 !important;
            border: 2px solid #ff9800;
        }

        .class-slot input {
            background: transparent;
            border: none;
            text-align: center;
            font-family: inherit;
            font-size: inherit;
            font-weight: 600;
            color: inherit;
            width: 100%;
            outline: none;
        }

        /* Colores predefinidos para materias */
        .procesos-industriales {
            background: linear-gradient(135deg, #ff9a9e, #fecfef);
            color: #880e4f;
            font-weight: 500;
        }

        .fisica {
            background: linear-gradient(135deg, #ffecd2, #fcb69f);
            color: #bf360c;
            font-weight: 500;
        }

        .matematicas {
            background: linear-gradient(135deg, #a8edea, #fed6e3);
            color: #004d40;
            font-weight: 500;
        }

        .literatura {
            background: linear-gradient(135deg, #89f7fe, #66a6ff);
            color: #0d47a1;
            font-weight: 500;
        }

        .diseno {
            background: linear-gradient(135deg, #fa709a, #fee140);
            color: #e65100;
            font-weight: 500;
        }

        .fundamentos {
            background: linear-gradient(135deg, #a8caba, #5d4e75);
            color: white;
            font-weight: 500;
        }

        /* Colores adicionales para personalización */
        .color-1 { background: linear-gradient(135deg, #667eea, #764ba2); color: white; }
        .color-2 { background: linear-gradient(135deg, #f093fb, #f5576c); color: white; }
        .color-3 { background: linear-gradient(135deg, #4facfe, #00f2fe); color: white; }
        .color-4 { background: linear-gradient(135deg, #43e97b, #38f9d7); color: #004d40; }
        .color-5 { background: linear-gradient(135deg, #fa709a, #fee140); color: #e65100; }
        .color-6 { background: linear-gradient(135deg, #a8edea, #fed6e3); color: #004d40; }
        .color-7 { background: linear-gradient(135deg, #ffecd2, #fcb69f); color: #bf360c; }
        .color-8 { background: linear-gradient(135deg, #ff9a9e, #fecfef); color: #880e4f; }

        .class-name {
            font-weight: 600;
            font-size: 0.75rem;
            line-height: 1.2;
        }

        .class-code {
            font-size: 0.65rem;
            opacity: 0.8;
            margin-top: 2px;
        }

        .context-menu {
            position: fixed;
            background: white;
            border: 1px solid #ddd;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
            padding: 8px;
            z-index: 1000;
            display: none;
        }

        .context-menu-item {
            padding: 8px 12px;
            cursor: pointer;
            border-radius: 4px;
            font-size: 0.85rem;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .context-menu-item:hover {
            background: #f5f5f5;
        }

        .color-preview {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            border: 2px solid white;
            box-shadow: 0 2px 4px rgba(0,0,0,0.2);
        }

        .legend {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 15px;
            padding: 20px;
            background: rgba(252, 228, 236, 0.5);
        }

        .legend-item {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 8px 12px;
            background: white;
            border-radius: 20px;
            box-shadow: 0 3px 10px rgba(233, 30, 99, 0.1);
            font-size: 0.8rem;
            font-weight: 500;
        }

        .legend-color {
            width: 20px;
            height: 20px;
            border-radius: 50%;
        }

        /* Calendario de Evaluaciones */
        .calendar-section {
            margin: 20px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(233, 30, 99, 0.15);
            overflow: hidden;
        }

        .calendar-header {
            background: linear-gradient(135deg, #e91e63, #ad1457);
            color: white;
            padding: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .calendar-header h2 {
            margin: 0;
            font-size: 1.5rem;
            font-weight: 600;
        }

        .calendar-controls {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .calendar-controls button {
            background: rgba(255, 255, 255, 0.2);
            border: none;
            color: white;
            width: 35px;
            height: 35px;
            border-radius: 50%;
            font-size: 1.2rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .calendar-controls button:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: scale(1.1);
        }

        .calendar-controls span {
            font-size: 1.1rem;
            font-weight: 500;
            min-width: 150px;
            text-align: center;
        }

        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 1px;
            background: #f8bbd9;
            padding: 1px;
        }

        .calendar-day-header {
            background: linear-gradient(135deg, #f8bbd9, #f48fb1);
            color: #880e4f;
            padding: 10px;
            text-align: center;
            font-weight: 600;
            font-size: 0.9rem;
        }

        .calendar-day {
            background: white;
            min-height: 80px;
            padding: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            border: 2px solid transparent;
        }

        .calendar-day:hover {
            background: #fce4ec;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.2);
        }

        .calendar-day.today {
            background: linear-gradient(135deg, #e91e63, #ad1457);
            color: white;
            font-weight: bold;
        }

        .calendar-day.today:hover {
            background: linear-gradient(135deg, #c2185b, #880e4f);
        }

        .calendar-day.other-month {
            background: #f5f5f5;
            color: #ccc;
        }

        .calendar-day.other-month:hover {
            background: #f0f0f0;
        }

        .day-number {
            font-weight: 600;
            margin-bottom: 5px;
        }

        .evaluation-item {
            background: rgba(233, 30, 99, 0.1);
            border-radius: 4px;
            padding: 2px 4px;
            margin: 2px 0;
            font-size: 0.7rem;
            display: flex;
            align-items: center;
            gap: 2px;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .evaluation-item:hover {
            background: rgba(233, 30, 99, 0.2);
            transform: scale(1.02);
        }

        .evaluation-item.completed {
            opacity: 0.6;
            text-decoration: line-through;
        }

        .evaluation-item .checkbox {
            width: 12px;
            height: 12px;
            border: 1px solid #e91e63;
            border-radius: 2px;
            margin-right: 4px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 8px;
        }

        .evaluation-item .checkbox.checked {
            background: #e91e63;
            color: white;
        }

        .evaluation-legend {
            display: flex;
            justify-content: center;
            gap: 20px;
            padding: 15px;
            background: rgba(252, 228, 236, 0.5);
            flex-wrap: wrap;
        }

        .evaluation-legend .legend-item {
            display: flex;
            align-items: center;
            gap: 5px;
            font-size: 0.85rem;
            color: #880e4f;
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            z-index: 2000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
        }

        .modal-content {
            background: white;
            margin: 10% auto;
            padding: 0;
            border-radius: 15px;
            width: 90%;
            max-width: 500px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
        }

        .modal-header {
            background: linear-gradient(135deg, #e91e63, #ad1457);
            color: white;
            padding: 20px;
            border-radius: 15px 15px 0 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .modal-header h3 {
            margin: 0;
            font-size: 1.3rem;
        }

        .close {
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
            line-height: 1;
        }

        .close:hover {
            opacity: 0.7;
        }

        .modal-body {
            padding: 20px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
            color: #880e4f;
        }

        .form-group input,
        .form-group select {
            width: 100%;
            padding: 10px;
            border: 2px solid #f8bbd9;
            border-radius: 8px;
            font-family: inherit;
            font-size: 0.9rem;
            transition: border-color 0.3s ease;
        }

        .form-group input:focus,
        .form-group select:focus {
            outline: none;
            border-color: #e91e63;
        }

        .form-buttons {
            display: flex;
            gap: 10px;
            justify-content: flex-end;
            margin-top: 20px;
        }

        .form-buttons button {
            padding: 10px 20px;
            border: none;
            border-radius: 8px;
            font-family: inherit;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        #saveEvaluation {
            background: linear-gradient(135deg, #e91e63, #ad1457);
            color: white;
        }

        #saveEvaluation:hover {
            background: linear-gradient(135deg, #c2185b, #880e4f);
            transform: translateY(-2px);
        }

        #cancelEvaluation {
            background: #f5f5f5;
            color: #666;
        }

        #cancelEvaluation:hover {
            background: #e0e0e0;
        }

        /* Sección de Organización Personal */
        .personal-organization {
            margin: 20px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(233, 30, 99, 0.15);
            overflow: hidden;
        }

        .section-header {
            background: linear-gradient(135deg, #e91e63, #ad1457);
            color: white;
            padding: 25px;
            text-align: center;
        }

        .section-header h2 {
            margin: 0;
            font-size: 1.8rem;
            font-weight: 600;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .organization-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            padding: 20px;
        }

        .organization-card {
            background: rgba(252, 228, 236, 0.3);
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.1);
            transition: transform 0.3s ease;
        }

        .organization-card:hover {
            transform: translateY(-5px);
        }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid rgba(233, 30, 99, 0.2);
        }

        .card-header h3 {
            margin: 0;
            color: #880e4f;
            font-size: 1.2rem;
            font-weight: 600;
        }

        .add-task-btn, .add-goal-btn, .add-habit-btn {
            background: linear-gradient(135deg, #e91e63, #ad1457);
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .add-task-btn:hover, .add-goal-btn:hover, .add-habit-btn:hover {
            background: linear-gradient(135deg, #c2185b, #880e4f);
            transform: scale(1.05);
        }

        /* To-Do List Styles */
        .todo-sections {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .priority-section h4 {
            margin: 0 0 10px 0;
            font-size: 1rem;
            font-weight: 600;
        }

        .priority-high { color: #d32f2f; }
        .priority-medium { color: #f57c00; }
        .priority-low { color: #388e3c; }

        .task-list {
            min-height: 40px;
            background: white;
            border-radius: 8px;
            padding: 10px;
            border: 2px dashed rgba(233, 30, 99, 0.2);
        }

        .task-item {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 8px;
            margin: 5px 0;
            background: rgba(233, 30, 99, 0.05);
            border-radius: 6px;
            transition: all 0.3s ease;
        }

        .task-item:hover {
            background: rgba(233, 30, 99, 0.1);
            transform: translateX(5px);
        }

        .task-item.completed {
            opacity: 0.6;
            text-decoration: line-through;
        }

        .task-checkbox {
            width: 18px;
            height: 18px;
            border: 2px solid #e91e63;
            border-radius: 4px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            color: white;
        }

        .task-checkbox.checked {
            background: #e91e63;
        }

        .task-text {
            flex: 1;
            font-size: 0.9rem;
            color: #880e4f;
        }

        .delete-task {
            color: #d32f2f;
            cursor: pointer;
            font-size: 1.2rem;
            opacity: 0.7;
            transition: opacity 0.3s ease;
        }

        .delete-task:hover {
            opacity: 1;
        }

        /* Goals Styles */
        .goals-sections {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .goal-category h4 {
            margin: 0 0 10px 0;
            font-size: 1rem;
            font-weight: 600;
            color: #880e4f;
        }

        .goals-list {
            min-height: 40px;
            background: white;
            border-radius: 8px;
            padding: 10px;
            border: 2px dashed rgba(233, 30, 99, 0.2);
        }

        .goal-item {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 8px;
            margin: 5px 0;
            background: rgba(233, 30, 99, 0.05);
            border-radius: 6px;
            transition: all 0.3s ease;
        }

        .goal-item:hover {
            background: rgba(233, 30, 99, 0.1);
            transform: translateX(5px);
        }

        .goal-item.completed {
            opacity: 0.6;
            text-decoration: line-through;
        }

        .goal-checkbox {
            width: 18px;
            height: 18px;
            border: 2px solid #e91e63;
            border-radius: 4px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            color: white;
        }

        .goal-checkbox.checked {
            background: #e91e63;
        }

        .goal-text {
            flex: 1;
            font-size: 0.9rem;
            color: #880e4f;
        }

        .delete-goal {
            color: #d32f2f;
            cursor: pointer;
            font-size: 1.2rem;
            opacity: 0.7;
            transition: opacity 0.3s ease;
        }

        .delete-goal:hover {
            opacity: 1;
        }

        /* Habits Tracker Styles */
        .habits-card {
            grid-column: span 2;
        }

        .habits-tracker {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .habit-row {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 10px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(233, 30, 99, 0.1);
        }

        .habit-name {
            min-width: 150px;
            font-weight: 600;
            color: #880e4f;
            font-size: 0.9rem;
        }

        .habit-days {
            display: flex;
            gap: 5px;
            flex: 1;
        }

        .habit-day {
            width: 30px;
            height: 30px;
            border: 2px solid rgba(233, 30, 99, 0.3);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 0.8rem;
            font-weight: 600;
            color: #880e4f;
            transition: all 0.3s ease;
        }

        .habit-day:hover {
            border-color: #e91e63;
            transform: scale(1.1);
        }

        .habit-day.completed {
            background: linear-gradient(135deg, #e91e63, #ad1457);
            color: white;
            border-color: #e91e63;
        }

        .habit-day.today {
            border-color: #e91e63;
            border-width: 3px;
        }

        .delete-habit {
            color: #d32f2f;
            cursor: pointer;
            font-size: 1.2rem;
            opacity: 0.7;
            transition: opacity 0.3s ease;
        }

        .delete-habit:hover {
            opacity: 1;
        }

        /* Mood Tracker Styles */
        .mood-card {
            grid-column: span 2;
        }

        .mood-date {
            font-size: 0.9rem;
            color: rgba(255, 255, 255, 0.8);
            font-weight: 500;
        }

        .mood-selector {
            margin-bottom: 20px;
        }

        .mood-options {
            display: flex;
            justify-content: space-between;
            gap: 10px;
            margin-bottom: 10px;
        }

        .mood-option {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.8rem;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            box-shadow: 0 3px 10px rgba(233, 30, 99, 0.1);
        }

        .mood-option:hover {
            transform: scale(1.2);
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.2);
        }

        .mood-option.selected {
            background: linear-gradient(135deg, #e91e63, #ad1457);
            transform: scale(1.2);
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.3);
        }

        .mood-labels {
            display: flex;
            justify-content: space-between;
            gap: 10px;
        }

        .mood-labels span {
            font-size: 0.7rem;
            color: #880e4f;
            text-align: center;
            width: 50px;
            font-weight: 500;
        }

        .reflection-box {
            background: white;
            border-radius: 8px;
            padding: 15px;
            box-shadow: 0 3px 10px rgba(233, 30, 99, 0.1);
        }

        .reflection-box textarea {
            width: 100%;
            min-height: 80px;
            border: none;
            resize: vertical;
            font-family: inherit;
            font-size: 0.9rem;
            color: #880e4f;
            background: transparent;
            outline: none;
        }

        .reflection-box button {
            background: linear-gradient(135deg, #e91e63, #ad1457);
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            cursor: pointer;
            margin-top: 10px;
            transition: all 0.3s ease;
        }

        .reflection-box button:hover {
            background: linear-gradient(135deg, #c2185b, #880e4f);
            transform: translateY(-2px);
        }

        /* Estilos del Sistema de Seguimiento Personal */
        .tracking-section {
            margin: 20px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(233, 30, 99, 0.15);
            overflow: hidden;
        }

        .tracking-subtitle {
            margin: 5px 0 0 0;
            font-size: 1rem;
            opacity: 0.9;
            font-weight: 400;
        }

        .tracking-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            padding: 30px;
        }

        /* Panel de Hoy */
        .today-tracking {
            grid-column: span 2;
            background: rgba(252, 228, 236, 0.3);
            border-radius: 15px;
            padding: 25px;
            text-align: center;
        }

        .today-tracking h3 {
            margin: 0 0 10px 0;
            color: #880e4f;
            font-size: 1.4rem;
            font-weight: 600;
        }

        .today-date {
            color: #ad1457;
            font-size: 1rem;
            margin-bottom: 25px;
            font-weight: 500;
        }

        .today-activities {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .activity-card {
            background: white;
            border-radius: 15px;
            padding: 20px;
            display: flex;
            align-items: center;
            gap: 15px;
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.1);
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .activity-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(233, 30, 99, 0.2);
        }

        .activity-card.completed {
            background: linear-gradient(135deg, #e8f5e8, #f1f8e9);
            border: 2px solid #4caf50;
        }

        .activity-icon {
            font-size: 2.5rem;
            flex-shrink: 0;
        }

        .activity-info {
            flex: 1;
            text-align: left;
        }

        .activity-info h4 {
            margin: 0 0 5px 0;
            color: #880e4f;
            font-size: 1.1rem;
            font-weight: 600;
        }

        .activity-info p {
            margin: 0;
            color: #ad1457;
            font-size: 0.9rem;
            opacity: 0.8;
        }

        .activity-toggle {
            width: 60px;
            height: 30px;
            background: #ddd;
            border-radius: 15px;
            position: relative;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .activity-toggle.active {
            background: linear-gradient(135deg, #4caf50, #45a049);
        }

        .toggle-circle {
            width: 26px;
            height: 26px;
            background: white;
            border-radius: 50%;
            position: absolute;
            top: 2px;
            left: 2px;
            transition: all 0.3s ease;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }

        .activity-toggle.active .toggle-circle {
            transform: translateX(30px);
        }

        .daily-score {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
        }

        .score-circle {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            background: linear-gradient(135deg, #e91e63, #ad1457);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            position: relative;
            box-shadow: 0 10px 30px rgba(233, 30, 99, 0.3);
        }

        .score-number {
            font-size: 2.5rem;
            font-weight: 700;
        }

        .score-total {
            font-size: 1.2rem;
            opacity: 0.8;
        }

        .score-message {
            font-size: 1.1rem;
            color: #880e4f;
            font-weight: 600;
        }

        /* Progreso Semanal */
        .weekly-progress {
            background: rgba(252, 228, 236, 0.3);
            border-radius: 15px;
            padding: 25px;
        }

        .weekly-progress h3 {
            margin: 0 0 20px 0;
            color: #880e4f;
            font-size: 1.3rem;
            font-weight: 600;
            text-align: center;
        }

        .week-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 10px;
            margin-bottom: 25px;
        }

        .week-day {
            background: white;
            border-radius: 10px;
            padding: 15px 8px;
            text-align: center;
            box-shadow: 0 3px 10px rgba(233, 30, 99, 0.1);
        }

        .week-day.today {
            border: 2px solid #e91e63;
            background: rgba(233, 30, 99, 0.05);
        }

        .week-day-name {
            font-size: 0.8rem;
            color: #880e4f;
            font-weight: 600;
            margin-bottom: 8px;
        }

        .week-day-number {
            font-size: 1.1rem;
            color: #ad1457;
            font-weight: 600;
            margin-bottom: 10px;
        }

        .week-activities {
            display: flex;
            justify-content: center;
            gap: 3px;
        }

        .week-activity {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.7rem;
            border: 2px solid #ddd;
        }

        .week-activity.completed {
            border-color: #4caf50;
            background: #4caf50;
            color: white;
        }

        .weekly-stats {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .stat-card {
            background: white;
            border-radius: 10px;
            padding: 15px;
            display: flex;
            align-items: center;
            gap: 15px;
            box-shadow: 0 3px 10px rgba(233, 30, 99, 0.1);
        }

        .stat-icon {
            font-size: 2rem;
            flex-shrink: 0;
        }

        .stat-info {
            flex: 1;
        }

        .stat-number {
            font-size: 1.8rem;
            font-weight: 700;
            color: #e91e63;
            margin-bottom: 2px;
        }

        .stat-label {
            font-size: 0.9rem;
            color: #880e4f;
            margin-bottom: 8px;
        }

        .progress-bar {
            width: 100%;
            height: 8px;
            background: #f0f0f0;
            border-radius: 4px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            border-radius: 4px;
            transition: width 0.5s ease;
        }

        .study-progress {
            background: linear-gradient(135deg, #2196f3, #1976d2);
        }

        .gym-progress {
            background: linear-gradient(135deg, #ff9800, #f57c00);
        }

        .protein-progress {
            background: linear-gradient(135deg, #4caf50, #388e3c);
        }

        /* Progreso Mensual */
        .monthly-progress {
            background: rgba(252, 228, 236, 0.3);
            border-radius: 15px;
            padding: 25px;
        }

        .monthly-progress h3 {
            margin: 0 0 20px 0;
            color: #880e4f;
            font-size: 1.3rem;
            font-weight: 600;
            text-align: center;
        }

        .month-selector {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
            margin-bottom: 25px;
        }

        .month-selector button {
            background: linear-gradient(135deg, #e91e63, #ad1457);
            color: white;
            border: none;
            width: 35px;
            height: 35px;
            border-radius: 50%;
            font-size: 1.2rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .month-selector button:hover {
            background: linear-gradient(135deg, #c2185b, #880e4f);
            transform: scale(1.1);
        }

        .month-selector span {
            font-size: 1.1rem;
            font-weight: 600;
            color: #880e4f;
            min-width: 120px;
            text-align: center;
        }

        .monthly-stats {
            display: flex;
            flex-direction: column;
            gap: 20px;
            margin-bottom: 25px;
        }

        .month-stat-card {
            background: white;
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.1);
        }

        .month-stat-header {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 15px;
        }

        .month-stat-icon {
            font-size: 1.5rem;
        }

        .month-stat-title {
            font-size: 1.1rem;
            font-weight: 600;
            color: #880e4f;
        }

        .month-stat-number {
            font-size: 2.5rem;
            font-weight: 700;
            color: #e91e63;
            margin-bottom: 5px;
        }

        .month-stat-total {
            font-size: 0.9rem;
            color: #ad1457;
            margin-bottom: 15px;
        }

        .month-progress-bar {
            width: 100%;
            height: 12px;
            background: #f0f0f0;
            border-radius: 6px;
            overflow: hidden;
            margin-bottom: 10px;
        }

        .month-progress-fill {
            height: 100%;
            border-radius: 6px;
            transition: width 0.8s ease;
        }

        .month-percentage {
            text-align: right;
            font-size: 1rem;
            font-weight: 600;
            color: #880e4f;
        }

        .monthly-achievements {
            background: white;
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.1);
        }

        .monthly-achievements h4 {
            margin: 0 0 15px 0;
            color: #880e4f;
            font-size: 1.1rem;
            font-weight: 600;
            text-align: center;
        }

        .achievements-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
        }

        .achievement-item {
            background: rgba(252, 228, 236, 0.5);
            border-radius: 10px;
            padding: 15px;
            text-align: center;
            border: 2px solid transparent;
            transition: all 0.3s ease;
        }

        .achievement-item.unlocked {
            border-color: #ffd700;
            background: linear-gradient(135deg, #fff9c4, #ffeaa7);
        }

        .achievement-icon {
            font-size: 2rem;
            margin-bottom: 8px;
        }

        .achievement-title {
            font-size: 0.9rem;
            font-weight: 600;
            color: #880e4f;
            margin-bottom: 5px;
        }

        .achievement-desc {
            font-size: 0.8rem;
            color: #ad1457;
            opacity: 0.8;
        }

        /* Progreso Anual */
        .yearly-progress {
            grid-column: span 2;
            background: rgba(252, 228, 236, 0.3);
            border-radius: 15px;
            padding: 25px;
        }

        .yearly-progress h3 {
            margin: 0 0 25px 0;
            color: #880e4f;
            font-size: 1.4rem;
            font-weight: 600;
            text-align: center;
        }

        .year-overview {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .year-stat {
            background: white;
            border-radius: 15px;
            padding: 25px;
            text-align: center;
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.1);
        }

        .year-stat-icon {
            font-size: 3rem;
            margin-bottom: 15px;
        }

        .year-stat-number {
            font-size: 2.8rem;
            font-weight: 700;
            color: #e91e63;
            margin-bottom: 8px;
        }

        .year-stat-label {
            font-size: 1rem;
            color: #880e4f;
            margin-bottom: 10px;
        }

        .year-stat-streak {
            font-size: 0.9rem;
            color: #ad1457;
            font-weight: 600;
        }

        .year-chart {
            background: white;
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 25px;
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.1);
        }

        .year-chart h4 {
            margin: 0 0 20px 0;
            color: #880e4f;
            font-size: 1.2rem;
            font-weight: 600;
            text-align: center;
        }

        .months-chart {
            display: grid;
            grid-template-columns: repeat(12, 1fr);
            gap: 8px;
        }

        .month-bar {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
        }

        .month-name {
            font-size: 0.7rem;
            color: #880e4f;
            font-weight: 600;
            writing-mode: vertical-rl;
            text-orientation: mixed;
        }

        .month-bars {
            display: flex;
            flex-direction: column;
            gap: 2px;
            height: 100px;
            justify-content: flex-end;
        }

        .month-activity-bar {
            width: 20px;
            border-radius: 2px;
            min-height: 4px;
            transition: all 0.3s ease;
        }

        .year-achievements {
            background: white;
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.1);
        }

        .year-achievements h4 {
            margin: 0 0 20px 0;
            color: #880e4f;
            font-size: 1.2rem;
            font-weight: 600;
            text-align: center;
        }

        .year-achievements-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
        }

        @media (max-width: 768px) {
            .tracking-container {
                grid-template-columns: 1fr;
                gap: 20px;
                padding: 20px;
            }

            .today-tracking {
                grid-column: span 1;
            }

            .yearly-progress {
                grid-column: span 1;
            }

            .today-activities {
                grid-template-columns: 1fr;
            }

            .activity-card {
                padding: 15px;
            }

            .activity-icon {
                font-size: 2rem;
            }

            .score-circle {
                width: 100px;
                height: 100px;
            }

            .score-number {
                font-size: 2rem;
            }

            .year-overview {
                grid-template-columns: 1fr;
            }

            .months-chart {
                grid-template-columns: repeat(6, 1fr);
                gap: 5px;
            }

            .month-name {
                font-size: 0.6rem;
            }

            .month-activity-bar {
                width: 15px;
            }

            .tracking-section {
                margin: 10px;
            }
        }

        /* Estilos del Timer Pomodoro */
        .pomodoro-section {
            margin: 20px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(233, 30, 99, 0.15);
            overflow: hidden;
        }

        .pomodoro-subtitle {
            margin: 5px 0 0 0;
            font-size: 1rem;
            opacity: 0.9;
            font-weight: 400;
        }

        .pomodoro-container {
            display: grid;
            grid-template-columns: 2fr 1fr 1fr;
            gap: 30px;
            padding: 30px;
            align-items: start;
        }

        .pomodoro-main {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 25px;
        }

        /* Reloj de Arena */
        .hourglass-container {
            position: relative;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .hourglass {
            width: 120px;
            height: 180px;
            position: relative;
            filter: drop-shadow(0 10px 20px rgba(233, 30, 99, 0.3));
        }

        .hourglass-top, .hourglass-bottom {
            width: 100%;
            height: 70px;
            position: absolute;
            overflow: hidden;
        }

        .hourglass-top {
            top: 0;
            background: linear-gradient(135deg, rgba(233, 30, 99, 0.1), rgba(173, 20, 87, 0.1));
            border-radius: 60px 60px 0 0;
            border: 3px solid #e91e63;
            border-bottom: none;
        }

        .hourglass-bottom {
            bottom: 0;
            background: linear-gradient(135deg, rgba(233, 30, 99, 0.1), rgba(173, 20, 87, 0.1));
            border-radius: 0 0 60px 60px;
            border: 3px solid #e91e63;
            border-top: none;
        }

        .hourglass-middle {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 20px;
            height: 20px;
            background: #e91e63;
            border-radius: 50%;
            z-index: 10;
        }

        .sand-top, .sand-bottom {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background: linear-gradient(135deg, #ffd700, #ffb347);
            border-radius: 0 0 50px 50px;
            transition: height 1s ease-in-out;
        }

        .sand-top {
            height: 100%;
            border-radius: 50px 50px 0 0;
        }

        .sand-bottom {
            height: 0%;
            border-radius: 0 0 50px 50px;
        }

        .hourglass-frame {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            border: 3px solid #e91e63;
            border-radius: 60px;
            background: transparent;
            z-index: 5;
        }

        .hourglass-frame::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 30px;
            height: 2px;
            background: #e91e63;
            z-index: 15;
        }

        /* Animación de la arena */
        .hourglass.running .sand-top {
            animation: sandFalling var(--timer-duration) linear forwards;
        }

        .hourglass.running .sand-bottom {
            animation: sandRising var(--timer-duration) linear forwards;
        }

        @keyframes sandFalling {
            from { height: 100%; }
            to { height: 0%; }
        }

        @keyframes sandRising {
            from { height: 0%; }
            to { height: 100%; }
        }

        /* Display del Timer */
        .timer-display {
            text-align: center;
            margin: 20px 0;
        }

        .timer-time {
            font-size: 3.5rem;
            font-weight: 700;
            color: #e91e63;
            text-shadow: 2px 2px 4px rgba(233, 30, 99, 0.3);
            margin-bottom: 10px;
            font-family: 'Courier New', monospace;
        }

        .timer-session {
            font-size: 1.3rem;
            font-weight: 600;
            color: #880e4f;
            margin-bottom: 5px;
        }

        .timer-cycle {
            font-size: 1rem;
            color: #ad1457;
            font-weight: 500;
        }

        /* Controles */
        .timer-controls {
            display: flex;
            gap: 15px;
            justify-content: center;
        }

        .control-btn {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            padding: 15px 20px;
            border: none;
            border-radius: 15px;
            font-family: inherit;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.2);
        }

        .start-btn {
            background: linear-gradient(135deg, #4caf50, #45a049);
            color: white;
        }

        .start-btn:hover {
            background: linear-gradient(135deg, #45a049, #3d8b40);
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(76, 175, 80, 0.3);
        }

        .pause-btn {
            background: linear-gradient(135deg, #ff9800, #f57c00);
            color: white;
        }

        .pause-btn:hover {
            background: linear-gradient(135deg, #f57c00, #ef6c00);
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(255, 152, 0, 0.3);
        }

        .reset-btn {
            background: linear-gradient(135deg, #f44336, #d32f2f);
            color: white;
        }

        .reset-btn:hover {
            background: linear-gradient(135deg, #d32f2f, #c62828);
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(244, 67, 54, 0.3);
        }

        .btn-icon {
            font-size: 1.5rem;
        }

        .btn-text {
            font-size: 0.9rem;
        }

        /* Panel de Configuración */
        .pomodoro-config {
            background: rgba(252, 228, 236, 0.3);
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.1);
        }

        .pomodoro-config h3 {
            margin: 0 0 20px 0;
            color: #880e4f;
            font-size: 1.2rem;
            text-align: center;
        }

        .config-group {
            margin-bottom: 15px;
        }

        .config-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
            color: #880e4f;
            font-size: 0.9rem;
        }

        .config-group input {
            width: 100%;
            padding: 10px;
            border: 2px solid #f8bbd9;
            border-radius: 8px;
            font-family: inherit;
            font-size: 0.9rem;
            text-align: center;
            font-weight: 600;
            color: #880e4f;
        }

        .config-group input:focus {
            outline: none;
            border-color: #e91e63;
            box-shadow: 0 0 10px rgba(233, 30, 99, 0.2);
        }

        .config-save-btn {
            width: 100%;
            padding: 12px;
            background: linear-gradient(135deg, #e91e63, #ad1457);
            color: white;
            border: none;
            border-radius: 8px;
            font-family: inherit;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-top: 10px;
        }

        .config-save-btn:hover {
            background: linear-gradient(135deg, #c2185b, #880e4f);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.3);
        }

        /* Estadísticas */
        .pomodoro-stats {
            background: rgba(252, 228, 236, 0.3);
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(233, 30, 99, 0.1);
        }

        .pomodoro-stats h3 {
            margin: 0 0 20px 0;
            color: #880e4f;
            font-size: 1.2rem;
            text-align: center;
        }

        .stats-grid {
            display: grid;
            gap: 15px;
            margin-bottom: 20px;
        }

        .stat-item {
            text-align: center;
            background: white;
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 3px 10px rgba(233, 30, 99, 0.1);
        }

        .stat-number {
            font-size: 1.8rem;
            font-weight: 700;
            color: #e91e63;
            margin-bottom: 5px;
        }

        .stat-label {
            font-size: 0.9rem;
            color: #880e4f;
            font-weight: 500;
        }

        .reset-stats-btn {
            width: 100%;
            padding: 10px;
            background: linear-gradient(135deg, #757575, #616161);
            color: white;
            border: none;
            border-radius: 8px;
            font-family: inherit;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.85rem;
        }

        .reset-stats-btn:hover {
            background: linear-gradient(135deg, #616161, #424242);
            transform: translateY(-2px);
        }

        @media (max-width: 768px) {
            .pomodoro-container {
                grid-template-columns: 1fr;
                gap: 20px;
                padding: 20px;
            }

            .hourglass {
                width: 100px;
                height: 150px;
            }

            .timer-time {
                font-size: 2.5rem;
            }

            .timer-controls {
                flex-wrap: wrap;
                gap: 10px;
            }

            .control-btn {
                padding: 12px 15px;
                min-width: 80px;
            }

            .btn-icon {
                font-size: 1.2rem;
            }

            .btn-text {
                font-size: 0.8rem;
            }

            .pomodoro-section {
                margin: 10px;
            }
        }

        /* Contenedor sin scroll para móvil */
        .schedule-container {
            overflow: visible;
        }

        /* Indicador de scroll oculto */
        .scroll-indicator {
            display: none;
        }

        @media (max-width: 768px) {
            .mobile-instructions {
                display: inline;
            }

            .desktop-instructions {
                display: none;
            }

            .instructions {
                margin: 10px;
                padding: 12px;
                font-size: 0.8rem;
            }

            .schedule-grid {
                font-size: 0.6rem;
                margin: 10px;
                grid-template-columns: 60px repeat(7, 1fr);
                gap: 0.5px;
                width: 100%;
                max-width: 100%;
                overflow: visible;
            }
            
            .schedule-container {
                margin: 0;
                border-radius: 15px;
                overflow: visible;
                box-shadow: 0 10px 25px rgba(233, 30, 99, 0.15);
            }
            
            .scroll-indicator {
                display: none;
            }
            
            .header h1 {
                font-size: 2rem;
            }
            
            .time-header, .day-header {
                padding: 8px 2px;
                font-size: 0.7rem;
            }

            .time-slot {
                padding: 8px 2px;
                font-size: 0.6rem;
                line-height: 1.1;
            }
            
            .class-slot {
                padding: 2px;
                font-size: 0.55rem;
                min-height: 40px;
                line-height: 1.1;
            }

            .class-name {
                font-size: 0.55rem;
                line-height: 1.1;
                margin-bottom: 1px;
            }

            .class-code {
                font-size: 0.45rem;
                margin-top: 1px;
            }

            .calendar-section {
                margin: 10px;
            }

            .calendar-header {
                padding: 12px;
                flex-direction: row;
                justify-content: space-between;
                align-items: center;
            }

            .calendar-header h2 {
                font-size: 1.2rem;
            }

            .calendar-controls {
                display: flex;
                align-items: center;
                gap: 8px;
            }

            .calendar-controls button {
                width: 28px;
                height: 28px;
                font-size: 1rem;
            }

            .calendar-controls span {
                font-size: 0.9rem;
                min-width: 100px;
                text-align: center;
            }

            .calendar-container {
                overflow: visible;
                margin: 0;
            }

            .calendar-grid {
                display: grid;
                grid-template-columns: repeat(7, 1fr);
                gap: 0.5px;
                background: #f8bbd9;
                padding: 0.5px;
                width: 100%;
                max-width: 100%;
            }

            .calendar-day-header {
                background: linear-gradient(135deg, #f8bbd9, #f48fb1);
                color: #880e4f;
                padding: 8px 2px;
                text-align: center;
                font-weight: 600;
                font-size: 0.7rem;
                display: flex;
                align-items: center;
                justify-content: center;
                min-height: 30px;
            }

            .calendar-day {
                background: white;
                min-height: 70px;
                padding: 4px 2px;
                cursor: pointer;
                transition: all 0.3s ease;
                position: relative;
                border: 2px solid transparent;
                display: flex;
                flex-direction: column;
                font-size: 0.65rem;
            }

            .calendar-day:hover {
                background: #fce4ec;
                transform: translateY(-1px);
                box-shadow: 0 3px 10px rgba(233, 30, 99, 0.2);
            }

            .calendar-day.today {
                background: linear-gradient(135deg, #e91e63, #ad1457);
                color: white;
                font-weight: bold;
            }

            .calendar-day.today:hover {
                background: linear-gradient(135deg, #c2185b, #880e4f);
            }

            .calendar-day.other-month {
                background: #f5f5f5;
                color: #ccc;
            }

            .calendar-day.other-month:hover {
                background: #f0f0f0;
            }

            .day-number {
                font-weight: 600;
                margin-bottom: 2px;
                font-size: 0.75rem;
                text-align: center;
            }

            .evaluation-item {
                background: rgba(233, 30, 99, 0.15);
                border-radius: 3px;
                padding: 2px 1px;
                margin: 1px 0;
                font-size: 0.55rem;
                display: flex;
                align-items: center;
                gap: 2px;
                cursor: pointer;
                transition: all 0.2s ease;
                line-height: 1.1;
                overflow: hidden;
                text-overflow: ellipsis;
                white-space: nowrap;
                max-width: 100%;
            }

            .evaluation-item:hover {
                background: rgba(233, 30, 99, 0.25);
                transform: scale(1.02);
            }

            .evaluation-item .checkbox {
                width: 10px;
                height: 10px;
                border: 1px solid #e91e63;
                border-radius: 2px;
                margin-right: 2px;
                cursor: pointer;
                display: flex;
                align-items: center;
                justify-content: center;
                font-size: 6px;
                flex-shrink: 0;
            }

            .evaluation-legend {
                flex-direction: column;
                align-items: center;
                gap: 10px;
            }

            .modal-content {
                width: 95%;
                margin: 5% auto;
                max-height: 90vh;
                overflow-y: auto;
            }
            
            .modal-header {
                position: sticky;
                top: 0;
                z-index: 10;
            }
            
            .form-group input,
            .form-group select {
                font-size: 16px; /* Evita zoom en iOS */
                padding: 12px;
            }

            .personal-organization {
                margin: 10px;
            }

            .organization-grid {
                grid-template-columns: 1fr;
                padding: 15px;
            }

            .habits-card, .mood-card {
                grid-column: span 1;
            }

            .mood-options {
                flex-wrap: wrap;
                justify-content: center;
            }

            .mood-labels {
                flex-wrap: wrap;
                justify-content: center;
            }

            .habit-name {
                min-width: 120px;
                font-size: 0.8rem;
            }

            .habit-day {
                width: 25px;
                height: 25px;
                font-size: 0.7rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>✨ Mi Horario Universitario ✨</h1>
            <p>Semana académica organizada con amor 💕</p>
        </div>

        <div class="instructions">
            💡 <strong>Instrucciones:</strong> 
            <span class="desktop-instructions">Doble clic para editar • Click derecho para cambiar color</span>
            <span class="mobile-instructions">Toca para editar • Mantén presionado para cambiar color</span>
            • Los cambios se guardan automáticamente
        </div>

        <div class="scroll-indicator">
            👆 Desliza horizontalmente para ver todos los días →
        </div>

        <div class="schedule-container">
            <div class="schedule-grid" id="scheduleGrid">
            <!-- Headers -->
            <div class="time-header">Horario</div>
            <div class="day-header">Lunes</div>
            <div class="day-header">Martes</div>
            <div class="day-header">Miércoles</div>
            <div class="day-header">Jueves</div>
            <div class="day-header">Viernes</div>
            <div class="day-header">Sábado</div>
            <div class="day-header">Domingo</div>

            <!-- 07:00 - 08:00 -->
            <div class="time-slot">07:00<br>08:00</div>
            <div class="class-slot empty" data-time="07:00" data-day="lunes"></div>
            <div class="class-slot empty" data-time="07:00" data-day="martes"></div>
            <div class="class-slot empty" data-time="07:00" data-day="miercoles"></div>
            <div class="class-slot empty" data-time="07:00" data-day="jueves"></div>
            <div class="class-slot empty" data-time="07:00" data-day="viernes"></div>
            <div class="class-slot empty" data-time="07:00" data-day="sabado"></div>
            <div class="class-slot empty" data-time="07:00" data-day="domingo"></div>

            <!-- 08:30 - 09:40 -->
            <div class="time-slot">08:30<br>09:40</div>
            <div class="class-slot procesos-industriales" data-time="08:30" data-day="lunes">
                <div>
                    <div class="class-name">Procesos Industriales</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="08:30" data-day="martes"></div>
            <div class="class-slot empty" data-time="08:30" data-day="miercoles"></div>
            <div class="class-slot fisica" data-time="08:30" data-day="jueves">
                <div>
                    <div class="class-name">Física III</div>
                    <div class="class-code">FIS301 (J1)</div>
                </div>
            </div>
            <div class="class-slot fundamentos" data-time="08:30" data-day="viernes">
                <div>
                    <div class="class-name">Fundamentos de Ciencias</div>
                    <div class="class-code">TICS314 (V1)</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="08:30" data-day="sabado"></div>
            <div class="class-slot empty" data-time="08:30" data-day="domingo"></div>

            <!-- 10:00 - 11:10 -->
            <div class="time-slot">10:00<br>11:10</div>
            <div class="class-slot procesos-industriales" data-time="10:00" data-day="lunes">
                <div>
                    <div class="class-name">Procesos Industriales</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="10:00" data-day="martes"></div>
            <div class="class-slot empty" data-time="10:00" data-day="miercoles"></div>
            <div class="class-slot empty" data-time="10:00" data-day="jueves"></div>
            <div class="class-slot fundamentos" data-time="10:00" data-day="viernes">
                <div>
                    <div class="class-name">Fundamentos de Ciencias</div>
                    <div class="class-code">TICS314 (V2)</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="10:00" data-day="sabado"></div>
            <div class="class-slot empty" data-time="10:00" data-day="domingo"></div>

            <!-- 11:30 - 12:40 -->
            <div class="time-slot">11:30<br>12:40</div>
            <div class="class-slot procesos-industriales" data-time="11:30" data-day="lunes">
                <div>
                    <div class="class-name">Procesos Industriales</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="11:30" data-day="martes"></div>
            <div class="class-slot empty" data-time="11:30" data-day="miercoles"></div>
            <div class="class-slot empty" data-time="11:30" data-day="jueves"></div>
            <div class="class-slot empty" data-time="11:30" data-day="viernes"></div>
            <div class="class-slot empty" data-time="11:30" data-day="sabado"></div>
            <div class="class-slot empty" data-time="11:30" data-day="domingo"></div>

            <!-- 13:00 - 14:10 -->
            <div class="time-slot">13:00<br>14:10</div>
            <div class="class-slot empty" data-time="13:00" data-day="lunes"></div>
            <div class="class-slot literatura" data-time="13:00" data-day="martes">
                <div>
                    <div class="class-name">Literatura y Humanidades</div>
                    <div class="class-code">CORE202 (M4A)</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="13:00" data-day="miercoles"></div>
            <div class="class-slot empty" data-time="13:00" data-day="jueves"></div>
            <div class="class-slot literatura" data-time="13:00" data-day="viernes">
                <div>
                    <div class="class-name">Literatura y Humanidades</div>
                    <div class="class-code">CORE202 (V4A)</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="13:00" data-day="sabado"></div>
            <div class="class-slot empty" data-time="13:00" data-day="domingo"></div>

            <!-- 13:30 - 14:40 -->
            <div class="time-slot">13:30<br>14:40</div>
            <div class="class-slot fisica" data-time="13:30" data-day="lunes">
                <div>
                    <div class="class-name">Física III</div>
                    <div class="class-code">FIS301 (L4B)</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="13:30" data-day="martes"></div>
            <div class="class-slot diseno" data-time="13:30" data-day="miercoles">
                <div>
                    <div class="class-name">Taller de Diseño en Ingeniería</div>
                    <div class="class-code">TEI201 (W4B)</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="13:30" data-day="jueves"></div>
            <div class="class-slot empty" data-time="13:30" data-day="viernes"></div>
            <div class="class-slot empty" data-time="13:30" data-day="sabado"></div>
            <div class="class-slot empty" data-time="13:30" data-day="domingo"></div>

            <!-- 15:00 - 16:10 -->
            <div class="time-slot">15:00<br>16:10</div>
            <div class="class-slot fisica" data-time="15:00" data-day="lunes">
                <div>
                    <div class="class-name">Física III</div>
                    <div class="class-code">FIS301 (L5)</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="15:00" data-day="martes"></div>
            <div class="class-slot diseno" data-time="15:00" data-day="miercoles">
                <div>
                    <div class="class-name">Taller de Diseño en Ingeniería</div>
                    <div class="class-code">TEI201 (W5)</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="15:00" data-day="jueves"></div>
            <div class="class-slot empty" data-time="15:00" data-day="viernes"></div>
            <div class="class-slot empty" data-time="15:00" data-day="sabado"></div>
            <div class="class-slot empty" data-time="15:00" data-day="domingo"></div>

            <!-- 16:30 - 17:40 -->
            <div class="time-slot">16:30<br>17:40</div>
            <div class="class-slot matematicas" data-time="16:30" data-day="lunes">
                <div>
                    <div class="class-name">Ecuaciones Diferenciales</div>
                    <div class="class-code">MAT208 (L6)</div>
                </div>
            </div>
            <div class="class-slot matematicas" data-time="16:30" data-day="martes">
                <div>
                    <div class="class-name">Ecuaciones Diferenciales</div>
                    <div class="class-code">MAT208 (M6)</div>
                </div>
            </div>
            <div class="class-slot matematicas" data-time="16:30" data-day="miercoles">
                <div>
                    <div class="class-name">Ecuaciones Diferenciales</div>
                    <div class="class-code">MAT208 (W6)</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="16:30" data-day="jueves"></div>
            <div class="class-slot matematicas" data-time="16:30" data-day="viernes">
                <div>
                    <div class="class-name">Ecuaciones Diferenciales</div>
                    <div class="class-code">MAT208 (V6)</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="16:30" data-day="sabado"></div>
            <div class="class-slot empty" data-time="16:30" data-day="domingo"></div>

            <!-- 18:00 - 19:10 -->
            <div class="time-slot">18:00<br>19:10</div>
            <div class="class-slot empty" data-time="18:00" data-day="lunes"></div>
            <div class="class-slot empty" data-time="18:00" data-day="martes"></div>
            <div class="class-slot fundamentos" data-time="18:00" data-day="miercoles">
                <div>
                    <div class="class-name">Fundamentos de Ciencias</div>
                    <div class="class-code">TICS314 (W7)</div>
                </div>
            </div>
            <div class="class-slot empty" data-time="18:00" data-day="jueves"></div>
            <div class="class-slot empty" data-time="18:00" data-day="viernes"></div>
            <div class="class-slot empty" data-time="18:00" data-day="sabado"></div>
            <div class="class-slot empty" data-time="18:00" data-day="domingo"></div>

            <!-- 20:00 - 21:00 -->
            <div class="time-slot">20:00<br>21:00</div>
            <div class="class-slot empty" data-time="20:00" data-day="lunes"></div>
            <div class="class-slot empty" data-time="20:00" data-day="martes"></div>
            <div class="class-slot empty" data-time="20:00" data-day="miercoles"></div>
            <div class="class-slot empty" data-time="20:00" data-day="jueves"></div>
            <div class="class-slot empty" data-time="20:00" data-day="viernes"></div>
            <div class="class-slot empty" data-time="20:00" data-day="sabado"></div>
            <div class="class-slot empty" data-time="20:00" data-day="domingo"></div>

            <!-- 21:00 - 22:00 -->
            <div class="time-slot">21:00<br>22:00</div>
            <div class="class-slot empty" data-time="21:00" data-day="lunes"></div>
            <div class="class-slot empty" data-time="21:00" data-day="martes"></div>
            <div class="class-slot empty" data-time="21:00" data-day="miercoles"></div>
            <div class="class-slot empty" data-time="21:00" data-day="jueves"></div>
            <div class="class-slot empty" data-time="21:00" data-day="viernes"></div>
            <div class="class-slot empty" data-time="21:00" data-day="sabado"></div>
            <div class="class-slot empty" data-time="21:00" data-day="domingo"></div>

            <!-- 22:00 - 23:00 -->
            <div class="time-slot">22:00<br>23:00</div>
            <div class="class-slot empty" data-time="22:00" data-day="lunes"></div>
            <div class="class-slot empty" data-time="22:00" data-day="martes"></div>
            <div class="class-slot empty" data-time="22:00" data-day="miercoles"></div>
            <div class="class-slot empty" data-time="22:00" data-day="jueves"></div>
            <div class="class-slot empty" data-time="22:00" data-day="viernes"></div>
            <div class="class-slot empty" data-time="22:00" data-day="sabado"></div>
            <div class="class-slot empty" data-time="22:00" data-day="domingo"></div>
            </div>
        </div>

        <div class="legend">
            <div class="legend-item">
                <div class="legend-color" style="background: linear-gradient(135deg, #ff9a9e, #fecfef);"></div>
                <span>Procesos Industriales</span>
            </div>
            <div class="legend-item">
                <div class="legend-color" style="background: linear-gradient(135deg, #ffecd2, #fcb69f);"></div>
                <span>Física III</span>
            </div>
            <div class="legend-item">
                <div class="legend-color" style="background: linear-gradient(135deg, #a8edea, #fed6e3);"></div>
                <span>Ecuaciones Diferenciales</span>
            </div>
            <div class="legend-item">
                <div class="legend-color" style="background: linear-gradient(135deg, #89f7fe, #66a6ff);"></div>
                <span>Literatura y Humanidades</span>
            </div>
            <div class="legend-item">
                <div class="legend-color" style="background: linear-gradient(135deg, #fa709a, #fee140);"></div>
                <span>Taller de Diseño</span>
            </div>
            <div class="legend-item">
                <div class="legend-color" style="background: linear-gradient(135deg, #a8caba, #5d4e75);"></div>
                <span>Fundamentos de Ciencias</span>
            </div>
        </div>

        <!-- Calendario de Evaluaciones -->
        <div class="calendar-section">
            <div class="calendar-header">
                <h2>📅 Calendario de Evaluaciones</h2>
                <div class="calendar-controls">
                    <button id="prevMonth">‹</button>
                    <span id="currentMonth"></span>
                    <button id="nextMonth">›</button>
                </div>
            </div>
            
            <div class="calendar-container">
                <div class="calendar-grid" id="calendarGrid">
                    <!-- Calendar will be generated here -->
                </div>
            </div>

            <div class="evaluation-legend">
                <div class="legend-item">
                    <span>📚</span> Académico
                </div>
                <div class="legend-item">
                    <span>🏋️‍♀️</span> Actividades
                </div>
                <div class="legend-item">
                    <span>🦷</span> Salud
                </div>
                <div class="legend-item">
                    <span>👫</span> Personal
                </div>
                <div class="legend-item">
                    <span>💼</span> Trabajo
                </div>
                <div class="legend-item">
                    <span>📱</span> Contenido
                </div>
            </div>
        </div>

        <!-- Sistema de Seguimiento Personal -->
        <div class="tracking-section">
            <div class="section-header">
                <h2>📊 MI SEGUIMIENTO PERSONAL</h2>
                <p class="tracking-subtitle">Estudios • Gym • Proteína - ¡Construyendo hábitos exitosos! 💪</p>
            </div>
            
            <div class="tracking-container">
                <!-- Panel de Hoy -->
                <div class="today-tracking">
                    <h3>🌟 ¿Cómo va tu día hoy?</h3>
                    <div class="today-date" id="trackingDate"></div>
                    
                    <div class="today-activities">
                        <div class="activity-card" data-activity="study">
                            <div class="activity-icon">📚</div>
                            <div class="activity-info">
                                <h4>Estudié</h4>
                                <p>¿Dedicaste tiempo al estudio?</p>
                            </div>
                            <div class="activity-toggle" id="studyToggle" onclick="toggleActivity('study')">
                                <div class="toggle-circle"></div>
                            </div>
                        </div>
                        
                        <div class="activity-card" data-activity="gym">
                            <div class="activity-icon">🏋️‍♀️</div>
                            <div class="activity-info">
                                <h4>Fui al Gym</h4>
                                <p>¿Hiciste ejercicio hoy?</p>
                            </div>
                            <div class="activity-toggle" id="gymToggle" onclick="toggleActivity('gym')">
                                <div class="toggle-circle"></div>
                            </div>
                        </div>
                        
                        <div class="activity-card" data-activity="protein">
                            <div class="activity-icon">🥤</div>
                            <div class="activity-info">
                                <h4>Tomé Proteína</h4>
                                <p>¿Consumiste tu proteína?</p>
                            </div>
                            <div class="activity-toggle" id="proteinToggle" onclick="toggleActivity('protein')">
                                <div class="toggle-circle"></div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="daily-score">
                        <div class="score-circle">
                            <div class="score-number" id="dailyScore">0</div>
                            <div class="score-total">/3</div>
                        </div>
                        <div class="score-message" id="scoreMessage">¡Comienza tu día!</div>
                    </div>
                </div>
                
                <!-- Progreso Semanal -->
                <div class="weekly-progress">
                    <h3>📅 Progreso de esta Semana</h3>
                    <div class="week-grid" id="weekGrid">
                        <!-- Se genera dinámicamente -->
                    </div>
                    
                    <div class="weekly-stats">
                        <div class="stat-card">
                            <div class="stat-icon">📚</div>
                            <div class="stat-info">
                                <div class="stat-number" id="weekStudy">0</div>
                                <div class="stat-label">días estudiando</div>
                                <div class="progress-bar">
                                    <div class="progress-fill study-progress" id="weekStudyBar"></div>
                                </div>
                            </div>
                        </div>
                        
                        <div class="stat-card">
                            <div class="stat-icon">🏋️‍♀️</div>
                            <div class="stat-info">
                                <div class="stat-number" id="weekGym">0</div>
                                <div class="stat-label">días en el gym</div>
                                <div class="progress-bar">
                                    <div class="progress-fill gym-progress" id="weekGymBar"></div>
                                </div>
                            </div>
                        </div>
                        
                        <div class="stat-card">
                            <div class="stat-icon">🥤</div>
                            <div class="stat-info">
                                <div class="stat-number" id="weekProtein">0</div>
                                <div class="stat-label">días con proteína</div>
                                <div class="progress-bar">
                                    <div class="progress-fill protein-progress" id="weekProteinBar"></div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Progreso Mensual -->
                <div class="monthly-progress">
                    <h3>🗓️ Progreso del Mes</h3>
                    <div class="month-selector">
                        <button onclick="changeMonth(-1)">‹</button>
                        <span id="currentMonthName"></span>
                        <button onclick="changeMonth(1)">›</button>
                    </div>
                    
                    <div class="monthly-stats">
                        <div class="month-stat-card">
                            <div class="month-stat-header">
                                <span class="month-stat-icon">📚</span>
                                <span class="month-stat-title">Estudio</span>
                            </div>
                            <div class="month-stat-number" id="monthStudy">0</div>
                            <div class="month-stat-total" id="monthStudyTotal">/ 30 días</div>
                            <div class="month-progress-bar">
                                <div class="month-progress-fill study-progress" id="monthStudyBar"></div>
                            </div>
                            <div class="month-percentage" id="monthStudyPercent">0%</div>
                        </div>
                        
                        <div class="month-stat-card">
                            <div class="month-stat-header">
                                <span class="month-stat-icon">🏋️‍♀️</span>
                                <span class="month-stat-title">Gym</span>
                            </div>
                            <div class="month-stat-number" id="monthGym">0</div>
                            <div class="month-stat-total" id="monthGymTotal">/ 30 días</div>
                            <div class="month-progress-bar">
                                <div class="month-progress-fill gym-progress" id="monthGymBar"></div>
                            </div>
                            <div class="month-percentage" id="monthGymPercent">0%</div>
                        </div>
                        
                        <div class="month-stat-card">
                            <div class="month-stat-header">
                                <span class="month-stat-icon">🥤</span>
                                <span class="month-stat-title">Proteína</span>
                            </div>
                            <div class="month-stat-number" id="monthProtein">0</div>
                            <div class="month-stat-total" id="monthProteinTotal">/ 30 días</div>
                            <div class="month-progress-bar">
                                <div class="month-progress-fill protein-progress" id="monthProteinBar"></div>
                            </div>
                            <div class="month-percentage" id="monthProteinPercent">0%</div>
                        </div>
                    </div>
                    
                    <div class="monthly-achievements">
                        <h4>🏆 Logros del Mes</h4>
                        <div class="achievements-grid" id="monthlyAchievements">
                            <!-- Se genera dinámicamente -->
                        </div>
                    </div>
                </div>
                
                <!-- Progreso Anual -->
                <div class="yearly-progress">
                    <h3>🎯 Progreso del Año 2025</h3>
                    
                    <div class="year-overview">
                        <div class="year-stat">
                            <div class="year-stat-icon">📚</div>
                            <div class="year-stat-info">
                                <div class="year-stat-number" id="yearStudy">0</div>
                                <div class="year-stat-label">días estudiando</div>
                                <div class="year-stat-streak">Racha: <span id="studyStreak">0</span> días</div>
                            </div>
                        </div>
                        
                        <div class="year-stat">
                            <div class="year-stat-icon">🏋️‍♀️</div>
                            <div class="year-stat-info">
                                <div class="year-stat-number" id="yearGym">0</div>
                                <div class="year-stat-label">días en el gym</div>
                                <div class="year-stat-streak">Racha: <span id="gymStreak">0</span> días</div>
                            </div>
                        </div>
                        
                        <div class="year-stat">
                            <div class="year-stat-icon">🥤</div>
                            <div class="year-stat-info">
                                <div class="year-stat-number" id="yearProtein">0</div>
                                <div class="year-stat-label">días con proteína</div>
                                <div class="year-stat-streak">Racha: <span id="proteinStreak">0</span> días</div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="year-chart">
                        <h4>📈 Progreso por Meses</h4>
                        <div class="months-chart" id="yearChart">
                            <!-- Se genera dinámicamente -->
                        </div>
                    </div>
                    
                    <div class="year-achievements">
                        <h4>🌟 Logros del Año</h4>
                        <div class="year-achievements-grid" id="yearAchievements">
                            <!-- Se genera dinámicamente -->
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Timer Pomodoro -->
        <div class="pomodoro-section">
            <div class="section-header">
                <h2>⏰ TIMER POMODORO</h2>
                <p class="pomodoro-subtitle">Técnica de productividad con estilo ✨</p>
            </div>
            
            <div class="pomodoro-container">
                <div class="pomodoro-main">
                    <!-- Reloj de Arena Animado -->
                    <div class="hourglass-container">
                        <div class="hourglass">
                            <div class="hourglass-top">
                                <div class="sand-top" id="sandTop"></div>
                            </div>
                            <div class="hourglass-middle"></div>
                            <div class="hourglass-bottom">
                                <div class="sand-bottom" id="sandBottom"></div>
                            </div>
                            <div class="hourglass-frame"></div>
                        </div>
                    </div>
                    
                    <!-- Display del Timer -->
                    <div class="timer-display">
                        <div class="timer-time" id="timerTime">25:00</div>
                        <div class="timer-session" id="timerSession">Sesión de Trabajo</div>
                        <div class="timer-cycle">Ciclo <span id="currentCycle">1</span> de 4</div>
                    </div>
                    
                    <!-- Controles -->
                    <div class="timer-controls">
                        <button class="control-btn start-btn" id="startBtn" onclick="startTimer()">
                            <span class="btn-icon">▶️</span>
                            <span class="btn-text">Iniciar</span>
                        </button>
                        <button class="control-btn pause-btn" id="pauseBtn" onclick="pauseTimer()" style="display: none;">
                            <span class="btn-icon">⏸️</span>
                            <span class="btn-text">Pausar</span>
                        </button>
                        <button class="control-btn reset-btn" id="resetBtn" onclick="resetTimer()">
                            <span class="btn-icon">🔄</span>
                            <span class="btn-text">Reiniciar</span>
                        </button>
                    </div>
                </div>
                
                <!-- Panel de Configuración -->
                <div class="pomodoro-config">
                    <h3>⚙️ Configuración</h3>
                    <div class="config-group">
                        <label>🍅 Trabajo (min):</label>
                        <input type="number" id="workTime" value="25" min="1" max="60">
                    </div>
                    <div class="config-group">
                        <label>☕ Descanso corto (min):</label>
                        <input type="number" id="shortBreak" value="5" min="1" max="30">
                    </div>
                    <div class="config-group">
                        <label>🌟 Descanso largo (min):</label>
                        <input type="number" id="longBreak" value="15" min="1" max="60">
                    </div>
                    <button class="config-save-btn" onclick="saveConfig()">💾 Guardar</button>
                </div>
                
                <!-- Estadísticas -->
                <div class="pomodoro-stats">
                    <h3>📊 Estadísticas de Hoy</h3>
                    <div class="stats-grid">
                        <div class="stat-item">
                            <div class="stat-number" id="completedSessions">0</div>
                            <div class="stat-label">Sesiones</div>
                        </div>
                        <div class="stat-item">
                            <div class="stat-number" id="totalTime">0h 0m</div>
                            <div class="stat-label">Tiempo Total</div>
                        </div>
                        <div class="stat-item">
                            <div class="stat-number" id="currentStreak">0</div>
                            <div class="stat-label">Racha</div>
                        </div>
                    </div>
                    <button class="reset-stats-btn" onclick="resetStats()">🗑️ Limpiar Stats</button>
                </div>
            </div>
        </div>

        <!-- Sección de Organización Personal -->
        <div class="personal-organization">
            <div class="section-header">
                <h2>🌱 SECCIÓN ORGANIZACIÓN PERSONAL</h2>
            </div>

            <div class="organization-grid">
                <!-- To-Do List Semanal -->
                <div class="organization-card">
                    <div class="card-header">
                        <h3>📝 To-Do List Semanal</h3>
                        <button class="add-task-btn" onclick="addNewTask()">+ Agregar</button>
                    </div>
                    <div class="todo-sections">
                        <div class="priority-section">
                            <h4 class="priority-high">🔴 Prioridad Alta</h4>
                            <div class="task-list" id="highPriority"></div>
                        </div>
                        <div class="priority-section">
                            <h4 class="priority-medium">🟡 Prioridad Media</h4>
                            <div class="task-list" id="mediumPriority"></div>
                        </div>
                        <div class="priority-section">
                            <h4 class="priority-low">🟢 Prioridad Baja</h4>
                            <div class="task-list" id="lowPriority"></div>
                        </div>
                    </div>
                </div>

                <!-- Metas del Mes -->
                <div class="organization-card">
                    <div class="card-header">
                        <h3>🎯 Metas del Mes</h3>
                        <button class="add-goal-btn" onclick="addNewGoal()">+ Agregar</button>
                    </div>
                    <div class="goals-sections">
                        <div class="goal-category">
                            <h4>📚 Académicas</h4>
                            <div class="goals-list" id="academicGoals"></div>
                        </div>
                        <div class="goal-category">
                            <h4>💖 Personales</h4>
                            <div class="goals-list" id="personalGoals"></div>
                        </div>
                        <div class="goal-category">
                            <h4>🌟 Hábitos Nuevos</h4>
                            <div class="goals-list" id="habitGoals"></div>
                        </div>
                    </div>
                </div>

                <!-- Seguimiento de Hábitos -->
                <div class="organization-card habits-card">
                    <div class="card-header">
                        <h3>📊 Seguimiento de Hábitos</h3>
                        <button class="add-habit-btn" onclick="addNewHabit()">+ Agregar</button>
                    </div>
                    <div class="habits-tracker" id="habitsTracker">
                        <!-- Hábitos predefinidos -->
                        <div class="habit-row" data-habit="sleep">
                            <div class="habit-name">😴 Dormir 8h</div>
                            <div class="habit-days" id="sleep-days"></div>
                        </div>
                        <div class="habit-row" data-habit="study">
                            <div class="habit-name">📖 Estudiar 2h</div>
                            <div class="habit-days" id="study-days"></div>
                        </div>
                        <div class="habit-row" data-habit="water">
                            <div class="habit-name">💧 Tomar agua</div>
                            <div class="habit-days" id="water-days"></div>
                        </div>
                        <div class="habit-row" data-habit="exercise">
                            <div class="habit-name">🏃‍♀️ Ejercicio</div>
                            <div class="habit-days" id="exercise-days"></div>
                        </div>
                    </div>
                </div>

                <!-- Diario de Ánimo -->
                <div class="organization-card mood-card">
                    <div class="card-header">
                        <h3>💭 Diario de Ánimo</h3>
                        <div class="mood-date" id="moodDate"></div>
                    </div>
                    <div class="mood-selector">
                        <div class="mood-options">
                            <div class="mood-option" data-mood="amazing" onclick="selectMood('amazing')">🤩</div>
                            <div class="mood-option" data-mood="happy" onclick="selectMood('happy')">😊</div>
                            <div class="mood-option" data-mood="good" onclick="selectMood('good')">🙂</div>
                            <div class="mood-option" data-mood="okay" onclick="selectMood('okay')">😐</div>
                            <div class="mood-option" data-mood="tired" onclick="selectMood('tired')">😴</div>
                            <div class="mood-option" data-mood="stressed" onclick="selectMood('stressed')">😰</div>
                            <div class="mood-option" data-mood="sad" onclick="selectMood('sad')">😢</div>
                        </div>
                        <div class="mood-labels">
                            <span>Increíble</span>
                            <span>Feliz</span>
                            <span>Bien</span>
                            <span>Normal</span>
                            <span>Cansado</span>
                            <span>Estresado</span>
                            <span>Triste</span>
                        </div>
                    </div>
                    <div class="reflection-box">
                        <textarea id="dailyReflection" placeholder="¿Cómo te sentiste hoy? Escribe una reflexión rápida..."></textarea>
                        <button onclick="saveReflection()">💾 Guardar</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal para agregar tareas -->
    <div class="modal" id="taskModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3>Agregar Nueva Tarea</h3>
                <span class="close" onclick="closeTaskModal()">&times;</span>
            </div>
            <div class="modal-body">
                <div class="form-group">
                    <label>Tarea:</label>
                    <input type="text" id="taskDescription" placeholder="Describe tu tarea...">
                </div>
                <div class="form-group">
                    <label>Prioridad:</label>
                    <select id="taskPriority">
                        <option value="high">🔴 Alta</option>
                        <option value="medium">🟡 Media</option>
                        <option value="low">🟢 Baja</option>
                    </select>
                </div>
                <div class="form-buttons">
                    <button onclick="saveTask()">Guardar</button>
                    <button onclick="closeTaskModal()">Cancelar</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal para agregar metas -->
    <div class="modal" id="goalModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3>Agregar Nueva Meta</h3>
                <span class="close" onclick="closeGoalModal()">&times;</span>
            </div>
            <div class="modal-body">
                <div class="form-group">
                    <label>Meta:</label>
                    <input type="text" id="goalDescription" placeholder="Describe tu meta...">
                </div>
                <div class="form-group">
                    <label>Categoría:</label>
                    <select id="goalCategory">
                        <option value="academic">📚 Académica</option>
                        <option value="personal">💖 Personal</option>
                        <option value="habit">🌟 Hábito Nuevo</option>
                    </select>
                </div>
                <div class="form-buttons">
                    <button onclick="saveGoal()">Guardar</button>
                    <button onclick="closeGoalModal()">Cancelar</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal para agregar hábitos -->
    <div class="modal" id="habitModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3>Agregar Nuevo Hábito</h3>
                <span class="close" onclick="closeHabitModal()">&times;</span>
            </div>
            <div class="modal-body">
                <div class="form-group">
                    <label>Hábito:</label>
                    <input type="text" id="habitDescription" placeholder="Ej: Meditar 10 min">
                </div>
                <div class="form-group">
                    <label>Emoji:</label>
                    <input type="text" id="habitEmoji" placeholder="🧘‍♀️" maxlength="2">
                </div>
                <div class="form-buttons">
                    <button onclick="saveHabit()">Guardar</button>
                    <button onclick="closeHabitModal()">Cancelar</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal para agregar evaluaciones -->
    <div class="modal" id="evaluationModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3>Agregar Evaluación</h3>
                <span class="close" id="closeModal">&times;</span>
            </div>
            <div class="modal-body">
                <div class="form-group">
                    <label>Tipo de evaluación:</label>
                    <select id="evaluationType">
                        <optgroup label="📖 Académico">
                            <option value="📚">📚 Prueba/Examen</option>
                            <option value="📝">📝 Tarea/Trabajo</option>
                            <option value="💬">💬 Presentación</option>
                            <option value="📊">📊 Proyecto</option>
                            <option value="📖">📖 Estudiar</option>
                        </optgroup>
                        <optgroup label="🏃‍♀️ Actividades">
                            <option value="🏋️‍♀️">🏋️‍♀️ Gimnasio</option>
                            <option value="🏃‍♀️">🏃‍♀️ Ejercicio</option>
                            <option value="🧘‍♀️">🧘‍♀️ Yoga/Meditación</option>
                            <option value="🎵">🎵 Música/Hobby</option>
                        </optgroup>
                        <optgroup label="🏥 Salud">
                            <option value="🦷">🦷 Dentista</option>
                            <option value="👩‍⚕️">👩‍⚕️ Médico</option>
                            <option value="💊">💊 Farmacia</option>
                        </optgroup>
                        <optgroup label="👥 Personal">
                            <option value="👨‍👩‍👧‍👦">👨‍👩‍👧‍👦 Familia</option>
                            <option value="👫">👫 Amigos</option>
                            <option value="💕">💕 Cita</option>
                            <option value="🛒">🛒 Compras</option>
                            <option value="🏠">🏠 Casa/Limpieza</option>
                        </optgroup>
                        <optgroup label="💼 Trabajo">
                            <option value="💼">💼 Trabajo</option>
                            <option value="📞">📞 Reunión</option>
                            <option value="✉️">✉️ Trámites</option>
                        </optgroup>
                        <optgroup label="📱 Contenido">
                            <option value="📱">📱 Live TikTok</option>
                            <option value="💡">💡 Emprendimiento</option>
                        </optgroup>
                    </select>
                </div>
                <div class="form-group">
                    <label>Materia:</label>
                    <input type="text" id="evaluationSubject" placeholder="Ej: Física III">
                </div>
                <div class="form-group">
                    <label>Descripción:</label>
                    <input type="text" id="evaluationDescription" placeholder="Ej: Parcial 1 - Cinemática">
                </div>
                <div class="form-group">
                    <label>Hora (opcional):</label>
                    <input type="time" id="evaluationTime">
                </div>
                <div class="form-buttons">
                    <button id="saveEvaluation">Guardar</button>
                    <button id="cancelEvaluation">Cancelar</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Context Menu -->
    <div class="context-menu" id="contextMenu">
        <div class="context-menu-item" data-color="empty">
            <div class="color-preview" style="background: #fce4ec;"></div>
            <span>Vacío</span>
        </div>
        <div class="context-menu-item" data-color="color-1">
            <div class="color-preview" style="background: linear-gradient(135deg, #667eea, #764ba2);"></div>
            <span>Morado</span>
        </div>
        <div class="context-menu-item" data-color="color-2">
            <div class="color-preview" style="background: linear-gradient(135deg, #f093fb, #f5576c);"></div>
            <span>Rosa Intenso</span>
        </div>
        <div class="context-menu-item" data-color="color-3">
            <div class="color-preview" style="background: linear-gradient(135deg, #4facfe, #00f2fe);"></div>
            <span>Azul Cielo</span>
        </div>
        <div class="context-menu-item" data-color="color-4">
            <div class="color-preview" style="background: linear-gradient(135deg, #43e97b, #38f9d7);"></div>
            <span>Verde Menta</span>
        </div>
        <div class="context-menu-item" data-color="color-5">
            <div class="color-preview" style="background: linear-gradient(135deg, #fa709a, #fee140);"></div>
            <span>Rosa Amarillo</span>
        </div>
        <div class="context-menu-item" data-color="color-6">
            <div class="color-preview" style="background: linear-gradient(135deg, #a8edea, #fed6e3);"></div>
            <span>Aqua Rosa</span>
        </div>
        <div class="context-menu-item" data-color="color-7">
            <div class="color-preview" style="background: linear-gradient(135deg, #ffecd2, #fcb69f);"></div>
            <span>Durazno</span>
        </div>
        <div class="context-menu-item" data-color="color-8">
            <div class="color-preview" style="background: linear-gradient(135deg, #ff9a9e, #fecfef);"></div>
            <span>Rosa Suave</span>
        </div>
    </div>

    <script>
        let currentEditingCell = null;
        let contextMenu = document.getElementById('contextMenu');
        let currentContextCell = null;
        
        // Variables del calendario
        let currentDate = new Date();
        let currentMonth = currentDate.getMonth();
        let currentYear = currentDate.getFullYear();
        let selectedDate = null;
        let evaluations = {};

        const monthNames = [
            'Enero', 'Febrero', 'Marzo', 'Abril', 'Mayo', 'Junio',
            'Julio', 'Agosto', 'Septiembre', 'Octubre', 'Noviembre', 'Diciembre'
        ];

        const dayNames = ['Dom', 'Lun', 'Mar', 'Mié', 'Jue', 'Vie', 'Sáb'];

        // Cargar datos guardados
        function loadScheduleData() {
            const savedData = localStorage.getItem('scheduleData');
            if (savedData) {
                const data = JSON.parse(savedData);
                Object.keys(data).forEach(key => {
                    const cell = document.querySelector(`[data-time="${data[key].time}"][data-day="${data[key].day}"]`);
                    if (cell) {
                        cell.innerHTML = data[key].content;
                        cell.className = `class-slot ${data[key].className}`;
                    }
                });
            }
        }

        // Guardar datos del horario
        function saveScheduleData() {
            const cells = document.querySelectorAll('.class-slot[data-time][data-day]');
            const data = {};
            
            cells.forEach((cell, index) => {
                data[index] = {
                    time: cell.dataset.time,
                    day: cell.dataset.day,
                    content: cell.innerHTML,
                    className: cell.className.replace('class-slot ', '')
                };
            });
            
            localStorage.setItem('scheduleData', JSON.stringify(data));
        }

        // Cargar evaluaciones guardadas
        function loadEvaluations() {
            const savedEvaluations = localStorage.getItem('evaluations');
            if (savedEvaluations) {
                evaluations = JSON.parse(savedEvaluations);
            }
        }

        // Guardar evaluaciones
        function saveEvaluations() {
            localStorage.setItem('evaluations', JSON.stringify(evaluations));
        }

        // Generar calendario
        function generateCalendar(month, year) {
            const firstDay = new Date(year, month, 1).getDay();
            const daysInMonth = new Date(year, month + 1, 0).getDate();
            const daysInPrevMonth = new Date(year, month, 0).getDate();
            
            const calendarGrid = document.getElementById('calendarGrid');
            const currentMonthSpan = document.getElementById('currentMonth');
            
            // Mostrar mes y año actual
            currentMonthSpan.textContent = `${monthNames[month]} ${year}`;
            
            let calendarHTML = '';
            
            // Headers de días de la semana
            dayNames.forEach(day => {
                calendarHTML += `<div class="calendar-day-header">${day}</div>`;
            });
            
            // Días del mes anterior (grises)
            for (let i = firstDay - 1; i >= 0; i--) {
                const day = daysInPrevMonth - i;
                const prevMonth = month === 0 ? 11 : month - 1;
                const prevYear = month === 0 ? year - 1 : year;
                const dateKey = `${prevYear}-${prevMonth.toString().padStart(2, '0')}-${day.toString().padStart(2, '0')}`;
                
                calendarHTML += `<div class="calendar-day other-month" data-date="${dateKey}">
                    <div class="day-number">${day}</div>
                </div>`;
            }
            
            // Días del mes actual
            const today = new Date();
            const todayDay = today.getDate();
            const todayMonth = today.getMonth();
            const todayYear = today.getFullYear();
            
            for (let day = 1; day <= daysInMonth; day++) {
                const dateKey = `${year}-${month.toString().padStart(2, '0')}-${day.toString().padStart(2, '0')}`;
                
                // Verificar si es el día de hoy
                const isToday = (year === todayYear && month === todayMonth && day === todayDay);
                const todayClass = isToday ? 'today' : '';
                
                let evaluationsHTML = '';
                if (evaluations[dateKey]) {
                    evaluations[dateKey].forEach((evaluation, index) => {
                        const completedClass = evaluation.completed ? 'completed' : '';
                        const checkedClass = evaluation.completed ? 'checked' : '';
                        evaluationsHTML += `
                            <div class="evaluation-item ${completedClass}" data-date="${dateKey}" data-index="${index}">
                                <div class="checkbox ${checkedClass}" onclick="toggleEvaluation('${dateKey}', ${index})">${evaluation.completed ? '✓' : ''}</div>
                                ${evaluation.type} ${evaluation.subject}
                            </div>
                        `;
                    });
                }
                
                calendarHTML += `<div class="calendar-day ${todayClass}" data-date="${dateKey}" onclick="selectDate('${dateKey}')">
                    <div class="day-number">${day}</div>
                    ${evaluationsHTML}
                </div>`;
            }
            
            // Días del mes siguiente (grises)
            const totalCells = 42; // 6 semanas completas
            const cellsUsed = 7 + firstDay + daysInMonth; // headers + días anteriores + días actuales
            const remainingCells = totalCells - cellsUsed;
            
            for (let day = 1; day <= remainingCells; day++) {
                const nextMonth = month === 11 ? 0 : month + 1;
                const nextYear = month === 11 ? year + 1 : year;
                const dateKey = `${nextYear}-${nextMonth.toString().padStart(2, '0')}-${day.toString().padStart(2, '0')}`;
                
                calendarHTML += `<div class="calendar-day other-month" data-date="${dateKey}">
                    <div class="day-number">${day}</div>
                </div>`;
            }
            
            calendarGrid.innerHTML = calendarHTML;
        }

        // Seleccionar fecha
        function selectDate(dateKey) {
            selectedDate = dateKey;
            document.getElementById('evaluationModal').style.display = 'block';
        }

        // Toggle evaluación completada
        function toggleEvaluation(dateKey, index) {
            if (evaluations[dateKey] && evaluations[dateKey][index]) {
                evaluations[dateKey][index].completed = !evaluations[dateKey][index].completed;
                saveEvaluations();
                generateCalendar(currentMonth, currentYear);
            }
        }

        // Agregar evaluación
        function addEvaluation() {
            if (!selectedDate) return;
            
            const type = document.getElementById('evaluationType').value;
            const subject = document.getElementById('evaluationSubject').value.trim();
            const description = document.getElementById('evaluationDescription').value.trim();
            const time = document.getElementById('evaluationTime').value;
            
            if (!subject || !description) {
                alert('Por favor completa todos los campos obligatorios');
                return;
            }
            
            if (!evaluations[selectedDate]) {
                evaluations[selectedDate] = [];
            }
            
            evaluations[selectedDate].push({
                type: type,
                subject: subject,
                description: description,
                time: time,
                completed: false
            });
            
            saveEvaluations();
            generateCalendar(currentMonth, currentYear);
            closeModal();
        }

        // Cerrar modal
        function closeModal() {
            document.getElementById('evaluationModal').style.display = 'none';
            selectedDate = null;
            
            // Limpiar formulario
            document.getElementById('evaluationType').value = '📚';
            document.getElementById('evaluationSubject').value = '';
            document.getElementById('evaluationDescription').value = '';
            document.getElementById('evaluationTime').value = '';
        }

        // Navegación del calendario
        function previousMonth() {
            currentMonth--;
            if (currentMonth < 0) {
                currentMonth = 11;
                currentYear--;
            }
            generateCalendar(currentMonth, currentYear);
        }

        function nextMonth() {
            currentMonth++;
            if (currentMonth > 11) {
                currentMonth = 0;
                currentYear++;
            }
            generateCalendar(currentMonth, currentYear);
        }

        // Editar celda
        function editCell(cell) {
            if (currentEditingCell) {
                finishEditing();
            }

            currentEditingCell = cell;
            const currentText = cell.textContent.trim();
            
            cell.classList.add('editing');
            cell.innerHTML = `<input type="text" value="${currentText}" maxlength="50">`;
            
            const input = cell.querySelector('input');
            input.focus();
            input.select();

            input.addEventListener('blur', finishEditing);
            input.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    finishEditing();
                }
            });
        }

        function finishEditing() {
            if (!currentEditingCell) return;

            const input = currentEditingCell.querySelector('input');
            const newText = input ? input.value.trim() : '';
            
            currentEditingCell.classList.remove('editing');
            
            if (newText) {
                currentEditingCell.innerHTML = `<div><div class="class-name">${newText}</div></div>`;
                if (currentEditingCell.classList.contains('empty')) {
                    currentEditingCell.classList.remove('empty');
                    currentEditingCell.classList.add('color-1');
                }
            } else {
                currentEditingCell.innerHTML = '';
                currentEditingCell.className = 'class-slot empty';
            }

            saveScheduleData();
            currentEditingCell = null;
        }

        // Context menu
        function showContextMenu(e, cell) {
            e.preventDefault();
            currentContextCell = cell;
            
            contextMenu.style.display = 'block';
            contextMenu.style.left = e.pageX + 'px';
            contextMenu.style.top = e.pageY + 'px';
        }

        function hideContextMenu() {
            contextMenu.style.display = 'none';
            currentContextCell = null;
        }

        // Variables del Sistema de Seguimiento Personal
        let personalTracking = {};
        let trackingMonth = new Date().getMonth();
        let trackingYear = new Date().getFullYear();

        // Variables para organización personal
        let tasks = { high: [], medium: [], low: [] };
        let goals = { academic: [], personal: [], habit: [] };
        let habits = {};
        let moods = {};
        let reflections = {};

        // Funciones para To-Do List
        function addNewTask() {
            document.getElementById('taskModal').style.display = 'block';
        }

        function closeTaskModal() {
            document.getElementById('taskModal').style.display = 'none';
            document.getElementById('taskDescription').value = '';
            document.getElementById('taskPriority').value = 'high';
        }

        function saveTask() {
            const description = document.getElementById('taskDescription').value.trim();
            const priority = document.getElementById('taskPriority').value;
            
            if (!description) {
                alert('Por favor describe la tarea');
                return;
            }
            
            const task = {
                id: Date.now(),
                description: description,
                completed: false
            };
            
            tasks[priority].push(task);
            saveTasks();
            renderTasks();
            closeTaskModal();
        }

        function toggleTask(priority, taskId) {
            const task = tasks[priority].find(t => t.id === taskId);
            if (task) {
                task.completed = !task.completed;
                saveTasks();
                renderTasks();
            }
        }

        function deleteTask(priority, taskId) {
            tasks[priority] = tasks[priority].filter(t => t.id !== taskId);
            saveTasks();
            renderTasks();
        }

        function renderTasks() {
            ['high', 'medium', 'low'].forEach(priority => {
                const container = document.getElementById(priority + 'Priority');
                container.innerHTML = '';
                
                tasks[priority].forEach(task => {
                    const taskElement = document.createElement('div');
                    taskElement.className = `task-item ${task.completed ? 'completed' : ''}`;
                    taskElement.innerHTML = `
                        <div class="task-checkbox ${task.completed ? 'checked' : ''}" onclick="toggleTask('${priority}', ${task.id})">
                            ${task.completed ? '✓' : ''}
                        </div>
                        <div class="task-text">${task.description}</div>
                        <div class="delete-task" onclick="deleteTask('${priority}', ${task.id})">×</div>
                    `;
                    container.appendChild(taskElement);
                });
            });
        }

        function saveTasks() {
            localStorage.setItem('tasks', JSON.stringify(tasks));
        }

        function loadTasks() {
            const savedTasks = localStorage.getItem('tasks');
            if (savedTasks) {
                tasks = JSON.parse(savedTasks);
            }
        }

        // Funciones para Metas
        function addNewGoal() {
            document.getElementById('goalModal').style.display = 'block';
        }

        function closeGoalModal() {
            document.getElementById('goalModal').style.display = 'none';
            document.getElementById('goalDescription').value = '';
            document.getElementById('goalCategory').value = 'academic';
        }

        function saveGoal() {
            const description = document.getElementById('goalDescription').value.trim();
            const category = document.getElementById('goalCategory').value;
            
            if (!description) {
                alert('Por favor describe la meta');
                return;
            }
            
            const goal = {
                id: Date.now(),
                description: description,
                completed: false
            };
            
            goals[category].push(goal);
            saveGoals();
            renderGoals();
            closeGoalModal();
        }

        function toggleGoal(category, goalId) {
            const goal = goals[category].find(g => g.id === goalId);
            if (goal) {
                goal.completed = !goal.completed;
                saveGoals();
                renderGoals();
            }
        }

        function deleteGoal(category, goalId) {
            goals[category] = goals[category].filter(g => g.id !== goalId);
            saveGoals();
            renderGoals();
        }

        function renderGoals() {
            const categoryMap = { academic: 'academicGoals', personal: 'personalGoals', habit: 'habitGoals' };
            
            Object.keys(goals).forEach(category => {
                const container = document.getElementById(categoryMap[category]);
                container.innerHTML = '';
                
                goals[category].forEach(goal => {
                    const goalElement = document.createElement('div');
                    goalElement.className = `goal-item ${goal.completed ? 'completed' : ''}`;
                    goalElement.innerHTML = `
                        <div class="goal-checkbox ${goal.completed ? 'checked' : ''}" onclick="toggleGoal('${category}', ${goal.id})">
                            ${goal.completed ? '✓' : ''}
                        </div>
                        <div class="goal-text">${goal.description}</div>
                        <div class="delete-goal" onclick="deleteGoal('${category}', ${goal.id})">×</div>
                    `;
                    container.appendChild(goalElement);
                });
            });
        }

        function saveGoals() {
            localStorage.setItem('goals', JSON.stringify(goals));
        }

        function loadGoals() {
            const savedGoals = localStorage.getItem('goals');
            if (savedGoals) {
                goals = JSON.parse(savedGoals);
            }
        }

        // Funciones para Hábitos
        function addNewHabit() {
            document.getElementById('habitModal').style.display = 'block';
        }

        function closeHabitModal() {
            document.getElementById('habitModal').style.display = 'none';
            document.getElementById('habitDescription').value = '';
            document.getElementById('habitEmoji').value = '';
        }

        function saveHabit() {
            const description = document.getElementById('habitDescription').value.trim();
            const emoji = document.getElementById('habitEmoji').value.trim() || '⭐';
            
            if (!description) {
                alert('Por favor describe el hábito');
                return;
            }
            
            const habitId = 'habit_' + Date.now();
            habits[habitId] = {
                name: `${emoji} ${description}`,
                days: {}
            };
            
            saveHabits();
            renderHabits();
            closeHabitModal();
        }

        function toggleHabit(habitId, day) {
            if (!habits[habitId].days[day]) {
                habits[habitId].days[day] = true;
            } else {
                delete habits[habitId].days[day];
            }
            saveHabits();
            renderHabits();
        }

        function deleteHabit(habitId) {
            delete habits[habitId];
            saveHabits();
            renderHabits();
        }

        function renderHabits() {
            const container = document.getElementById('habitsTracker');
            const today = new Date();
            const currentWeek = [];
            
            // Generar días de la semana actual
            for (let i = 0; i < 7; i++) {
                const date = new Date(today);
                date.setDate(today.getDate() - today.getDay() + i);
                currentWeek.push(date);
            }
            
            // Limpiar hábitos personalizados anteriores
            const customHabits = container.querySelectorAll('.habit-row:not([data-habit="sleep"]):not([data-habit="study"]):not([data-habit="water"]):not([data-habit="exercise"])');
            customHabits.forEach(habit => habit.remove());
            
            // Renderizar hábitos personalizados
            Object.keys(habits).forEach(habitId => {
                const habitRow = document.createElement('div');
                habitRow.className = 'habit-row';
                habitRow.innerHTML = `
                    <div class="habit-name">${habits[habitId].name}</div>
                    <div class="habit-days" id="${habitId}-days"></div>
                    <div class="delete-habit" onclick="deleteHabit('${habitId}')">×</div>
                `;
                container.appendChild(habitRow);
            });
            
            // Renderizar días para todos los hábitos
            const allHabits = ['sleep', 'study', 'water', 'exercise', ...Object.keys(habits)];
            allHabits.forEach(habitId => {
                const daysContainer = document.getElementById(habitId + '-days');
                if (daysContainer) {
                    daysContainer.innerHTML = '';
                    
                    currentWeek.forEach((date, index) => {
                        const dayKey = date.toISOString().split('T')[0];
                        const isToday = date.toDateString() === today.toDateString();
                        const isCompleted = habits[habitId] && habits[habitId].days && habits[habitId].days[dayKey];
                        
                        const dayElement = document.createElement('div');
                        dayElement.className = `habit-day ${isCompleted ? 'completed' : ''} ${isToday ? 'today' : ''}`;
                        dayElement.textContent = ['D', 'L', 'M', 'X', 'J', 'V', 'S'][index];
                        dayElement.onclick = () => toggleHabit(habitId, dayKey);
                        
                        daysContainer.appendChild(dayElement);
                    });
                }
            });
        }

        function saveHabits() {
            localStorage.setItem('habits', JSON.stringify(habits));
        }

        function loadHabits() {
            const savedHabits = localStorage.getItem('habits');
            if (savedHabits) {
                habits = JSON.parse(savedHabits);
            }
            
            // Inicializar hábitos predefinidos si no existen
            const defaultHabits = ['sleep', 'study', 'water', 'exercise'];
            defaultHabits.forEach(habitId => {
                if (!habits[habitId]) {
                    habits[habitId] = { days: {} };
                }
            });
        }

        // Funciones para Diario de Ánimo
        function selectMood(mood) {
            const today = new Date().toISOString().split('T')[0];
            moods[today] = mood;
            
            // Actualizar UI
            document.querySelectorAll('.mood-option').forEach(option => {
                option.classList.remove('selected');
            });
            document.querySelector(`[data-mood="${mood}"]`).classList.add('selected');
            
            saveMoods();
        }

        function saveReflection() {
            const today = new Date().toISOString().split('T')[0];
            const reflection = document.getElementById('dailyReflection').value.trim();
            
            if (reflection) {
                reflections[today] = reflection;
                saveReflections();
                alert('Reflexión guardada 💕');
            }
        }

        function loadTodayMood() {
            const today = new Date().toISOString().split('T')[0];
            const todayMood = moods[today];
            const todayReflection = reflections[today];
            
            // Actualizar fecha
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('moodDate').textContent = new Date().toLocaleDateString('es-ES', options);
            
            // Mostrar mood seleccionado
            if (todayMood) {
                document.querySelector(`[data-mood="${todayMood}"]`).classList.add('selected');
            }
            
            // Mostrar reflexión
            if (todayReflection) {
                document.getElementById('dailyReflection').value = todayReflection;
            }
        }

        function saveMoods() {
            localStorage.setItem('moods', JSON.stringify(moods));
        }

        function loadMoods() {
            const savedMoods = localStorage.getItem('moods');
            if (savedMoods) {
                moods = JSON.parse(savedMoods);
            }
        }

        function saveReflections() {
            localStorage.setItem('reflections', JSON.stringify(reflections));
        }

        function loadReflections() {
            const savedReflections = localStorage.getItem('reflections');
            if (savedReflections) {
                reflections = JSON.parse(savedReflections);
            }
        }

        // Variables del Timer Pomodoro
        let timerInterval = null;
        let timeLeft = 25 * 60; // 25 minutos en segundos
        let isRunning = false;
        let currentSession = 'work'; // 'work', 'shortBreak', 'longBreak'
        let currentCycle = 1;
        let totalCycles = 4;
        let pomodoroStats = {
            completedSessions: 0,
            totalTime: 0,
            currentStreak: 0,
            lastSessionDate: null
        };

        // Configuración del timer
        let timerConfig = {
            workTime: 25,
            shortBreak: 5,
            longBreak: 15
        };

        // Sistema de notificaciones
        let notificationPermission = false;

        // Solicitar permisos de notificación
        function requestNotificationPermission() {
            if ('Notification' in window) {
                Notification.requestPermission().then(function(permission) {
                    notificationPermission = permission === 'granted';
                    if (notificationPermission) {
                        showNotification('🔔 Notificaciones activadas', 'Recibirás recordatorios de tus evaluaciones y timer Pomodoro', 'success');
                    }
                });
            }
        }

        // Mostrar notificación
        function showNotification(title, body, type = 'info', vibrate = true) {
            // Notificación nativa del navegador
            if (notificationPermission && 'Notification' in window) {
                const notification = new Notification(title, {
                    body: body,
                    icon: 'data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="50" cy="50" r="40" fill="%23e91e63"/><text x="50" y="60" text-anchor="middle" fill="white" font-size="40">⏰</text></svg>',
                    badge: 'data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="50" cy="50" r="40" fill="%23e91e63"/></svg>',
                    tag: type,
                    requireInteraction: type === 'timer'
                });

                // Auto cerrar después de 5 segundos (excepto timer)
                if (type !== 'timer') {
                    setTimeout(() => notification.close(), 5000);
                }
            }

            // Vibración en móviles
            if (vibrate && 'vibrate' in navigator) {
                if (type === 'timer') {
                    navigator.vibrate([200, 100, 200, 100, 200]);
                } else {
                    navigator.vibrate([100, 50, 100]);
                }
            }

            // Sonido personalizado
            playNotificationSound(type);
        }

        // Reproducir sonidos de notificación
        function playNotificationSound(type) {
            const audioContext = new (window.AudioContext || window.webkitAudioContext)();
            
            function createBeep(frequency, duration, volume = 0.3) {
                const oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();
                
                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);
                
                oscillator.frequency.value = frequency;
                oscillator.type = 'sine';
                
                gainNode.gain.setValueAtTime(0, audioContext.currentTime);
                gainNode.gain.linearRampToValueAtTime(volume, audioContext.currentTime + 0.01);
                gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + duration);
                
                oscillator.start(audioContext.currentTime);
                oscillator.stop(audioContext.currentTime + duration);
            }

            switch(type) {
                case 'timer':
                    // Sonido de finalización del timer
                    createBeep(800, 0.2);
                    setTimeout(() => createBeep(600, 0.2), 200);
                    setTimeout(() => createBeep(800, 0.3), 400);
                    break;
                case 'start':
                    // Sonido de inicio
                    createBeep(600, 0.15);
                    setTimeout(() => createBeep(800, 0.15), 150);
                    break;
                case 'pause':
                    // Sonido de pausa
                    createBeep(400, 0.2);
                    break;
                case 'reset':
                    // Sonido de reset
                    createBeep(300, 0.1);
                    setTimeout(() => createBeep(250, 0.1), 100);
                    break;
                case 'evaluation':
                    // Sonido de recordatorio de evaluación
                    createBeep(700, 0.2);
                    setTimeout(() => createBeep(900, 0.2), 250);
                    setTimeout(() => createBeep(700, 0.2), 500);
                    break;
                default:
                    // Sonido genérico
                    createBeep(500, 0.2);
            }
        }

        // Funciones del Timer Pomodoro
        function startTimer() {
            if (!isRunning) {
                isRunning = true;
                document.getElementById('startBtn').style.display = 'none';
                document.getElementById('pauseBtn').style.display = 'flex';
                
                // Iniciar animación del reloj de arena
                const hourglass = document.querySelector('.hourglass');
                hourglass.classList.add('running');
                hourglass.style.setProperty('--timer-duration', timeLeft + 's');
                
                showNotification('🍅 Timer iniciado', `Sesión de ${getSessionName()} comenzada`, 'start');
                
                timerInterval = setInterval(() => {
                    timeLeft--;
                    updateTimerDisplay();
                    
                    if (timeLeft <= 0) {
                        completeSession();
                    }
                }, 1000);
            }
        }

        function pauseTimer() {
            if (isRunning) {
                isRunning = false;
                clearInterval(timerInterval);
                document.getElementById('startBtn').style.display = 'flex';
                document.getElementById('pauseBtn').style.display = 'none';
                
                // Pausar animación
                const hourglass = document.querySelector('.hourglass');
                hourglass.classList.remove('running');
                
                showNotification('⏸️ Timer pausado', 'Puedes continuar cuando estés listo', 'pause');
            }
        }

        function resetTimer() {
            isRunning = false;
            clearInterval(timerInterval);
            document.getElementById('startBtn').style.display = 'flex';
            document.getElementById('pauseBtn').style.display = 'none';
            
            // Reset animación
            const hourglass = document.querySelector('.hourglass');
            hourglass.classList.remove('running');
            resetHourglassAnimation();
            
            // Reset tiempo según la sesión actual
            timeLeft = getSessionDuration(currentSession) * 60;
            updateTimerDisplay();
            
            showNotification('🔄 Timer reiniciado', 'Listo para una nueva sesión', 'reset');
        }

        function completeSession() {
            isRunning = false;
            clearInterval(timerInterval);
            document.getElementById('startBtn').style.display = 'flex';
            document.getElementById('pauseBtn').style.display = 'none';
            
            // Reset animación
            const hourglass = document.querySelector('.hourglass');
            hourglass.classList.remove('running');
            resetHourglassAnimation();
            
            // Actualizar estadísticas
            if (currentSession === 'work') {
                pomodoroStats.completedSessions++;
                pomodoroStats.totalTime += timerConfig.workTime;
                pomodoroStats.currentStreak++;
                pomodoroStats.lastSessionDate = new Date().toDateString();
            }
            
            // Determinar siguiente sesión
            if (currentSession === 'work') {
                if (currentCycle === totalCycles) {
                    currentSession = 'longBreak';
                    currentCycle = 1;
                } else {
                    currentSession = 'shortBreak';
                    currentCycle++;
                }
            } else {
                currentSession = 'work';
            }
            
            timeLeft = getSessionDuration(currentSession) * 60;
            updateTimerDisplay();
            savePomodoroStats();
            
            // Notificación de finalización
            const sessionName = currentSession === 'work' ? 'trabajo' : 'descanso';
            showNotification(
                '✅ ¡Sesión completada!', 
                `Tiempo para ${sessionName}. ¡Excelente trabajo!`, 
                'timer'
            );
        }

        function getSessionName() {
            switch(currentSession) {
                case 'work': return 'Trabajo';
                case 'shortBreak': return 'Descanso Corto';
                case 'longBreak': return 'Descanso Largo';
                default: return 'Trabajo';
            }
        }

        function getSessionDuration(session) {
            switch(session) {
                case 'work': return timerConfig.workTime;
                case 'shortBreak': return timerConfig.shortBreak;
                case 'longBreak': return timerConfig.longBreak;
                default: return timerConfig.workTime;
            }
        }

        function updateTimerDisplay() {
            const minutes = Math.floor(timeLeft / 60);
            const seconds = timeLeft % 60;
            const timeString = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            
            document.getElementById('timerTime').textContent = timeString;
            document.getElementById('timerSession').textContent = getSessionName();
            document.getElementById('currentCycle').textContent = currentCycle;
            
            // Cambiar colores según la sesión
            const timerTime = document.getElementById('timerTime');
            const timerSession = document.getElementById('timerSession');
            
            if (currentSession === 'work') {
                timerTime.style.color = '#e91e63';
                timerSession.style.color = '#880e4f';
            } else {
                timerTime.style.color = '#4caf50';
                timerSession.style.color = '#2e7d32';
            }
            
            // Actualizar estadísticas
            updateStatsDisplay();
        }

        function resetHourglassAnimation() {
            const sandTop = document.getElementById('sandTop');
            const sandBottom = document.getElementById('sandBottom');
            
            if (currentSession === 'work') {
                sandTop.style.height = '100%';
                sandBottom.style.height = '0%';
            } else {
                sandTop.style.height = '100%';
                sandBottom.style.height = '0%';
            }
        }

        function saveConfig() {
            timerConfig.workTime = parseInt(document.getElementById('workTime').value);
            timerConfig.shortBreak = parseInt(document.getElementById('shortBreak').value);
            timerConfig.longBreak = parseInt(document.getElementById('longBreak').value);
            
            localStorage.setItem('pomodoroConfig', JSON.stringify(timerConfig));
            
            // Si no está corriendo, actualizar el tiempo actual
            if (!isRunning) {
                timeLeft = getSessionDuration(currentSession) * 60;
                updateTimerDisplay();
            }
            
            showNotification('⚙️ Configuración guardada', 'Los nuevos tiempos se aplicarán en la próxima sesión');
        }

        function loadPomodoroConfig() {
            const savedConfig = localStorage.getItem('pomodoroConfig');
            if (savedConfig) {
                timerConfig = JSON.parse(savedConfig);
                document.getElementById('workTime').value = timerConfig.workTime;
                document.getElementById('shortBreak').value = timerConfig.shortBreak;
                document.getElementById('longBreak').value = timerConfig.longBreak;
            }
        }

        function savePomodoroStats() {
            localStorage.setItem('pomodoroStats', JSON.stringify(pomodoroStats));
        }

        function loadPomodoroStats() {
            const savedStats = localStorage.getItem('pomodoroStats');
            if (savedStats) {
                pomodoroStats = JSON.parse(savedStats);
                
                // Reset streak si es un nuevo día
                const today = new Date().toDateString();
                if (pomodoroStats.lastSessionDate !== today) {
                    pomodoroStats.currentStreak = 0;
                }
            }
        }

        function updateStatsDisplay() {
            document.getElementById('completedSessions').textContent = pomodoroStats.completedSessions;
            
            const hours = Math.floor(pomodoroStats.totalTime / 60);
            const minutes = pomodoroStats.totalTime % 60;
            document.getElementById('totalTime').textContent = `${hours}h ${minutes}m`;
            
            document.getElementById('currentStreak').textContent = pomodoroStats.currentStreak;
        }

        function resetStats() {
            if (confirm('¿Estás segura de que quieres limpiar todas las estadísticas?')) {
                pomodoroStats = {
                    completedSessions: 0,
                    totalTime: 0,
                    currentStreak: 0,
                    lastSessionDate: null
                };
                savePomodoroStats();
                updateStatsDisplay();
                showNotification('🗑️ Estadísticas limpiadas', 'Comenzando desde cero');
            }
        }

        // Sistema de recordatorios de evaluaciones
        function checkUpcomingEvaluations() {
            const today = new Date();
            const tomorrow = new Date(today);
            tomorrow.setDate(tomorrow.getDate() + 1);
            
            const todayKey = today.toISOString().split('T')[0].replace(/-/g, '-');
            const tomorrowKey = tomorrow.toISOString().split('T')[0].replace(/-/g, '-');
            
            // Verificar evaluaciones de hoy
            if (evaluations[todayKey]) {
                evaluations[todayKey].forEach(evaluation => {
                    if (!evaluation.completed) {
                        showNotification(
                            '📚 Evaluación HOY',
                            `${evaluation.type} ${evaluation.subject}: ${evaluation.description}`,
                            'evaluation'
                        );
                    }
                });
            }
            
            // Verificar evaluaciones de mañana
            if (evaluations[tomorrowKey]) {
                evaluations[tomorrowKey].forEach(evaluation => {
                    showNotification(
                        '⏰ Evaluación MAÑANA',
                        `${evaluation.type} ${evaluation.subject}: ${evaluation.description}`,
                        'evaluation'
                    );
                });
            }
        }

        // Funciones del Sistema de Seguimiento Personal
        function toggleActivity(activity) {
            const today = new Date().toISOString().split('T')[0];
            
            if (!personalTracking[today]) {
                personalTracking[today] = {};
            }
            
            personalTracking[today][activity] = !personalTracking[today][activity];
            
            savePersonalTracking();
            updateTodayDisplay();
            updateWeeklyProgress();
            updateMonthlyProgress();
            updateYearlyProgress();
            
            // Feedback visual y sonoro
            const toggle = document.getElementById(activity + 'Toggle');
            const card = document.querySelector(`[data-activity="${activity}"]`);
            
            if (personalTracking[today][activity]) {
                toggle.classList.add('active');
                card.classList.add('completed');
                showNotification(`✅ ¡${getActivityName(activity)} completado!`, 'Excelente trabajo, sigue así 💪');
            } else {
                toggle.classList.remove('active');
                card.classList.remove('completed');
            }
        }

        function getActivityName(activity) {
            const names = {
                study: 'Estudio',
                gym: 'Gym',
                protein: 'Proteína'
            };
            return names[activity] || activity;
        }

        function updateTodayDisplay() {
            const today = new Date().toISOString().split('T')[0];
            const todayData = personalTracking[today] || {};
            
            // Actualizar fecha
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('trackingDate').textContent = new Date().toLocaleDateString('es-ES', options);
            
            // Actualizar toggles
            ['study', 'gym', 'protein'].forEach(activity => {
                const toggle = document.getElementById(activity + 'Toggle');
                const card = document.querySelector(`[data-activity="${activity}"]`);
                
                if (todayData[activity]) {
                    toggle.classList.add('active');
                    card.classList.add('completed');
                } else {
                    toggle.classList.remove('active');
                    card.classList.remove('completed');
                }
            });
            
            // Actualizar score
            const completedCount = Object.values(todayData).filter(Boolean).length;
            document.getElementById('dailyScore').textContent = completedCount;
            
            const messages = [
                '¡Comienza tu día!',
                '¡Buen comienzo! 🌟',
                '¡Vas muy bien! 💪',
                '¡Día perfecto! 🎉'
            ];
            document.getElementById('scoreMessage').textContent = messages[completedCount];
        }

        function updateWeeklyProgress() {
            const today = new Date();
            const startOfWeek = new Date(today);
            startOfWeek.setDate(today.getDate() - today.getDay());
            
            const weekGrid = document.getElementById('weekGrid');
            weekGrid.innerHTML = '';
            
            const weekStats = { study: 0, gym: 0, protein: 0 };
            
            for (let i = 0; i < 7; i++) {
                const date = new Date(startOfWeek);
                date.setDate(startOfWeek.getDate() + i);
                const dateKey = date.toISOString().split('T')[0];
                const dayData = personalTracking[dateKey] || {};
                
                const isToday = date.toDateString() === today.toDateString();
                
                const dayElement = document.createElement('div');
                dayElement.className = `week-day ${isToday ? 'today' : ''}`;
                
                dayElement.innerHTML = `
                    <div class="week-day-name">${['Dom', 'Lun', 'Mar', 'Mié', 'Jue', 'Vie', 'Sáb'][i]}</div>
                    <div class="week-day-number">${date.getDate()}</div>
                    <div class="week-activities">
                        <div class="week-activity ${dayData.study ? 'completed' : ''}" title="Estudio">📚</div>
                        <div class="week-activity ${dayData.gym ? 'completed' : ''}" title="Gym">🏋️‍♀️</div>
                        <div class="week-activity ${dayData.protein ? 'completed' : ''}" title="Proteína">🥤</div>
                    </div>
                `;
                
                weekGrid.appendChild(dayElement);
                
                // Contar para estadísticas
                if (dayData.study) weekStats.study++;
                if (dayData.gym) weekStats.gym++;
                if (dayData.protein) weekStats.protein++;
            }
            
            // Actualizar estadísticas semanales
            ['study', 'gym', 'protein'].forEach(activity => {
                const count = weekStats[activity];
                const percentage = (count / 7) * 100;
                
                document.getElementById('week' + activity.charAt(0).toUpperCase() + activity.slice(1)).textContent = count;
                document.getElementById('week' + activity.charAt(0).toUpperCase() + activity.slice(1) + 'Bar').style.width = percentage + '%';
            });
        }

        function updateMonthlyProgress() {
            const monthStats = getMonthStats(trackingMonth, trackingYear);
            const daysInMonth = new Date(trackingYear, trackingMonth + 1, 0).getDate();
            
            // Actualizar nombre del mes
            document.getElementById('currentMonthName').textContent = `${monthNames[trackingMonth]} ${trackingYear}`;
            
            // Actualizar estadísticas
            ['study', 'gym', 'protein'].forEach(activity => {
                const count = monthStats[activity];
                const percentage = (count / daysInMonth) * 100;
                
                const activityName = activity.charAt(0).toUpperCase() + activity.slice(1);
                document.getElementById('month' + activityName).textContent = count;
                document.getElementById('month' + activityName + 'Total').textContent = `/ ${daysInMonth} días`;
                document.getElementById('month' + activityName + 'Bar').style.width = percentage + '%';
                document.getElementById('month' + activityName + 'Percent').textContent = Math.round(percentage) + '%';
            });
            
            // Actualizar logros del mes
            updateMonthlyAchievements(monthStats, daysInMonth);
        }

        function getMonthStats(month, year) {
            const stats = { study: 0, gym: 0, protein: 0 };
            const daysInMonth = new Date(year, month + 1, 0).getDate();
            
            for (let day = 1; day <= daysInMonth; day++) {
                const dateKey = `${year}-${(month + 1).toString().padStart(2, '0')}-${day.toString().padStart(2, '0')}`;
                const dayData = personalTracking[dateKey] || {};
                
                if (dayData.study) stats.study++;
                if (dayData.gym) stats.gym++;
                if (dayData.protein) stats.protein++;
            }
            
            return stats;
        }

        function updateMonthlyAchievements(stats, daysInMonth) {
            const achievements = [
                {
                    icon: '🔥',
                    title: 'Estudiante Dedicado',
                    desc: 'Estudiar 20+ días',
                    unlocked: stats.study >= 20
                },
                {
                    icon: '💪',
                    title: 'Guerrera del Gym',
                    desc: 'Gym 15+ días',
                    unlocked: stats.gym >= 15
                },
                {
                    icon: '🥤',
                    title: 'Proteína Master',
                    desc: 'Proteína 25+ días',
                    unlocked: stats.protein >= 25
                },
                {
                    icon: '⭐',
                    title: 'Mes Perfecto',
                    desc: 'Completar todo 20+ días',
                    unlocked: Math.min(stats.study, stats.gym, stats.protein) >= 20
                },
                {
                    icon: '🎯',
                    title: 'Constancia',
                    desc: 'Completar 80% del mes',
                    unlocked: (stats.study + stats.gym + stats.protein) >= (daysInMonth * 3 * 0.8)
                },
                {
                    icon: '👑',
                    title: 'Reina de Hábitos',
                    desc: 'Completar 90% del mes',
                    unlocked: (stats.study + stats.gym + stats.protein) >= (daysInMonth * 3 * 0.9)
                }
            ];
            
            const achievementsGrid = document.getElementById('monthlyAchievements');
            achievementsGrid.innerHTML = '';
            
            achievements.forEach(achievement => {
                const achievementElement = document.createElement('div');
                achievementElement.className = `achievement-item ${achievement.unlocked ? 'unlocked' : ''}`;
                achievementElement.innerHTML = `
                    <div class="achievement-icon">${achievement.icon}</div>
                    <div class="achievement-title">${achievement.title}</div>
                    <div class="achievement-desc">${achievement.desc}</div>
                `;
                achievementsGrid.appendChild(achievementElement);
            });
        }

        function updateYearlyProgress() {
            const yearStats = getYearStats(trackingYear);
            const streaks = calculateStreaks();
            
            // Actualizar estadísticas anuales
            document.getElementById('yearStudy').textContent = yearStats.study;
            document.getElementById('yearGym').textContent = yearStats.gym;
            document.getElementById('yearProtein').textContent = yearStats.protein;
            
            document.getElementById('studyStreak').textContent = streaks.study;
            document.getElementById('gymStreak').textContent = streaks.gym;
            document.getElementById('proteinStreak').textContent = streaks.protein;
            
            // Actualizar gráfico anual
            updateYearChart(trackingYear);
            
            // Actualizar logros anuales
            updateYearAchievements(yearStats, streaks);
        }

        function getYearStats(year) {
            const stats = { study: 0, gym: 0, protein: 0 };
            
            for (let month = 0; month < 12; month++) {
                const monthStats = getMonthStats(month, year);
                stats.study += monthStats.study;
                stats.gym += monthStats.gym;
                stats.protein += monthStats.protein;
            }
            
            return stats;
        }

        function calculateStreaks() {
            const today = new Date();
            const streaks = { study: 0, gym: 0, protein: 0 };
            
            ['study', 'gym', 'protein'].forEach(activity => {
                let currentStreak = 0;
                let date = new Date(today);
                
                while (true) {
                    const dateKey = date.toISOString().split('T')[0];
                    const dayData = personalTracking[dateKey] || {};
                    
                    if (dayData[activity]) {
                        currentStreak++;
                        date.setDate(date.getDate() - 1);
                    } else {
                        break;
                    }
                }
                
                streaks[activity] = currentStreak;
            });
            
            return streaks;
        }

        function updateYearChart(year) {
            const yearChart = document.getElementById('yearChart');
            yearChart.innerHTML = '';
            
            for (let month = 0; month < 12; month++) {
                const monthStats = getMonthStats(month, year);
                const daysInMonth = new Date(year, month + 1, 0).getDate();
                
                const monthBar = document.createElement('div');
                monthBar.className = 'month-bar';
                
                const studyHeight = (monthStats.study / daysInMonth) * 100;
                const gymHeight = (monthStats.gym / daysInMonth) * 100;
                const proteinHeight = (monthStats.protein / daysInMonth) * 100;
                
                monthBar.innerHTML = `
                    <div class="month-name">${monthNames[month].substring(0, 3)}</div>
                    <div class="month-bars">
                        <div class="month-activity-bar study-progress" style="height: ${studyHeight}px" title="Estudio: ${monthStats.study} días"></div>
                        <div class="month-activity-bar gym-progress" style="height: ${gymHeight}px" title="Gym: ${monthStats.gym} días"></div>
                        <div class="month-activity-bar protein-progress" style="height: ${proteinHeight}px" title="Proteína: ${monthStats.protein} días"></div>
                    </div>
                `;
                
                yearChart.appendChild(monthBar);
            }
        }

        function updateYearAchievements(yearStats, streaks) {
            const achievements = [
                {
                    icon: '🎓',
                    title: 'Estudiante del Año',
                    desc: 'Estudiar 200+ días',
                    unlocked: yearStats.study >= 200
                },
                {
                    icon: '🏆',
                    title: 'Atleta Dedicada',
                    desc: 'Gym 150+ días',
                    unlocked: yearStats.gym >= 150
                },
                {
                    icon: '💎',
                    title: 'Nutrición Pro',
                    desc: 'Proteína 250+ días',
                    unlocked: yearStats.protein >= 250
                },
                {
                    icon: '🔥',
                    title: 'Racha de Fuego',
                    desc: 'Racha de 30+ días',
                    unlocked: Math.max(streaks.study, streaks.gym, streaks.protein) >= 30
                },
                {
                    icon: '⚡',
                    title: 'Súper Racha',
                    desc: 'Racha de 60+ días',
                    unlocked: Math.max(streaks.study, streaks.gym, streaks.protein) >= 60
                },
                {
                    icon: '👸',
                    title: 'Reina de la Constancia',
                    desc: 'Completar 80% del año',
                    unlocked: (yearStats.study + yearStats.gym + yearStats.protein) >= (365 * 3 * 0.8)
                }
            ];
            
            const yearAchievements = document.getElementById('yearAchievements');
            yearAchievements.innerHTML = '';
            
            achievements.forEach(achievement => {
                const achievementElement = document.createElement('div');
                achievementElement.className = `achievement-item ${achievement.unlocked ? 'unlocked' : ''}`;
                achievementElement.innerHTML = `
                    <div class="achievement-icon">${achievement.icon}</div>
                    <div class="achievement-title">${achievement.title}</div>
                    <div class="achievement-desc">${achievement.desc}</div>
                `;
                yearAchievements.appendChild(achievementElement);
            });
        }

        function changeMonth(direction) {
            trackingMonth += direction;
            
            if (trackingMonth < 0) {
                trackingMonth = 11;
                trackingYear--;
            } else if (trackingMonth > 11) {
                trackingMonth = 0;
                trackingYear++;
            }
            
            updateMonthlyProgress();
        }

        function savePersonalTracking() {
            localStorage.setItem('personalTracking', JSON.stringify(personalTracking));
        }

        function loadPersonalTracking() {
            const saved = localStorage.getItem('personalTracking');
            if (saved) {
                personalTracking = JSON.parse(saved);
            }
        }

        // Event listeners
        document.addEventListener('DOMContentLoaded', function() {
            loadScheduleData();
            loadEvaluations();
            generateCalendar(currentMonth, currentYear);
            
            // Cargar datos del sistema de seguimiento personal
            loadPersonalTracking();
            
            // Cargar datos de organización personal
            loadTasks();
            loadGoals();
            loadHabits();
            loadMoods();
            loadReflections();
            
            // Inicializar displays del seguimiento personal
            updateTodayDisplay();
            updateWeeklyProgress();
            updateMonthlyProgress();
            updateYearlyProgress();
            
            // Renderizar secciones
            renderTasks();
            renderGoals();
            renderHabits();
            loadTodayMood();
            
            // Inicializar timer Pomodoro
            loadPomodoroConfig();
            loadPomodoroStats();
            timeLeft = getSessionDuration(currentSession) * 60;
            updateTimerDisplay();
            resetHourglassAnimation();
            
            // Solicitar permisos de notificación
            setTimeout(requestNotificationPermission, 2000);
            
            // Verificar evaluaciones cada 30 minutos
            checkUpcomingEvaluations();
            setInterval(checkUpcomingEvaluations, 30 * 60 * 1000);

            // Funcionalidad táctil mejorada para móvil
            let touchStartTime = 0;
            let touchStartX = 0;
            let touchStartY = 0;
            let longPressTimer = null;

            document.querySelectorAll('.class-slot[data-time][data-day]').forEach(cell => {
                // Doble click para escritorio
                cell.addEventListener('dblclick', function() {
                    editCell(this);
                });

                // Context menu para escritorio
                cell.addEventListener('contextmenu', function(e) {
                    showContextMenu(e, this);
                });

                // Touch events para móvil
                cell.addEventListener('touchstart', function(e) {
                    touchStartTime = Date.now();
                    touchStartX = e.touches[0].clientX;
                    touchStartY = e.touches[0].clientY;
                    
                    // Long press para context menu en móvil
                    longPressTimer = setTimeout(() => {
                        navigator.vibrate && navigator.vibrate(50);
                        showContextMenu({
                            pageX: touchStartX,
                            pageY: touchStartY,
                            preventDefault: () => {}
                        }, this);
                    }, 500);
                });

                cell.addEventListener('touchmove', function(e) {
                    const moveX = Math.abs(e.touches[0].clientX - touchStartX);
                    const moveY = Math.abs(e.touches[0].clientY - touchStartY);
                    
                    // Si se mueve mucho, cancelar long press
                    if (moveX > 10 || moveY > 10) {
                        clearTimeout(longPressTimer);
                    }
                });

                cell.addEventListener('touchend', function(e) {
                    clearTimeout(longPressTimer);
                    const touchDuration = Date.now() - touchStartTime;
                    
                    // Tap rápido para editar (menos de 300ms)
                    if (touchDuration < 300) {
                        editCell(this);
                    }
                });
            });

            // Context menu items
            document.querySelectorAll('.context-menu-item').forEach(item => {
                item.addEventListener('click', function() {
                    if (currentContextCell) {
                        const colorClass = this.dataset.color;
                        
                        // Remover todas las clases de color
                        currentContextCell.className = 'class-slot';
                        
                        if (colorClass === 'empty') {
                            currentContextCell.classList.add('empty');
                            currentContextCell.innerHTML = '';
                        } else {
                            currentContextCell.classList.add(colorClass);
                        }
                        
                        saveScheduleData();
                    }
                    hideContextMenu();
                });
            });

            // Cerrar context menu al hacer click fuera
            document.addEventListener('click', function(e) {
                if (!contextMenu.contains(e.target)) {
                    hideContextMenu();
                }
            });

            // Cerrar edición al hacer click fuera
            document.addEventListener('click', function(e) {
                if (currentEditingCell && !currentEditingCell.contains(e.target)) {
                    finishEditing();
                }
            });

            // Event listeners del calendario
            document.getElementById('prevMonth').addEventListener('click', previousMonth);
            document.getElementById('nextMonth').addEventListener('click', nextMonth);
            
            // Funcionalidad táctil para el calendario en móvil
            document.addEventListener('click', function(e) {
                if (e.target.closest('.calendar-day') && !e.target.closest('.evaluation-item')) {
                    const calendarDay = e.target.closest('.calendar-day');
                    const dateKey = calendarDay.dataset.date;
                    if (dateKey && !calendarDay.classList.contains('other-month')) {
                        selectDate(dateKey);
                    }
                }
            });
            
            // Modal events
            document.getElementById('closeModal').addEventListener('click', closeModal);
            document.getElementById('saveEvaluation').addEventListener('click', addEvaluation);
            document.getElementById('cancelEvaluation').addEventListener('click', closeModal);
            
            // Cerrar modal al hacer click fuera
            document.getElementById('evaluationModal').addEventListener('click', function(e) {
                if (e.target === this) {
                    closeModal();
                }
            });

            // Cerrar modales de organización personal
            document.getElementById('taskModal').addEventListener('click', function(e) {
                if (e.target === this) {
                    closeTaskModal();
                }
            });

            document.getElementById('goalModal').addEventListener('click', function(e) {
                if (e.target === this) {
                    closeGoalModal();
                }
            });

            document.getElementById('habitModal').addEventListener('click', function(e) {
                if (e.target === this) {
                    closeHabitModal();
                }
            });
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'96993c11a2b4e9ba',t:'MTc1NDI1OTk2NS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
