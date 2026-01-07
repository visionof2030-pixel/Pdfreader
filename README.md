<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0, user-scalable=yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#1a5c9e">
    <meta name="HandheldFriendly" content="true">
    <meta name="MobileOptimized" content="width">
    <title>نظام استخراج وتحليل نتائج الطلاب المتكامل</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.css">
    <style>
        /* إعادة الضبط الأساسية لجميع الأجهزة */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            -webkit-text-size-adjust: 100%;
            -moz-text-size-adjust: 100%;
            -ms-text-size-adjust: 100%;
        }

        :root {
            --primary-color: #1a5c9e;
            --secondary-color: #25d366;
            --danger-color: #dc3545;
            --warning-color: #ff9800;
            --success-color: #4caf50;
            --info-color: #2196f3;
            --dark-color: #2c3e50;
            --light-color: #f8f9fa;
            --text-color: #333;
            --text-light: #666;
            --border-color: #ddd;
            --shadow: 0 2px 10px rgba(0,0,0,0.1);
            --transition: all 0.3s ease;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Tajawal', 'Roboto', 'Helvetica Neue', Arial, sans-serif;
            background: linear-gradient(135deg, #f0f2f5 0%, #f0f8ff 100%);
            color: var(--text-color);
            line-height: 1.6;
            min-height: 100vh;
            padding: 0;
            margin: 0;
            overflow-x: hidden;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        /* دعم خاص للأجهزة المختلفة */
        @supports (-webkit-touch-callout: none) {
            /* خاص لأجهزة iOS */
            body {
                padding-bottom: env(safe-area-inset-bottom);
            }
            
            .card, .tab-content {
                border-radius: 20px;
            }
        }

        /* دعم خاص لهواوي */
        @media screen and (-huawei) {
            body {
                font-family: 'HarmonyOS Sans', sans-serif;
            }
        }

        /* تحسينات للشاشات الصغيرة جداً */
        @media (max-width: 320px) {
            html {
                font-size: 13px;
            }
            
            .container {
                padding: 5px;
            }
        }

        /* تحسينات للشاشات المتوسطة */
        @media (min-width: 321px) and (max-width: 768px) {
            html {
                font-size: 14px;
            }
            
            .container {
                padding: 10px;
            }
        }

        /* تحسينات للشاشات الكبيرة */
        @media (min-width: 769px) {
            html {
                font-size: 16px;
            }
            
            .container {
                padding: 20px;
                max-width: 1200px;
            }
        }

        /* تحسينات للشاشات الكبيرة جداً */
        @media (min-width: 1400px) {
            .container {
                max-width: 1400px;
            }
        }

        /* الحاوية الرئيسية */
        .container {
            margin: 0 auto;
            width: 100%;
            position: relative;
        }

        /* العنوان الرئيسي */
        .main-title {
            text-align: center;
            color: var(--primary-color);
            margin: 1rem 0 0.5rem;
            font-size: clamp(1.5rem, 4vw, 2.5rem);
            font-weight: 800;
            padding: 0 1rem;
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .app-description {
            text-align: center;
            color: var(--text-light);
            margin: 0 auto 2rem;
            font-size: clamp(0.9rem, 2vw, 1.1rem);
            max-width: 800px;
            padding: 0 1rem;
            line-height: 1.8;
        }

        /* التبويبات */
        .tabs {
            display: flex;
            margin-bottom: 1rem;
            border-bottom: 2px solid var(--border-color);
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
            scrollbar-width: none;
            position: sticky;
            top: 0;
            background: white;
            z-index: 100;
            padding: 0.5rem;
            border-radius: 12px 12px 0 0;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }

        .tabs::-webkit-scrollbar {
            display: none;
        }

        .tab {
            padding: 0.8rem 1.2rem;
            cursor: pointer;
            font-weight: 600;
            border-radius: 8px 8px 0 0;
            background: var(--light-color);
            margin-left: 0.3rem;
            white-space: nowrap;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            transition: var(--transition);
            border: 1px solid transparent;
            font-size: clamp(0.8rem, 2vw, 0.9rem);
        }

        .tab i {
            font-size: 1.1rem;
        }

        .tab.active {
            background: var(--primary-color);
            color: white;
            border-color: var(--primary-color);
            transform: translateY(-2px);
            box-shadow: var(--shadow);
        }

        .tab:hover:not(.active) {
            background: #e9ecef;
            transform: translateY(-1px);
        }

        .tab-content {
            display: none;
            background: white;
            padding: 1.5rem;
            border-radius: 0 0 12px 12px;
            margin-bottom: 1.5rem;
            box-shadow: var(--shadow);
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .tab-content.active {
            display: block;
        }

        /* بطاقة الاستخراج */
        .card {
            background: white;
            width: 100%;
            padding: 1.5rem;
            border-radius: 16px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
            margin: 0 auto 1.5rem;
            border: 1px solid rgba(0,0,0,0.05);
        }

        h2 {
            color: var(--dark-color);
            text-align: center;
            margin-bottom: 0.5rem;
            font-size: clamp(1.3rem, 3vw, 1.8rem);
            font-weight: 700;
        }

        .subtitle {
            text-align: center;
            color: var(--text-light);
            margin-bottom: 1.5rem;
            font-size: clamp(0.85rem, 2vw, 0.95rem);
            line-height: 1.6;
        }

        /* قسم الإعدادات */
        .config-section {
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
            padding: 1.5rem;
            border-radius: 12px;
            border: 1px solid var(--border-color);
            margin-bottom: 1.5rem;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }

        .section-title {
            color: var(--dark-color);
            margin-top: 0;
            margin-bottom: 1rem;
            font-size: 1.1rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            font-weight: 600;
        }

        .section-title i {
            color: var(--primary-color);
            font-size: 1.2rem;
        }

        /* إدخال API */
        .api-config {
            display: grid;
            grid-template-columns: 1fr auto;
            gap: 0.8rem;
            margin-bottom: 1rem;
        }

        @media (max-width: 480px) {
            .api-config {
                grid-template-columns: 1fr;
            }
            
            .btn-save {
                width: 100%;
            }
        }

        .api-input {
            padding: 0.8rem 1rem;
            border: 2px solid var(--border-color);
            border-radius: 8px;
            font-size: 0.95rem;
            font-family: 'Courier New', monospace;
            transition: var(--transition);
            background: white;
            width: 100%;
        }

        .api-input:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 3px rgba(26, 92, 158, 0.1);
        }

        .btn-save {
            background: linear-gradient(to right, var(--success-color), #2e7d32);
            color: white;
            border: none;
            padding: 0.8rem 1.5rem;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: var(--transition);
            display: flex;
            align-items: center;
            gap: 0.5rem;
            white-space: nowrap;
        }

        .btn-save:hover {
            background: linear-gradient(to right, #2e7d32, #1b5e20);
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(76, 175, 80, 0.3);
        }

        .btn-save:active {
            transform: translateY(0);
        }

        /* حالة API */
        .api-status {
            padding: 0.8rem 1rem;
            border-radius: 6px;
            font-size: 0.85rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            margin-top: 0.8rem;
        }

        .status-valid {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .status-invalid {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .status-info {
            background: #e3f2fd;
            color: #1565c0;
            border: 1px solid #90caf9;
        }

        /* اختيار النموذج */
        .model-select-container {
            margin: 1rem 0;
        }

        .model-select {
            width: 100%;
            padding: 0.8rem 1rem;
            border: 2px solid var(--border-color);
            border-radius: 8px;
            font-size: 0.95rem;
            background: white;
            cursor: pointer;
            transition: var(--transition);
            appearance: none;
            background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%232c3e50' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
            background-repeat: no-repeat;
            background-position: right 1rem center;
            background-size: 1rem;
            padding-right: 3rem;
        }

        .model-select:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 3px rgba(26, 92, 158, 0.1);
        }

        .model-info {
            margin-top: 0.8rem;
            padding: 0.8rem 1rem;
            background: #e3f2fd;
            border-radius: 6px;
            color: #1565c0;
            font-size: 0.85rem;
            border: 1px solid #90caf9;
            line-height: 1.5;
        }

        /* منطقة رفع الملفات */
        .upload-area {
            border: 3px dashed var(--primary-color);
            padding: 2.5rem 1rem;
            text-align: center;
            cursor: pointer;
            border-radius: 12px;
            background: linear-gradient(135deg, #f8fbff 0%, #e6f2ff 100%);
            margin-bottom: 1.5rem;
            transition: var(--transition);
            position: relative;
            overflow: hidden;
        }

        .upload-area:hover {
            background: linear-gradient(135deg, #e6f2ff 0%, #d4e6ff 100%);
            border-color: #155fa0;
            transform: translateY(-2px);
        }

        .upload-area.dragover {
            background: linear-gradient(135deg, #d4edda 0%, #c3e6cb 100%);
            border-color: var(--success-color);
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(76, 175, 80, 0.4); }
            70% { box-shadow: 0 0 0 10px rgba(76, 175, 80, 0); }
            100% { box-shadow: 0 0 0 0 rgba(76, 175, 80, 0); }
        }

        .upload-area i {
            font-size: 3rem;
            color: var(--primary-color);
            margin-bottom: 1rem;
            display: block;
        }

        .upload-text {
            font-size: 1.1rem;
            color: var(--dark-color);
            margin-bottom: 0.5rem;
            font-weight: 600;
        }

        .upload-info {
            color: var(--text-light);
            font-size: 0.85rem;
            margin-top: 0.5rem;
        }

        /* شريط التقدم */
        .progress-container {
            width: 100%;
            background: var(--light-color);
            border-radius: 10px;
            margin: 1rem 0;
            overflow: hidden;
            height: 8px;
        }

        .progress-bar {
            height: 100%;
            background: linear-gradient(90deg, var(--primary-color), var(--info-color));
            border-radius: 10px;
            width: 0%;
            transition: width 0.3s ease;
        }

        /* زر الاستخراج */
        .btn-extract {
            background: linear-gradient(to right, var(--danger-color), #c0392b);
            color: white;
            border: none;
            padding: 1rem;
            border-radius: 10px;
            width: 100%;
            cursor: pointer;
            font-size: 1.1rem;
            font-weight: 700;
            margin-top: 1rem;
            transition: var(--transition);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.8rem;
            box-shadow: 0 4px 15px rgba(231, 76, 60, 0.3);
        }

        .btn-extract:hover:not(:disabled) {
            background: linear-gradient(to right, #c0392b, #a93226);
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(231, 76, 60, 0.4);
        }

        .btn-extract:active {
            transform: translateY(-1px);
        }

        .btn-extract:disabled {
            background: #bdc3c7;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

        /* منطقة النتائج */
        #result {
            background: var(--light-color);
            padding: 1.5rem;
            border-radius: 12px;
            min-height: 200px;
            border: 1px solid var(--border-color);
            margin-top: 1.5rem;
            font-family: 'Arial', sans-serif;
            line-height: 1.8;
            max-height: 500px;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }

        /* تنسيق الجداول في النتائج */
        .result-table {
            border-collapse: collapse;
            width: 100%;
            margin: 1rem 0;
            border: 1px solid var(--border-color);
            background: white;
            font-size: 0.9rem;
            direction: ltr;
        }

        .result-table th {
            background: #f2f2f2;
            padding: 0.8rem;
            text-align: center;
            border: 1px solid var(--border-color);
            font-weight: bold;
            color: var(--dark-color);
            position: sticky;
            top: 0;
        }

        .result-table td {
            padding: 0.6rem;
            border: 1px solid var(--border-color);
            text-align: center;
            font-family: 'Courier New', monospace;
        }

        .result-table tr:hover {
            background: #f8f9fa;
        }

        /* القوائم في النتائج */
        .result-list {
            padding-right: 1.5rem;
            margin: 1rem 0;
        }

        .result-list li {
            margin-bottom: 0.5rem;
            padding: 0.3rem 0;
            border-bottom: 1px dashed #eee;
        }

        /* الأزرار العامة */
        .actions {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 0.8rem;
            margin-top: 1.5rem;
        }

        @media (max-width: 480px) {
            .actions {
                grid-template-columns: 1fr;
            }
        }

        button {
            background: var(--secondary-color);
            color: white;
            border: none;
            padding: 0.9rem 1.2rem;
            font-size: 0.95rem;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: var(--transition);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
            min-height: 50px;
            text-align: center;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
        }

        button:active {
            transform: translateY(0);
        }

        button.secondary {
            background: linear-gradient(to right, #6c757d, #5a6268);
        }

        button.secondary:hover {
            background: linear-gradient(to right, #5a6268, #4a5258);
        }

        button.danger {
            background: linear-gradient(to right, var(--danger-color), #c82333);
        }

        button.danger:hover {
            background: linear-gradient(to right, #c82333, #b21f2d);
        }

        button.whatsapp {
            background: linear-gradient(to right, #25d366, #128C7E);
        }

        button.whatsapp:hover {
            background: linear-gradient(to right, #128C7E, #075E54);
        }

        /* الجداول */
        .table-responsive {
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
            margin-top: 1rem;
            border-radius: 8px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
            position: relative;
        }

        .students-table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            min-width: 600px;
            font-size: 0.9rem;
        }

        .students-table th {
            background: var(--primary-color);
            color: white;
            padding: 0.8rem;
            text-align: center;
            font-weight: 600;
            position: sticky;
            top: 0;
            z-index: 10;
        }

        .students-table td {
            padding: 0.7rem;
            text-align: center;
            border-bottom: 1px solid #eee;
        }

        .students-table tr:hover {
            background: #f8f9fa;
        }

        .students-table tr:nth-child(even) {
            background: #f9f9f9;
        }

        /* أزرار الحذف */
        .delete-btn {
            background: var(--danger-color);
            color: white;
            border: none;
            padding: 0.5rem 1rem;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.8rem;
            display: flex;
            align-items: center;
            gap: 0.3rem;
            margin: 0 auto;
            transition: var(--transition);
        }

        .delete-btn:hover {
            background: #c82333;
            transform: scale(1.05);
        }

        /* بطاقات الملخص */
        .summary-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 1rem;
            margin-bottom: 1.5rem;
        }

        .summary-card {
            background: white;
            padding: 1.2rem;
            border-radius: 10px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            text-align: center;
            transition: var(--transition);
            border: 1px solid #eee;
        }

        .summary-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 10px rgba(0,0,0,0.15);
        }

        .summary-card h3 {
            margin-top: 0;
            color: var(--primary-color);
            font-size: 0.95rem;
            margin-bottom: 0.8rem;
        }

        .summary-card .value {
            font-size: 1.8rem;
            font-weight: bold;
            margin: 0.5rem 0;
            color: var(--dark-color);
        }

        .summary-card .subtext {
            font-size: 0.8rem;
            color: var(--text-light);
        }

        /* الرسوم البيانية */
        .charts-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 1rem;
            margin-bottom: 1.5rem;
        }

        @media (max-width: 768px) {
            .charts-container {
                grid-template-columns: 1fr;
            }
        }

        .chart-box {
            background: white;
            border: 1px solid var(--border-color);
            border-radius: 10px;
            padding: 1.2rem;
            height: 300px;
            display: flex;
            flex-direction: column;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }

        .chart-box h3 {
            margin: 0 0 1rem 0;
            color: var(--primary-color);
            font-size: 1rem;
            text-align: center;
        }

        .chart-box canvas {
            flex: 1;
            width: 100% !important;
            height: 100% !important;
        }

        /* المستويات */
        .level-badge {
            color: white;
            font-weight: bold;
            padding: 0.4rem 0.8rem;
            border-radius: 20px;
            font-size: 0.8rem;
            display: inline-block;
            min-width: 70px;
            text-align: center;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .level-excellent { background: linear-gradient(135deg, #4caf50, #2e7d32); }
        .level-verygood { background: linear-gradient(135deg, #009688, #00695c); }
        .level-good { background: linear-gradient(135deg, #2196f3, #1565c0); }
        .level-pass { background: linear-gradient(135deg, #ff9800, #ef6c00); }
        .level-weak { background: linear-gradient(135deg, #f44336, #c62828); }

        /* الرسائل */
        .toast {
            font-family: inherit;
            border-radius: 8px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.15);
            padding: 1rem;
            margin: 0.5rem;
        }

        .toast-success {
            background: linear-gradient(135deg, #4caf50, #2e7d32);
        }

        .toast-error {
            background: linear-gradient(135deg, #f44336, #c62828);
        }

        .toast-warning {
            background: linear-gradient(135deg, #ff9800, #ef6c00);
        }

        .toast-info {
            background: linear-gradient(135deg, #2196f3, #1565c0);
        }

        /* التحميل */
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 1s ease-in-out infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* إحصائيات المعالجة */
        .stats-bar {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            margin-top: 1rem;
            padding: 0.8rem;
            background: var(--light-color);
            border-radius: 8px;
            border: 1px solid var(--border-color);
            font-size: 0.85rem;
            color: var(--text-light);
            gap: 0.5rem;
        }

        .stats-bar > div {
            flex: 1;
            min-width: 120px;
            text-align: center;
            padding: 0.3rem;
        }

        /* التقارير */
        .report-content {
            background: white;
            padding: 1.5rem;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.1);
            margin-top: 1.5rem;
            line-height: 1.8;
        }

        /* أدوات الإدخال */
        .input-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1rem;
            margin-bottom: 1rem;
        }

        .input-group {
            display: flex;
            flex-direction: column;
        }

        .input-group label {
            margin-bottom: 0.5rem;
            font-weight: 600;
            color: var(--dark-color);
            font-size: 0.9rem;
        }

        .input-group input,
        .input-group select,
        .input-group textarea {
            padding: 0.8rem 1rem;
            border: 2px solid var(--border-color);
            border-radius: 8px;
            font-size: 0.95rem;
            background: white;
            transition: var(--transition);
            width: 100%;
            font-family: inherit;
        }

        .input-group input:focus,
        .input-group select:focus,
        .input-group textarea:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 3px rgba(26, 92, 158, 0.1);
        }

        /* إخفاء العناصر */
        .hidden {
            display: none !important;
        }

        /* تأثيرات اللمس للهواتف */
        @media (hover: none) and (pointer: coarse) {
            button, .tab, .delete-btn {
                min-height: 44px;
                min-width: 44px;
            }
            
            .api-input, .model-select, .input-group input, .input-group select {
                font-size: 16px; /* يمنع التكبير في iOS */
            }
            
            .students-table td, .students-table th {
                padding: 0.8rem 0.4rem;
            }
        }

        /* تحسينات الطباعة */
        @media print {
            .tabs, .actions, button, .no-print {
                display: none !important;
            }
            
            .tab-content {
                display: block !important;
                box-shadow: none;
                margin: 0;
                padding: 0;
            }
            
            .card {
                box-shadow: none;
                border: 1px solid #ddd;
            }
        }

        /* تذييل الصفحة */
        .footer {
            text-align: center;
            margin-top: 2rem;
            padding-top: 1.5rem;
            border-top: 1px solid var(--border-color);
            color: var(--text-light);
            font-size: 0.85rem;
            line-height: 1.6;
        }

        .footer i {
            color: var(--primary-color);
            margin: 0 0.3rem;
        }

        /* تأثيرات متقدمة */
        .pulse {
            animation: pulse 2s infinite;
        }

        .shake {
            animation: shake 0.5s ease-in-out;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            10%, 30%, 50%, 70%, 90% { transform: translateX(-5px); }
            20%, 40%, 60%, 80% { transform: translateX(5px); }
        }

        /* شريط البحث */
        .search-box {
            position: relative;
            margin: 1rem 0;
        }

        .search-box input {
            width: 100%;
            padding: 0.8rem 1rem 0.8rem 3rem;
            border: 2px solid var(--border-color);
            border-radius: 8px;
            font-size: 0.95rem;
            transition: var(--transition);
        }

        .search-box i {
            position: absolute;
            left: 1rem;
            top: 50%;
            transform: translateY(-50%);
            color: var(--text-light);
        }

        /* زر العودة للأعلى */
        .scroll-top {
            position: fixed;
            bottom: 2rem;
            right: 1rem;
            background: var(--primary-color);
            color: white;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            z-index: 1000;
            transition: var(--transition);
            opacity: 0;
            visibility: hidden;
        }

        .scroll-top.show {
            opacity: 1;
            visibility: visible;
        }

        .scroll-top:hover {
            background: #155fa0;
            transform: translateY(-3px);
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- زر العودة للأعلى -->
        <div class="scroll-top" id="scrollTop" onclick="scrollToTop()">
            <i class="fas fa-chevron-up"></i>
        </div>

        <h1 class="main-title">🚀 نظام استخراج وتحليل نتائج الطلاب المتكامل</h1>
        <p class="app-description">
            نظام متكامل يدعم جميع الأجهزة لاستخراج النصوص من ملفات PDF والصور مع الحفاظ الكامل على تنسيق الجداول والترتيب
        </p>
        
        <!-- التبويبات -->
        <div class="tabs">
            <div class="tab active" onclick="switchTab('extract')" id="tab-extract">
                <i class="fas fa-file-import"></i>
                <span>استخراج النصوص</span>
            </div>
            <div class="tab" onclick="switchTab('input')" id="tab-input">
                <i class="fas fa-database"></i>
                <span>إدارة البيانات</span>
            </div>
            <div class="tab" onclick="switchTab('analysis')" id="tab-analysis">
                <i class="fas fa-chart-bar"></i>
                <span>تحليل النتائج</span>
            </div>
            <div class="tab" onclick="switchTab('report')" id="tab-report">
                <i class="fas fa-file-pdf"></i>
                <span>التقرير النهائي</span>
            </div>
        </div>
        
        <!-- تبويب استخراج النصوص -->
        <div id="extract-tab" class="tab-content active">
            <div class="card">
                <h2><i class="fas fa-file-alt"></i> مستخرج النصوص الذكي</h2>
                <p class="subtitle">استخرج النصوص من ملفات PDF والصور مع الحفاظ الكامل على تنسيق الجداول والترتيب</p>
                
                <div class="config-section">
                    <div class="section-title">
                        <i class="fas fa-key"></i>
                        <span>إعدادات Google Gemini API</span>
                    </div>
                    
                    <div class="api-config">
                        <input type="password" id="apiKeyInput" class="api-input" 
                               placeholder="أدخل مفتاح Google Gemini API هنا..." 
                               inputmode="email" autocomplete="off">
                        <button id="saveApiBtn" class="btn-save">
                            <i class="fas fa-save"></i> حفظ المفتاح
                        </button>
                    </div>
                    
                    <div id="apiStatus" class="api-status status-invalid">
                        <i class="fas fa-times-circle"></i>
                        <span id="apiStatusText">لم يتم إضافة مفتاح API بعد</span>
                    </div>
                    
                    <div id="modelTesting" class="api-status status-info hidden">
                        <i class="fas fa-spinner fa-spin"></i>
                        <span>جاري اختبار النماذج المتاحة...</span>
                    </div>
                    
                    <div class="section-title">
                        <i class="fas fa-brain"></i>
                        <span>اختيار نموذج Gemini</span>
                    </div>
                    
                    <div class="model-select-container">
                        <select id="modelSelect" class="model-select">
                            <option value="">-- اختر النموذج --</option>
                            <!-- سيتم تعبئة النماذج تلقائياً -->
                        </select>
                    </div>
                    
                    <div id="modelInfo" class="model-info">
                        <i class="fas fa-info-circle"></i>
                        <span id="modelInfoText">سيتم تحميل قائمة النماذج المتاحة بعد إضافة مفتاح API</span>
                    </div>
                </div>
                
                <div class="upload-area" id="dropZone">
                    <i class="fas fa-cloud-upload-alt"></i>
                    <div class="upload-text" id="fileLabel">اسحب ملف PDF أو صورة هنا أو انقر للاختيار</div>
                    <div class="upload-info" id="fileInfo">الحد الأقصى: 10MB | المدعوم: PDF, JPG, PNG, GIF, BMP, WebP</div>
                    <input type="file" id="fileInput" accept=".pdf,.jpg,.jpeg,.png,.gif,.bmp,.webp,image/*" style="display:none">
                </div>
                
                <div class="progress-container">
                    <div class="progress-bar" id="progressBar"></div>
                </div>
                
                <button id="btnExtract" class="btn-extract" disabled>
                    <i class="fas fa-magic"></i> استخراج وتحليل تلقائي
                </button>
                
                <div class="stats-bar">
                    <div id="charCount">عدد الأحرف: 0</div>
                    <div id="wordCount">عدد الكلمات: 0</div>
                    <div id="processingTime">زمن المعالجة: 0 ثانية</div>
                </div>
                
                <div id="result">
                    <div style="text-align: center; color: #7f8c8d; padding: 40px;">
                        <i class="fas fa-file-alt" style="font-size: 48px; margin-bottom: 15px; color: #bdc3c7;"></i>
                        <h3 style="color: #95a5a6;">النتائج ستظهر هنا</h3>
                        <p style="margin-top: 10px;">بعد استخراج النصوص، سيتم معالجتها وتحليلها تلقائياً مع الحفاظ الكامل على التنسيق.</p>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- تبويب إدارة البيانات -->
        <div id="input-tab" class="tab-content">
            <div class="card">
                <h2><i class="fas fa-database"></i> إدارة البيانات المستخرجة</h2>
                <p class="subtitle">عرض وتعديل البيانات التي تم استخراجها تلقائياً مع الحفاظ على التنسيق</p>
                
                <div class="search-box">
                    <i class="fas fa-search"></i>
                    <input type="text" id="searchInput" placeholder="ابحث عن طالب أو مادة أو فصل...">
                </div>
                
                <div class="actions">
                    <button onclick="processExtractedData()" id="processDataBtn">
                        <i class="fas fa-robot"></i>
                        <span>معالجة البيانات المستخرجة</span>
                    </button>
                    <button onclick="clearAllData()" class="danger">
                        <i class="fas fa-trash-alt"></i>
                        <span>مسح جميع البيانات</span>
                    </button>
                    <button onclick="refreshDataView()" class="secondary">
                        <i class="fas fa-sync-alt"></i>
                        <span>تحديث العرض</span>
                    </button>
                </div>
                
                <div id="extractedDataSection" class="hidden">
                    <h3 style="margin-top: 20px;"><i class="fas fa-code"></i> البيانات المستخرجة</h3>
                    <div style="background: #f8f9fa; padding: 15px; border-radius: 10px; margin-top: 10px;">
                        <pre id="rawDataPreview" style="white-space: pre-wrap; font-family: 'Courier New', monospace; max-height: 300px; overflow-y: auto; direction: ltr; font-size: 14px; line-height: 1.4;"></pre>
                    </div>
                </div>
                
                <h3 style="margin-top: 20px;"><i class="fas fa-users"></i> الطلاب المستخرجون <span id="studentsCount" style="background: var(--primary-color); color: white; padding: 2px 8px; border-radius: 12px; font-size: 0.8rem;">0</span></h3>
                
                <div class="table-responsive">
                    <table class="students-table" id="studentsList">
                        <thead>
                            <tr>
                                <th>#</th>
                                <th>اسم الطالب (ID)</th>
                                <th>المادة</th>
                                <th>الفصل</th>
                                <th>الدرجة</th>
                                <th>المستوى</th>
                                <th>الإجراءات</th>
                            </tr>
                        </thead>
                        <tbody id="studentsTableBody">
                            <!-- سيتم إضافة الطلاب هنا -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
        
        <!-- تبويب تحليل النتائج -->
        <div id="analysis-tab" class="tab-content">
            <div class="card">
                <h2><i class="fas fa-chart-bar"></i> تحليل النتائج</h2>
                <p class="subtitle">تحليل إحصائي متقدم للبيانات المستخرجة مع رسوم بيانية تفاعلية</p>
                
                <div id="analysis-alert" class="api-status status-info">
                    <i class="fas fa-exclamation-circle"></i>
                    <span>لا توجد بيانات لعرض التحليل. يرجى استخراج البيانات أولاً.</span>
                </div>
                
                <div class="summary-cards" id="summaryCards">
                    <!-- سيتم إضافة بطاقات الملخص هنا -->
                </div>
                
                <div class="charts-container">
                    <div class="chart-box">
                        <h3>توزيع الطلاب حسب المستوى</h3>
                        <canvas id="levelChart"></canvas>
                    </div>
                    <div class="chart-box">
                        <h3>متوسط الدرجات حسب المادة</h3>
                        <canvas id="subjectChart"></canvas>
                    </div>
                </div>
                
                <div class="charts-container">
                    <div class="chart-box">
                        <h3>توزيع الدرجات</h3>
                        <canvas id="scoreDistributionChart"></canvas>
                    </div>
                    <div class="chart-box">
                        <h3>الأداء حسب الفصل</h3>
                        <canvas id="classPerformanceChart"></canvas>
                    </div>
                </div>
                
                <div style="margin-top: 20px; background: white; padding: 20px; border-radius: 10px; box-shadow: 0 2px 10px rgba(0,0,0,0.1);">
                    <h3 style="color: var(--primary-color); margin-bottom: 15px; border-bottom: 2px solid #eee; padding-bottom: 10px;">
                        <i class="fas fa-list-ol"></i> تفاصيل النتائج حسب المستوى
                    </h3>
                    <div id="levelDetailsTable">
                        <!-- سيتم إضافة جدول تفاصيل المستويات هنا -->
                    </div>
                </div>
            </div>
        </div>
        
        <!-- تبويب التقرير النهائي -->
        <div id="report-tab" class="tab-content">
            <div class="card">
                <h2><i class="fas fa-file-pdf"></i> التقرير النهائي</h2>
                <p class="subtitle">تقرير شامل للبيانات المستخرجة والتحليل الإحصائي جاهز للطباعة والمشاركة</p>
                
                <div id="report-alert" class="api-status status-info">
                    <i class="fas fa-exclamation-circle"></i>
                    <span>لا توجد بيانات لإنشاء التقرير. يرجى استخراج البيانات أولاً.</span>
                </div>
                
                <div class="actions">
                    <button onclick="generatePDF()" id="pdfBtn">
                        <i class="fas fa-download"></i>
                        <span>تحميل PDF</span>
                    </button>
                    <button onclick="printReport()" class="secondary">
                        <i class="fas fa-print"></i>
                        <span>طباعة التقرير</span>
                    </button>
                    <button onclick="exportToExcel()" class="secondary">
                        <i class="fas fa-file-excel"></i>
                        <span>تصدير Excel</span>
                    </button>
                    <button onclick="shareReport()" class="whatsapp">
                        <i class="fab fa-whatsapp"></i>
                        <span>مشاركة</span>
                    </button>
                </div>
                
                <div id="reportContent" class="report-content">
                    <!-- سيتم عرض التقرير هنا -->
                </div>
            </div>
        </div>
        
        <div class="footer">
            <p><i class="fas fa-code"></i> نظام استخراج وتحليل نتائج الطلاب المتكامل | يدعم جميع الأجهزة | v4.0</p>
            <p style="font-size: 0.8rem; margin-top: 5px; color: #888;">
                <i class="fas fa-mobile-alt"></i> يدعم: iOS • Android • Huawei • Windows • macOS • Linux
            </p>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script type="module">
        // استيراد مكتبة Google Gemini AI
        import { GoogleGenerativeAI } from "https://esm.run/@google/generative-ai@0.1.0";

        // تهيئة Toastr
        toastr.options = {
            "closeButton": true,
            "debug": false,
            "newestOnTop": true,
            "progressBar": true,
            "positionClass": "toast-top-center",
            "preventDuplicates": true,
            "onclick": null,
            "showDuration": "300",
            "hideDuration": "1000",
            "timeOut": "5000",
            "extendedTimeOut": "1000",
            "showEasing": "swing",
            "hideEasing": "linear",
            "showMethod": "fadeIn",
            "hideMethod": "fadeOut",
            "rtl": true
        };

        // تخزين بيانات التطبيق
        const appState = {
            API_KEY: localStorage.getItem('gemini_api_key') || '',
            students: [],
            classes: ['2/أ', '2/ب', '2/ج', '2/د', '2/هـ', '2/و', 'غير محدد'],
            extractedData: '',
            availableModels: [],
            selectedModel: localStorage.getItem('selected_model') || '',
            fileType: '',
            studentIdCounter: 1,
            processingStartTime: null,
            lastExtractedData: '',
            preservedFormatting: true
        };

        // عناصر واجهة المستخدم
        const elements = {
            // عناصر استخراج النصوص
            apiKeyInput: document.getElementById('apiKeyInput'),
            saveApiBtn: document.getElementById('saveApiBtn'),
            apiStatus: document.getElementById('apiStatus'),
            apiStatusText: document.getElementById('apiStatusText'),
            modelTesting: document.getElementById('modelTesting'),
            modelSelect: document.getElementById('modelSelect'),
            modelInfo: document.getElementById('modelInfo'),
            modelInfoText: document.getElementById('modelInfoText'),
            fileInput: document.getElementById('fileInput'),
            dropZone: document.getElementById('dropZone'),
            fileLabel: document.getElementById('fileLabel'),
            fileInfo: document.getElementById('fileInfo'),
            btnExtract: document.getElementById('btnExtract'),
            result: document.getElementById('result'),
            progressBar: document.getElementById('progressBar'),
            charCount: document.getElementById('charCount'),
            wordCount: document.getElementById('wordCount'),
            processingTime: document.getElementById('processingTime'),
            
            // عناصر إدارة البيانات
            searchInput: document.getElementById('searchInput'),
            alertMessage: document.getElementById('alert-message'),
            extractedDataSection: document.getElementById('extractedDataSection'),
            rawDataPreview: document.getElementById('rawDataPreview'),
            studentsTableBody: document.getElementById('studentsTableBody'),
            studentsCount: document.getElementById('studentsCount'),
            processDataBtn: document.getElementById('processDataBtn'),
            
            // عناصر التحليل
            analysisAlert: document.getElementById('analysis-alert'),
            summaryCards: document.getElementById('summaryCards'),
            levelDetailsTable: document.getElementById('levelDetailsTable'),
            
            // عناصر التقرير
            reportAlert: document.getElementById('report-alert'),
            reportContent: document.getElementById('reportContent'),
            pdfBtn: document.getElementById('pdfBtn'),
            
            // التبويبات
            tabs: {
                extract: document.getElementById('tab-extract'),
                input: document.getElementById('tab-input'),
                analysis: document.getElementById('tab-analysis'),
                report: document.getElementById('tab-report')
            }
        };

        // الرسم البياني
        let charts = {
            levelChart: null,
            subjectChart: null,
            scoreDistributionChart: null,
            classPerformanceChart: null
        };

        // تهيئة التطبيق
        function initApp() {
            console.log("🚀 جاري تهيئة النظام...");
            
            // تهيئة إعدادات API
            if (appState.API_KEY) {
                elements.apiKeyInput.value = "••••••••" + appState.API_KEY.slice(-4);
                updateApiStatus(true);
                loadAvailableModels();
            }
            
            // تحميل البيانات المحفوظة
            loadFromLocalStorage();
            
            // إعداد معالجات الأحداث
            setupEventHandlers();
            
            // إعداد شريط التمرير
            setupScrollTop();
            
            // تحسين الأداء للهواتف
            optimizeForMobile();
            
            console.log("✅ تم تهيئة النظام بنجاح");
            
            // إظهار رسالة ترحيب
            setTimeout(() => {
                toastr.info("مرحباً! النظام يدعم جميع الأجهزة ويحافظ على تنسيق الجداول");
            }, 1000);
        }

        // إعداد معالجات الأحداث
        function setupEventHandlers() {
            // استخراج النصوص
            elements.saveApiBtn.addEventListener('click', saveApiKey);
            elements.modelSelect.addEventListener('change', function() {
                appState.selectedModel = this.value;
                localStorage.setItem('selected_model', this.value);
                updateModelInfo();
            });
            elements.dropZone.addEventListener('click', () => elements.fileInput.click());
            elements.fileInput.addEventListener('change', handleFileSelect);
            elements.btnExtract.addEventListener('click', extractAndAnalyze);
            
            // إدخال البيانات
            elements.searchInput.addEventListener('input', searchStudents);
            elements.processDataBtn.addEventListener('click', processExtractedData);
            
            // سحب وإفلات الملفات
            ['dragover', 'dragleave', 'drop'].forEach(eventName => {
                elements.dropZone.addEventListener(eventName, (e) => {
                    e.preventDefault();
                    e.stopPropagation();
                    
                    if (eventName === 'dragover') {
                        elements.dropZone.classList.add('dragover');
                        elements.dropZone.style.borderColor = 'var(--success-color)';
                    } else if (eventName === 'dragleave' || eventName === 'drop') {
                        elements.dropZone.classList.remove('dragover');
                        elements.dropZone.style.borderColor = 'var(--primary-color)';
                        
                        if (eventName === 'drop' && e.dataTransfer.files.length) {
                            elements.fileInput.files = e.dataTransfer.files;
                            handleFileSelect();
                        }
                    }
                });
            });
            
            // عرض/إخفاء مفتاح API
            elements.apiKeyInput.addEventListener('focus', function() {
                if (appState.API_KEY && this.value.includes('••••')) {
                    this.value = appState.API_KEY;
                }
            });
            
            elements.apiKeyInput.addEventListener('blur', function() {
                if (appState.API_KEY && !this.value.includes('••••')) {
                    this.value = "••••••••" + appState.API_KEY.slice(-4);
                }
            });
            
            // تحديث الإحصائيات عند الكتابة في النتائج
            elements.result.addEventListener('input', updateStats);
            
            // زر العودة للأعلى
            window.addEventListener('scroll', toggleScrollTop);
        }

        // إعداد شريط التمرير
        function setupScrollTop() {
            window.onscroll = function() {
                toggleScrollTop();
            };
        }

        function toggleScrollTop() {
            const scrollTop = document.getElementById('scrollTop');
            if (document.body.scrollTop > 200 || document.documentElement.scrollTop > 200) {
                scrollTop.classList.add('show');
            } else {
                scrollTop.classList.remove('show');
            }
        }

        function scrollToTop() {
            window.scrollTo({
                top: 0,
                behavior: 'smooth'
            });
        }

        // تحسين الأداء للهواتف
        function optimizeForMobile() {
            // تعطيل تأثيرات معينة على الهواتف القديمة
            if ('ontouchstart' in window || navigator.maxTouchPoints) {
                // إضافة delay للتفاعلات على الهواتف
                const interactiveElements = document.querySelectorAll('button, .tab, .delete-btn');
                interactiveElements.forEach(el => {
                    el.style.touchAction = 'manipulation';
                });
            }
            
            // تحسين الأداء على iOS
            if (navigator.userAgent.match(/iPhone|iPad|iPod/i)) {
                document.body.style.webkitOverflowScrolling = 'touch';
            }
            
            // تحسين للأندرويد
            if (navigator.userAgent.match(/Android/i)) {
                document.body.style.webkitTapHighlightColor = 'transparent';
            }
            
            // دعم خاص لهواوي
            if (navigator.userAgent.match(/Huawei|Honor/i)) {
                document.body.style.fontFamily = "'HarmonyOS Sans', sans-serif";
            }
        }

        // إدارة إعدادات API
        function updateApiStatus(isValid) {
            if (isValid && appState.API_KEY) {
                elements.apiStatus.className = 'api-status status-valid';
                elements.apiStatusText.innerHTML = '<i class="fas fa-check-circle"></i> مفتاح API صالح ومحفوظ';
                elements.btnExtract.disabled = false;
            } else {
                elements.apiStatus.className = 'api-status status-invalid';
                elements.apiStatusText.innerHTML = '<i class="fas fa-times-circle"></i> يرجى إضافة مفتاح API صالح';
                elements.btnExtract.disabled = true;
            }
        }

        async function saveApiKey() {
            const inputKey = elements.apiKeyInput.value.trim();
            
            if (inputKey.includes('••••')) {
                updateApiStatus(true);
                return;
            }
            
            if (inputKey === '') {
                localStorage.removeItem('gemini_api_key');
                appState.API_KEY = '';
                elements.apiKeyInput.value = '';
                updateApiStatus(false);
                toastr.success('تم مسح مفتاح API بنجاح');
                return;
            }
            
            if (!inputKey.startsWith('AIza')) {
                toastr.error('يبدو أن مفتاح API غير صحيح. يجب أن يبدأ المفتاح بـ "AIza"');
                return;
            }
            
            // اختبار الاتصال بالـ API
            elements.modelTesting.classList.remove('hidden');
            elements.modelTesting.innerHTML = '<i class="fas fa-spinner fa-spin"></i> جاري اختبار الاتصال بالنماذج...';
            
            try {
                const isValid = await testApiConnection(inputKey);
                if (isValid) {
                    appState.API_KEY = inputKey;
                    localStorage.setItem('gemini_api_key', appState.API_KEY);
                    elements.apiKeyInput.value = "••••••••" + appState.API_KEY.slice(-4);
                    updateApiStatus(true);
                    
                    // تحميل النماذج المتاحة
                    await loadAvailableModels();
                    
                    toastr.success('تم حفظ مفتاح API بنجاح وتم تحميل النماذج المتاحة');
                } else {
                    toastr.error('مفتاح API غير صالح أو غير قادر على الاتصال بخدمات Google AI');
                }
            } catch (error) {
                toastr.error('حدث خطأ أثناء اختبار الاتصال: ' + error.message);
            } finally {
                elements.modelTesting.classList.add('hidden');
            }
        }

        async function testApiConnection(apiKey) {
            try {
                const response = await fetch(`https://generativelanguage.googleapis.com/v1/models?key=${apiKey}`);
                if (!response.ok) {
                    throw new Error(`خطأ في الاتصال: ${response.status}`);
                }
                const data = await response.json();
                console.log("📊 النماذج المتاحة:", data.models);
                return true;
            } catch (error) {
                console.error("❌ خطأ في اختبار الاتصال:", error);
                return false;
            }
        }

        async function loadAvailableModels() {
            if (!appState.API_KEY) return;
            
            elements.modelTesting.classList.remove('hidden');
            elements.modelTesting.innerHTML = '<i class="fas fa-spinner fa-spin"></i> جاري تحميل النماذج المتاحة...';
            
            try {
                const response = await fetch(`https://generativelanguage.googleapis.com/v1/models?key=${appState.API_KEY}`);
                if (!response.ok) {
                    throw new Error(`خطأ في تحميل النماذج: ${response.status}`);
                }
                
                const data = await response.json();
                appState.availableModels = data.models || [];
                
                // تصفية نماذج Gemini فقط
                const geminiModels = appState.availableModels.filter(model => 
                    model.name && (
                        model.name.includes('gemini') || 
                        model.name.includes('models/gemini') ||
                        (model.displayName && model.displayName.includes('Gemini'))
                    )
                );
                
                // تحديث قائمة النماذج
                updateModelSelect(geminiModels);
                
                elements.modelInfoText.textContent = `تم العثور على ${geminiModels.length} نموذج متاح`;
                
            } catch (error) {
                console.error("❌ خطأ في تحميل النماذج:", error);
                elements.modelInfoText.textContent = 'خطأ في تحميل النماذج. تأكد من اتصال الإنترنت وصحة مفتاح API.';
            } finally {
                elements.modelTesting.classList.add('hidden');
            }
        }

        function updateModelSelect(models) {
            elements.modelSelect.innerHTML = '<option value="">-- اختر النموذج --</option>';
            
            if (models.length === 0) {
                elements.modelSelect.innerHTML += '<option value="" disabled>لا توجد نماذج متاحة</option>';
                return;
            }
            
            // فرز النماذج حسب الأفضلية
            const sortedModels = models.sort((a, b) => {
                const getPriority = (model) => {
                    const name = model.name.toLowerCase();
                    if (name.includes('1.5-flash')) return 1;
                    if (name.includes('1.5-pro')) return 2;
                    if (name.includes('2.0')) return 3;
                    if (name.includes('vision')) return 4;
                    if (name.includes('pro')) return 5;
                    if (name.includes('gemini')) return 6;
                    return 7;
                };
                return getPriority(a) - getPriority(b);
            });
            
            sortedModels.forEach(model => {
                const modelName = model.name.split('/').pop();
                const option = document.createElement('option');
                option.value = model.name;
                
                // تسمية النماذج بشكل مفهوم
                let displayName = model.displayName || modelName;
                if (modelName.includes('gemini-1.5-flash')) displayName = 'Gemini 1.5 Flash (الأسرع والأفضل للجداول)';
                else if (modelName.includes('gemini-1.5-pro')) displayName = 'Gemini 1.5 Pro (دقة عالية في الجداول)';
                else if (modelName.includes('gemini-2.0')) displayName = 'Gemini 2.0 (الأحدث)';
                else if (modelName.includes('gemini-pro-vision')) displayName = 'Gemini Pro Vision (للصور والجداول)';
                else if (modelName.includes('gemini-pro')) displayName = 'Gemini Pro (للنصوص والجداول)';
                else if (modelName.includes('gemini-ultra')) displayName = 'Gemini Ultra (المتقدم)';
                
                option.textContent = displayName;
                elements.modelSelect.appendChild(option);
            });
            
            // تحديد النموذج المختار مسبقاً
            if (appState.selectedModel) {
                elements.modelSelect.value = appState.selectedModel;
            } else if (sortedModels.length > 0) {
                // اختيار أول نموذج يدعم Flash
                const flashModel = sortedModels.find(m => m.name.includes('flash'));
                if (flashModel) {
                    elements.modelSelect.value = flashModel.name;
                    appState.selectedModel = flashModel.name;
                } else {
                    elements.modelSelect.value = sortedModels[0].name;
                    appState.selectedModel = sortedModels[0].name;
                }
                updateModelInfo();
            }
        }

        function updateModelInfo() {
            if (!appState.selectedModel) return;
            
            const modelName = appState.selectedModel.split('/').pop();
            let info = '';
            
            if (modelName.includes('flash')) {
                info = '✅ النموذج الأسرع والأقل تكلفة. ممتاز في الحفاظ على تنسيق الجداول والنصوص.';
            } else if (modelName.includes('1.5-pro')) {
                info = '✨ النموذج الأقوى والأكثر دقة. يحافظ على التنسيق المعقد والجداول الكبيرة.';
            } else if (modelName.includes('vision')) {
                info = '📷 مصمم خصيصاً للصور. يستخرج النصوص والجداول من الصور بدقة عالية مع الحفاظ على التنسيق.';
            } else if (modelName.includes('pro')) {
                info = '📄 نموذج متوازن للاستخدام العام. يحافظ على تنسيق النصوص والجداول.';
            } else if (modelName.includes('ultra')) {
                info = '🚀 النموذج الأكثر تطوراً. للأعمال المتقدمة والمعقدة مع دقة عالية في التنسيق.';
            } else {
                info = '🤖 نموذج Gemini للذكاء الاصطناعي. يحافظ على تنسيق النصوص والجداول.';
            }
            
            elements.modelInfoText.textContent = info;
        }

        // معالجة تحميل الملفات
        function handleFileSelect() {
            if (elements.fileInput.files[0]) {
                const file = elements.fileInput.files[0];
                const fileSize = (file.size / 1024 / 1024).toFixed(2);
                const fileName = file.name.length > 30 ? file.name.substring(0, 27) + '...' : file.name;
                
                elements.fileLabel.innerHTML = `<i class="fas fa-file"></i> ${fileName}`;
                elements.fileInfo.innerHTML = `<i class="fas fa-info-circle"></i> حجم الملف: ${fileSize} MB | النوع: ${file.type}`;
                
                // تحديد نوع الملف
                appState.fileType = file.type;
                
                // اقتراح النموذج المناسب تلقائياً
                suggestModelForFile(file);
            }
        }

        function suggestModelForFile(file) {
            if (!appState.availableModels.length) return;
            
            let suggestedModel = '';
            
            if (file.type.startsWith('image/')) {
                // البحث عن نموذج يدعم الصور
                suggestedModel = appState.availableModels.find(m => 
                    m.name.includes('vision') || 
                    m.name.includes('1.5-pro') ||
                    (m.supportedGenerationMethods && m.supportedGenerationMethods.includes('generateContent'))
                );
            } else if (file.type === 'application/pdf') {
                // البحث عن نموذج قوي للنصوص والجداول
                suggestedModel = appState.availableModels.find(m => 
                    m.name.includes('1.5-flash') || 
                    m.name.includes('1.5-pro') ||
                    m.name.includes('pro')
                );
            }
            
            if (suggestedModel) {
                elements.modelSelect.value = suggestedModel.name;
                appState.selectedModel = suggestedModel.name;
                updateModelInfo();
                
                toastr.info(`تم اختيار ${suggestedModel.name.split('/').pop()} تلقائياً للحفاظ على تنسيق الملف`);
            }
        }

        // استخراج وتحليل البيانات مع الحفاظ على التنسيق
        async function extractAndAnalyze() {
            if (!appState.API_KEY) {
                toastr.error('يرجى إضافة مفتاح API أولاً');
                return;
            }
            
            if (!appState.selectedModel) {
                toastr.error('يرجى اختيار نموذج من القائمة');
                return;
            }
            
            if (!elements.fileInput.files[0]) {
                toastr.error('الرجاء اختيار ملف أولاً');
                return;
            }
            
            const file = elements.fileInput.files[0];
            const maxSize = 10 * 1024 * 1024;
            
            if (file.size > maxSize) {
                toastr.error('حجم الملف كبير جداً. الحد الأقصى هو 10MB');
                return;
            }
            
            // بدء المعالجة
            appState.processingStartTime = Date.now();
            elements.btnExtract.disabled = true;
            elements.btnExtract.innerHTML = '<span class="loading"></span> جاري الاستخراج مع الحفاظ على التنسيق...';
            elements.progressBar.style.width = '10%';
            
            try {
                const genAI = new GoogleGenerativeAI(appState.API_KEY);
                const model = genAI.getGenerativeModel({ model: appState.selectedModel });
                
                elements.progressBar.style.width = '30%';
                
                const reader = new FileReader();
                
                reader.onloadend = async () => {
                    try {
                        const base64Data = reader.result.split(',')[1];
                        elements.progressBar.style.width = '50%';
                        
                        // بناء النص التوضيحي للحفاظ على التنسيق
                        const promptText = buildFormattedExtractionPrompt(file.type);
                        
                        elements.progressBar.style.width = '70%';
                        
                        const result = await model.generateContent([
                            promptText,
                            { inlineData: { data: base64Data, mimeType: file.type } }
                        ]);
                        
                        const response = await result.response;
                        const extractedText = response.text();
                        
                        // حفظ البيانات المستخرجة
                        appState.extractedData = extractedText;
                        appState.lastExtractedData = extractedText;
                        
                        // عرض البيانات مع الحفاظ على التنسيق
                        displayFormattedResults(extractedText);
                        
                        elements.progressBar.style.width = '100%';
                        
                        // معالجة البيانات تلقائياً مع الحفاظ على IDs
                        const processedData = await processExtractedDataWithIDs(extractedText);
                        
                        if (processedData.length > 0) {
                            toastr.success(`تم استخراج ${processedData.length} طالب بنجاح مع الحفاظ على التنسيق!`);
                            
                            // الانتقال إلى تبويب البيانات
                            setTimeout(() => {
                                switchTab('input');
                                toastr.info('يمكنك الآن مراجعة البيانات المستخرجة');
                            }, 1000);
                        } else {
                            toastr.warning('تم استخراج البيانات بنجاح. يرجى مراجعة وتعديل البيانات يدوياً.');
                        }
                        
                    } catch (apiError) {
                        console.error("❌ API Error:", apiError);
                        handleApiError(apiError);
                    } finally {
                        finishProcessing();
                    }
                };
                
                reader.onerror = () => {
                    toastr.error('حدث خطأ أثناء قراءة الملف');
                    finishProcessing();
                };
                
                reader.readAsDataURL(file);
                
            } catch (error) {
                console.error("❌ General error:", error);
                toastr.error(`حدث خطأ: ${error.message}`);
                finishProcessing();
            }
        }

        function buildFormattedExtractionPrompt(fileType) {
            let prompt = `أنا أريد استخراج نتائج الطلاب من هذا الملف مع الحفاظ الكامل على التنسيق والترتيب والجداول.

المطلوب استخراج البيانات التالية بدقة مع الحفاظ على الشكل الأصلي:
1. أسماء الطلاب الكاملة (مع إنشاء ID فريد لكل طالب)
2. الدرجات (من 40 أو النسبة المئوية)
3. المواد الدراسية
4. الفصول أو الأقسام
5. أي بيانات إضافية موجودة

تعليمات مهمة للغاية:
1. الحفاظ على تنسيق الجداول: اترك الجداول كما هي مع الحفاظ على الأعمدة والصفوف
2. الحفاظ على الترتيب: لا تغير ترتيب البيانات الأصلي
3. إنشاء IDs: لكل طالب، أنشئ ID فريداً بالشكل: student_[رقم]_[ثلاثة أحرف عشوائية]
4. التنسيق: استخدم علامات الجداول (|) لفصل الأعمدة و (-) للفواصل
5. الأرقام: حافظ على الأرقام العربية والإنجليزية كما هي
6. إذا كانت الدرجات بنسبة مئوية، حولها إلى درجة من 40 (الدرجة = (النسبة × 40) ÷ 100)
7. إذا لم يتم ذكر المادة، استخدم "عام"
8. إذا لم يتم ذكر الفصل، استخدم "غير محدد"

الشكل المطلوب للبيانات مع الحفاظ على التنسيق:
| ID | الاسم | المادة | الفصل | الدرجة/40 |
|----|-------|--------|-------|-----------|
| student_001_abc | أحمد محمد | الرياضيات | 2/أ | 35 |
| student_002_def | سارة علي | اللغة العربية | 2/ب | 28 |
| student_003_ghi | محمد حسن | العلوم | 2/ج | 32 |

مثال للجداول المعقدة:
┌────────────┬────────────┬────────────┬────────────┐
│     ID     │    الاسم   │   المادة   │   الدرجة   │
├────────────┼────────────┼────────────┼────────────┤
│ student_004_jkl │ فاطمة عبدالله │ الدراسات │     38     │
│ student_005_mno │ خالد إبراهيم │ الإنجليزية │     29     │
└────────────┴────────────┴────────────┴────────────┘`;

            if (fileType.startsWith('image/')) {
                prompt += `

⚠️ ملاحظة: هذا ملف صورة، لذا ركز على:
1. قراءة النصوص بوضوح ودقة
2. التعرف على الجداول مع الحفاظ على حدودها
3. تتبع الصفوف والأعمدة بدقة
4. التعرف على الأرقام العربية والإنجليزية`;
            } else if (fileType === 'application/pdf') {
                prompt += `

⚠️ ملاحظة: هذا ملف PDF، لذا:
1. استخرج البيانات من جميع الصفحات
2. حافظ على ترتيب الجداول عبر الصفحات
3. تعرف على التنسيقات المختلفة (غامق، مائل، خطوط)
4. احتفظ بالعناوين والعناوين الفرعية`;
            }
            
            return prompt;
        }

        async function processExtractedDataWithIDs(extractedText) {
            try {
                console.log("🔧 معالجة البيانات مع الحفاظ على التنسيق والIDs...");
                
                const lines = extractedText.split('\n');
                const students = [];
                const usedIDs = new Set();
                
                for (const line of lines) {
                    const studentData = parseFormattedStudentLine(line, usedIDs);
                    if (studentData) {
                        students.push(studentData);
                        usedIDs.add(studentData.id);
                    }
                }
                
                // إذا لم نجد بيانات، حاول البحث بأنماط مختلفة
                if (students.length === 0) {
                    const altStudents = alternativeParsingWithIDs(extractedText, usedIDs);
                    students.push(...altStudents);
                }
                
                // حفظ الطلاب المستخرجين
                if (students.length > 0) {
                    appState.students = students;
                    updateStudentsTable();
                    updateAnalysis();
                    saveToLocalStorage();
                    
                    // تحديث عرض البيانات الأولية
                    elements.extractedDataSection.classList.remove('hidden');
                    elements.rawDataPreview.textContent = extractedText;
                    
                    // تحديث الإحصائيات
                    updateStats();
                    
                    // إظهار رسالة نجاح
                    showSuccessMessage(`تم استخراج ${students.length} طالب بنجاح مع الحفاظ على التنسيق`);
                }
                
                return students;
                
            } catch (error) {
                console.error("❌ خطأ في المعالجة:", error);
                toastr.error('حدث خطأ أثناء معالجة البيانات: ' + error.message);
                return [];
            }
        }

        function parseFormattedStudentLine(line, usedIDs) {
            const cleanLine = line.trim();
            if (!cleanLine || cleanLine.length < 3) return null;
            
            // أنماط مختلفة مع دعم التنسيق
            const patterns = [
                // النمط مع ID وجداول: | ID | الاسم | المادة | الفصل | الدرجة |
                /\|\s*(student_\d+_[a-z]{3})\s*\|\s*([^|]+)\s*\|\s*([^|]+)\s*\|\s*([^|]+)\s*\|\s*([\d\.]+)\s*\|/i,
                // النمط مع ID: student_001_abc | أحمد محمد | الرياضيات | 2/أ | 35
                /(student_\d+_[a-z]{3})\s*[\|\\-]\s*([^|\\-]+)\s*[\|\\-]\s*([^|\\-]+)\s*[\|\\-]\s*([^|\\-]+)\s*[\|\\-]\s*([\d\.]+)/i,
                // النمط مع جدول Unicode
                /([^\s]+)\s+([^\s]+(?:\s+[^\s]+)*)\s+([^\s]+)\s+([^\s]+)\s+([\d\.]+)/,
                // النمط بسيط: الاسم - المادة - الفصل - الدرجة
                /([^-\|]+)\s*[\-\|]\s*([^-\|]+)\s*[\-\|]\s*([^-\|]+)\s*[\-\|]\s*([\d\.]+)/
            ];
            
            for (const pattern of patterns) {
                const match = cleanLine.match(pattern);
                if (match) {
                    let id, name, subject, className, score;
                    
                    if (pattern === patterns[0] || pattern === patterns[1]) {
                        id = match[1].trim();
                        name = match[2].trim();
                        subject = match[3].trim();
                        className = match[4].trim();
                        score = parseFloat(match[5]);
                    } else if (pattern === patterns[2]) {
                        id = match[1].trim();
                        name = match[2].trim();
                        subject = match[3].trim();
                        className = match[4].trim();
                        score = parseFloat(match[5]);
                    } else if (pattern === patterns[3]) {
                        name = match[1].trim();
                        subject = match[2].trim();
                        className = match[3].trim();
                        score = parseFloat(match[4]);
                        id = generateStudentID(name, usedIDs);
                    }
                    
                    // تحويل النسبة المئوية إلى درجة من 40
                    if (score > 40 && score <= 100) {
                        score = (score * 40) / 100;
                    }
                    
                    // التأكد من صحة الدرجة وإنشاء ID إذا لزم الأمر
                    if (score >= 0 && score <= 40 && name.length > 1) {
                        if (!id || usedIDs.has(id)) {
                            id = generateStudentID(name, usedIDs);
                        }
                        
                        return {
                            id: id,
                            name: cleanArabicText(name),
                            subject: cleanArabicText(subject) || "عام",
                            className: cleanArabicText(className) || "غير محدد",
                            score: parseFloat(score.toFixed(1)),
                            level: getLevel(score),
                            originalLine: cleanLine
                        };
                    }
                }
            }
            
            return null;
        }

        function generateStudentID(name, usedIDs) {
            const baseName = name.replace(/[^a-zA-Zأ-ي]/g, '').substring(0, 3).toLowerCase();
            let id, counter = 1;
            
            do {
                const randomChars = Math.random().toString(36).substring(2, 5);
                id = `student_${appState.studentIdCounter++}_${baseName || randomChars}`;
            } while (usedIDs.has(id));
            
            return id;
        }

        function alternativeParsingWithIDs(text, usedIDs) {
            const students = [];
            const lines = text.split('\n');
            
            for (const line of lines) {
                const cleanLine = line.trim();
                if (cleanLine.length < 2) continue;
                
                // البحث عن أرقام في السطر
                const numberMatches = cleanLine.match(/(\d+\.?\d*)/g);
                if (numberMatches) {
                    for (const numberStr of numberMatches) {
                        let score = parseFloat(numberStr);
                        
                        // تحويل النسبة المئوية
                        if (score > 40 && score <= 100) {
                            score = (score * 40) / 100;
                        }
                        
                        if (score >= 0 && score <= 40) {
                            // استخراج الاسم (إزالة الأرقام والرموز)
                            let name = cleanLine.replace(/(\d+\.?\d*)/g, '')
                                              .replace(/[^\u0600-\u06FF\s]/g, '')
                                              .trim();
                            
                            if (name.length > 1) {
                                const id = generateStudentID(name, usedIDs);
                                usedIDs.add(id);
                                
                                students.push({
                                    id: id,
                                    name: cleanArabicText(name),
                                    subject: "عام",
                                    className: "غير محدد",
                                    score: parseFloat(score.toFixed(1)),
                                    level: getLevel(score),
                                    originalLine: cleanLine
                                });
                            }
                        }
                    }
                }
            }
            
            return students;
        }

        function cleanArabicText(text) {
            if (!text) return '';
            // إزالة الرموز والمسافات الزائدة مع الحفاظ على التنسيق
            return text.replace(/[^\u0600-\u06FFa-zA-Z0-9\s\/\-]/g, '')
                      .replace(/\s+/g, ' ')
                      .trim();
        }

        function handleApiError(apiError) {
            let errorMessage = 'حدث خطأ أثناء معالجة الملف';
            
            if (apiError.message.includes('404') || apiError.message.includes('not found')) {
                errorMessage = 'النموذج المحدد غير متوفر. جرب اختيار نموذج آخر من القائمة.';
                
                // اقتراح نموذج بديل
                if (appState.availableModels.length > 0) {
                    const altModel = appState.availableModels.find(m => 
                        !m.name.includes('flash') && 
                        m.name.includes('gemini')
                    );
                    
                    if (altModel) {
                        errorMessage += ` جرب استخدام ${altModel.name.split('/').pop()}`;
                    }
                }
            } else if (apiError.message.includes('API key not valid')) {
                errorMessage = 'مفتاح API غير صالح. يرجى التحقق من المفتاح وإعادة المحاولة.';
            } else if (apiError.message.includes('quota')) {
                errorMessage = 'تم تجاوز الحد المسموح. جرب استخدام نموذج مختلف أو مفتاح API آخر.';
            } else if (apiError.message.includes('safety')) {
                errorMessage = 'تم رفض المحتوى لاعتبارات السلامة. حاول مع ملف آخر.';
            } else {
                errorMessage += ': ' + apiError.message.split(':')[0];
            }
            
            toastr.error(errorMessage);
            
            elements.result.innerHTML = `
                <div style="text-align: center; color: var(--danger-color); padding: 30px;">
                    <i class="fas fa-exclamation-triangle" style="font-size: 48px; margin-bottom: 15px;"></i>
                    <h3 style="color: #c0392b;">خطأ في المعالجة</h3>
                    <p style="margin-top: 10px; color: var(--text-light);">${errorMessage}</p>
                    <div style="margin-top: 20px; display: flex; flex-wrap: wrap; gap: 10px; justify-content: center;">
                        <button onclick="retryWithDifferentModel()" style="background: var(--primary-color); color: white; border: none; padding: 10px 20px; border-radius: 6px; cursor: pointer; display: flex; align-items: center; gap: 5px;">
                            <i class="fas fa-sync-alt"></i> المحاولة بنموذج مختلف
                        </button>
                        <button onclick="loadAvailableModels()" style="background: var(--secondary-color); color: white; border: none; padding: 10px 20px; border-radius: 6px; cursor: pointer; display: flex; align-items: center; gap: 5px;">
                            <i class="fas fa-redo"></i> تحديث النماذج
                        </button>
                    </div>
                </div>
            `;
        }

        function retryWithDifferentModel() {
            if (appState.availableModels.length > 1) {
                const currentIndex = appState.availableModels.findIndex(m => m.name === appState.selectedModel);
                const nextIndex = (currentIndex + 1) % appState.availableModels.length;
                const newModel = appState.availableModels[nextIndex];
                
                elements.modelSelect.value = newModel.name;
                appState.selectedModel = newModel.name;
                updateModelInfo();
                
                toastr.info(`تم التغيير إلى النموذج: ${newModel.name.split('/').pop()}`);
                
                if (elements.fileInput.files[0]) {
                    setTimeout(() => extractAndAnalyze(), 1000);
                }
            }
        }

        function displayFormattedResults(text) {
            // تقسيم النص إلى أسطر
            const lines = text.split('\n');
            let formattedHTML = '';
            
            // التحقق مما إذا كان النص يحتوي على جداول
            const hasTables = text.includes('|') || text.includes('┌') || text.includes('├') || text.includes('└');
            
            if (hasTables && appState.preservedFormatting) {
                // عرض الجداول مع تنسيق HTML
                formattedHTML = formatTablesHTML(text);
            } else {
                // عرض النص العادي مع الحفاظ على التنسيق
                formattedHTML = formatPlainTextHTML(text);
            }
            
            elements.result.innerHTML = `
                <div style="background: var(--light-color); padding: 15px; border-radius: 8px; margin-bottom: 20px; border-right: 4px solid var(--primary-color);">
                    <h3 style="color: var(--dark-color); margin-top: 0; display: flex; align-items: center; gap: 10px;">
                        <i class="fas fa-check-circle" style="color: var(--success-color);"></i>
                        تم استخراج البيانات بنجاح مع الحفاظ على التنسيق
                    </h3>
                    <p style="color: var(--text-light); margin: 5px 0;">
                        <i class="fas fa-table"></i> تم التعرف على ${hasTables ? 'جداول' : 'نصوص'} 
                        <span style="margin: 0 10px;">|</span>
                        <i class="fas fa-font"></i> ${text.length} حرف
                        <span style="margin: 0 10px;">|</span>
                        <i class="fas fa-clock"></i> ${((Date.now() - appState.processingStartTime) / 1000).toFixed(1)} ثانية
                    </p>
                </div>
                <div style="background: white; padding: 20px; border-radius: 8px; border: 1px solid var(--border-color); max-height: 400px; overflow-y: auto; direction: ltr; font-family: 'Courier New', monospace;">
                    ${formattedHTML}
                </div>
                <div style="margin-top: 20px; text-align: center; display: flex; flex-wrap: wrap; gap: 10px; justify-content: center;">
                    <button onclick="processExtractedData()" style="background: var(--primary-color); color: white; border: none; padding: 12px 24px; border-radius: 6px; cursor: pointer; display: flex; align-items: center; gap: 8px;">
                        <i class="fas fa-robot"></i> معالجة البيانات تلقائياً
                    </button>
                    <button onclick="manualDataEdit()" style="background: var(--secondary-color); color: white; border: none; padding: 12px 24px; border-radius: 6px; cursor: pointer; display: flex; align-items: center; gap: 8px;">
                        <i class="fas fa-edit"></i> تحرير يدوي
                    </button>
                    <button onclick="toggleFormatting()" style="background: var(--info-color); color: white; border: none; padding: 12px 24px; border-radius: 6px; cursor: pointer; display: flex; align-items: center; gap: 8px;">
                        <i class="fas fa-code"></i> ${appState.preservedFormatting ? 'إخفاء التنسيق' : 'إظهار التنسيق'}
                    </button>
                </div>
            `;
            
            // تحديث الإحصائيات
            updateStats();
        }

        function formatTablesHTML(text) {
            const lines = text.split('\n');
            let html = '<div style="font-family: monospace; line-height: 1.4;">';
            let inTable = false;
            let tableHTML = '';
            
            for (let line of lines) {
                line = line.trim();
                
                // اكتشاف بداية الجدول
                if (line.includes('┌') || (line.includes('|') && line.split('|').length > 2)) {
                    inTable = true;
                    tableHTML = '<div style="overflow-x: auto; margin: 10px 0;">';
                    tableHTML += '<table style="border-collapse: collapse; width: 100%; background: white; border: 1px solid #ddd;">';
                    
                    // إضافة السطر الأول كرأس
                    const cells = line.split('|').filter(cell => cell.trim());
                    if (cells.length > 0) {
                        tableHTML += '<thead><tr style="background: #f5f5f5;">';
                        cells.forEach(cell => {
                            tableHTML += `<th style="border: 1px solid #ddd; padding: 8px; text-align: center; font-weight: bold;">${cell.trim()}</th>`;
                        });
                        tableHTML += '</tr></thead><tbody>';
                    }
                }
                // اكتشاف نهاية الجدول
                else if (line.includes('└') || (inTable && line === '')) {
                    inTable = false;
                    tableHTML += '</tbody></table></div>';
                    html += tableHTML;
                    tableHTML = '';
                }
                // معالجة صفوف الجدول
                else if (inTable && (line.includes('|') || line.includes('├') || line.includes('┤'))) {
                    if (line.includes('├') || line.includes('┤')) {
                        // خطوط الفصل، نتخطاها
                        continue;
                    }
                    
                    const cells = line.split('|').filter(cell => cell.trim());
                    if (cells.length > 0) {
                        tableHTML += '<tr>';
                        cells.forEach((cell, index) => {
                            const style = index === 0 ? 'font-family: monospace; font-weight: bold; color: var(--primary-color);' : '';
                            tableHTML += `<td style="border: 1px solid #ddd; padding: 6px; ${style}">${cell.trim()}</td>`;
                        });
                        tableHTML += '</tr>';
                    }
                }
                // النص العادي
                else if (!inTable && line) {
                    html += `<div style="margin: 5px 0; padding: 5px; border-left: 3px solid var(--info-color); background: #f8f9fa;">${line}</div>`;
                }
            }
            
            html += '</div>';
            return html;
        }

        function formatPlainTextHTML(text) {
            const lines = text.split('\n');
            let html = '<div style="white-space: pre-wrap; word-wrap: break-word;">';
            
            lines.forEach(line => {
                if (line.trim()) {
                    // تلوين IDs
                    if (line.includes('student_')) {
                        line = line.replace(/(student_\d+_[a-z]{3})/gi, '<span style="color: var(--primary-color); font-weight: bold;">$1</span>');
                    }
                    // تلوين الأرقام
                    line = line.replace(/(\d+\.?\d*)/g, '<span style="color: var(--success-color); font-weight: bold;">$1</span>');
                    html += line + '<br>';
                }
            });
            
            html += '</div>';
            return html;
        }

        function toggleFormatting() {
            appState.preservedFormatting = !appState.preservedFormatting;
            if (appState.lastExtractedData) {
                displayFormattedResults(appState.lastExtractedData);
            }
        }

        function manualDataEdit() {
            elements.result.innerHTML = `
                <div style="background: var(--light-color); padding: 15px; border-radius: 8px; margin-bottom: 20px; border-right: 4px solid var(--warning-color);">
                    <h3 style="color: var(--dark-color); margin-top: 0; display: flex; align-items: center; gap: 10px;">
                        <i class="fas fa-edit" style="color: var(--warning-color);"></i>
                        تحرير البيانات يدوياً
                    </h3>
                    <p style="color: var(--text-light); margin: 5px 0;">
                        قم بتحرير البيانات مع الحفاظ على تنسيق الجداول (استخدم | لفصل الأعمدة)
                    </p>
                </div>
                <textarea id="manualEdit" style="width: 100%; height: 300px; padding: 15px; border: 2px solid var(--border-color); border-radius: 8px; font-family: 'Courier New', monospace; direction: ltr; font-size: 14px; line-height: 1.4;">${appState.extractedData}</textarea>
                <div style="margin-top: 20px; text-align: center; display: flex; flex-wrap: wrap; gap: 10px; justify-content: center;">
                    <button onclick="processManualEdit()" style="background: var(--primary-color); color: white; border: none; padding: 12px 24px; border-radius: 6px; cursor: pointer; display: flex; align-items: center; gap: 8px;">
                        <i class="fas fa-cogs"></i> معالجة النص المحرر
                    </button>
                    <button onclick="displayFormattedResults(appState.extractedData)" style="background: var(--secondary-color); color: white; border: none; padding: 12px 24px; border-radius: 6px; cursor: pointer; display: flex; align-items: center; gap: 8px;">
                        <i class="fas fa-times"></i> إلغاء
                    </button>
                </div>
            `;
        }

        function processManualEdit() {
            const editedText = document.getElementById('manualEdit').value;
            appState.extractedData = editedText;
            appState.lastExtractedData = editedText;
            processExtractedData();
        }

        function finishProcessing() {
            elements.btnExtract.disabled = false;
            elements.btnExtract.innerHTML = '<i class="fas fa-magic"></i> استخراج وتحليل تلقائي';
            
            setTimeout(() => {
                elements.progressBar.style.width = '0%';
            }, 1000);
        }

        // معالجة البيانات المستخرجة يدوياً
        async function processExtractedData() {
            if (!appState.extractedData) {
                toastr.error('لا توجد بيانات مستخرجة. يرجى استخراج النصوص أولاً.');
                return;
            }
            
            const button = event?.target?.closest('button');
            if (button) {
                button.innerHTML = '<span class="loading"></span> جاري المعالجة...';
                button.disabled = true;
            }
            
            try {
                const students = await processExtractedDataWithIDs(appState.extractedData);
                
                if (students.length > 0) {
                    toastr.success(`تمت معالجة ${students.length} طالب بنجاح مع الحفاظ على التنسيق`);
                    
                    // تحديث عرض البيانات
                    elements.extractedDataSection.classList.remove('hidden');
                    elements.rawDataPreview.textContent = appState.extractedData;
                    
                    // الانتقال إلى تحليل النتائج
                    setTimeout(() => switchTab('analysis'), 500);
                } else {
                    toastr.warning('لم يتم العثور على بيانات طلاب في النص المستخرج. يرجى مراجعة تنسيق البيانات.');
                    manualDataEdit();
                }
                
            } catch (error) {
                console.error("❌ خطأ في معالجة البيانات:", error);
                toastr.error('حدث خطأ أثناء معالجة البيانات: ' + error.message);
            } finally {
                if (button) {
                    button.innerHTML = '<i class="fas fa-robot"></i> معالجة البيانات المستخرجة';
                    button.disabled = false;
                }
            }
        }

        // إدارة البيانات
        function getLevel(score) {
            if (score >= 36) return {name: 'ممتاز', class: 'excellent'};
            if (score >= 32) return {name: 'جيد جدًا', class: 'verygood'};
            if (score >= 28) return {name: 'جيد', class: 'good'};
            if (score >= 20) return {name: 'مقبول', class: 'pass'};
            return {name: 'ضعيف', class: 'weak'};
        }

        function updateStudentsTable() {
            const tbody = elements.studentsTableBody;
            const students = appState.students;
            
            if (students.length === 0) {
                tbody.innerHTML = `
                    <tr>
                        <td colspan="7" style="text-align:center; padding:30px; color:var(--text-light);">
                            <i class="fas fa-users-slash" style="font-size:2rem; display:block; margin-bottom:10px; color:#adb5bd;"></i>
                            لا توجد بيانات، يرجى استخراج البيانات أولاً
                        </td>
                    </tr>
                `;
                elements.studentsCount.textContent = '0';
                return;
            }
            
            let html = '';
            
            students.forEach((student, index) => {
                html += `
                    <tr>
                        <td>${index + 1}</td>
                        <td>
                            <div style="font-weight: bold;">${student.name}</div>
                            <div style="font-size: 0.8rem; color: var(--primary-color); font-family: monospace;">${student.id}</div>
                        </td>
                        <td>${student.subject}</td>
                        <td>${student.className}</td>
                        <td><strong style="color: var(--dark-color);">${student.score}</strong></td>
                        <td><span class="level-badge level-${student.level.class}">${student.level.name}</span></td>
                        <td>
                            <button class="delete-btn" onclick="deleteStudent('${student.id}')">
                                <i class="fas fa-trash"></i> حذف
                            </button>
                        </td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
            elements.studentsCount.textContent = students.length;
        }

        function deleteStudent(id) {
            if (confirm('هل أنت متأكد من حذف هذا الطالب؟')) {
                appState.students = appState.students.filter(student => student.id !== id);
                updateStudentsTable();
                updateAnalysis();
                toastr.success('تم حذف الطالب بنجاح');
                saveToLocalStorage();
            }
        }

        function searchStudents() {
            const searchTerm = elements.searchInput.value.toLowerCase();
            const rows = elements.studentsTableBody.querySelectorAll('tr');
            
            rows.forEach(row => {
                const text = row.textContent.toLowerCase();
                row.style.display = text.includes(searchTerm) ? '' : 'none';
            });
        }

        function clearAllData() {
            if (confirm('هل أنت متأكد من مسح جميع البيانات؟ هذا الإجراء لا يمكن التراجع عنه.')) {
                appState.students = [];
                appState.extractedData = '';
                appState.studentIdCounter = 1;
                updateStudentsTable();
                updateAnalysis();
                elements.extractedDataSection.classList.add('hidden');
                elements.searchInput.value = '';
                toastr.success('تم مسح جميع البيانات بنجاح');
                saveToLocalStorage();
            }
        }

        function refreshDataView() {
            updateStudentsTable();
            if (appState.extractedData) {
                elements.extractedDataSection.classList.remove('hidden');
                elements.rawDataPreview.textContent = appState.extractedData;
            }
            toastr.info('تم تحديث العرض');
        }

        // تحليل البيانات
        function updateAnalysis() {
            const students = appState.students;
            
            if (students.length === 0) {
                elements.analysisAlert.classList.remove('hidden');
                elements.summaryCards.innerHTML = `
                    <div style="text-align: center; padding: 40px; color: var(--text-light); background: var(--light-color); border-radius: 10px; grid-column: 1 / -1;">
                        <i class="fas fa-chart-bar" style="font-size: 3rem; margin-bottom: 15px; color: #adb5bd;"></i>
                        <h3>لا توجد بيانات لعرض التحليل</h3>
                        <p>يرجى استخراج البيانات أولاً</p>
                    </div>
                `;
                elements.levelDetailsTable.innerHTML = '';
                return;
            }
            
            elements.analysisAlert.classList.add('hidden');
            
            const totalStudents = students.length;
            const totalScore = students.reduce((sum, student) => sum + student.score, 0);
            const avgScore = totalScore / totalStudents;
            const passedStudents = students.filter(student => student.score >= 20).length;
            const passRate = totalStudents > 0 ? (passedStudents / totalStudents * 100).toFixed(1) : 0;
            
            const levelCounts = {
                'ممتاز': 0, 'جيد جدًا': 0, 'جيد': 0, 'مقبول': 0, 'ضعيف': 0
            };
            
            students.forEach(student => {
                levelCounts[student.level.name]++;
            });
            
            const subjectCounts = {};
            const classCounts = {};
            
            students.forEach(student => {
                // عدّ المواد
                if (!subjectCounts[student.subject]) {
                    subjectCounts[student.subject] = {count: 0, totalScore: 0};
                }
                subjectCounts[student.subject].count++;
                subjectCounts[student.subject].totalScore += student.score;
                
                // عدّ الفصول
                if (!classCounts[student.className]) {
                    classCounts[student.className] = {count: 0, totalScore: 0};
                }
                classCounts[student.className].count++;
                classCounts[student.className].totalScore += student.score;
            });
            
            updateSummaryCards(totalStudents, avgScore, passRate, levelCounts);
            updateCharts(levelCounts, subjectCounts, classCounts, students);
            updateLevelDetailsTable(levelCounts, students.length);
        }

        function updateSummaryCards(totalStudents, avgScore, passRate, levelCounts) {
            const highestLevel = Object.entries(levelCounts).reduce((a, b) => a[1] > b[1] ? a : b)[0];
            const highestCount = levelCounts[highestLevel];
            
            elements.summaryCards.innerHTML = `
                <div class="summary-card">
                    <h3><i class="fas fa-users"></i> عدد الطلاب</h3>
                    <div class="value">${totalStudents}</div>
                    <div class="subtext">طالب مستخرج</div>
                </div>
                <div class="summary-card">
                    <h3><i class="fas fa-chart-line"></i> متوسط الدرجات</h3>
                    <div class="value">${avgScore.toFixed(1)}</div>
                    <div class="subtext">من 40</div>
                </div>
                <div class="summary-card">
                    <h3><i class="fas fa-percentage"></i> نسبة النجاح</h3>
                    <div class="value">${passRate}%</div>
                    <div class="subtext">درجة النجاح ≥ 20</div>
                </div>
                <div class="summary-card">
                    <h3><i class="fas fa-trophy"></i> أعلى مستوى</h3>
                    <div class="value">${highestLevel}</div>
                    <div class="subtext">${highestCount} طالب</div>
                </div>
            `;
        }

        function updateCharts(levelCounts, subjectCounts, classCounts, students) {
            // تدمير الرسوم البيانية القديمة
            Object.values(charts).forEach(chart => {
                if (chart) chart.destroy();
            });
            
            // رسم بياني للمستويات
            try {
                const levelCtx = document.getElementById('levelChart').getContext('2d');
                charts.levelChart = new Chart(levelCtx, {
                    type: 'doughnut',
                    data: {
                        labels: Object.keys(levelCounts),
                        datasets: [{
                            data: Object.values(levelCounts),
                            backgroundColor: [
                                'rgba(76, 175, 80, 0.8)',
                                'rgba(0, 150, 136, 0.8)',
                                'rgba(33, 150, 243, 0.8)',
                                'rgba(255, 152, 0, 0.8)',
                                'rgba(244, 67, 54, 0.8)'
                            ],
                            borderColor: [
                                'rgb(76, 175, 80)',
                                'rgb(0, 150, 136)',
                                'rgb(33, 150, 243)',
                                'rgb(255, 152, 0)',
                                'rgb(244, 67, 54)'
                            ],
                            borderWidth: 2
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: { 
                                position: 'bottom',
                                labels: { padding: 20 }
                            }
                        }
                    }
                });
            } catch (error) {
                console.error("خطأ في رسم بياني المستويات:", error);
            }
            
            // رسم بياني للمواد
            try {
                const subjectLabels = Object.keys(subjectCounts);
                const subjectAverages = subjectLabels.map(subject => 
                    (subjectCounts[subject].totalScore / subjectCounts[subject].count).toFixed(1)
                );
                
                const subjectCtx = document.getElementById('subjectChart').getContext('2d');
                charts.subjectChart = new Chart(subjectCtx, {
                    type: 'bar',
                    data: {
                        labels: subjectLabels,
                        datasets: [{
                            label: 'متوسط الدرجة',
                            data: subjectAverages,
                            backgroundColor: 'rgba(26, 92, 158, 0.7)',
                            borderColor: 'rgb(26, 92, 158)',
                            borderWidth: 1
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        scales: {
                            y: {
                                beginAtZero: true,
                                max: 40,
                                ticks: { 
                                    stepSize: 5,
                                    callback: value => value + ' درجة'
                                }
                            }
                        }
                    }
                });
            } catch (error) {
                console.error("خطأ في رسم بياني المواد:", error);
            }
            
            // رسم بياني لتوزيع الدرجات
            try {
                const scoreRanges = ['0-9', '10-19', '20-29', '30-34', '35-40'];
                const scoreCounts = [0, 0, 0, 0, 0];
                
                students.forEach(student => {
                    if (student.score <= 9) scoreCounts[0]++;
                    else if (student.score <= 19) scoreCounts[1]++;
                    else if (student.score <= 29) scoreCounts[2]++;
                    else if (student.score <= 34) scoreCounts[3]++;
                    else scoreCounts[4]++;
                });
                
                const scoreCtx = document.getElementById('scoreDistributionChart').getContext('2d');
                charts.scoreDistributionChart = new Chart(scoreCtx, {
                    type: 'line',
                    data: {
                        labels: scoreRanges,
                        datasets: [{
                            label: 'عدد الطلاب',
                            data: scoreCounts,
                            backgroundColor: 'rgba(156, 39, 176, 0.2)',
                            borderColor: 'rgb(156, 39, 176)',
                            borderWidth: 3,
                            tension: 0.3,
                            fill: true
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        scales: {
                            y: {
                                beginAtZero: true,
                                ticks: { stepSize: 1 }
                            }
                        }
                    }
                });
            } catch (error) {
                console.error("خطأ في رسم بياني توزيع الدرجات:", error);
            }
            
            // رسم بياني للأداء حسب الفصل
            try {
                const classLabels = Object.keys(classCounts).sort();
                const classAverages = classLabels.map(className => 
                    (classCounts[className].totalScore / classCounts[className].count).toFixed(1)
                );
                
                const classCtx = document.getElementById('classPerformanceChart').getContext('2d');
                charts.classPerformanceChart = new Chart(classCtx, {
                    type: 'radar',
                    data: {
                        labels: classLabels,
                        datasets: [{
                            label: 'متوسط الدرجة',
                            data: classAverages,
                            backgroundColor: 'rgba(255, 193, 7, 0.2)',
                            borderColor: 'rgb(255, 193, 7)',
                            borderWidth: 2,
                            pointBackgroundColor: 'rgb(255, 193, 7)'
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        scales: {
                            r: {
                                beginAtZero: true,
                                max: 40,
                                ticks: { stepSize: 10 }
                            }
                        }
                    }
                });
            } catch (error) {
                console.error("خطأ في رسم بياني الأداء حسب الفصل:", error);
            }
        }

        function updateLevelDetailsTable(levelCounts, totalStudents) {
            const levelRanges = {
                'ممتاز': '36 - 40',
                'جيد جدًا': '32 - 35.99',
                'جيد': '28 - 31.99',
                'مقبول': '20 - 27.99',
                'ضعيف': '0 - 19.99'
            };
            
            let tableHTML = `
                <div style="display: grid; grid-template-columns: 1fr 1fr 1fr 1fr; border-bottom: 2px solid var(--border-color); font-weight: bold; background: var(--light-color); border-radius: 8px 8px 0 0; overflow: hidden;">
                    <div style="padding: 12px; text-align: center;">المستوى</div>
                    <div style="padding: 12px; text-align: center;">نطاق الدرجات</div>
                    <div style="padding: 12px; text-align: center;">عدد الطلاب</div>
                    <div style="padding: 12px; text-align: center;">النسبة</div>
                </div>
            `;
            
            const levels = ['ممتاز', 'جيد جدًا', 'جيد', 'مقبول', 'ضعيف'];
            
            levels.forEach(level => {
                const count = levelCounts[level] || 0;
                const percentage = totalStudents > 0 ? ((count / totalStudents) * 100).toFixed(1) : '0';
                const percentageBar = count > 0 ? 
                    `<div style="background: ${getLevelColor(level)}; width: ${percentage}%; height: 8px; border-radius: 4px;"></div>` : '';
                
                tableHTML += `
                    <div style="display: grid; grid-template-columns: 1fr 1fr 1fr 1fr; border-bottom: 1px solid var(--border-color); align-items: center;">
                        <div style="padding: 12px; text-align: center;">
                            <span class="level-badge level-${level}">${level}</span>
                        </div>
                        <div style="padding: 12px; text-align: center; color: var(--text-light);">${levelRanges[level]}</div>
                        <div style="padding: 12px; text-align: center; font-weight: bold;">${count}</div>
                        <div style="padding: 12px; text-align: center;">
                            <div style="display: flex; align-items: center; gap: 10px;">
                                <span style="font-weight: bold; color: var(--dark-color);">${percentage}%</span>
                                ${percentageBar}
                            </div>
                        </div>
                    </div>
                `;
            });
            
            elements.levelDetailsTable.innerHTML = tableHTML;
        }

        function getLevelColor(level) {
            switch(level) {
                case 'ممتاز': return '#4caf50';
                case 'جيد جدًا': return '#009688';
                case 'جيد': return '#2196f3';
                case 'مقبول': return '#ff9800';
                case 'ضعيف': return '#f44336';
                default: return '#666';
            }
        }

        // التقرير النهائي
        async function generatePDF() {
            if (appState.students.length === 0) {
                toastr.error('لا توجد بيانات لإنشاء تقرير');
                return;
            }
            
            const button = event.target.closest('button');
            const originalText = button.innerHTML;
            button.innerHTML = '<span class="loading"></span> جاري إنشاء PDF...';
            button.disabled = true;
            
            updateReportContent();
            
            // انتظر حتى يتم تحميل الرسوم البيانية
            await new Promise(resolve => setTimeout(resolve, 500));
            
            try {
                const element = document.getElementById('reportContent');
                const canvas = await html2canvas(element, {
                    scale: 2,
                    backgroundColor: "#ffffff",
                    useCORS: true,
                    logging: false,
                    allowTaint: true,
                    onclone: function(clonedDoc) {
                        // تحسين العرض للـ PDF
                        const clonedElement = clonedDoc.getElementById('reportContent');
                        if (clonedElement) {
                            clonedElement.style.width = '800px';
                            clonedElement.style.margin = '0 auto';
                        }
                    }
                });
                
                const imgData = canvas.toDataURL("image/jpeg", 0.95);
                const { jsPDF } = window.jspdf;
                const pdf = new jsPDF("p", "mm", "a4");
                const pdfWidth = pdf.internal.pageSize.getWidth();
                const pdfHeight = pdf.internal.pageSize.getHeight();
                
                // حساب ارتفاع الصورة مع الحفاظ على النسبة
                const imgWidth = canvas.width;
                const imgHeight = canvas.height;
                const ratio = imgWidth / imgHeight;
                let finalWidth = pdfWidth - 20; // هامش 10mm من كل جانب
                let finalHeight = finalWidth / ratio;
                
                // إذا كان الارتفاع أكبر من الصفحة، نقوم بتقسيمها
                if (finalHeight > pdfHeight - 20) {
                    finalHeight = pdfHeight - 20;
                    finalWidth = finalHeight * ratio;
                }
                
                // حساب الإحداثيات لتوسيط الصورة
                const x = (pdfWidth - finalWidth) / 2;
                const y = (pdfHeight - finalHeight) / 2;
                
                pdf.addImage(imgData, "JPEG", x, y, finalWidth, finalHeight);
                pdf.save("تقرير_النتائج_المستخرجة.pdf");
                
                toastr.success('تم حفظ التقرير بنجاح');
                
            } catch (error) {
                console.error('❌ خطأ في إنشاء PDF:', error);
                toastr.error('حدث خطأ أثناء إنشاء التقرير: ' + error.message);
            } finally {
                button.innerHTML = originalText;
                button.disabled = false;
            }
        }

        function updateReportContent() {
            const students = appState.students;
            
            if (students.length === 0) {
                elements.reportAlert.classList.remove('hidden');
                elements.reportContent.innerHTML = '';
                return;
            }
            
            elements.reportAlert.classList.add('hidden');
            
            const totalStudents = students.length;
            const totalScore = students.reduce((sum, student) => sum + student.score, 0);
            const avgScore = totalScore / totalStudents;
            const passedStudents = students.filter(student => student.score >= 20).length;
            const passRate = (passedStudents / totalStudents * 100).toFixed(1);
            
            const levelCounts = {
                'ممتاز': 0, 'جيد جدًا': 0, 'جيد': 0, 'مقبول': 0, 'ضعيف': 0
            };
            
            students.forEach(student => {
                levelCounts[student.level.name]++;
            });
            
            const now = new Date();
            const dateStr = now.toLocaleDateString('ar-SA', {
                weekday: 'long',
                year: 'numeric',
                month: 'long',
                day: 'numeric'
            });
            
            let reportHTML = `
                <div style="text-align: center; margin-bottom: 30px; padding: 20px; background: linear-gradient(135deg, var(--primary-color), #0d47a1); color: white; border-radius: 15px; box-shadow: 0 4px 20px rgba(0,0,0,0.2);">
                    <h1 style="margin-bottom: 10px; font-size: 28px; font-weight: 800;">📊 تقرير نتائج الطلاب المستخرجة</h1>
                    <p style="margin-bottom: 5px; font-size: 16px; opacity: 0.9;">التقرير تم إنشاؤه تلقائياً من البيانات المستخرجة</p>
                    <p style="font-size: 14px; opacity: 0.8;">${dateStr}</p>
                </div>
                
                <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-bottom: 30px;">
                    <div style="background: linear-gradient(135deg, #e3f2fd 0%, #bbdefb 100%); padding: 20px; border-radius: 12px; text-align: center; border: 2px solid var(--primary-color); box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
                        <h3 style="color: var(--primary-color); margin-bottom: 10px; font-size: 16px;"><i class="fas fa-users"></i> إجمالي الطلاب</h3>
                        <div style="font-size: 32px; font-weight: 800; color: #0d47a1; margin: 10px 0;">${totalStudents}</div>
                        <div style="font-size: 14px; color: #666;">طالب مستخرج</div>
                    </div>
                    <div style="background: linear-gradient(135deg, #e8f5e9 0%, #c8e6c9 100%); padding: 20px; border-radius: 12px; text-align: center; border: 2px solid #4caf50; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
                        <h3 style="color: #2e7d32; margin-bottom: 10px; font-size: 16px;"><i class="fas fa-chart-line"></i> متوسط الدرجات</h3>
                        <div style="font-size: 32px; font-weight: 800; color: #1b5e20; margin: 10px 0;">${avgScore.toFixed(1)}</div>
                        <div style="font-size: 14px; color: #666;">من 40 درجة</div>
                    </div>
                    <div style="background: linear-gradient(135deg, #fff3e0 0%, #ffe0b2 100%); padding: 20px; border-radius: 12px; text-align: center; border: 2px solid #ff9800; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
                        <h3 style="color: #ef6c00; margin-bottom: 10px; font-size: 16px;"><i class="fas fa-percentage"></i> نسبة النجاح</h3>
                        <div style="font-size: 32px; font-weight: 800; color: #e65100; margin: 10px 0;">${passRate}%</div>
                        <div style="font-size: 14px; color: #666;">${passedStudents} طالب</div>
                    </div>
                </div>
                
                <div style="background: white; padding: 25px; border-radius: 12px; margin-bottom: 30px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); border: 1px solid #e0e0e0;">
                    <h2 style="color: var(--primary-color); margin-bottom: 20px; border-bottom: 3px solid var(--primary-color); padding-bottom: 10px; font-size: 20px;">
                        <i class="fas fa-chart-pie"></i> توزيع الطلاب حسب المستوى
                    </h2>
                    <div style="height: 300px; position: relative;">
                        <canvas id="reportLevelChart"></canvas>
                    </div>
                </div>
                
                <div style="background: white; padding: 25px; border-radius: 12px; margin-bottom: 30px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); border: 1px solid #e0e0e0;">
                    <h2 style="color: var(--primary-color); margin-bottom: 20px; border-bottom: 3px solid var(--primary-color); padding-bottom: 10px; font-size: 20px;">
                        <i class="fas fa-list-ol"></i> تفاصيل النتائج (${totalStudents} طالب)
                    </h2>
                    <div style="overflow-x: auto;">
                        <table style="width: 100%; border-collapse: collapse; border: 1px solid #ddd; font-size: 14px;">
                            <thead>
                                <tr style="background: linear-gradient(135deg, var(--primary-color) 0%, #0d47a1 100%); color: white;">
                                    <th style="padding: 12px; text-align: center; border: 1px solid #0d47a1; font-weight: 600;">#</th>
                                    <th style="padding: 12px; text-align: center; border: 1px solid #0d47a1; font-weight: 600;">ID</th>
                                    <th style="padding: 12px; text-align: center; border: 1px solid #0d47a1; font-weight: 600;">الاسم</th>
                                    <th style="padding: 12px; text-align: center; border: 1px solid #0d47a1; font-weight: 600;">المادة</th>
                                    <th style="padding: 12px; text-align: center; border: 1px solid #0d47a1; font-weight: 600;">الفصل</th>
                                    <th style="padding: 12px; text-align: center; border: 1px solid #0d47a1; font-weight: 600;">الدرجة</th>
                                    <th style="padding: 12px; text-align: center; border: 1px solid #0d47a1; font-weight: 600;">المستوى</th>
                                </tr>
                            </thead>
                            <tbody>
            `;
            
            students.forEach((student, index) => {
                const rowColor = index % 2 === 0 ? '#f8f9fa' : '#ffffff';
                reportHTML += `
                    <tr style="background: ${rowColor};">
                        <td style="padding: 10px; text-align: center; border: 1px solid #eee; font-weight: bold; color: var(--dark-color);">${index + 1}</td>
                        <td style="padding: 10px; text-align: center; border: 1px solid #eee; font-family: monospace; font-size: 12px; color: var(--primary-color);">${student.id}</td>
                        <td style="padding: 10px; text-align: center; border: 1px solid #eee;">${student.name}</td>
                        <td style="padding: 10px; text-align: center; border: 1px solid #eee;">${student.subject}</td>
                        <td style="padding: 10px; text-align: center; border: 1px solid #eee;">${student.className}</td>
                        <td style="padding: 10px; text-align: center; border: 1px solid #eee; font-weight: bold; color: var(--primary-color);">${student.score}</td>
                        <td style="padding: 10px; text-align: center; border: 1px solid #eee;">
                            <span style="color: #fff; padding: 6px 12px; border-radius: 20px; background: ${getLevelColor(student.level.name)}; display: inline-block; min-width: 80px; font-weight: bold; font-size: 12px;">
                                ${student.level.name}
                            </span>
                        </td>
                    </tr>
                `;
            });
            
            reportHTML += `
                            </tbody>
                        </table>
                    </div>
                </div>
                
                <div style="background: linear-gradient(135deg, #f5f5f5 0%, #eeeeee 100%); padding: 20px; border-radius: 12px; text-align: center; margin-top: 30px; border: 1px solid #ddd; box-shadow: 0 2px 4px rgba(0,0,0,0.1);">
                    <p style="color: #666; margin-bottom: 10px; font-size: 14px;">
                        <i class="fas fa-robot"></i> تم إنشاء هذا التقرير تلقائياً بواسطة نظام استخراج وتحليل نتائج الطلاب المتكامل
                    </p>
                    <p style="color: #888; font-size: 12px; margin-bottom: 5px;">
                        <i class="fas fa-clock"></i> ${now.toLocaleString('ar-SA')}
                    </p>
                    <p style="color: #888; font-size: 12px;">
                        <i class="fas fa-code"></i> يدعم جميع الأجهزة ويحافظ على تنسيق الجداول
                    </p>
                </div>
            `;
            
            elements.reportContent.innerHTML = reportHTML;
            
            // رسم الرسم البياني للتقرير
            setTimeout(() => {
                try {
                    const reportLevelCtx = document.getElementById('reportLevelChart').getContext('2d');
                    new Chart(reportLevelCtx, {
                        type: 'pie',
                        data: {
                            labels: Object.keys(levelCounts),
                            datasets: [{
                                data: Object.values(levelCounts),
                                backgroundColor: [
                                    'rgba(76, 175, 80, 0.9)',
                                    'rgba(0, 150, 136, 0.9)',
                                    'rgba(33, 150, 243, 0.9)',
                                    'rgba(255, 152, 0, 0.9)',
                                    'rgba(244, 67, 54, 0.9)'
                                ],
                                borderColor: '#fff',
                                borderWidth: 2
                            }]
                        },
                        options: {
                            responsive: true,
                            maintainAspectRatio: false,
                            plugins: {
                                legend: { 
                                    position: 'bottom',
                                    labels: {
                                        padding: 20,
                                        font: { size: 12 }
                                    }
                                }
                            }
                        }
                    });
                } catch (error) {
                    console.error("❌ خطأ في رسم الرسم البياني للتقرير:", error);
                }
            }, 100);
        }

        function printReport() {
            if (appState.students.length === 0) {
                toastr.error('لا توجد بيانات للطباعة');
                return;
            }
            
            updateReportContent();
            setTimeout(() => {
                const printWindow = window.open('', '_blank');
                printWindow.document.write(`
                    <!DOCTYPE html>
                    <html dir="rtl">
                    <head>
                        <meta charset="UTF-8">
                        <title>تقرير النتائج المستخرجة</title>
                        <style>
                            @media print {
                                @page { margin: 20mm; }
                            }
                            body { 
                                font-family: 'Tajawal', Arial, sans-serif; 
                                margin: 20px; 
                                direction: rtl; 
                                line-height: 1.6;
                                color: #333;
                            }
                            h1 { 
                                color: #1a5c9e; 
                                text-align: center; 
                                margin-bottom: 20px;
                                padding-bottom: 15px;
                                border-bottom: 3px solid #1a5c9e;
                            }
                            .summary { 
                                display: flex; 
                                justify-content: space-around; 
                                margin: 30px 0;
                                flex-wrap: wrap;
                                gap: 15px;
                            }
                            .summary-item { 
                                text-align: center; 
                                padding: 15px;
                                border-radius: 10px;
                                background: #f8f9fa;
                                flex: 1;
                                min-width: 200px;
                                border: 2px solid #ddd;
                            }
                            table { 
                                width: 100%; 
                                border-collapse: collapse; 
                                margin: 20px 0;
                                font-size: 12px;
                            }
                            th { 
                                background: #1a5c9e; 
                                color: white; 
                                padding: 12px;
                                border: 1px solid #0d47a1;
                                font-weight: 600;
                            }
                            td { 
                                padding: 10px; 
                                border: 1px solid #ddd; 
                                text-align: center;
                            }
                            tr:nth-child(even) {
                                background: #f9f9f9;
                            }
                            .footer { 
                                text-align: center; 
                                margin-top: 40px; 
                                color: #666;
                                padding-top: 20px;
                                border-top: 2px solid #eee;
                                font-size: 11px;
                            }
                            .level-badge {
                                color: white;
                                padding: 5px 10px;
                                border-radius: 15px;
                                font-size: 11px;
                                font-weight: bold;
                                display: inline-block;
                                min-width: 70px;
                            }
                        </style>
                        <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
                    </head>
                    <body>
                        ${document.getElementById('reportContent').innerHTML}
                    </body>
                    </html>
                `);
                printWindow.document.close();
                printWindow.focus();
                
                // الانتظار قليلاً قبل الطباعة للسماح بتحميل المحتوى
                setTimeout(() => {
                    printWindow.print();
                    printWindow.close();
                }, 500);
            }, 500);
        }

        function exportToExcel() {
            if (appState.students.length === 0) {
                toastr.error('لا توجد بيانات للتصدير');
                return;
            }
            
            // إنشاء بيانات CSV
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            
            // العنوان والمعلومات
            csvContent += "تقرير نتائج الطلاب المستخرجة\r\n";
            csvContent += `تاريخ الإنشاء: ${new Date().toLocaleString('ar-SA')}\r\n`;
            csvContent += `عدد الطلاب: ${appState.students.length}\r\n\r\n`;
            
            // رأس الجدول
            csvContent += "م,ID,اسم الطالب,المادة,الفصل,الدرجة,المستوى\r\n";
            
            // البيانات
            appState.students.forEach((student, index) => {
                csvContent += `${index + 1},${student.id},${student.name},${student.subject},${student.className},${student.score},${student.level.name}\r\n`;
            });
            
            // الإحصائيات
            const totalStudents = appState.students.length;
            const totalScore = appState.students.reduce((sum, student) => sum + student.score, 0);
            const avgScore = totalScore / totalStudents;
            const passedStudents = appState.students.filter(student => student.score >= 20).length;
            const passRate = (passedStudents / totalStudents * 100).toFixed(1);
            
            csvContent += "\r\nالإحصائيات:\r\n";
            csvContent += `إجمالي الطلاب,${totalStudents}\r\n`;
            csvContent += `متوسط الدرجات,${avgScore.toFixed(1)}\r\n`;
            csvContent += `نسبة النجاح,${passRate}%\r\n`;
            csvContent += `عدد الناجحين,${passedStudents}\r\n`;
            
            // إنشاء رابط التحميل
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", "نتائج_الطلاب_المستخرجة.csv");
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            toastr.success('تم تصدير البيانات إلى ملف Excel');
        }

        function shareReport() {
            if (appState.students.length === 0) {
                toastr.error('لا توجد بيانات للمشاركة');
                return;
            }
            
            const text = `تقرير نتائج الطلاب:\nإجمالي الطلاب: ${appState.students.length}\nمتوسط الدرجات: ${(appState.students.reduce((s, st) => s + st.score, 0) / appState.students.length).toFixed(1)}\nنسبة النجاح: ${((appState.students.filter(s => s.score >= 20).length / appState.students.length) * 100).toFixed(1)}%`;
            
            if (navigator.share) {
                navigator.share({
                    title: 'تقرير نتائج الطلاب',
                    text: text,
                    url: window.location.href
                }).catch(error => {
                    console.error('❌ خطأ في المشاركة:', error);
                    toastr.error('حدث خطأ أثناء المشاركة');
                });
            } else {
                // نسخ إلى الحافظة
                navigator.clipboard.writeText(text).then(() => {
                    toastr.success('تم نسخ التقرير إلى الحافظة');
                }).catch(() => {
                    // طريقة بديلة للهواتف القديمة
                    const textArea = document.createElement('textarea');
                    textArea.value = text;
                    document.body.appendChild(textArea);
                    textArea.select();
                    document.execCommand('copy');
                    document.body.removeChild(textArea);
                    toastr.success('تم نسخ التقرير إلى الحافظة');
                });
            }
        }

        // وظائف المساعدة
        function switchTab(tabName) {
            // إخفاء جميع التبويبات
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // إزالة النشاط من جميع التبويبات
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // إظهار التبويب المحدد
            const tabElement = document.getElementById(tabName + '-tab');
            if (tabElement) {
                tabElement.classList.add('active');
                // تمرير سلس للهواتف
                tabElement.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }
            
            // تفعيل زر التبويب
            if (elements.tabs[tabName]) {
                elements.tabs[tabName].classList.add('active');
            }
            
            // تحديث المحتوى حسب التبويب
            if (tabName === 'analysis') {
                updateAnalysis();
            } else if (tabName === 'report') {
                updateReportContent();
            }
            
            // إخفاء لوحة المفاتيح على الهواتف
            if ('ontouchstart' in window) {
                document.activeElement.blur();
            }
        }

        function updateStats() {
            const text = elements.result.textContent || '';
            const charCount = text.replace(/\s/g, '').length;
            const wordCount = text.trim().split(/\s+/).filter(word => word.length > 0).length;
            
            elements.charCount.textContent = `عدد الأحرف: ${charCount}`;
            elements.wordCount.textContent = `عدد الكلمات: ${wordCount}`;
            
            if (appState.processingStartTime) {
                const processingTime = ((Date.now() - appState.processingStartTime) / 1000).toFixed(1);
                elements.processingTime.textContent = `زمن المعالجة: ${processingTime} ثانية`;
            }
        }

        function showSuccessMessage(message) {
            toastr.success(message);
            // تأثير اهتزاز بسيط على زر المعالجة
            elements.processDataBtn.classList.add('shake');
            setTimeout(() => {
                elements.processDataBtn.classList.remove('shake');
            }, 500);
        }

        function saveToLocalStorage() {
            try {
                const data = {
                    students: appState.students,
                    extractedData: appState.extractedData,
                    selectedModel: appState.selectedModel,
                    studentIdCounter: appState.studentIdCounter,
                    lastUpdated: new Date().toISOString()
                };
                localStorage.setItem('studentResultsData', JSON.stringify(data));
                console.log("💾 تم حفظ البيانات في التخزين المحلي");
            } catch (error) {
                console.error("❌ خطأ في حفظ البيانات:", error);
            }
        }

        function loadFromLocalStorage() {
            try {
                const savedData = localStorage.getItem('studentResultsData');
                if (savedData) {
                    const data = JSON.parse(savedData);
                    appState.students = data.students || [];
                    appState.extractedData = data.extractedData || '';
                    appState.selectedModel = data.selectedModel || '';
                    appState.studentIdCounter = data.studentIdCounter || 1;
                    
                    if (appState.selectedModel) {
                        elements.modelSelect.value = appState.selectedModel;
                    }
                    
                    updateStudentsTable();
                    updateAnalysis();
                    
                    // عرض البيانات المستخرجة إذا كانت موجودة
                    if (appState.extractedData) {
                        elements.extractedDataSection.classList.remove('hidden');
                        elements.rawDataPreview.textContent = appState.extractedData;
                        appState.lastExtractedData = appState.extractedData;
                    }
                    
                    console.log("📂 تم تحميل البيانات من التخزين المحلي");
                }
            } catch (error) {
                console.error("❌ خطأ في تحميل البيانات:", error);
            }
        }

        // التعامل مع التغير في اتجاه الشاشة
        window.addEventListener('orientationchange', function() {
            // إعادة تحميل الرسوم البيانية عند تغيير الاتجاه
            setTimeout(() => {
                if (appState.students.length > 0) {
                    updateAnalysis();
                }
            }, 300);
        });

        // التعامل مع اتصال/انفصال الإنترنت
        window.addEventListener('online', () => {
            toastr.info('تم استعادة الاتصال بالإنترنت');
        });

        window.addEventListener('offline', () => {
            toastr.warning('فقدان الاتصال بالإنترنت. البيانات مخزنة محلياً.');
        });

        // تهيئة التطبيق
        document.addEventListener('DOMContentLoaded', initApp);

        // جعل الدوال متاحة عالمياً
        window.switchTab = switchTab;
        window.processExtractedData = processExtractedData;
        window.processManualEdit = processManualEdit;
        window.deleteStudent = deleteStudent;
        window.clearAllData = clearAllData;
        window.refreshDataView = refreshDataView;
        window.retryWithDifferentModel = retryWithDifferentModel;
        window.loadAvailableModels = loadAvailableModels;
        window.generatePDF = generatePDF;
        window.printReport = printReport;
        window.exportToExcel = exportToExcel;
        window.shareReport = shareReport;
        window.scrollToTop = scrollToTop;
        window.toggleFormatting = toggleFormatting;
        window.manualDataEdit = manualDataEdit;

    </script>
</body>
</html>