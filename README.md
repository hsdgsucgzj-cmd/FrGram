<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FrGram Pro - Расширенный чат с поддержкой и админкой</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        :root {
            --primary: #0088cc;
            --secondary: #00b894;
            --accent: #e84393;
            --ai-color: #6c5ce7;
            --support-color: #00cec9;
            --admin-primary: #4361ee;
            --admin-secondary: #3a0ca3;
            --admin-accent: #7209b7;
            --admin-success: #4cc9f0;
            --admin-warning: #f72585;
            --admin-dark: #0a0e16;
            --admin-darker: #050811;
            --admin-light: #ffffff;
            --admin-gray: rgba(255, 255, 255, 0.7);
            --admin-card-bg: rgba(255, 255, 255, 0.05);
            --admin-sidebar-width: 280px;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Segoe UI Emoji', sans-serif;
            background: linear-gradient(135deg, var(--admin-dark) 0%, var(--admin-darker) 100%);
            color: var(--admin-light);
            height: 100vh;
            overflow: hidden;
        }
        
        /* Контейнер приложения */
        .app-container {
            display: flex;
            height: 100vh;
            overflow: hidden;
        }
        
        /* Основной контейнер чата */
        .main-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            position: relative;
        }
        
        /* Хедер чата */
        .chat-header {
            background: rgba(10, 14, 22, 0.95);
            backdrop-filter: blur(20px);
            padding: 15px 30px;
            border-bottom: 1px solid rgba(67, 97, 238, 0.2);
            display: flex;
            justify-content: space-between;
            align-items: center;
            z-index: 100;
        }
        
        .chat-header-left {
            display: flex;
            align-items: center;
            gap: 20px;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 12px;
        }
        
        .logo h1 {
            font-size: 24px;
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            font-weight: 800;
        }
        
        .logo-badge {
            background: var(--accent);
            color: white;
            padding: 2px 10px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 600;
        }
        
        .chat-tabs {
            display: flex;
            gap: 5px;
            background: rgba(255, 255, 255, 0.08);
            border-radius: 15px;
            padding: 5px;
        }
        
        .chat-tab {
            padding: 8px 20px;
            border-radius: 12px;
            border: none;
            background: none;
            color: var(--admin-gray);
            cursor: pointer;
            font-weight: 500;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .chat-tab.active {
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            color: white;
            box-shadow: 0 5px 15px rgba(0, 136, 204, 0.3);
        }
        
        .chat-tab.support-tab.active {
            background: linear-gradient(45deg, var(--support-color), #00a8a8);
        }
        
        .chat-tab.ai-tab.active {
            background: linear-gradient(45deg, var(--ai-color), #5b4bd8);
        }
        
        .chat-header-right {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .user-profile {
            display: flex;
            align-items: center;
            gap: 12px;
            cursor: pointer;
            padding: 8px 15px;
            border-radius: 15px;
            transition: background 0.3s;
        }
        
        .user-profile:hover {
            background: rgba(255, 255, 255, 0.08);
        }
        
        .user-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 18px;
        }
        
        /* Контейнер сообщений */
        .messages-container {
            flex: 1;
            overflow-y: auto;
            padding: 30px;
            display: flex;
            flex-direction: column;
            gap: 20px;
            position: relative;
        }
        
        /* Панель ввода сообщений */
        .input-panel {
            padding: 20px 30px;
            background: rgba(10, 14, 22, 0.95);
            backdrop-filter: blur(20px);
            border-top: 1px solid rgba(67, 97, 238, 0.2);
        }
        
        .input-tools {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
            flex-wrap: wrap;
        }
        
        .tool-button {
            padding: 8px 15px;
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            color: var(--admin-light);
            cursor: pointer;
            font-size: 14px;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: all 0.3s;
        }
        
        .tool-button:hover {
            background: rgba(67, 97, 238, 0.2);
            transform: translateY(-2px);
        }
        
        .input-wrapper {
            display: flex;
            gap: 15px;
            align-items: flex-end;
        }
        
        .message-input-container {
            flex: 1;
            background: rgba(255, 255, 255, 0.08);
            border-radius: 20px;
            padding: 5px 20px;
            display: flex;
            align-items: center;
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: border-color 0.3s;
        }
        
        .message-input-container:focus-within {
            border-color: var(--primary);
        }
        
        .message-input {
            flex: 1;
            background: none;
            border: none;
            color: var(--admin-light);
            font-size: 16px;
            outline: none;
            min-height: 50px;
            max-height: 200px;
            resize: none;
            padding: 10px 0;
            font-family: inherit;
        }
        
        .input-actions {
            display: flex;
            gap: 10px;
            margin-left: 10px;
        }
        
        .action-button {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            border: none;
            background: rgba(255, 255, 255, 0.1);
            color: var(--admin-light);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }
        
        .action-button:hover {
            background: rgba(67, 97, 238, 0.3);
            transform: scale(1.1);
        }
        
        .send-button {
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            width: 50px;
            height: 50px;
        }
        
        /* Сообщения */
        .message {
            max-width: 70%;
            padding: 20px;
            border-radius: 25px;
            animation: fadeIn 0.3s ease;
            position: relative;
            word-wrap: break-word;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .message-incoming {
            align-self: flex-start;
            background: rgba(255, 255, 255, 0.1);
            border-bottom-left-radius: 5px;
        }
        
        .message-outgoing {
            align-self: flex-end;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            border-bottom-right-radius: 5px;
        }
        
        .message-ai {
            align-self: flex-start;
            background: linear-gradient(135deg, var(--ai-color), #5b4bd8);
            border-bottom-left-radius: 5px;
        }
        
        .message-support {
            align-self: flex-start;
            background: linear-gradient(135deg, var(--support-color), #00a8a8);
            border-bottom-left-radius: 5px;
        }
        
        .message-system {
            align-self: center;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 15px 25px;
            text-align: center;
            max-width: 80%;
            font-size: 14px;
            color: var(--admin-gray);
        }
        
        .message-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }
        
        .message-sender {
            font-weight: 600;
            font-size: 14px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .message-time {
            font-size: 12px;
            opacity: 0.8;
        }
        
        .message-content {
            line-height: 1.6;
            margin-bottom: 10px;
        }
        
        .message-actions {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }
        
        .message-action {
            padding: 5px 12px;
            background: rgba(255, 255, 255, 0.1);
            border: none;
            border-radius: 15px;
            color: var(--admin-light);
            font-size: 12px;
            cursor: pointer;
            transition: background 0.3s;
        }
        
        .message-action:hover {
            background: rgba(255, 255, 255, 0.2);
        }
        
        /* Боковая панель (чаты/контакты) */
        .sidebar {
            width: 350px;
            background: rgba(10, 14, 22, 0.95);
            backdrop-filter: blur(20px);
            border-left: 1px solid rgba(67, 97, 238, 0.2);
            display: flex;
            flex-direction: column;
            padding: 20px;
            overflow-y: auto;
        }
        
        .sidebar-section {
            margin-bottom: 30px;
        }
        
        .sidebar-title {
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 15px;
            color: var(--admin-light);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .chat-list {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        .chat-item {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 15px;
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.3s;
            border: 1px solid transparent;
        }
        
        .chat-item:hover {
            background: rgba(67, 97, 238, 0.1);
            border-color: rgba(67, 97, 238, 0.3);
        }
        
        .chat-item.active {
            background: rgba(67, 97, 238, 0.15);
            border-color: var(--admin-primary);
        }
        
        .chat-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            font-weight: bold;
        }
        
        .chat-info {
            flex: 1;
            min-width: 0;
        }
        
        .chat-name {
            font-weight: 600;
            margin-bottom: 5px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .chat-last-message {
            font-size: 13px;
            color: var(--admin-gray);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .chat-meta {
            display: flex;
            flex-direction: column;
            align-items: flex-end;
            gap: 5px;
        }
        
        .chat-time {
            font-size: 12px;
            color: var(--admin-gray);
        }
        
        .chat-unread {
            background: var(--admin-warning);
            color: white;
            font-size: 11px;
            padding: 2px 8px;
            border-radius: 10px;
            font-weight: 600;
            min-width: 20px;
            text-align: center;
        }
        
        /* Быстрые ответы в поддержке */
        .quick-replies {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 20px;
        }
        
        .quick-reply {
            padding: 10px 15px;
            background: rgba(0, 206, 201, 0.1);
            border: 1px solid rgba(0, 206, 201, 0.3);
            border-radius: 15px;
            color: var(--admin-light);
            cursor: pointer;
            font-size: 14px;
            transition: all 0.3s;
        }
        
        .quick-reply:hover {
            background: rgba(0, 206, 201, 0.2);
            transform: translateY(-2px);
        }
        
        /* ИИ помощник */
        .ai-suggestions {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 20px;
        }
        
        .ai-suggestion {
            padding: 10px 15px;
            background: rgba(108, 92, 231, 0.1);
            border: 1px solid rgba(108, 92, 231, 0.3);
            border-radius: 15px;
            color: var(--admin-light);
            cursor: pointer;
            font-size: 14px;
            transition: all 0.3s;
        }
        
        .ai-suggestion:hover {
            background: rgba(108, 92, 231, 0.2);
            transform: translateY(-2px);
        }
        
        /* Админ панель */
        .admin-panel-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(10px);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            padding: 20px;
        }
        
        .admin-panel-overlay.show {
            display: flex;
            animation: fadeIn 0.3s ease;
        }
        
        .admin-panel {
            background: rgba(10, 14, 22, 0.98);
            backdrop-filter: blur(30px);
            border-radius: 25px;
            width: 100%;
            max-width: 1200px;
            height: 90vh;
            display: flex;
            overflow: hidden;
            border: 1px solid rgba(67, 97, 238, 0.3);
            box-shadow: 0 30px 60px rgba(0, 0, 0, 0.5);
        }
        
        .admin-sidebar {
            width: 280px;
            background: rgba(5, 8, 17, 0.8);
            border-right: 1px solid rgba(67, 97, 238, 0.2);
            padding: 30px 20px;
            display: flex;
            flex-direction: column;
            overflow-y: auto;
        }
        
        .admin-main-content {
            flex: 1;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }
        
        .admin-header {
            padding: 25px 30px;
            border-bottom: 1px solid rgba(67, 97, 238, 0.2);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .admin-content {
            flex: 1;
            overflow-y: auto;
            padding: 30px;
        }
        
        /* Уведомления */
        .notifications-panel {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 3000;
            display: flex;
            flex-direction: column;
            gap: 10px;
            max-width: 350px;
        }
        
        .notification {
            background: rgba(10, 14, 22, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 15px;
            padding: 15px 20px;
            border-left: 4px solid var(--admin-primary);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            animation: slideIn 0.3s ease;
            transform: translateX(400px);
            transition: transform 0.3s ease;
        }
        
        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateX(400px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
        
        .notification.show {
            transform: translateX(0);
        }
        
        /* Модальные окна */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(5px);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 4000;
        }
        
        .modal-overlay.show {
            display: flex;
            animation: fadeIn 0.3s ease;
        }
        
        .modal {
            background: rgba(10, 14, 22, 0.98);
            backdrop-filter: blur(30px);
            border-radius: 20px;
            padding: 30px;
            width: 90%;
            max-width: 500px;
            border: 1px solid rgba(67, 97, 238, 0.3);
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5);
        }
        
        /* Статистика в реальном времени */
        .live-stats {
            position: fixed;
            bottom: 20px;
            left: 20px;
            background: rgba(10, 14, 22, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 15px;
            padding: 15px 20px;
            border: 1px solid rgba(67, 97, 238, 0.3);
            display: flex;
            gap: 20px;
            z-index: 100;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
        }
        
        .stat-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
        }
        
        .stat-value {
            font-size: 18px;
            font-weight: 700;
            color: var(--admin-success);
        }
        
        .stat-label {
            font-size: 11px;
            color: var(--admin-gray);
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        /* Индикатор набора текста */
        .typing-indicator {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 15px 20px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            margin: 10px 0;
            align-self: flex-start;
            max-width: 200px;
        }
        
        .typing-dots {
            display: flex;
            gap: 4px;
        }
        
        .typing-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: var(--admin-gray);
            animation: typing 1.4s infinite;
        }
        
        .typing-dot:nth-child(2) { animation-dela
