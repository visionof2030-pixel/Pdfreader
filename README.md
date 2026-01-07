<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>نظام استخراج وتحليل نتائج الطلاب المتكامل</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* إعادة تعيين عام */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            -webkit-text-size-adjust: 100%;
            -moz-text-size-adjust: 100%;
            -ms-text-size-adjust: 100%;
        }

        /* تحسينات للهواتف */
        html {
            font-size: 14px;
        }

        @media (max-width: 360px) { html { font-size: 12px; } }
        @media (min-width: 361px) and (max-width: 480px) { html { font-size: 13px; } }
        @media (min-width: 481px) and (max-width: 768px) { html { font-size: 14px; } }
        @media (min-width: 769px) { html { font-size: 16px; } }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Segoe UI Arabic', Tahoma, Arial, sans-serif;
            background: linear-gradient(135deg, #f0f2f5 0%, #f0f8ff 100%);
            color: #333;
            line-height: 1.6;
            padding: 10px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        .container {
            width: 100%;
            max-width: 1200px;
            margin: 0 auto;
            flex: 1;
        }

        /* العنوان الرئيسي */
        .main-title {
            text-align: center;
            color: #1a5c9e;
            margin: 10px 0 20px 0;
            font-size: 1.8rem;
            font-weight: bold;
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
            padding: 0 10px;
        }

        @media (max-width: 480px) {
            .main-title {
                font-size: 1.5rem;
                margin: 5px 0 15px 0;
            }
        }

        .app-description {
            text-align: center;
            color: #666;
            margin-bottom: 25px;
            font-size: 0.95rem;
            line-height: 1.5;
            padding: 0 10px;
        }

        /* تبويبات النظام */
        .tabs {
            display: flex;
            margin-bottom: 15px;
            border-bottom: 2px solid #ddd;
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
            scrollbar-width: none;
            padding: 0 5px;
        }

        .tabs::-webkit-scrollbar {
            display: none;
        }

        .tab {
            padding: 12px 16px;
            cursor: pointer;
            font-weight: bold;
            border-radius: 8px 8px 0 0;
            background: #e9ecef;
            margin-left: 5px;
            white-space: nowrap;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.9rem;
            transition: all 0.3s;
            min-height: 44px; /* لللمس السهل */
        }

        @media (max-width: 480px) {
            .tab {
                padding: 10px 12px;
                font-size: 0.85rem;
            }
        }

        .tab.active {
            background: #1a5c9e;
            color: white;
        }

        .tab:hover {
            background: #d0d7e0;
        }

        .tab-content {
            display: none;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }

        @media (max-width: 480px) {
            .tab-content {
                padding: 15px;
                margin: 0 -5px 15px -5px;
                border-radius: 8px;
            }
        }

        .tab-content.active {
            display: block;
        }

        /* بطاقة استخراج النصوص */
        .card {
            background: white;
            width: 100%;
            padding: 25px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
            margin: 0 auto 20px auto;
        }

        @media (max-width: 480px) {
            .card {
                padding: 20px 15px;
                border-radius: 12px;
            }
        }

        h2 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 10px;
            font-size: 1.6rem;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        @media (max-width: 480px) {
            h2 {
                font-size: 1.4rem;
            }
        }

        .subtitle {
            text-align: center;
            color: #7f8c8d;
            margin-bottom: 25px;
            font-size: 0.95rem;
            line-height: 1.5;
        }

        /* إعدادات API */
        .config-section {
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
            padding: 20px;
            border-radius: 12px;
            border: 1px solid #dee2e6;
            margin-bottom: 20px;
        }

        @media (max-width: 480px) {
            .config-section {
                padding: 15px;
            }
        }

        .section-title {
            color: #2c3e50;
            margin-top: 0;
            margin-bottom: 15px;
            font-size: 1.1rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .api-config {
            display: grid;
            grid-template-columns: 1fr auto;
            gap: 10px;
            margin-bottom: 15px;
        }

        @media (max-width: 480px) {
            .api-config {
                grid-template-columns: 1fr;
            }
        }

        .api-input {
            padding: 12px 15px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 0.95rem;
            font-family: 'Courier New', monospace;
            width: 100%;
            min-height: 44px;
        }

        .api-input:focus {
            outline: none;
            border-color: #1a5c9e;
            box-shadow: 0 0 0 3px rgba(26, 92, 158, 0.1);
        }

        .btn-save {
            background: linear-gradient(to right, #27ae60, #219653);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            font-size: 0.95rem;
            min-height: 44px;
            white-space: nowrap;
        }

        @media (max-width: 480px) {
            .btn-save {
                width: 100%;
            }
        }

        .btn-save:active {
            transform: scale(0.98);
        }

        .api-status {
            padding: 12px;
            border-radius: 8px;
            font-size: 0.9rem;
            display: flex;
            align-items: center;
            gap: 10px;
            margin-top: 10px;
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

        /* نموذج الاختيار */
        .model-select-container {
            margin: 15px 0;
        }

        .model-select {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 0.95rem;
            background: white;
            cursor: pointer;
            min-height: 44px;
        }

        .model-select:focus {
            outline: none;
            border-color: #1a5c9e;
            box-shadow: 0 0 0 3px rgba(26, 92, 158, 0.1);
        }

        .model-info {
            margin-top: 10px;
            padding: 12px;
            background: #e3f2fd;
            border-radius: 8px;
            color: #1565c0;
            font-size: 0.9rem;
            border: 1px solid #90caf9;
            line-height: 1.5;
        }

        /* رفع الملفات */
        .upload-area {
            border: 3px dashed #3498db;
            padding: 30px 15px;
            text-align: center;
            cursor: pointer;
            border-radius: 12px;
            background: linear-gradient(135deg, #f8fbff 0%, #e6f2ff 100%);
            margin-bottom: 20px;
            transition: all 0.3s;
            min-height: 150px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        @media (max-width: 480px) {
            .upload-area {
                padding: 25px 10px;
                min-height: 120px;
            }
        }

        .upload-area:hover {
            background: linear-gradient(135deg, #e6f2ff 0%, #d4e6ff 100%);
            border-color: #2980b9;
        }

        .upload-area.dragover {
            background: linear-gradient(135deg, #d4edda 0%, #c3e6cb 100%);
            border-color: #27ae60;
        }

        .upload-icon {
            font-size: 3rem;
            color: #3498db;
            margin-bottom: 15px;
        }

        @media (max-width: 480px) {
            .upload-icon {
                font-size: 2.5rem;
                margin-bottom: 10px;
            }
        }

        .upload-text {
            font-size: 1.1rem;
            color: #2c3e50;
            margin-bottom: 8px;
            font-weight: 500;
        }

        @media (max-width: 480px) {
            .upload-text {
                font-size: 1rem;
            }
        }

        .upload-info {
            color: #7f8c8d;
            font-size: 0.9rem;
            margin-top: 5px;
        }

        /* شريط التقدم */
        .progress-bar {
            height: 6px;
            background: #3498db;
            border-radius: 3px;
            margin: 15px 0;
            width: 0%;
            transition: width 0.3s;
        }

        /* أزرار العمل */
        .btn-extract {
            background: linear-gradient(to right, #e74c3c, #c0392b);
            color: white;
            border: none;
            padding: 16px;
            border-radius: 10px;
            width: 100%;
            cursor: pointer;
            font-size: 1.1rem;
            font-weight: bold;
            margin-top: 10px;
            min-height: 50px;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .btn-extract:active {
            transform: scale(0.98);
        }

        .btn-extract:disabled {
            background: #bdc3c7;
            cursor: not-allowed;
        }

        /* النتائج مع التنسيق */
        #result {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 12px;
            min-height: 200px;
            border: 1px solid #dee2e6;
            margin-top: 20px;
            overflow: auto;
            max-height: 500px;
            font-family: 'Arial', 'Segoe UI Arabic', sans-serif;
            line-height: 1.8;
        }

        @media (max-width: 480px) {
            #result {
                padding: 15px;
                max-height: 400px;
            }
        }

        /* تنسيق الجداول في النتائج */
        .result-table {
            border-collapse: collapse;
            width: 100%;
            margin: 15px 0;
            border: 1px solid #ddd;
            background: white;
            font-size: 0.9rem;
            table-layout: fixed;
            word-wrap: break-word;
        }

        .result-table th {
            background: #f2f2f2;
            padding: 12px 8px;
            text-align: right;
            border: 1px solid #ddd;
            font-weight: bold;
            font-size: 0.95rem;
            word-break: break-word;
        }

        .result-table td {
            padding: 10px 8px;
            border: 1px solid #ddd;
            text-align: right;
            vertical-align: top;
            word-break: break-word;
            position: relative;
        }

        /* تنسيق الأسماء الطويلة */
        .student-name {
            display: inline-block;
            max-width: 100%;
            padding: 2px 4px;
            border-radius: 4px;
            margin: 2px 0;
            background: #f8f9fa;
            border: 1px solid #e9ecef;
            word-break: break-word;
            white-space: normal;
            line-height: 1.4;
        }

        .student-name.multi-line {
            display: flex;
            flex-direction: column;
        }

        .student-name .name-line {
            display: block;
        }

        /* إدخال البيانات - تصميم متجاوب */
        .input-section {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        @media (max-width: 480px) {
            .input-section {
                padding: 15px;
            }
        }

        .input-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 15px;
            margin-bottom: 15px;
        }

        @media (max-width: 480px) {
            .input-grid {
                grid-template-columns: 1fr;
                gap: 12px;
            }
        }

        .input-group {
            display: flex;
            flex-direction: column;
        }

        .input-group label {
            margin-bottom: 8px;
            font-weight: bold;
            color: #555;
            font-size: 0.95rem;
        }

        .input-group input,
        .input-group select {
            padding: 12px 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 0.95rem;
            background: white;
            transition: border-color 0.3s;
            width: 100%;
            min-height: 44px;
        }

        .input-group input:focus,
        .input-group select:focus {
            outline: none;
            border-color: #1a5c9e;
            box-shadow: 0 0 0 3px rgba(26, 92, 158, 0.1);
        }

        /* الأزرار العامة */
        .actions {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 20px;
        }

        @media (max-width: 480px) {
            .actions {
                flex-direction: column;
            }
        }

        button {
            background: #25d366;
            color: #fff;
            border: none;
            padding: 14px 20px;
            font-size: 0.95rem;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            flex: 1;
            min-width: 120px;
            min-height: 44px;
            transition: all 0.3s;
            touch-action: manipulation;
        }

        @media (max-width: 480px) {
            button {
                width: 100%;
                min-width: unset;
                padding: 16px 20px;
            }
        }

        button:active {
            transform: scale(0.98);
        }

        button:hover {
            filter: brightness(1.1);
        }

        button.secondary {
            background: #6c757d;
        }

        button.secondary:hover {
            background: #5a6268;
        }

        button.danger {
            background: #dc3545;
        }

        button.danger:hover {
            background: #c82333;
        }

        button.warning {
            background: #ffc107;
            color: #212529;
        }

        button.warning:hover {
            background: #e0a800;
        }

        button.info {
            background: #17a2b8;
        }

        button.info:hover {
            background: #138496;
        }

        /* الجداول */
        .students-table-container {
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
            margin-top: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            background: white;
        }

        .students-table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            min-width: 600px;
            table-layout: fixed;
        }

        @media (max-width: 480px) {
            .students-table {
                min-width: 500px;
                font-size: 0.9rem;
            }
        }

        .students-table th {
            background: #1a5c9e;
            color: white;
            padding: 14px 10px;
            text-align: center;
            font-weight: bold;
            font-size: 0.95rem;
            white-space: nowrap;
        }

        .students-table td {
            padding: 12px 10px;
            text-align: center;
            border-bottom: 1px solid #eee;
            vertical-align: middle;
            word-break: break-word;
        }

        .students-table tr:hover {
            background: #f8f9fa;
        }

        /* رموز المستويات */
        .level-badge {
            color: #fff;
            font-weight: bold;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 0.85rem;
            display: inline-block;
            min-width: 80px;
            text-align: center;
        }

        .level-excellent { background: #4caf50; }
        .level-verygood { background: #009688; }
        .level-good { background: #2196f3; }
        .level-pass { background: #ff9800; }
        .level-weak { background: #f44336; }

        /* أزرار الإجراءات */
        .action-btn {
            padding: 8px 12px;
            border-radius: 6px;
            font-size: 0.85rem;
            display: flex;
            align-items: center;
            gap: 5px;
            margin: 2px;
            min-height: 36px;
        }

        .delete-btn {
            background: #dc3545;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.85rem;
            display: flex;
            align-items: center;
            gap: 5px;
            margin: 0 auto;
        }

        .delete-btn:active {
            transform: scale(0.95);
        }

        /* بطاقات التحليل */
        .summary-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }

        @media (max-width: 480px) {
            .summary-cards {
                grid-template-columns: repeat(2, 1fr);
                gap: 10px;
            }
        }

        .summary-card {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            text-align: center;
            transition: transform 0.3s;
        }

        @media (max-width: 480px) {
            .summary-card {
                padding: 15px;
            }
        }

        .summary-card:hover {
            transform: translateY(-3px);
        }

        .summary-card h3 {
            margin: 0 0 10px 0;
            color: #1a5c9e;
            font-size: 0.95rem;
            font-weight: 600;
        }

        .summary-card .value {
            font-size: 1.8rem;
            font-weight: bold;
            margin: 10px 0;
            color: #333;
        }

        @media (max-width: 480px) {
            .summary-card .value {
                font-size: 1.5rem;
            }
        }

        .summary-card .subtext {
            font-size: 0.85rem;
            color: #666;
        }

        /* الرسوم البيانية */
        .charts-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 20px;
        }

        @media (max-width: 480px) {
            .charts-container {
                grid-template-columns: 1fr;
                gap: 15px;
            }
        }

        .chart-box {
            background: white;
            border: 1px solid #ddd;
            border-radius: 10px;
            padding: 20px;
            height: 300px;
            display: flex;
            flex-direction: column;
        }

        @media (max-width: 480px) {
            .chart-box {
                height: 250px;
                padding: 15px;
            }
        }

        .chart-box h3 {
            margin: 0 0 15px 0;
            color: #1a5c9e;
            font-size: 1rem;
            text-align: center;
        }

        .chart-box canvas {
            flex: 1;
            width: 100% !important;
            height: 100% !important;
        }

        /* الرسائل */
        .alert {
            padding: 15px;
            border-radius: 8px;
            margin: 10px 0;
            display: flex;
            align-items: center;
            gap: 10px;
            animation: slideIn 0.3s ease;
            font-size: 0.95rem;
        }

        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .alert.success {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .alert.warning {
            background: #fff3cd;
            color: #856404;
            border: 1px solid #ffeaa7;
        }

        .alert.error {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .alert.info {
            background: #e3f2fd;
            color: #1565c0;
            border: 1px solid #90caf9;
        }

        /* مخفي */
        .hidden {
            display: none !important;
        }

        /* التحميل */
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid #f3f3f3;
            border-top: 3px solid #3498db;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-right: 10px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* تنسيقات النصوص في النتائج */
        .text-bold {
            font-weight: bold;
            color: #2c3e50;
        }

        .text-italic {
            font-style: italic;
            color: #7f8c8d;
        }

        .text-highlight {
            background: #fff3cd;
            padding: 2px 4px;
            border-radius: 3px;
        }

        .text-muted {
            color: #95a5a6;
            font-size: 0.9em;
        }

        /* الروابط */
        a {
            color: #3498db;
            text-decoration: none;
            transition: color 0.3s;
        }

        a:hover {
            color: #2980b9;
            text-decoration: underline;
        }

        /* التمرير السلس */
        .smooth-scroll {
            scroll-behavior: smooth;
        }

        /* تحسينات للـ iOS */
        @supports (-webkit-touch-callout: none) {
            .upload-area,
            button,
            .api-input,
            .model-select,
            .input-group input,
            .input-group select {
                -webkit-appearance: none;
                border-radius: 8px;
            }
        }

        /* تحسينات للـ Huawei/Honor */
        @media screen and (-webkit-min-device-pixel-ratio: 0) and (max-width: 1024px) {
            .card,
            .config-section,
            .summary-card,
            .chart-box {
                -webkit-backface-visibility: hidden;
                backface-visibility: hidden;
                transform: translateZ(0);
            }
        }

        /* دعم متصفحات مختلفة */
        ::-webkit-input-placeholder { color: #95a5a6; }
        ::-moz-placeholder { color: #95a5a6; }
        :-ms-input-placeholder { color: #95a5a6; }
        ::-ms-input-placeholder { color: #95a5a6; }
        ::placeholder { color: #95a5a6; }

        /* تحسين العرض في الوضع الأفقي للهواتف */
        @media (max-height: 500px) and (orientation: landscape) {
            .container {
                padding: 5px;
            }
            
            .card {
                padding: 15px;
                margin-bottom: 10px;
            }
            
            .tabs {
                margin-bottom: 10px;
            }
            
            .tab-content {
                padding: 15px;
                margin-bottom: 10px;
            }
            
            .upload-area {
                min-height: 100px;
                padding: 20px 10px;
            }
        }

        /* تحسينات للشاشات الكبيرة */
        @media (min-width: 1200px) {
            .container {
                padding: 20px;
            }
            
            .card {
                padding: 30px;
            }
            
            .summary-cards {
                grid-template-columns: repeat(4, 1fr);
            }
        }

        /* دعم الأجهزة اللوحية */
        @media (min-width: 769px) and (max-width: 1024px) {
            .input-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .summary-cards {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .charts-container {
                grid-template-columns: repeat(2, 1fr);
            }
        }

        /* تحسينات للأجهزة القديمة */
        @media (max-width: 320px) {
            html {
                font-size: 11px;
            }
            
            .tab {
                padding: 8px 10px;
                font-size: 0.8rem;
            }
            
            button {
                padding: 12px 15px;
                font-size: 0.9rem;
            }
        }

        /* تحسينات للأجهزة المزدوجة الشاشة */
        @media (max-width: 540px) and (max-height: 720px) {
            .container {
                padding: 8px;
            }
            
            .main-title {
                margin: 8px 0 15px 0;
            }
            
            .card {
                padding: 18px 12px;
            }
        }

        /* تنسيقات خاصة للتقرير */
        .report-table {
            width: 100%;
            border-collapse: collapse;
            margin: 15px 0;
            font-size: 0.9rem;
        }

        .report-table th {
            background: #1a5c9e;
            color: white;
            padding: 12px;
            text-align: center;
            border: 1px solid #ddd;
        }

        .report-table td {
            padding: 10px;
            text-align: center;
            border: 1px solid #ddd;
        }

        /* تنسيق النصوص متعددة الأسطر */
        .multi-line-text {
            white-space: pre-wrap;
            word-wrap: break-word;
            line-height: 1.6;
        }

        /* تنسيق الأسماء في الخلايا */
        .name-cell {
            position: relative;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 8px;
        }

        .name-content {
            display: inline-block;
            max-width: 100%;
            text-align: center;
            line-height: 1.4;
        }

        /* ظلال للبطاقات */
        .card-shadow {
            box-shadow: 0 4px 6px rgba(0,0,0,0.1), 0 1px 3px rgba(0,0,0,0.08);
        }

        /* هوامش آمنة للشاشات */
        .safe-area {
            padding-left: env(safe-area-inset-left);
            padding-right: env(safe-area-inset-right);
            padding-top: env(safe-area-inset-top);
            padding-bottom: env(safe-area-inset-bottom);
        }

        @supports (padding: max(0px)) {
            .safe-area {
                padding-left: max(env(safe-area-inset-left), 10px);
                padding-right: max(env(safe-area-inset-right), 10px);
                padding-top: max(env(safe-area-inset-top), 10px);
                padding-bottom: max(env(safe-area-inset-bottom), 10px);
            }
        }

        /* تنسيق النصوص العربية بشكل أفضل */
        .arabic-text {
            font-feature-settings: "calt" 1, "liga" 1, "kern" 1;
            text-rendering: optimizeLegibility;
            -webkit-font-smoothing: antialiased;
            letter-spacing: -0.01em;
        }

        /* تحسينات للشاشات عالية الدقة */
        @media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
            .card,
            .config-section,
            .summary-card {
                border-width: 0.5px;
            }
        }
    </style>
</head>
<body class="smooth-scroll safe-area arabic-text">
    <div class="container">
        <h1 class="main-title">🚀 نظام استخراج وتحليل نتائج الطلاب المتكامل</h1>
        <p class="app-description">
            نظام متكامل لاستخراج النصوص من ملفات PDF والصور مع الحفاظ على التنسيق الكامل للجداول والترتيب
        </p>
        
        <!-- تبويبات النظام -->
        <div class="tabs">
            <div class="tab active" onclick="switchTab('extract')">
                <i class="fas fa-file-import"></i>
                <span>استخراج النصوص</span>
            </div>
            <div class="tab" onclick="switchTab('input')">
                <i class="fas fa-database"></i>
                <span>إدارة البيانات</span>
            </div>
            <div class="tab" onclick="switchTab('analysis')">
                <i class="fas fa-chart-bar"></i>
                <span>تحليل النتائج</span>
            </div>
            <div class="tab" onclick="switchTab('report')">
                <i class="fas fa-file-pdf"></i>
                <span>التقرير النهائي</span>
            </div>
        </div>
        
        <!-- تبويب استخراج النصوص -->
        <div id="extract-tab" class="tab-content active">
            <div class="card card-shadow">
                <h2><i class="fas fa-file-alt"></i> مستخرج النصوص الذكي</h2>
                <p class="subtitle">استخرج النصوص من ملفات PDF والصور مع الحفاظ الكامل على تنسيق الجداول والترتيب الأصلي</p>
                
                <div class="config-section">
                    <div class="section-title">
                        <i class="fas fa-key"></i>
                        <span>إعدادات Google Gemini API</span>
                    </div>
                    
                    <div class="api-config">
                        <input type="password" id="apiKeyInput" class="api-input" 
                               placeholder="أدخل مفتاح Google Gemini API هنا..." 
                               value="" autocomplete="off" spellcheck="false">
                        <button id="saveApiBtn" class="btn-save">
                            <i class="fas fa-save"></i> حفظ المفتاح
                        </button>
                    </div>
                    
                    <div id="apiStatus" class="api-status status-invalid">
                        <i class="fas fa-times-circle"></i>
                        <span id="apiStatusText">لم يتم إضافة مفتاح API بعد</span>
                    </div>
                    
                    <div id="modelTesting" class="api-status status-info hidden">
                        <span class="loading"></span>
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
                    <div class="upload-icon">
                        <i class="fas fa-cloud-upload-alt"></i>
                    </div>
                    <div class="upload-text" id="fileLabel">اسحب ملف PDF أو صورة هنا أو انقر للاختيار</div>
                    <div class="upload-info" id="fileInfo">الحد الأقصى: 10MB | المدعوم: PDF, JPG, PNG, GIF, BMP, WebP</div>
                    <input type="file" id="fileInput" accept=".pdf,.jpg,.jpeg,.png,.gif,.bmp,.webp,application/pdf,image/*" 
                           style="display:none">
                </div>
                
                <div class="progress-bar" id="progressBar"></div>
                
                <button id="btnExtract" class="btn-extract" disabled>
                    <i class="fas fa-magic"></i> استخراج وتحليل تلقائي
                </button>
                
                <div id="result">
                    <div style="text-align: center; color: #7f8c8d; padding: 40px;">
                        <i class="fas fa-file-alt" style="font-size: 48px; margin-bottom: 15px; color: #bdc3c7;"></i>
                        <h3 style="color: #95a5a6;">النتائج ستظهر هنا</h3>
                        <p style="margin-top: 10px;">بعد استخراج النصوص، سيتم معالجتها وتحليلها تلقائياً مع الحفاظ على التنسيق الأصلي.</p>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- تبويب إدارة البيانات -->
        <div id="input-tab" class="tab-content">
            <div class="card card-shadow">
                <h2><i class="fas fa-database"></i> إدارة البيانات المستخرجة</h2>
                <p class="subtitle">عرض وتعديل وتنظيم البيانات التي تم استخراجها مع الحفاظ على التنسيق الأصلي</p>
                
                <div id="alert-message" class="alert hidden"></div>
                
                <div class="actions">
                    <button onclick="processExtractedData()" id="processDataBtn" class="info">
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
                    <button onclick="exportCurrentData()" class="warning">
                        <i class="fas fa-file-export"></i>
                        <span>تصدير البيانات</span>
                    </button>
                </div>
                
                <div id="extractedDataSection" class="hidden">
                    <h3 style="margin-top: 25px; color: #1a5c9e; border-bottom: 2px solid #e9ecef; padding-bottom: 10px;">
                        <i class="fas fa-database"></i> البيانات المستخرجة (النص الخام)
                    </h3>
                    <div style="background: #f8f9fa; padding: 15px; border-radius: 10px; margin-top: 15px; border: 1px solid #dee2e6;">
                        <pre id="rawDataPreview" style="white-space: pre-wrap; font-family: 'Courier New', monospace; max-height: 200px; overflow-y: auto; direction: ltr; font-size: 0.9rem; line-height: 1.5; padding: 10px; background: white; border-radius: 5px;"></pre>
                    </div>
                </div>
                
                <h3 style="margin-top: 25px; color: #1a5c9e; border-bottom: 2px solid #e9ecef; padding-bottom: 10px;">
                    <i class="fas fa-users"></i> الطلاب المستخرجون (${appState.students.length})
                </h3>
                <div class="students-table-container">
                    <table class="students-table" id="studentsList">
                        <thead>
                            <tr>
                                <th style="width: 50px;">#</th>
                                <th style="width: 200px;">اسم الطالب</th>
                                <th style="width: 120px;">المادة</th>
                                <th style="width: 80px;">الفصل</th>
                                <th style="width: 80px;">الدرجة</th>
                                <th style="width: 100px;">المستوى</th>
                                <th style="width: 100px;">الإجراءات</th>
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
            <div class="card card-shadow">
                <h2><i class="fas fa-chart-bar"></i> تحليل النتائج</h2>
                <p class="subtitle">تحليل إحصائي متقدم للبيانات المستخرجة مع رسوم بيانية تفاعلية</p>
                
                <div id="analysis-alert" class="alert warning">
                    <i class="fas fa-exclamation-circle"></i>
                    <span>لا توجد بيانات لعرض التحليل. يرجى استخراج البيانات أولاً.</span>
                </div>
                
                <div class="summary-cards" id="summaryCards">
                    <!-- سيتم إضافة بطاقات الملخص هنا -->
                </div>
                
                <div class="charts-container">
                    <div class="chart-box">
                        <h3><i class="fas fa-chart-pie"></i> توزيع الطلاب حسب المستوى</h3>
                        <canvas id="levelChart"></canvas>
                    </div>
                    <div class="chart-box">
                        <h3><i class="fas fa-chart-bar"></i> متوسط الدرجات حسب المادة</h3>
                        <canvas id="subjectChart"></canvas>
                    </div>
                </div>
                
                <div style="margin-top: 25px; background: white; padding: 20px; border-radius: 10px; box-shadow: 0 2px 10px rgba(0,0,0,0.1); border: 1px solid #e9ecef;">
                    <h3 style="color: #1a5c9e; margin-bottom: 15px; border-bottom: 2px solid #e9ecef; padding-bottom: 10px;">
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
            <div class="card card-shadow">
                <h2><i class="fas fa-file-pdf"></i> التقرير النهائي</h2>
                <p class="subtitle">تقرير شامل للبيانات المستخرجة والتحليل الإحصائي جاهز للطباعة والتوزيع</p>
                
                <div id="report-alert" class="alert warning">
                    <i class="fas fa-exclamation-circle"></i>
                    <span>لا توجد بيانات لإنشاء التقرير. يرجى استخراج البيانات أولاً.</span>
                </div>
                
                <div class="actions">
                    <button onclick="generatePDF()" id="pdfBtn" class="info">
                        <i class="fas fa-download"></i>
                        <span>تحميل PDF</span>
                    </button>
                    <button onclick="printReport()" class="secondary">
                        <i class="fas fa-print"></i>
                        <span>طباعة التقرير</span>
                    </button>
                    <button onclick="exportToExcel()" class="warning">
                        <i class="fas fa-file-excel"></i>
                        <span>تصدير Excel</span>
                    </button>
                    <button onclick="shareReport()" class="success">
                        <i class="fas fa-share-alt"></i>
                        <span>مشاركة</span>
                    </button>
                </div>
                
                <div id="reportContent" style="margin-top: 25px; background: white; padding: 25px; border-radius: 10px; box-shadow: 0 2px 15px rgba(0,0,0,0.1); border: 1px solid #e9ecef;">
                    <!-- سيتم عرض التقرير هنا -->
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script type="module">
        // استيراد مكتبة Google Gemini AI
        import { GoogleGenerativeAI } from "https://esm.run/@google/generative-ai@0.1.0";

        // تخزين بيانات التطبيق
        const appState = {
            API_KEY: localStorage.getItem('gemini_api_key') || '',
            students: [],
            classes: ['2/أ', '2/ب', '2/ج', '2/د', '2/هـ', '2/و', '2/ز', '2/ح', 'غير محدد'],
            extractedData: '',
            availableModels: [],
            selectedModel: localStorage.getItem('selected_model') || '',
            fileType: '',
            nameCounter: 1,
            processedTables: []
        };

        // عناصر واجهة المستخدم
        const elements = {
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
            alertMessage: document.getElementById('alert-message'),
            extractedDataSection: document.getElementById('extractedDataSection'),
            rawDataPreview: document.getElementById('rawDataPreview'),
            studentsTableBody: document.getElementById('studentsTableBody'),
            processDataBtn: document.getElementById('processDataBtn'),
            analysisAlert: document.getElementById('analysis-alert'),
            summaryCards: document.getElementById('summaryCards'),
            levelDetailsTable: document.getElementById('levelDetailsTable'),
            reportAlert: document.getElementById('report-alert'),
            reportContent: document.getElementById('reportContent'),
            pdfBtn: document.getElementById('pdfBtn')
        };

        // تهيئة التطبيق
        function initApp() {
            console.log("جاري تهيئة التطبيق...");
            
            // تحميل البيانات المحفوظة
            loadFromLocalStorage();
            
            // تهيئة إعدادات API
            if (appState.API_KEY) {
                elements.apiKeyInput.value = "••••••••" + appState.API_KEY.slice(-4);
                updateApiStatus(true);
                setTimeout(() => loadAvailableModels(), 500);
            }
            
            // إعداد معالجات الأحداث
            setupEventHandlers();
            
            // تحديث العرض
            updateStudentsTable();
            updateAnalysis();
            
            console.log("تم تهيئة التطبيق بنجاح");
        }

        // إعداد معالجات الأحداث
        function setupEventHandlers() {
            // API Events
            elements.saveApiBtn.addEventListener('click', saveApiKey);
            elements.apiKeyInput.addEventListener('focus', handleApiKeyFocus);
            elements.apiKeyInput.addEventListener('blur', handleApiKeyBlur);
            
            // Model Events
            elements.modelSelect.addEventListener('change', handleModelChange);
            
            // File Events
            elements.dropZone.addEventListener('click', () => elements.fileInput.click());
            elements.fileInput.addEventListener('change', handleFileSelect);
            elements.btnExtract.addEventListener('click', extractAndAnalyze);
            
            // Drag and Drop Events
            setupDragAndDrop();
            
            // Data Management Events
            elements.processDataBtn.addEventListener('click', processExtractedData);
            
            // Keyboard Events
            document.addEventListener('keydown', handleKeyboardShortcuts);
        }

        function handleApiKeyFocus() {
            if (appState.API_KEY && this.value.includes('••••')) {
                this.value = appState.API_KEY;
            }
        }

        function handleApiKeyBlur() {
            if (appState.API_KEY && !this.value.includes('••••')) {
                this.value = "••••••••" + appState.API_KEY.slice(-4);
            }
        }

        function handleModelChange() {
            appState.selectedModel = this.value;
            localStorage.setItem('selected_model', this.value);
            updateModelInfo();
        }

        function setupDragAndDrop() {
            ['dragover', 'dragleave', 'drop'].forEach(eventName => {
                elements.dropZone.addEventListener(eventName, (e) => {
                    e.preventDefault();
                    e.stopPropagation();
                    
                    if (eventName === 'dragover') {
                        elements.dropZone.classList.add('dragover');
                    } else if (eventName === 'dragleave' || eventName === 'drop') {
                        elements.dropZone.classList.remove('dragover');
                        
                        if (eventName === 'drop' && e.dataTransfer.files.length) {
                            elements.fileInput.files = e.dataTransfer.files;
                            handleFileSelect();
                        }
                    }
                });
            });
        }

        function handleKeyboardShortcuts(e) {
            // Ctrl/Cmd + S لحفظ البيانات
            if ((e.ctrlKey || e.metaKey) && e.key === 's') {
                e.preventDefault();
                saveApiKey();
            }
            
            // Ctrl/Cmd + E لاستخراج البيانات
            if ((e.ctrlKey || e.metaKey) && e.key === 'e') {
                e.preventDefault();
                if (!elements.btnExtract.disabled) {
                    extractAndAnalyze();
                }
            }
        }

        // إدارة API
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
                showAlert('تم مسح مفتاح API بنجاح', 'success');
                return;
            }
            
            if (!inputKey.startsWith('AIza')) {
                showAlert('يبدو أن مفتاح API غير صحيح. يجب أن يبدأ المفتاح بـ "AIza"', 'error');
                return;
            }
            
            elements.modelTesting.classList.remove('hidden');
            elements.modelTesting.innerHTML = '<span class="loading"></span> جاري اختبار الاتصال بالنماذج...';
            
            try {
                const isValid = await testApiConnection(inputKey);
                if (isValid) {
                    appState.API_KEY = inputKey;
                    localStorage.setItem('gemini_api_key', appState.API_KEY);
                    elements.apiKeyInput.value = "••••••••" + appState.API_KEY.slice(-4);
                    updateApiStatus(true);
                    
                    await loadAvailableModels();
                    showAlert('تم حفظ مفتاح API بنجاح وتم تحميل النماذج المتاحة', 'success');
                } else {
                    showAlert('مفتاح API غير صالح أو غير قادر على الاتصال بخدمات Google AI', 'error');
                }
            } catch (error) {
                showAlert('حدث خطأ أثناء اختبار الاتصال: ' + error.message, 'error');
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
                console.log("النماذج المتاحة:", data.models);
                return true;
            } catch (error) {
                console.error("خطأ في اختبار الاتصال:", error);
                return false;
            }
        }

        async function loadAvailableModels() {
            if (!appState.API_KEY) return;
            
            elements.modelTesting.classList.remove('hidden');
            elements.modelTesting.innerHTML = '<span class="loading"></span> جاري تحميل النماذج المتاحة...';
            
            try {
                const response = await fetch(`https://generativelanguage.googleapis.com/v1/models?key=${appState.API_KEY}`);
                if (!response.ok) {
                    throw new Error(`خطأ في تحميل النماذج: ${response.status}`);
                }
                
                const data = await response.json();
                appState.availableModels = data.models || [];
                
                const geminiModels = appState.availableModels.filter(model => 
                    model.name.includes('gemini') || 
                    model.name.includes('models/gemini')
                );
                
                updateModelSelect(geminiModels);
                elements.modelInfoText.textContent = `تم العثور على ${geminiModels.length} نموذج متاح`;
                
            } catch (error) {
                console.error("خطأ في تحميل النماذج:", error);
                elements.modelInfoText.textContent = 'خطأ في تحميل النماذج. تأكد من اتصال الإنترنت وصحة مفتاح API.';
            } finally {
                elements.modelTesting.classList.add('hidden');
            }
        }

        function updateModelSelect(models) {
            elements.modelSelect.innerHTML = '<option value="">-- اختر النموذج --</option>';
            
            const sortedModels = models.sort((a, b) => {
                if (a.name.includes('1.5') && !b.name.includes('1.5')) return -1;
                if (!a.name.includes('1.5') && b.name.includes('1.5')) return 1;
                if (a.name.includes('vision') && !b.name.includes('vision')) return -1;
                if (!a.name.includes('vision') && b.name.includes('vision')) return 1;
                if (a.name.includes('pro') && !b.name.includes('pro')) return -1;
                if (!a.name.includes('pro') && b.name.includes('pro')) return 1;
                if (a.name.includes('flash') && !b.name.includes('flash')) return -1;
                if (!a.name.includes('flash') && b.name.includes('flash')) return 1;
                return 0;
            });
            
            sortedModels.forEach(model => {
                const modelName = model.name.split('/').pop();
                const option = document.createElement('option');
                option.value = model.name;
                
                let displayName = modelName;
                if (modelName.includes('gemini-1.5-flash')) displayName = 'Gemini 1.5 Flash (الأسرع)';
                else if (modelName.includes('gemini-1.5-pro')) displayName = 'Gemini 1.5 Pro (الأقوى)';
                else if (modelName.includes('gemini-pro-vision')) displayName = 'Gemini Pro Vision (للصور)';
                else if (modelName.includes('gemini-pro')) displayName = 'Gemini Pro (عام)';
                else if (modelName.includes('gemini-ultra')) displayName = 'Gemini Ultra (المتقدم)';
                else if (modelName.includes('gemini')) displayName = 'Gemini (افتراضي)';
                
                option.textContent = displayName;
                elements.modelSelect.appendChild(option);
            });
            
            if (appState.selectedModel) {
                elements.modelSelect.value = appState.selectedModel;
            } else if (sortedModels.length > 0) {
                const defaultModel = sortedModels.find(m => m.name.includes('gemini-1.5-flash')) ||
                                   sortedModels.find(m => m.name.includes('gemini-pro')) ||
                                   sortedModels[0];
                if (defaultModel) {
                    elements.modelSelect.value = defaultModel.name;
                    appState.selectedModel = defaultModel.name;
                }
            }
            
            updateModelInfo();
        }

        function updateModelInfo() {
            if (!appState.selectedModel) return;
            
            const modelName = appState.selectedModel.split('/').pop();
            let info = '';
            
            if (modelName.includes('flash')) {
                info = 'النموذج الأسرع والأقل تكلفة. مناسب لاستخراج النصوص السريعة.';
            } else if (modelName.includes('1.5-pro')) {
                info = 'النموذج الأقوى والأكثر دقة. مناسب للجداول والنصوص المعقدة.';
            } else if (modelName.includes('vision')) {
                info = 'مصمم خصيصاً للصور. يستخرج النصوص والبيانات من الصور بدقة عالية.';
            } else if (modelName.includes('pro')) {
                info = 'نموذج متوازن للاستخدام العام. مناسب لمعظم المهام.';
            } else if (modelName.includes('ultra')) {
                info = 'النموذج الأكثر تطوراً. للأعمال المتقدمة والمعقدة.';
            } else {
                info = 'نموذج Gemini للذكاء الاصطناعي.';
            }
            
            elements.modelInfoText.textContent = info;
        }

        // معالجة الملفات
        function handleFileSelect() {
            if (elements.fileInput.files[0]) {
                const file = elements.fileInput.files[0];
                const fileSize = (file.size / 1024 / 1024).toFixed(2);
                const fileName = file.name.length > 30 ? file.name.substring(0, 27) + '...' : file.name;
                
                elements.fileLabel.innerHTML = `<i class="fas fa-file"></i> ${fileName}`;
                elements.fileInfo.innerHTML = `<i class="fas fa-info-circle"></i> حجم الملف: ${fileSize} MB | النوع: ${file.type}`;
                
                appState.fileType = file.type;
                suggestModelForFile(file);
            }
        }

        function suggestModelForFile(file) {
            if (!appState.availableModels.length) return;
            
            let suggestedModel = '';
            
            if (file.type.startsWith('image/')) {
                suggestedModel = appState.availableModels.find(m => 
                    m.name.includes('vision') || 
                    m.name.includes('1.5') ||
                    m.supportedGenerationMethods?.includes('generateContent')
                );
            } else if (file.type === 'application/pdf') {
                suggestedModel = appState.availableModels.find(m => 
                    m.name.includes('1.5-pro') || 
                    m.name.includes('pro') ||
                    m.name.includes('flash')
                );
            }
            
            if (suggestedModel) {
                elements.modelSelect.value = suggestedModel.name;
                appState.selectedModel = suggestedModel.name;
                updateModelInfo();
                
                showAlert(`تم اختيار النموذج ${suggestedModel.name.split('/').pop()} تلقائياً للملف`, 'info');
            }
        }

        // استخراج وتحليل البيانات
        async function extractAndAnalyze() {
            if (!appState.API_KEY) {
                showAlert('يرجى إضافة مفتاح API أولاً', 'error');
                return;
            }
            
            if (!appState.selectedModel) {
                showAlert('يرجى اختيار نموذج من القائمة', 'error');
                return;
            }
            
            if (!elements.fileInput.files[0]) {
                showAlert('الرجاء اختيار ملف أولاً', 'error');
                return;
            }
            
            const file = elements.fileInput.files[0];
            const maxSize = 10 * 1024 * 1024;
            
            if (file.size > maxSize) {
                showAlert('حجم الملف كبير جداً. الحد الأقصى هو 10MB', 'error');
                return;
            }
            
            elements.btnExtract.disabled = true;
            elements.btnExtract.innerHTML = '<span class="loading"></span> جاري الاستخراج والتحليل...';
            elements.progressBar.style.width = '20%';
            
            try {
                const genAI = new GoogleGenerativeAI(appState.API_KEY);
                const model = genAI.getGenerativeModel({ model: appState.selectedModel });
                
                elements.progressBar.style.width = '40%';
                
                const reader = new FileReader();
                
                reader.onloadend = async () => {
                    try {
                        const base64Data = reader.result.split(',')[1];
                        elements.progressBar.style.width = '60%';
                        
                        const promptText = buildExtractionPrompt(file.type);
                        
                        elements.progressBar.style.width = '80%';
                        
                        const result = await model.generateContent([
                            promptText,
                            { inlineData: { data: base64Data, mimeType: file.type } }
                        ]);
                        
                        const response = await result.response;
                        const extractedText = response.text();
                        
                        appState.extractedData = extractedText;
                        
                        displayExtractedResults(extractedText);
                        
                        elements.progressBar.style.width = '100%';
                        
                        const processedData = await processExtractedDataAutomatically(extractedText);
                        
                        if (processedData.length > 0) {
                            showAlert(`تم استخراج ${processedData.length} طالب بنجاح!`, 'success');
                            setTimeout(() => switchTab('input'), 1000);
                        } else {
                            showAlert('تم استخراج البيانات بنجاح. يرجى مراجعة وتعديل البيانات يدوياً.', 'warning');
                        }
                        
                    } catch (apiError) {
                        console.error("API Error:", apiError);
                        handleApiError(apiError);
                    } finally {
                        finishProcessing();
                    }
                };
                
                reader.onerror = () => {
                    showAlert('حدث خطأ أثناء قراءة الملف', 'error');
                    finishProcessing();
                };
                
                reader.readAsDataURL(file);
                
            } catch (error) {
                console.error("General error:", error);
                showAlert(`حدث خطأ: ${error.message}`, 'error');
                finishProcessing();
            }
        }

        function buildExtractionPrompt(fileType) {
            return `أنا أريد استخراج نتائج الطلاب من هذا الملف مع الحفاظ الكامل على التنسيق والترتيب الأصلي.

المطلوب استخراج البيانات التالية بدقة مع الحفاظ على تنسيق الجداول:
1. أسماء الطلاب الكاملة (مع الحفاظ على ترتيب الكلمات كما في الملف الأصلي)
2. الدرجات (من 40 أو النسبة المئوية)
3. المواد الدراسية
4. الفصول أو الأقسام

تعليمات مهمة جداً:
1. الحفاظ على تنسيق الجداول: يجب الحفاظ على هيكل الجداول كما هو في الملف الأصلي
2. الأسماء الطويلة: إذا كان الاسم يمتد لأكثر من سطر في الملف الأصلي، ضعه كما هو مع الحفاظ على التقسيم
3. الفواصل بين الخلايا: استخدم | لفصل الأعمدة و - للصفوف كما في الجداول النصية
4. ترتيب الأسماء: الحفاظ على ترتيب الأسماء كما في الملف الأصلي
5. النسب المئوية: إذا كانت الدرجات بنسبة مئوية، حولها إلى درجة من 40
6. القيم الافتراضية: إذا لم يتم ذكر المادة، استخدم "عام"، وإذا لم يتم ذكر الفصل، استخدم "غير محدد"

تنسيق المخرجات المطلوب:
| الرقم | اسم الطالب | المادة | الفصل | الدرجة/40 |
|-------|------------|--------|-------|-----------|

مثال مع أسماء طويلة:
| 1 | أحمد محمد عبدالله<br>علي حسن | الرياضيات | 2/أ | 35 |
| 2 | فاطمة خالد إبراهيم<br>سالم محمد | اللغة العربية | 2/ب | 28 |

ملاحظات إضافية:
- استخدم <br> للأسطر الجديدة داخل الخلايا
- الحفاظ على المسافات والفواصل كما في الملف الأصلي
- لا تدمج أسماء من خلايا مختلفة
- احترم حدود الخلايا كما تظهر في الملف الأصلي`;
        }

        // معالجة البيانات المستخرجة
        async function processExtractedDataAutomatically(extractedText) {
            try {
                console.log("معالجة البيانات المستخرجة تلقائياً...");
                
                // إعادة تعيين العداد
                appState.nameCounter = 1;
                appState.processedTables = [];
                
                const lines = extractedText.split('\n');
                const students = [];
                let inTable = false;
                let currentTable = [];
                
                for (const line of lines) {
                    const trimmedLine = line.trim();
                    
                    // اكتشاف بداية الجدول
                    if (trimmedLine.includes('|---') || trimmedLine.includes('|--') || 
                        (trimmedLine.includes('|') && trimmedLine.split('|').length > 3)) {
                        inTable = true;
                        currentTable.push(trimmedLine);
                    } 
                    // اكتشاف نهاية الجدول
                    else if (inTable && trimmedLine === '') {
                        if (currentTable.length > 0) {
                            const tableStudents = parseTableData(currentTable);
                            students.push(...tableStudents);
                            appState.processedTables.push({
                                data: [...currentTable],
                                students: tableStudents
                            });
                            currentTable = [];
                        }
                        inTable = false;
                    }
                    // إضافة سطر إلى الجدول الحالي
                    else if (inTable && trimmedLine.includes('|')) {
                        currentTable.push(trimmedLine);
                    }
                    // معالجة الأسطر خارج الجداول
                    else if (!inTable && trimmedLine) {
                        const studentData = parseStudentLine(trimmedLine);
                        if (studentData) {
                            students.push(studentData);
                        }
                    }
                }
                
                // معالجة آخر جدول إذا كان موجوداً
                if (currentTable.length > 0) {
                    const tableStudents = parseTableData(currentTable);
                    students.push(...tableStudents);
                    appState.processedTables.push({
                        data: [...currentTable],
                        students: tableStudents
                    });
                }
                
                // إذا لم نجد بيانات في الجداول، حاول البحث بأنماط مختلفة
                if (students.length === 0) {
                    const altStudents = alternativeParsing(extractedText);
                    students.push(...altStudents);
                }
                
                // إزالة التكرارات
                const uniqueStudents = [];
                const seenIds = new Set();
                
                for (const student of students) {
                    if (!seenIds.has(student.id)) {
                        seenIds.add(student.id);
                        uniqueStudents.push(student);
                    }
                }
                
                // حفظ الطلاب المستخرجين
                if (uniqueStudents.length > 0) {
                    appState.students = uniqueStudents;
                    updateStudentsTable();
                    updateAnalysis();
                    saveToLocalStorage();
                    
                    elements.extractedDataSection.classList.remove('hidden');
                    elements.rawDataPreview.textContent = extractedText;
                }
                
                return uniqueStudents;
                
            } catch (error) {
                console.error("خطأ في المعالجة التلقائية:", error);
                return [];
            }
        }

        function parseTableData(tableLines) {
            const students = [];
            
            // تجاهل سطر الرأس إذا كان موجوداً
            const startIndex = tableLines[0].includes('---') ? 1 : 0;
            
            for (let i = startIndex; i < tableLines.length; i++) {
                const line = tableLines[i];
                if (!line.includes('|')) continue;
                
                const parts = line.split('|').filter(part => part.trim() !== '');
                
                if (parts.length >= 4) {
                    try {
                        let number, name, subject, className, score;
                        
                        if (parts.length === 4) {
                            // تنسيق: رقم | اسم | فصل | درجة
                            number = cleanText(parts[0]);
                            name = cleanText(parts[1]);
                            className = cleanText(parts[2]);
                            score = parseScore(parts[3]);
                            subject = "عام";
                        } else if (parts.length >= 5) {
                            // تنسيق: رقم | اسم | مادة | فصل | درجة
                            number = cleanText(parts[0]);
                            name = cleanText(parts[1]);
                            subject = cleanText(parts[2]);
                            className = cleanText(parts[3]);
                            score = parseScore(parts[4]);
                        }
                        
                        // تحويل النسبة المئوية إذا لزم الأمر
                        if (score > 40 && score <= 100) {
                            score = (score * 40) / 100;
                        }
                        
                        if (score >= 0 && score <= 40 && name && name.length > 1) {
                            // معالجة الأسماء متعددة الأسطر
                            const processedName = processMultiLineName(name);
                            
                            // إنشاء معرف فريد للطالب
                            const studentId = `student_${appState.nameCounter++}_${Date.now()}`;
                            
                            students.push({
                                id: studentId,
                                name: processedName,
                                subject: subject || "عام",
                                className: className || "غير محدد",
                                score: parseFloat(score.toFixed(1)),
                                level: getLevel(score),
                                originalLine: line,
                                tableIndex: i
                            });
                        }
                    } catch (error) {
                        console.warn("خطأ في معالجة سطر الجدول:", error, line);
                    }
                }
            }
            
            return students;
        }

        function processMultiLineName(nameText) {
            // تنظيف النص من الرموز غير المرغوبة
            let cleanedName = nameText
                .replace(/<br\s*\/?>/gi, '\n')  // تحويل <br> إلى سطر جديد
                .replace(/\\n/g, '\n')           // تحويل \n إلى سطر جديد
                .replace(/\s+/g, ' ')           // تقليص المسافات المتعددة
                .trim();
            
            // تقسيم الاسم إلى أسطر إذا كان يحتوي على فواصل أسطر
            const lines = cleanedName.split('\n').map(line => line.trim()).filter(line => line);
            
            if (lines.length > 1) {
                // للأسماء متعددة الأسطر، احتفظ بالتنسيق مع إضافة معرف
                return {
                    display: lines.join('<br>'),
                    lines: lines,
                    isMultiLine: true,
                    firstPart: lines[0],
                    secondPart: lines.length > 1 ? lines[1] : ''
                };
            }
            
            // للأسماء العادية، تقسيم إذا كانت طويلة جداً
            const words = cleanedName.split(' ');
            if (words.length > 3) {
                // تقسيم الاسم الطويل إلى جزأين
                const midIndex = Math.ceil(words.length / 2);
                const firstPart = words.slice(0, midIndex).join(' ');
                const secondPart = words.slice(midIndex).join(' ');
                
                return {
                    display: `${firstPart}<br>${secondPart}`,
                    lines: [firstPart, secondPart],
                    isMultiLine: true,
                    firstPart: firstPart,
                    secondPart: secondPart
                };
            }
            
            return {
                display: cleanedName,
                lines: [cleanedName],
                isMultiLine: false,
                firstPart: cleanedName,
                secondPart: ''
            };
        }

        function parseStudentLine(line) {
            const cleanLine = line.trim();
            if (!cleanLine || cleanLine.length < 3) return null;
            
            const patterns = [
                /([^\|]+)\s*\|\s*([^\|]+)\s*\|\s*([^\|]+)\s*\|\s*([\d\.]+)/,
                /([^\-]+)\s*\-\s*([^\-]+)\s*\-\s*([^\-]+)\s*\-\s*([\d\.]+)/,
                /([^،]+)\s*،\s*([^،]+)\s*،\s*([^،]+)\s*،\s*([\d\.]+)/,
                /([^:]+):\s*([\d\.]+)/,
                /([\d\.]+)\s*-\s*([^-]+)/
            ];
            
            for (const pattern of patterns) {
                const match = cleanLine.match(pattern);
                if (match) {
                    let name, subject, className, score;
                    
                    if (pattern === patterns[0] || pattern === patterns[1] || pattern === patterns[2]) {
                        name = match[1].trim();
                        subject = match[2].trim();
                        className = match[3].trim();
                        score = parseScore(match[4]);
                    } else if (pattern === patterns[3]) {
                        name = match[1].trim();
                        score = parseScore(match[2]);
                        subject = "عام";
                        className = "غير محدد";
                    } else if (pattern === patterns[4]) {
                        score = parseScore(match[1]);
                        name = match[2].trim();
                        subject = "عام";
                        className = "غير محدد";
                    }
                    
                    if (score > 40 && score <= 100) {
                        score = (score * 40) / 100;
                    }
                    
                    if (score >= 0 && score <= 40 && name && name.length > 1) {
                        const processedName = processMultiLineName(name);
                        const studentId = `student_${appState.nameCounter++}_${Date.now()}`;
                        
                        return {
                            id: studentId,
                            name: processedName,
                            subject: subject || "عام",
                            className: className || "غير محدد",
                            score: parseFloat(score.toFixed(1)),
                            level: getLevel(score),
                            originalLine: cleanLine
                        };
                    }
                }
            }
            
            return null;
        }

        function alternativeParsing(text) {
            const students = [];
            const lines = text.split('\n');
            
            for (const line of lines) {
                const cleanLine = line.trim();
                if (cleanLine.length < 2) continue;
                
                const numberMatches = cleanLine.match(/(\d+\.?\d*)/g);
                if (numberMatches) {
                    for (const numberStr of numberMatches) {
                        let score = parseScore(numberStr);
                        
                        if (score > 40 && score <= 100) {
                            score = (score * 40) / 100;
                        }
                        
                        if (score >= 0 && score <= 40) {
                            let name = cleanLine.replace(/(\d+\.?\d*)/g, '')
                                              .replace(/[^\u0600-\u06FF\u0750-\u077F\s]/g, '')
                                              .trim();
                            
                            if (name.length > 1) {
                                const processedName = processMultiLineName(name);
                                const studentId = `student_${appState.nameCounter++}_${Date.now()}`;
                                
                                students.push({
                                    id: studentId,
                                    name: processedName,
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

        function cleanText(text) {
            return text
                .replace(/[^\u0600-\u06FF\u0750-\u077F\u08A0-\u08FFa-zA-Z0-9\s\/\-\.،]/g, '')
                .replace(/\s+/g, ' ')
                .trim();
        }

        function parseScore(scoreText) {
            let text = scoreText.toString().trim();
            
            // تحويل الأرقام العربية إلى إنجليزية
            const arabicToEnglish = {
                '٠': '0', '١': '1', '٢': '2', '٣': '3', '٤': '4',
                '٥': '5', '٦': '6', '٧': '7', '٨': '8', '٩': '9',
                '٫': '.', ',': '.'
            };
            
            for (const [arabic, english] of Object.entries(arabicToEnglish)) {
                text = text.replace(new RegExp(arabic, 'g'), english);
            }
            
            // استخراج الأرقام فقط
            const numberMatch = text.match(/(\d+\.?\d*)/);
            return numberMatch ? parseFloat(numberMatch[1]) : NaN;
        }

        function getLevel(score) {
            if (score >= 36) return {name: 'ممتاز', class: 'excellent'};
            if (score >= 32) return {name: 'جيد جدًا', class: 'verygood'};
            if (score >= 28) return {name: 'جيد', class: 'good'};
            if (score >= 20) return {name: 'مقبول', class: 'pass'};
            return {name: 'ضعيف', class: 'weak'};
        }

        // عرض النتائج
        function displayExtractedResults(text) {
            let html = `
                <div style="background: #f8f9fa; padding: 15px; border-radius: 8px; margin-bottom: 20px; border-right: 4px solid #3498db;">
                    <h3 style="color: #2c3e50; margin-top: 0;">
                        <i class="fas fa-check-circle" style="color: #27ae60;"></i>
                        تم استخراج البيانات بنجاح
                    </h3>
                    <p style="color: #7f8c8d; margin: 0;">
                        ${text.length} حرف مستخرج. جاري معالجة البيانات تلقائياً...
                    </p>
                </div>
            `;
            
            // عرض الجداول المعالجة
            if (appState.processedTables.length > 0) {
                html += `<div style="margin-bottom: 20px;">
                    <h4 style="color: #1a5c9e; margin-bottom: 10px;"><i class="fas fa-table"></i> الجداول المستخرجة</h4>`;
                
                appState.processedTables.forEach((table, tableIndex) => {
                    html += `<div style="background: white; border: 1px solid #ddd; border-radius: 8px; padding: 15px; margin-bottom: 15px;">
                        <h5 style="color: #666; margin-bottom: 10px;">الجدول ${tableIndex + 1}</h5>
                        <pre style="white-space: pre-wrap; font-family: 'Courier New', monospace; background: #f8f9fa; padding: 10px; border-radius: 5px; font-size: 0.9rem; max-height: 200px; overflow-y: auto;">${table.data.join('\n')}</pre>
                        <div style="margin-top: 10px; font-size: 0.9rem; color: #666;">
                            <i class="fas fa-users"></i> تم استخراج ${table.students.length} طالب من هذا الجدول
                        </div>
                    </div>`;
                });
                
                html += `</div>`;
            }
            
            // عرض النص الكامل
            html += `
                <div style="background: white; padding: 20px; border-radius: 8px; border: 1px solid #e9ecef;">
                    <h4 style="color: #1a5c9e; margin-bottom: 10px;"><i class="fas fa-file-alt"></i> النص الكامل المستخرج</h4>
                    <pre style="white-space: pre-wrap; font-family: 'Courier New', monospace; line-height: 1.5; direction: ltr; text-align: left; font-size: 0.9rem; max-height: 300px; overflow-y: auto; padding: 15px; background: #f8f9fa; border-radius: 5px;">${text}</pre>
                </div>
            `;
            
            elements.result.innerHTML = html;
        }

        function handleApiError(apiError) {
            let errorMessage = 'حدث خطأ أثناء معالجة الملف';
            
            if (apiError.message.includes('404') || apiError.message.includes('not found')) {
                errorMessage = 'النموذج المحدد غير متوفر. جرب اختيار نموذج آخر من القائمة.';
                
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
            } else {
                errorMessage += ': ' + apiError.message;
            }
            
            showAlert(errorMessage, 'error');
            
            elements.result.innerHTML = `
                <div style="text-align: center; color: #e74c3c; padding: 40px;">
                    <i class="fas fa-exclamation-triangle" style="font-size: 48px; margin-bottom: 15px;"></i>
                    <h3 style="color: #c0392b;">خطأ في المعالجة</h3>
                    <p style="margin-top: 10px;">${errorMessage}</p>
                    <div style="margin-top: 20px;">
                        <button onclick="retryWithDifferentModel()" style="background: #3498db; color: white; border: none; padding: 10px 20px; border-radius: 6px; cursor: pointer; margin: 5px; font-size: 0.9rem;">
                            <i class="fas fa-sync-alt"></i> المحاولة بنموذج مختلف
                        </button>
                        <button onclick="loadAvailableModels()" style="background: #6c757d; color: white; border: none; padding: 10px 20px; border-radius: 6px; cursor: pointer; margin: 5px; font-size: 0.9rem;">
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
                
                showAlert(`تم التغيير إلى النموذج: ${newModel.name.split('/').pop()}`, 'info');
                
                setTimeout(() => {
                    if (elements.fileInput.files[0]) {
                        extractAndAnalyze();
                    }
                }, 1000);
            }
        }

        function finishProcessing() {
            elements.btnExtract.disabled = false;
            elements.btnExtract.innerHTML = '<i class="fas fa-magic"></i> استخراج وتحليل تلقائي';
            
            setTimeout(() => {
                elements.progressBar.style.width = '0%';
            }, 1000);
        }

        // إدارة البيانات
        async function processExtractedData() {
            if (!appState.extractedData) {
                showAlert('لا توجد بيانات مستخرجة. يرجى استخراج النصوص أولاً.', 'error');
                return;
            }
            
            const button = event?.target?.closest('button');
            if (button) {
                button.innerHTML = '<span class="loading"></span> جاري المعالجة...';
                button.disabled = true;
            }
            
            try {
                const students = await processExtractedDataAutomatically(appState.extractedData);
                
                if (students.length > 0) {
                    showAlert(`تمت معالجة ${students.length} طالب بنجاح`, 'success');
                    
                    elements.extractedDataSection.classList.remove('hidden');
                    elements.rawDataPreview.textContent = appState.extractedData;
                    
                    setTimeout(() => switchTab('analysis'), 500);
                } else {
                    showAlert('لم يتم العثور على بيانات طلاب في النص المستخرج. يرجى مراجعة تنسيق البيانات.', 'warning');
                    
                    elements.result.innerHTML = `
                        <div style="background: #f8f9fa; padding: 15px; border-radius: 8px; margin-bottom: 20px; border-right: 4px solid #ff9800;">
                            <h3 style="color: #2c3e50; margin-top: 0;">
                                <i class="fas fa-exclamation-triangle" style="color: #ff9800;"></i>
                                يحتاج إلى تدخل يدوي
                            </h3>
                            <p style="color: #7f8c8d; margin: 0;">
                                لم يتم التعرف على البيانات تلقائياً. يرجى تحرير النص يدوياً.
                            </p>
                        </div>
                        <textarea id="manualEdit" style="width: 100%; height: 300px; padding: 15px; border: 1px solid #ddd; border-radius: 8px; font-family: 'Courier New', monospace; direction: ltr; font-size: 0.95rem; resize: vertical;">${appState.extractedData}</textarea>
                        <div style="margin-top: 20px; text-align: center;">
                            <button onclick="processManualEdit()" style="background: #3498db; color: white; border: none; padding: 12px 24px; border-radius: 6px; cursor: pointer; margin: 5px; font-size: 0.95rem;">
                                <i class="fas fa-cogs"></i> معالجة النص المحرر
                            </button>
                        </div>
                    `;
                }
                
            } catch (error) {
                console.error("خطأ في معالجة البيانات:", error);
                showAlert('حدث خطأ أثناء معالجة البيانات: ' + error.message, 'error');
            } finally {
                if (button) {
                    button.innerHTML = '<i class="fas fa-robot"></i> معالجة البيانات المستخرجة';
                    button.disabled = false;
                }
            }
        }

        function processManualEdit() {
            const editedText = document.getElementById('manualEdit').value;
            appState.extractedData = editedText;
            processExtractedData();
        }

        function updateStudentsTable() {
            const tbody = elements.studentsTableBody;
            
            if (appState.students.length === 0) {
                tbody.innerHTML = `
                    <tr>
                        <td colspan="7" style="text-align:center; padding:30px; color:#666;">
                            <i class="fas fa-users-slash" style="font-size:2rem; display:block; margin-bottom:10px;"></i>
                            لا توجد بيانات، يرجى استخراج البيانات أولاً
                        </td>
                    </tr>
                `;
                return;
            }
            
            let html = '';
            
            appState.students.forEach((student, index) => {
                const nameDisplay = typeof student.name === 'object' ? student.name.display : student.name;
                
                html += `
                    <tr data-student-id="${student.id}">
                        <td>${index + 1}</td>
                        <td class="name-cell">
                            <div class="name-content" style="${typeof student.name === 'object' && student.name.isMultiLine ? 'text-align: center; line-height: 1.4;' : ''}">
                                ${nameDisplay}
                            </div>
                        </td>
                        <td>${student.subject}</td>
                        <td>${student.className}</td>
                        <td><strong>${student.score}</strong></td>
                        <td><span class="level-badge level-${student.level.class}">${student.level.name}</span></td>
                        <td>
                            <button class="delete-btn" onclick="deleteStudent('${student.id}')" title="حذف الطالب">
                                <i class="fas fa-trash"></i> حذف
                            </button>
                        </td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
        }

        function deleteStudent(id) {
            if (confirm('هل أنت متأكد من حذف هذا الطالب؟')) {
                appState.students = appState.students.filter(student => student.id !== id);
                updateStudentsTable();
                updateAnalysis();
                showAlert('تم حذف الطالب بنجاح', 'success');
                saveToLocalStorage();
            }
        }

        function clearAllData() {
            if (confirm('هل أنت متأكد من مسح جميع البيانات؟ هذا الإجراء لا يمكن التراجع عنه.')) {
                appState.students = [];
                appState.extractedData = '';
                appState.processedTables = [];
                appState.nameCounter = 1;
                
                updateStudentsTable();
                updateAnalysis();
                elements.extractedDataSection.classList.add('hidden');
                elements.rawDataPreview.textContent = '';
                
                showAlert('تم مسح جميع البيانات بنجاح', 'success');
                saveToLocalStorage();
            }
        }

        function refreshDataView() {
            updateStudentsTable();
            if (appState.extractedData) {
                elements.extractedDataSection.classList.remove('hidden');
                elements.rawDataPreview.textContent = appState.extractedData;
            }
            showAlert('تم تحديث العرض', 'info');
        }

        function exportCurrentData() {
            if (appState.students.length === 0) {
                showAlert('لا توجد بيانات للتصدير', 'error');
                return;
            }
            
            const dataStr = JSON.stringify({
                students: appState.students,
                extractedData: appState.extractedData,
                timestamp: new Date().toISOString()
            }, null, 2);
            
            const dataBlob = new Blob([dataStr], { type: 'application/json' });
            const url = URL.createObjectURL(dataBlob);
            
            const link = document.createElement('a');
            link.href = url;
            link.download = `students_data_${new Date().toISOString().slice(0, 10)}.json`;
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            URL.revokeObjectURL(url);
            
            showAlert('تم تصدير البيانات إلى ملف JSON', 'success');
        }

        // التحليل الإحصائي
        function updateAnalysis() {
            if (appState.students.length === 0) {
                elements.analysisAlert.classList.remove('hidden');
                elements.summaryCards.innerHTML = `
                    <div style="text-align: center; padding: 40px; color: #666; background: #f8f9fa; border-radius: 10px;">
                        <i class="fas fa-chart-bar"></i>
                        <h3>لا توجد بيانات لعرض التحليل</h3>
                        <p>يرجى استخراج البيانات أولاً</p>
                    </div>
                `;
                elements.levelDetailsTable.innerHTML = '';
                return;
            }
            
            elements.analysisAlert.classList.add('hidden');
            
            const totalStudents = appState.students.length;
            const totalScore = appState.students.reduce((sum, student) => sum + student.score, 0);
            const avgScore = totalScore / totalStudents;
            const passedStudents = appState.students.filter(student => student.score >= 20).length;
            const passRate = totalStudents > 0 ? (passedStudents / totalStudents * 100).toFixed(1) : 0;
            
            const levelCounts = {
                'ممتاز': 0, 'جيد جدًا': 0, 'جيد': 0, 'مقبول': 0, 'ضعيف': 0
            };
            
            appState.students.forEach(student => {
                levelCounts[student.level.name]++;
            });
            
            const subjectCounts = {};
            appState.students.forEach(student => {
                if (!subjectCounts[student.subject]) {
                    subjectCounts[student.subject] = {count: 0, totalScore: 0};
                }
                subjectCounts[student.subject].count++;
                subjectCounts[student.subject].totalScore += student.score;
            });
            
            updateSummaryCards(totalStudents, avgScore, passRate, levelCounts);
            updateCharts(levelCounts, subjectCounts);
            updateLevelDetailsTable(levelCounts);
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
                    <div class="subtext">${passedStudents} طالب</div>
                </div>
                <div class="summary-card">
                    <h3><i class="fas fa-trophy"></i> أعلى مستوى</h3>
                    <div class="value">${highestLevel}</div>
                    <div class="subtext">${highestCount} طالب</div>
                </div>
            `;
        }

        function updateCharts(levelCounts, subjectCounts) {
            // تدمير الرسوم البيانية القديمة
            ['levelChart', 'subjectChart'].forEach(chartId => {
                const chart = Chart.getChart(chartId);
                if (chart) chart.destroy();
            });
            
            // رسم بياني للمستويات
            try {
                const levelCtx = document.getElementById('levelChart').getContext('2d');
                new Chart(levelCtx, {
                    type: 'doughnut',
                    data: {
                        labels: Object.keys(levelCounts),
                        datasets: [{
                            data: Object.values(levelCounts),
                            backgroundColor: ['#4caf50', '#009688', '#2196f3', '#ff9800', '#f44336'],
                            borderWidth: 1,
                            borderColor: '#fff'
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
                console.error("خطأ في رسم بياني المستويات:", error);
            }
            
            // رسم بياني للمواد
            try {
                const subjectLabels = Object.keys(subjectCounts);
                const subjectAverages = subjectLabels.map(subject => 
                    (subjectCounts[subject].totalScore / subjectCounts[subject].count).toFixed(1)
                );
                
                const subjectCtx = document.getElementById('subjectChart').getContext('2d');
                new Chart(subjectCtx, {
                    type: 'bar',
                    data: {
                        labels: subjectLabels,
                        datasets: [{
                            label: 'متوسط الدرجة',
                            data: subjectAverages,
                            backgroundColor: '#1a5c9e',
                            borderWidth: 1,
                            borderColor: '#0d47a1'
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
                                },
                                grid: {
                                    color: 'rgba(0,0,0,0.1)'
                                }
                            },
                            x: {
                                grid: {
                                    display: false
                                }
                            }
                        },
                        plugins: {
                            legend: {
                                display: false
                            }
                        }
                    }
                });
            } catch (error) {
                console.error("خطأ في رسم بياني المواد:", error);
            }
        }

        function updateLevelDetailsTable(levelCounts) {
            const levelRanges = {
                'ممتاز': '36 - 40',
                'جيد جدًا': '32 - 35.99',
                'جيد': '28 - 31.99',
                'مقبول': '20 - 27.99',
                'ضعيف': '0 - 19.99'
            };
            
            let tableHTML = `
                <div style="display: grid; grid-template-columns: 1fr 1fr 1fr; border-bottom: 2px solid #1a5c9e; font-weight: bold; background: #1a5c9e; color: white; border-radius: 8px 8px 0 0;">
                    <div style="padding: 15px; text-align: center;">عدد الطلاب</div>
                    <div style="padding: 15px; text-align: center;">نطاق الدرجات</div>
                    <div style="padding: 15px; text-align: center;">المستوى</div>
                </div>
            `;
            
            const levels = ['ممتاز', 'جيد جدًا', 'جيد', 'مقبول', 'ضعيف'];
            
            levels.forEach((level, index) => {
                const count = levelCounts[level] || 0;
                const percentage = appState.students.length > 0 ? ((count / appState.students.length) * 100).toFixed(1) : '0';
                const isLast = index === levels.length - 1;
                
                tableHTML += `
                    <div style="display: grid; grid-template-columns: 1fr 1fr 1fr; border-bottom: ${isLast ? 'none' : '1px solid #eee'}; background: ${index % 2 === 0 ? '#f8f9fa' : 'white'}; border-radius: ${isLast ? '0 0 8px 8px' : '0'};">
                        <div style="padding: 12px; text-align: center; display: flex; flex-direction: column; justify-content: center;">
                            <strong style="font-size: 1.2rem;">${count}</strong>
                            <small style="color: #666; font-size: 0.85rem;">(${percentage}%)</small>
                        </div>
                        <div style="padding: 12px; text-align: center; display: flex; align-items: center; justify-content: center;">
                            ${levelRanges[level]}
                        </div>
                        <div style="padding: 12px; text-align: center; display: flex; align-items: center; justify-content: center;">
                            <span class="level-badge level-${level}">${level}</span>
                        </div>
                    </div>
                `;
            });
            
            elements.levelDetailsTable.innerHTML = tableHTML;
        }

        // التقرير النهائي
        async function generatePDF() {
            if (appState.students.length === 0) {
                showAlert('لا توجد بيانات لإنشاء تقرير', 'error');
                return;
            }
            
            const button = event.target.closest('button');
            const originalText = button.innerHTML;
            button.innerHTML = '<span class="loading"></span> جاري إنشاء PDF...';
            button.disabled = true;
            
            updateReportContent();
            
            setTimeout(() => {
                html2canvas(document.getElementById('reportContent'), {
                    scale: 2,
                    backgroundColor: "#fff",
                    useCORS: true,
                    logging: false
                }).then(canvas => {
                    const imgData = canvas.toDataURL("image/jpeg", 0.9);
                    const { jsPDF } = window.jspdf;
                    const pdf = new jsPDF("p", "mm", "a4");
                    
                    pdf.addImage(imgData, "JPEG", 0, 0, 210, 297);
                    pdf.save("تقرير_النتائج_المستخرجة.pdf");
                    
                    showAlert('تم حفظ التقرير بنجاح', 'success');
                }).catch(error => {
                    console.error('خطأ في إنشاء PDF:', error);
                    showAlert('حدث خطأ أثناء إنشاء التقرير', 'error');
                }).finally(() => {
                    button.innerHTML = originalText;
                    button.disabled = false;
                });
            }, 1000);
        }

        function updateReportContent() {
            if (appState.students.length === 0) {
                elements.reportAlert.classList.remove('hidden');
                elements.reportContent.innerHTML = '';
                return;
            }
            
            elements.reportAlert.classList.add('hidden');
            
            const totalStudents = appState.students.length;
            const totalScore = appState.students.reduce((sum, student) => sum + student.score, 0);
            const avgScore = totalScore / totalStudents;
            const passedStudents = appState.students.filter(student => student.score >= 20).length;
            const passRate = (passedStudents / totalStudents * 100).toFixed(1);
            
            const now = new Date();
            const dateStr = now.toLocaleDateString('ar-SA', {
                weekday: 'long',
                year: 'numeric',
                month: 'long',
                day: 'numeric'
            });
            const timeStr = now.toLocaleTimeString('ar-SA');
            
            let reportHTML = `
                <div style="text-align: center; margin-bottom: 30px;">
                    <h1 style="color: #1a5c9e; margin-bottom: 10px; font-size: 1.8rem;">📊 تقرير نتائج الطلاب المستخرجة</h1>
                    <p style="color: #666; margin-bottom: 5px; font-size: 0.95rem;">التقرير تم إنشاؤه تلقائياً من البيانات المستخرجة</p>
                    <p style="color: #888; font-size: 0.85rem;">${dateStr} - ${timeStr}</p>
                </div>
                
                <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-bottom: 30px;">
                    <div style="background: linear-gradient(135deg, #e3f2fd 0%, #bbdefb 100%); padding: 20px; border-radius: 10px; text-align: center; border: 2px solid #1a5c9e;">
                        <h3 style="color: #1a5c9e; margin-bottom: 10px; font-size: 1rem;"><i class="fas fa-users"></i> إجمالي الطلاب</h3>
                        <div style="font-size: 2.2rem; font-weight: bold; color: #0d47a1;">${totalStudents}</div>
                    </div>
                    <div style="background: linear-gradient(135deg, #e8f5e9 0%, #c8e6c9 100%); padding: 20px; border-radius: 10px; text-align: center; border: 2px solid #4caf50;">
                        <h3 style="color: #2e7d32; margin-bottom: 10px; font-size: 1rem;"><i class="fas fa-chart-line"></i> متوسط الدرجات</h3>
                        <div style="font-size: 2.2rem; font-weight: bold; color: #1b5e20;">${avgScore.toFixed(1)}</div>
                    </div>
                    <div style="background: linear-gradient(135deg, #fff3e0 0%, #ffe0b2 100%); padding: 20px; border-radius: 10px; text-align: center; border: 2px solid #ff9800;">
                        <h3 style="color: #ef6c00; margin-bottom: 10px; font-size: 1rem;"><i class="fas fa-percentage"></i> نسبة النجاح</h3>
                        <div style="font-size: 2.2rem; font-weight: bold; color: #e65100;">${passRate}%</div>
                    </div>
                </div>
            `;
            
            // إضافة جداول البيانات المستخرجة إذا كانت موجودة
            if (appState.processedTables.length > 0) {
                reportHTML += `
                    <div style="margin-bottom: 30px;">
                        <h2 style="color: #1a5c9e; margin-bottom: 15px; border-bottom: 2px solid #1a5c9e; padding-bottom: 8px; font-size: 1.3rem;">
                            <i class="fas fa-table"></i> الجداول المستخرجة
                        </h2>
                `;
                
                appState.processedTables.forEach((table, tableIndex) => {
                    reportHTML += `
                        <div style="background: #f8f9fa; padding: 15px; border-radius: 8px; margin-bottom: 15px; border: 1px solid #dee2e6;">
                            <h3 style="color: #666; margin-bottom: 10px; font-size: 1rem;">الجدول ${tableIndex + 1}</h3>
                            <pre style="white-space: pre-wrap; font-family: 'Courier New', monospace; background: white; padding: 10px; border-radius: 5px; font-size: 0.85rem; max-height: 150px; overflow-y: auto; direction: ltr;">${table.data.join('\n')}</pre>
                        </div>
                    `;
                });
                
                reportHTML += `</div>`;
            }
            
            // إضافة جدول الطلاب
            reportHTML += `
                <div style="margin-bottom: 30px;">
                    <h2 style="color: #1a5c9e; margin-bottom: 15px; border-bottom: 2px solid #1a5c9e; padding-bottom: 8px; font-size: 1.3rem;">
                        <i class="fas fa-list-ol"></i> تفاصيل نتائج الطلاب
                    </h2>
                    <div style="overflow-x: auto;">
                        <table class="report-table">
                            <thead>
                                <tr>
                                    <th>#</th>
                                    <th>اسم الطالب</th>
                                    <th>المادة</th>
                                    <th>الفصل</th>
                                    <th>الدرجة</th>
                                    <th>المستوى</th>
                                </tr>
                            </thead>
                            <tbody>
            `;
            
            appState.students.forEach((student, index) => {
                const nameDisplay = typeof student.name === 'object' ? student.name.display : student.name;
                reportHTML += `
                    <tr>
                        <td style="font-weight: bold;">${index + 1}</td>
                        <td style="text-align: right; direction: rtl;">${nameDisplay}</td>
                        <td>${student.subject}</td>
                        <td>${student.className}</td>
                        <td style="font-weight: bold; color: #1a5c9e;">${student.score}</td>
                        <td><span class="level-badge level-${student.level.class}" style="font-size: 0.8rem;">${student.level.name}</span></td>
                    </tr>
                `;
            });
            
            reportHTML += `
                            </tbody>
                        </table>
                    </div>
                </div>
                
                <div style="background: linear-gradient(135deg, #f5f5f5 0%, #eeeeee 100%); padding: 20px; border-radius: 10px; text-align: center; margin-top: 30px; border: 1px solid #ddd;">
                    <p style="color: #666; margin-bottom: 10px; font-size: 0.9rem;">
                        <i class="fas fa-robot"></i> تم إنشاء هذا التقرير تلقائياً بواسطة نظام استخراج وتحليل نتائج الطلاب المتكامل
                    </p>
                    <p style="color: #888; font-size: 0.8rem;">
                        <i class="fas fa-clock"></i> ${now.toLocaleString('ar-SA')}
                    </p>
                </div>
            `;
            
            elements.reportContent.innerHTML = reportHTML;
        }

        function printReport() {
            if (appState.students.length === 0) {
                showAlert('لا توجد بيانات للطباعة', 'error');
                return;
            }
            
            updateReportContent();
            setTimeout(() => {
                const printContent = document.getElementById('reportContent').innerHTML;
                const originalContent = document.body.innerHTML;
                
                document.body.innerHTML = printContent;
                window.print();
                document.body.innerHTML = originalContent;
                initApp();
            }, 500);
        }

        function exportToExcel() {
            if (appState.students.length === 0) {
                showAlert('لا توجد بيانات للتصدير', 'error');
                return;
            }
            
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            csvContent += "م,اسم الطالب,المادة,الفصل,الدرجة,المستوى\r\n";
            
            appState.students.forEach((student, index) => {
                const nameDisplay = typeof student.name === 'object' ? 
                    (student.name.firstPart + ' ' + (student.name.secondPart || '')) : 
                    student.name;
                csvContent += `${index + 1},"${nameDisplay}","${student.subject}","${student.className}",${student.score},"${student.level.name}"\r\n`;
            });
            
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `نتائج_الطلاب_${new Date().toISOString().slice(0, 10)}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showAlert('تم تصدير البيانات إلى ملف Excel', 'success');
        }

        function shareReport() {
            if (appState.students.length === 0) {
                showAlert('لا توجد بيانات للمشاركة', 'error');
                return;
            }
            
            const totalStudents = appState.students.length;
            const totalScore = appState.students.reduce((sum, student) => sum + student.score, 0);
            const avgScore = totalScore / totalStudents;
            const passedStudents = appState.students.filter(student => student.score >= 20).length;
            const passRate = (passedStudents / totalStudents * 100).toFixed(1);
            
            const shareText = `📊 تقرير نتائج الطلاب
إجمالي الطلاب: ${totalStudents}
متوسط الدرجات: ${avgScore.toFixed(1)} من 40
نسبة النجاح: ${passRate}%
تم إنشاؤه بواسطة نظام استخراج وتحليل نتائج الطلاب`;
            
            if (navigator.share) {
                navigator.share({
                    title: 'تقرير نتائج الطلاب',
                    text: shareText,
                    url: window.location.href
                }).catch(console.error);
            } else {
                // نسخ إلى الحافظة
                navigator.clipboard.writeText(shareText).then(() => {
                    showAlert('تم نسخ التقرير إلى الحافظة', 'success');
                }).catch(() => {
                    // طريقة بديلة للنسخ
                    const textArea = document.createElement('textarea');
                    textArea.value = shareText;
                    document.body.appendChild(textArea);
                    textArea.select();
                    document.execCommand('copy');
                    document.body.removeChild(textArea);
                    showAlert('تم نسخ التقرير إلى الحافظة', 'success');
                });
            }
        }

        // وظائف المساعدة
        function switchTab(tabName) {
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            const tabElement = document.getElementById(tabName + '-tab');
            if (tabElement) {
                tabElement.classList.add('active');
            }
            
            document.querySelectorAll('.tab').forEach(tab => {
                if (tab.textContent.includes(getTabText(tabName))) {
                    tab.classList.add('active');
                }
            });
            
            if (tabName === 'analysis') {
                setTimeout(updateAnalysis, 100);
            }
            
            if (tabName === 'report') {
                setTimeout(updateReportContent, 100);
            }
            
            // إعادة تعيين التمرير للقمة
            window.scrollTo(0, 0);
        }

        function getTabText(tabName) {
            switch(tabName) {
                case 'extract': return 'استخراج النصوص';
                case 'input': return 'إدارة البيانات';
                case 'analysis': return 'تحليل النتائج';
                case 'report': return 'التقرير النهائي';
                default: return '';
            }
        }

        function showAlert(message, type = 'info') {
            const icon = type === 'success' ? 'fas fa-check-circle' : 
                         type === 'error' ? 'fas fa-times-circle' : 
                         type === 'warning' ? 'fas fa-exclamation-triangle' : 
                         'fas fa-info-circle';
            
            const alertClass = type === 'success' ? 'success' : 
                              type === 'error' ? 'error' : 
                              type === 'warning' ? 'warning' : 'info';
            
            const alertDiv = document.createElement('div');
            alertDiv.className = `alert ${alertClass}`;
            alertDiv.innerHTML = `
                <i class="${icon}"></i>
                <span>${message}</span>
            `;
            
            document.querySelector('.container').insertBefore(alertDiv, document.querySelector('.tabs'));
            
            setTimeout(() => {
                if (alertDiv.parentNode) {
                    alertDiv.remove();
                }
            }, 5000);
        }

        function saveToLocalStorage() {
            try {
                const data = {
                    students: appState.students,
                    extractedData: appState.extractedData,
                    selectedModel: appState.selectedModel,
                    lastUpdated: new Date().toISOString()
                };
                localStorage.setItem('studentResultsData', JSON.stringify(data));
            } catch (error) {
                console.error('خطأ في حفظ البيانات:', error);
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
                    
                    if (appState.selectedModel) {
                        elements.modelSelect.value = appState.selectedModel;
                    }
                    
                    if (appState.extractedData) {
                        elements.extractedDataSection.classList.remove('hidden');
                        elements.rawDataPreview.textContent = appState.extractedData;
                    }
                }
            } catch (error) {
                console.error('خطأ في تحميل البيانات:', error);
            }
        }

        // تهيئة التطبيق
        initApp();

        // جعل الدوال متاحة عالمياً
        window.switchTab = switchTab;
        window.processExtractedData = processExtractedData;
        window.processManualEdit = processManualEdit;
        window.deleteStudent = deleteStudent;
        window.clearAllData = clearAllData;
        window.refreshDataView = refreshDataView;
        window.exportCurrentData = exportCurrentData;
        window.retryWithDifferentModel = retryWithDifferentModel;
        window.loadAvailableModels = loadAvailableModels;
        window.generatePDF = generatePDF;
        window.printReport = printReport;
        window.exportToExcel = exportToExcel;
        window.shareReport = shareReport;
    </script>
</body>
</html>