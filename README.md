<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="mobile-web-app-capable" content="yes">
    <title>📊 نظام استخراج وتحليل نتائج الطلاب</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* إعادة الضبط للهواتف */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            -webkit-text-size-adjust: 100%;
            -moz-text-size-adjust: 100%;
            -ms-text-size-adjust: 100%;
        }

        html {
            font-size: 14px;
            height: 100%;
        }

        @media (max-width: 320px) {
            html { font-size: 12px; }
        }
        @media (min-width: 321px) and (max-width: 480px) {
            html { font-size: 13px; }
        }
        @media (min-width: 481px) and (max-width: 768px) {
            html { font-size: 14px; }
        }
        @media (min-width: 769px) and (max-width: 1024px) {
            html { font-size: 15px; }
        }
        @media (min-width: 1025px) {
            html { font-size: 16px; }
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #f0f2f5 0%, #f8f9fa 100%);
            color: #333;
            line-height: 1.5;
            min-height: 100vh;
            padding: 10px;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        /* تصميم متجاوب للجميع */
        .container {
            width: 100%;
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 10px;
        }

        /* رأس النظام */
        .header {
            text-align: center;
            margin-bottom: 20px;
            padding: 15px;
            background: linear-gradient(135deg, #1a5c9e 0%, #0d47a1 100%);
            color: white;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }

        .header h1 {
            font-size: 1.8rem;
            margin-bottom: 5px;
            font-weight: 700;
        }

        .header p {
            font-size: 0.95rem;
            opacity: 0.9;
        }

        /* التبويبات */
        .tabs {
            display: flex;
            overflow-x: auto;
            margin-bottom: 15px;
            background: white;
            border-radius: 12px;
            padding: 5px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            -webkit-overflow-scrolling: touch;
            scrollbar-width: none;
        }

        .tabs::-webkit-scrollbar {
            display: none;
        }

        .tab {
            flex: 1;
            min-width: 120px;
            padding: 14px 12px;
            text-align: center;
            font-weight: 600;
            font-size: 0.9rem;
            color: #555;
            background: #f8f9fa;
            border: none;
            border-radius: 8px;
            margin: 0 3px;
            cursor: pointer;
            transition: all 0.3s;
            white-space: nowrap;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 6px;
        }

        .tab i {
            font-size: 1.2rem;
        }

        .tab.active {
            background: linear-gradient(135deg, #25d366 0%, #1da851 100%);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(37, 211, 102, 0.3);
        }

        .tab-content {
            display: none;
            background: white;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.08);
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .tab-content.active {
            display: block;
        }

        /* بطاقة الإعدادات */
        .config-card {
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 20px;
            border: 1px solid #dee2e6;
        }

        .section-title {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 15px;
            color: #1a5c9e;
            font-size: 1.1rem;
            font-weight: 600;
        }

        .section-title i {
            color: #25d366;
        }

        /* إدخال API */
        .api-input-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 15px;
        }

        @media (min-width: 768px) {
            .api-input-group {
                flex-direction: row;
            }
        }

        .api-input {
            flex: 1;
            padding: 14px 16px;
            border: 2px solid #ddd;
            border-radius: 10px;
            font-size: 1rem;
            font-family: 'Courier New', monospace;
            background: white;
            transition: all 0.3s;
        }

        .api-input:focus {
            border-color: #25d366;
            outline: none;
            box-shadow: 0 0 0 3px rgba(37, 211, 102, 0.1);
        }

        .btn {
            padding: 14px 24px;
            border: none;
            border-radius: 10px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            min-height: 50px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #25d366 0%, #1da851 100%);
            color: white;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(37, 211, 102, 0.3);
        }

        .btn-secondary {
            background: linear-gradient(135deg, #6c757d 0%, #5a6268 100%);
            color: white;
        }

        .btn-danger {
            background: linear-gradient(135deg, #dc3545 0%, #c82333 100%);
            color: white;
        }

        .btn-lg {
            padding: 18px 24px;
            font-size: 1.1rem;
            width: 100%;
        }

        /* منطقة رفع الملفات */
        .upload-area {
            border: 3px dashed #3498db;
            border-radius: 12px;
            padding: 40px 20px;
            text-align: center;
            background: linear-gradient(135deg, #f8fbff 0%, #e6f2ff 100%);
            margin-bottom: 20px;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
        }

        .upload-area:hover {
            border-color: #25d366;
            background: linear-gradient(135deg, #e6f2ff 0%, #d4e6ff 100%);
        }

        .upload-area.dragover {
            border-color: #25d366;
            background: linear-gradient(135deg, #d4edda 0%, #c3e6cb 100%);
        }

        .upload-icon {
            font-size: 3.5rem;
            color: #3498db;
            margin-bottom: 15px;
        }

        .upload-text {
            font-size: 1.1rem;
            color: #2c3e50;
            margin-bottom: 8px;
            font-weight: 500;
        }

        .upload-info {
            font-size: 0.9rem;
            color: #7f8c8d;
        }

        /* شريط التقدم */
        .progress-container {
            margin: 20px 0;
        }

        .progress-bar {
            height: 6px;
            background: #e9ecef;
            border-radius: 3px;
            overflow: hidden;
            margin-bottom: 10px;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #3498db, #25d366);
            width: 0%;
            transition: width 0.3s ease;
        }

        .progress-text {
            text-align: center;
            font-size: 0.9rem;
            color: #6c757d;
        }

        /* نتائج الاستخراج */
        .result-container {
            background: #f8f9fa;
            border-radius: 12px;
            padding: 20px;
            margin-top: 20px;
            border: 1px solid #dee2e6;
            max-height: 500px;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }

        .result-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid #dee2e6;
        }

        .result-title {
            color: #1a5c9e;
            font-size: 1.2rem;
            font-weight: 600;
        }

        /* تنسيق الجداول في النتائج */
        .result-table {
            width: 100%;
            border-collapse: collapse;
            margin: 15px 0;
            background: white;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }

        .result-table th {
            background: #1a5c9e;
            color: white;
            padding: 12px;
            text-align: right;
            font-weight: 600;
            font-size: 0.9rem;
        }

        .result-table td {
            padding: 10px 12px;
            border-bottom: 1px solid #e9ecef;
            text-align: right;
            font-size: 0.9rem;
        }

        .result-table tr:last-child td {
            border-bottom: none;
        }

        .result-table tr:hover {
            background: #f8f9fa;
        }

        /* قوائم في النتائج */
        .result-list {
            margin: 15px 0;
            padding-right: 20px;
        }

        .result-list li {
            margin-bottom: 8px;
            padding: 8px;
            background: white;
            border-radius: 6px;
            border-right: 3px solid #25d366;
        }

        /* إدخال البيانات */
        .input-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 15px;
            margin-bottom: 20px;
        }

        @media (min-width: 480px) {
            .input-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }

        @media (min-width: 768px) {
            .input-grid {
                grid-template-columns: repeat(4, 1fr);
            }
        }

        .input-group {
            display: flex;
            flex-direction: column;
        }

        .input-group label {
            margin-bottom: 8px;
            font-weight: 600;
            color: #495057;
            font-size: 0.9rem;
        }

        .input-group input,
        .input-group select {
            padding: 12px 14px;
            border: 2px solid #dee2e6;
            border-radius: 8px;
            font-size: 1rem;
            background: white;
            transition: all 0.3s;
        }

        .input-group input:focus,
        .input-group select:focus {
            border-color: #25d366;
            outline: none;
            box-shadow: 0 0 0 3px rgba(37, 211, 102, 0.1);
        }

        /* أزرار الإجراءات */
        .action-buttons {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 10px;
            margin: 20px 0;
        }

        /* جدول الطلاب */
        .table-container {
            overflow-x: auto;
            margin: 20px 0;
            border-radius: 10px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            -webkit-overflow-scrolling: touch;
        }

        .students-table {
            width: 100%;
            min-width: 600px;
            border-collapse: collapse;
            background: white;
        }

        .students-table th {
            background: linear-gradient(135deg, #1a5c9e 0%, #0d47a1 100%);
            color: white;
            padding: 14px;
            text-align: center;
            font-weight: 600;
            font-size: 0.9rem;
            position: sticky;
            top: 0;
        }

        .students-table td {
            padding: 12px;
            text-align: center;
            border-bottom: 1px solid #e9ecef;
            font-size: 0.9rem;
        }

        .students-table tr:hover {
            background: #f8f9fa;
        }

        .level-badge {
            display: inline-block;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            color: white;
            min-width: 80px;
        }

        .level-excellent { background: linear-gradient(135deg, #4caf50 0%, #2e7d32 100%); }
        .level-verygood { background: linear-gradient(135deg, #009688 0%, #00695c 100%); }
        .level-good { background: linear-gradient(135deg, #2196f3 0%, #0d47a1 100%); }
        .level-pass { background: linear-gradient(135deg, #ff9800 0%, #ef6c00 100%); }
        .level-weak { background: linear-gradient(135deg, #f44336 0%, #c62828 100%); }

        /* بطاقات الملخص */
        .summary-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 15px;
            margin: 20px 0;
        }

        .summary-card {
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.08);
            text-align: center;
            transition: transform 0.3s;
        }

        .summary-card:hover {
            transform: translateY(-5px);
        }

        .summary-card h3 {
            color: #1a5c9e;
            font-size: 0.95rem;
            margin-bottom: 10px;
            font-weight: 600;
        }

        .summary-card .value {
            font-size: 2rem;
            font-weight: 700;
            color: #2c3e50;
            margin: 10px 0;
        }

        .summary-card .subtext {
            font-size: 0.85rem;
            color: #6c757d;
        }

        /* الرسوم البيانية */
        .charts-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 20px;
            margin: 20px 0;
        }

        @media (min-width: 768px) {
            .charts-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }

        .chart-container {
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.08);
            height: 300px;
            display: flex;
            flex-direction: column;
        }

        .chart-container h3 {
            color: #1a5c9e;
            font-size: 1rem;
            margin-bottom: 15px;
            text-align: center;
            font-weight: 600;
        }

        .chart-container canvas {
            flex: 1;
            width: 100% !important;
            height: 100% !important;
        }

        /* الرسائل والتنبيهات */
        .alert {
            padding: 15px;
            border-radius: 10px;
            margin: 15px 0;
            display: flex;
            align-items: center;
            gap: 12px;
            animation: slideIn 0.3s ease;
            font-size: 0.95rem;
        }

        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .alert-success {
            background: linear-gradient(135deg, #d4edda 0%, #c3e6cb 100%);
            color: #155724;
            border: 1px solid #b1dfbb;
        }

        .alert-warning {
            background: linear-gradient(135deg, #fff3cd 0%, #ffeaa7 100%);
            color: #856404;
            border: 1px solid #ffeaa7;
        }

        .alert-error {
            background: linear-gradient(135deg, #f8d7da 0%, #f5c6cb 100%);
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .alert-info {
            background: linear-gradient(135deg, #e3f2fd 0%, #bbdefb 100%);
            color: #1565c0;
            border: 1px solid #90caf9;
        }

        /* التحميل */
        .loading {
            display: inline-block;
            width: 24px;
            height: 24px;
            border: 3px solid rgba(52, 152, 219, 0.3);
            border-radius: 50%;
            border-top-color: #3498db;
            animation: spin 1s ease-in-out infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* التقرير */
        .report-actions {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 10px;
            margin: 20px 0;
        }

        /* عناصر مخفية */
        .hidden {
            display: none !important;
        }

        /* الهوامش والأحجام للهواتف */
        @media (max-width: 767px) {
            .header {
                padding: 12px;
                margin-bottom: 15px;
            }
            
            .header h1 {
                font-size: 1.5rem;
            }
            
            .tab {
                padding: 12px 8px;
                min-width: 100px;
                font-size: 0.85rem;
            }
            
            .tab-content {
                padding: 15px;
            }
            
            .btn {
                padding: 12px 16px;
                font-size: 0.95rem;
            }
            
            .upload-area {
                padding: 30px 15px;
            }
            
            .upload-icon {
                font-size: 2.8rem;
            }
            
            .summary-card {
                padding: 15px;
            }
            
            .summary-card .value {
                font-size: 1.8rem;
            }
        }

        /* تحسينات للأيفون */
        @supports (-webkit-touch-callout: none) {
            .tabs {
                -webkit-overflow-scrolling: touch;
            }
            
            .table-container {
                -webkit-overflow-scrolling: touch;
            }
            
            .result-container {
                -webkit-overflow-scrolling: touch;
            }
            
            input, select, button {
                font-size: 16px; /* منع التكبير التلقائي في iOS */
            }
        }

        /* تحسينات لهواوي */
        @media (-webkit-device-pixel-ratio: 2) and (max-width: 768px) {
            .tab {
                padding: 14px 10px;
            }
            
            .btn {
                padding: 16px 20px;
            }
        }

        /* تذييل الصفحة */
        .footer {
            text-align: center;
            margin-top: 30px;
            padding-top: 20px;
            border-top: 1px solid #dee2e6;
            color: #6c757d;
            font-size: 0.85rem;
        }

        /* زر العودة للأعلى */
        .scroll-top {
            position: fixed;
            bottom: 20px;
            left: 20px;
            width: 50px;
            height: 50px;
            background: linear-gradient(135deg, #25d366 0%, #1da851 100%);
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 4px 12px rgba(37, 211, 102, 0.3);
            z-index: 1000;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .scroll-top.show {
            opacity: 1;
        }
    </style>
</head>
<body>
    <!-- زر العودة للأعلى -->
    <div class="scroll-top hidden" onclick="window.scrollTo({top: 0, behavior: 'smooth'})">
        <i class="fas fa-chevron-up"></i>
    </div>

    <div class="container">
        <!-- رأس النظام -->
        <div class="header">
            <h1><i class="fas fa-graduation-cap"></i> نظام استخراج وتحليل نتائج الطلاب</h1>
            <p>استخرج النصوص من ملفات PDF والصور مع الحفاظ على التنسيق والجداول</p>
        </div>

        <!-- التبويبات -->
        <div class="tabs">
            <div class="tab active" onclick="switchTab('extract')">
                <i class="fas fa-file-import"></i>
                <span>استخراج النصوص</span>
            </div>
            <div class="tab" onclick="switchTab('manage')">
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
            <div class="config-card">
                <div class="section-title">
                    <i class="fas fa-key"></i>
                    <span>إعدادات Google Gemini API</span>
                </div>
                
                <div class="api-input-group">
                    <input type="password" id="apiKeyInput" class="api-input" 
                           placeholder="أدخل مفتاح Gemini API هنا..." value="">
                    <button id="saveApiBtn" class="btn btn-primary">
                        <i class="fas fa-save"></i> حفظ المفتاح
                    </button>
                </div>
                
                <div id="apiStatus" class="alert alert-warning">
                    <i class="fas fa-exclamation-triangle"></i>
                    <span>يرجى إضافة مفتاح API للبدء</span>
                </div>
                
                <div class="section-title" style="margin-top: 20px;">
                    <i class="fas fa-brain"></i>
                    <span>نموذج Gemini المستخدم</span>
                </div>
                
                <div style="background: #e3f2fd; padding: 15px; border-radius: 10px; margin-top: 10px;">
                    <div style="display: flex; align-items: center; gap: 10px; margin-bottom: 10px;">
                        <i class="fas fa-bolt" style="color: #ff9800;"></i>
                        <strong style="color: #1565c0;">Gemini 2.0 Flash</strong>
                    </div>
                    <p style="color: #0d47a1; margin: 0; font-size: 0.9rem;">
                        النموذج الأسرع والأكثر فعالية لاستخراج النصوص مع الحفاظ على التنسيق والجداول
                    </p>
                </div>
            </div>
            
            <div class="upload-area" id="dropZone">
                <div class="upload-icon">
                    <i class="fas fa-cloud-upload-alt"></i>
                </div>
                <div class="upload-text" id="fileLabel">اسحب ملف PDF أو صورة هنا</div>
                <div class="upload-info" id="fileInfo">أو انقر للاختيار - الحد الأقصى: 10MB</div>
                <input type="file" id="fileInput" accept="application/pdf,image/*" class="hidden">
            </div>
            
            <div class="progress-container hidden" id="progressContainer">
                <div class="progress-bar">
                    <div class="progress-fill" id="progressFill"></div>
                </div>
                <div class="progress-text" id="progressText">0%</div>
            </div>
            
            <button id="btnExtract" class="btn btn-primary btn-lg" disabled>
                <i class="fas fa-magic"></i> استخراج وتحليل تلقائي
            </button>
            
            <div id="resultContainer" class="result-container hidden">
                <div class="result-header">
                    <div class="result-title">
                        <i class="fas fa-file-alt"></i> النتائج المستخرجة
                    </div>
                    <button onclick="copyResults()" class="btn btn-secondary" style="padding: 8px 16px;">
                        <i class="fas fa-copy"></i> نسخ
                    </button>
                </div>
                <div id="extractedResults">
                    <!-- ستظهر النتائج هنا -->
                </div>
            </div>
        </div>

        <!-- تبويب إدارة البيانات -->
        <div id="manage-tab" class="tab-content">
            <div id="manageAlert" class="alert hidden"></div>
            
            <div class="input-grid">
                <div class="input-group">
                    <label><i class="fas fa-user"></i> اسم الطالب</label>
                    <input type="text" id="studentName" placeholder="أدخل اسم الطالب">
                </div>
                <div class="input-group">
                    <label><i class="fas fa-book"></i> المادة</label>
                    <select id="subject">
                        <option value="الرياضيات">الرياضيات</option>
                        <option value="اللغة العربية">الغة العربية</option>
                        <option value="اللغة الإنجليزية">اللغة الإنجليزية</option>
                        <option value="العلوم">العلوم</option>
                        <option value="الدراسات">الدراسات</option>
                        <option value="الحاسب">الحاسب</option>
                        <option value="عام">عام</option>
                    </select>
                </div>
                <div class="input-group">
                    <label><i class="fas fa-school"></i> الفصل</label>
                    <select id="className">
                        <option value="1/أ">1/أ</option>
                        <option value="1/ب">1/ب</option>
                        <option value="1/ج">1/ج</option>
                        <option value="2/أ">2/أ</option>
                        <option value="2/ب">2/ب</option>
                        <option value="2/ج">2/ج</option>
                        <option value="غير محدد">غير محدد</option>
                    </select>
                </div>
                <div class="input-group">
                    <label><i class="fas fa-star"></i> الدرجة (من 40)</label>
                    <input type="number" id="score" min="0" max="40" step="0.5" placeholder="0-40">
                </div>
            </div>
            
            <div class="action-buttons">
                <button onclick="addStudent()" class="btn btn-primary">
                    <i class="fas fa-plus"></i> إضافة طالب
                </button>
                <button onclick="clearForm()" class="btn btn-secondary">
                    <i class="fas fa-broom"></i> تفريغ الحقول
                </button>
                <button onclick="processExtractedData()" class="btn btn-primary">
                    <i class="fas fa-robot"></i> معالجة البيانات
                </button>
                <button onclick="clearAllData()" class="btn btn-danger">
                    <i class="fas fa-trash"></i> مسح الكل
                </button>
            </div>
            
            <div class="table-container">
                <table class="students-table">
                    <thead>
                        <tr>
                            <th>#</th>
                            <th>اسم الطالب</th>
                            <th>المادة</th>
                            <th>الفصل</th>
                            <th>الدرجة</th>
                            <th>المستوى</th>
                            <th>الإجراءات</th>
                        </tr>
                    </thead>
                    <tbody id="studentsTableBody">
                        <!-- سيتم ملؤها تلقائياً -->
                    </tbody>
                </table>
            </div>
        </div>

        <!-- تبويب تحليل النتائج -->
        <div id="analysis-tab" class="tab-content">
            <div id="analysisAlert" class="alert alert-warning">
                <i class="fas fa-exclamation-triangle"></i>
                <span>لا توجد بيانات لعرض التحليل. يرجى استخراج البيانات أولاً.</span>
            </div>
            
            <div class="summary-cards" id="summaryCards">
                <!-- سيتم ملؤها تلقائياً -->
            </div>
            
            <div class="charts-grid">
                <div class="chart-container">
                    <h3>توزيع الطلاب حسب المستوى</h3>
                    <canvas id="levelChart"></canvas>
                </div>
                <div class="chart-container">
                    <h3>متوسط الدرجات حسب المادة</h3>
                    <canvas id="subjectChart"></canvas>
                </div>
            </div>
            
            <div style="background: white; padding: 20px; border-radius: 12px; margin-top: 20px; box-shadow: 0 4px 8px rgba(0,0,0,0.08);">
                <h3 style="color: #1a5c9e; margin-bottom: 15px; text-align: center;">
                    <i class="fas fa-list"></i> تفاصيل النتائج حسب المستوى
                </h3>
                <div id="levelDetailsTable">
                    <!-- سيتم ملؤها تلقائياً -->
                </div>
            </div>
        </div>

        <!-- تبويب التقرير النهائي -->
        <div id="report-tab" class="tab-content">
            <div id="reportAlert" class="alert alert-warning">
                <i class="fas fa-exclamation-triangle"></i>
                <span>لا توجد بيانات لإنشاء التقرير. يرجى استخراج البيانات أولاً.</span>
            </div>
            
            <div class="report-actions">
                <button onclick="generatePDF()" class="btn btn-primary">
                    <i class="fas fa-download"></i> تحميل PDF
                </button>
                <button onclick="printReport()" class="btn btn-secondary">
                    <i class="fas fa-print"></i> طباعة
                </button>
                <button onclick="exportToExcel()" class="btn btn-secondary">
                    <i class="fas fa-file-excel"></i> تصدير Excel
                </button>
                <button onclick="shareReport()" class="btn btn-primary">
                    <i class="fas fa-share"></i> مشاركة
                </button>
            </div>
            
            <div id="reportContent" style="background: white; padding: 20px; border-radius: 12px; margin-top: 20px; box-shadow: 0 4px 12px rgba(0,0,0,0.1);">
                <!-- سيتم ملؤها تلقائياً -->
            </div>
        </div>

        <!-- تذييل الصفحة -->
        <div class="footer">
            <p>
                <i class="fas fa-code"></i> نظام استخراج وتحليل نتائج الطلاب 
                | v2.0 | يدعم جميع الأجهزة
            </p>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script type="module">
        // استيراد مكتبة Google Gemini AI
        import { GoogleGenerativeAI } from "https://esm.run/@google/generative-ai@0.1.0";

        // تخزين بيانات التطبيق
        const appState = {
            API_KEY: localStorage.getItem('gemini_api_key') || '',
            students: new Map(), // استخدام Map للحفاظ على الترتيب وعدم التكرار
            extractedData: '',
            processing: false
        };

        // عناصر واجهة المستخدم
        const elements = {
            // تبويب الاستخراج
            apiKeyInput: document.getElementById('apiKeyInput'),
            saveApiBtn: document.getElementById('saveApiBtn'),
            apiStatus: document.getElementById('apiStatus'),
            fileInput: document.getElementById('fileInput'),
            dropZone: document.getElementById('dropZone'),
            fileLabel: document.getElementById('fileLabel'),
            fileInfo: document.getElementById('fileInfo'),
            btnExtract: document.getElementById('btnExtract'),
            progressContainer: document.getElementById('progressContainer'),
            progressFill: document.getElementById('progressFill'),
            progressText: document.getElementById('progressText'),
            resultContainer: document.getElementById('resultContainer'),
            extractedResults: document.getElementById('extractedResults'),
            
            // تبويب الإدارة
            manageAlert: document.getElementById('manageAlert'),
            studentName: document.getElementById('studentName'),
            subject: document.getElementById('subject'),
            className: document.getElementById('className'),
            score: document.getElementById('score'),
            studentsTableBody: document.getElementById('studentsTableBody'),
            
            // تبويب التحليل
            analysisAlert: document.getElementById('analysisAlert'),
            summaryCards: document.getElementById('summaryCards'),
            levelDetailsTable: document.getElementById('levelDetailsTable'),
            
            // تبويب التقرير
            reportAlert: document.getElementById('reportAlert'),
            reportContent: document.getElementById('reportContent'),
            
            // زر العودة للأعلى
            scrollTop: document.querySelector('.scroll-top')
        };

        // تهيئة التطبيق
        function initApp() {
            console.log("جاري تهيئة النظام...");
            
            // تهيئة إعدادات API
            if (appState.API_KEY) {
                elements.apiKeyInput.value = "••••••••" + appState.API_KEY.slice(-4);
                updateApiStatus(true);
            }
            
            // تحميل البيانات المحفوظة
            loadFromLocalStorage();
            
            // إعداد معالجات الأحداث
            setupEventHandlers();
            
            // إعداد زر العودة للأعلى
            setupScrollTop();
            
            // تحديث العرض
            updateStudentsTable();
            updateAnalysis();
            
            console.log("تم تهيئة النظام بنجاح");
        }

        // إعداد معالجات الأحداث
        function setupEventHandlers() {
            // زر حفظ API
            elements.saveApiBtn.addEventListener('click', saveApiKey);
            
            // رفع الملفات
            elements.dropZone.addEventListener('click', () => elements.fileInput.click());
            elements.fileInput.addEventListener('change', handleFileSelect);
            elements.btnExtract.addEventListener('click', extractAndAnalyze);
            
            // سحب وإفلات الملفات
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
            
            // إدخال البيانات
            elements.studentName.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') addStudent();
            });
            elements.score.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') addStudent();
            });
            
            // تحديث حالة زر الاستخراج عند إضافة API
            elements.apiKeyInput.addEventListener('input', () => {
                const hasApiKey = elements.apiKeyInput.value.trim().length > 0;
                elements.btnExtract.disabled = !hasApiKey;
            });
        }

        // إعداد زر العودة للأعلى
        function setupScrollTop() {
            window.addEventListener('scroll', () => {
                if (window.scrollY > 300) {
                    elements.scrollTop.classList.remove('hidden');
                    elements.scrollTop.classList.add('show');
                } else {
                    elements.scrollTop.classList.remove('show');
                    elements.scrollTop.classList.add('hidden');
                }
            });
        }

        // إدارة API
        function updateApiStatus(isValid) {
            if (isValid && appState.API_KEY) {
                elements.apiStatus.className = 'alert alert-success';
                elements.apiStatus.innerHTML = `
                    <i class="fas fa-check-circle"></i>
                    <span>مفتاح API صالح ومحفوظ</span>
                `;
                elements.btnExtract.disabled = false;
            } else {
                elements.apiStatus.className = 'alert alert-warning';
                elements.apiStatus.innerHTML = `
                    <i class="fas fa-exclamation-triangle"></i>
                    <span>يرجى إضافة مفتاح API صالح</span>
                `;
                elements.btnExtract.disabled = true;
            }
        }

        function saveApiKey() {
            const inputKey = elements.apiKeyInput.value.trim();
            
            // إذا كان الحقل يحتوي على نقاط (مفتاح مخفي)
            if (inputKey.includes('••••')) {
                updateApiStatus(true);
                return;
            }
            
            if (inputKey === '') {
                // مسح المفتاح
                localStorage.removeItem('gemini_api_key');
                appState.API_KEY = '';
                elements.apiKeyInput.value = '';
                updateApiStatus(false);
                showAlert('manageAlert', 'تم مسح مفتاح API بنجاح', 'success');
                return;
            }
            
            // التحقق من شكل مفتاح API
            if (!inputKey.startsWith('AIza')) {
                showAlert('manageAlert', 'يبدو أن مفتاح API غير صحيح. يجب أن يبدأ المفتاح بـ "AIza"', 'error');
                return;
            }
            
            // اختبار الاتصال بالـ API
            testApiConnection(inputKey).then(isValid => {
                if (isValid) {
                    appState.API_KEY = inputKey;
                    localStorage.setItem('gemini_api_key', appState.API_KEY);
                    elements.apiKeyInput.value = "••••••••" + appState.API_KEY.slice(-4);
                    updateApiStatus(true);
                    showAlert('manageAlert', 'تم حفظ مفتاح API بنجاح والاتصال بالنموذج!', 'success');
                } else {
                    showAlert('manageAlert', 'مفتاح API غير صالح أو غير قادر على الاتصال بخدمات Google AI', 'error');
                }
            }).catch(error => {
                showAlert('manageAlert', 'حدث خطأ أثناء اختبار الاتصال: ' + error.message, 'error');
            });
        }

        async function testApiConnection(apiKey) {
            try {
                const response = await fetch(`https://generativelanguage.googleapis.com/v1/models/gemini-2.0-flash?key=${apiKey}`);
                return response.ok;
            } catch (error) {
                console.error("خطأ في اختبار الاتصال:", error);
                return false;
            }
        }

        // معالجة الملفات
        function handleFileSelect() {
            if (elements.fileInput.files[0]) {
                const file = elements.fileInput.files[0];
                const fileSize = (file.size / 1024 / 1024).toFixed(2);
                const fileName = file.name.length > 25 ? file.name.substring(0, 22) + '...' : file.name;
                
                elements.fileLabel.innerHTML = `<i class="fas fa-file"></i> ${fileName}`;
                elements.fileInfo.innerHTML = `<i class="fas fa-info-circle"></i> ${fileSize} MB | ${file.type.split('/')[1] || 'ملف'}`;
            }
        }

        // استخراج النصوص
        async function extractAndAnalyze() {
            if (!appState.API_KEY) {
                showAlert('manageAlert', 'يرجى إضافة مفتاح API أولاً', 'error');
                return;
            }
            
            if (!elements.fileInput.files[0]) {
                showAlert('manageAlert', 'الرجاء اختيار ملف أولاً', 'error');
                return;
            }
            
            const file = elements.fileInput.files[0];
            const maxSize = 10 * 1024 * 1024;
            
            if (file.size > maxSize) {
                showAlert('manageAlert', 'حجم الملف كبير جداً. الحد الأقصى هو 10MB', 'error');
                return;
            }
            
            // بدء المعالجة
            appState.processing = true;
            elements.btnExtract.disabled = true;
            elements.btnExtract.innerHTML = '<span class="loading"></span> جاري الاستخراج...';
            elements.progressContainer.classList.remove('hidden');
            updateProgress(10);
            
            try {
                const genAI = new GoogleGenerativeAI(appState.API_KEY);
                const model = genAI.getGenerativeModel({ model: 'gemini-2.0-flash' });
                
                updateProgress(30);
                
                const reader = new FileReader();
                
                reader.onloadend = async () => {
                    try {
                        const base64Data = reader.result.split(',')[1];
                        updateProgress(50);
                        
                        // نص توضيحي للحفاظ على التنسيق والجداول
                        const promptText = `هذا ملف يحتوي على نتائج طلاب. المطلوب استخراج البيانات بدقة مع الحفاظ الكامل على:
                        1. التنسيق الأصلي (الجداول، القوائم، العناوين)
                        2. ترتيب البيانات كما هي في الملف
                        3. جميع الأسماء والدرجات والمواد
                        4. العلاقة بين الأسماء ودرجاتهم
                        
                        إذا كان هناك جداول:
                        - حافظ على شكل الجدول الأصلي
                        - احتفظ بترتيب الأعمدة والصفوف
                        - استخدم علامات الجداول المناسبة
                        
                        الشكل المطلوب:
                        • لكل طالب: اسم الطالب | المادة | الفصل | الدرجة
                        • إذا لم توجد معلومات: استخدم "غير محدد"
                        • إذا كانت الدرجات بنسبة مئوية: حولها إلى درجة من 40
                        
                        أعد البيانات بنفس الترتيب والتنسيق الموجود في الملف.`;
                        
                        updateProgress(70);
                        
                        const result = await model.generateContent([
                            promptText,
                            { inlineData: { data: base64Data, mimeType: file.type } }
                        ]);
                        
                        const response = await result.response;
                        const extractedText = response.text();
                        
                        appState.extractedData = extractedText;
                        updateProgress(90);
                        
                        // عرض النتائج مع الحفاظ على التنسيق
                        displayFormattedResults(extractedText);
                        updateProgress(100);
                        
                        // معالجة البيانات تلقائياً
                        setTimeout(() => {
                            processExtractedData();
                            switchTab('manage');
                            showAlert('manageAlert', `تم استخراج البيانات بنجاح!`, 'success');
                        }, 1000);
                        
                    } catch (apiError) {
                        console.error("API Error:", apiError);
                        handleApiError(apiError);
                    } finally {
                        finishProcessing();
                    }
                };
                
                reader.onerror = () => {
                    showAlert('manageAlert', 'حدث خطأ أثناء قراءة الملف', 'error');
                    finishProcessing();
                };
                
                reader.readAsDataURL(file);
                
            } catch (error) {
                console.error("General error:", error);
                showAlert('manageAlert', `حدث خطأ: ${error.message}`, 'error');
                finishProcessing();
            }
        }

        function updateProgress(percentage) {
            elements.progressFill.style.width = percentage + '%';
            elements.progressText.textContent = percentage + '%';
            
            if (percentage === 100) {
                elements.progressText.textContent = 'اكتمل!';
            }
        }

        function finishProcessing() {
            appState.processing = false;
            elements.btnExtract.disabled = false;
            elements.btnExtract.innerHTML = '<i class="fas fa-magic"></i> استخراج وتحليل تلقائي';
            
            setTimeout(() => {
                elements.progressContainer.classList.add('hidden');
                elements.progressFill.style.width = '0%';
                elements.progressText.textContent = '0%';
            }, 2000);
        }

        function handleApiError(apiError) {
            let errorMessage = 'حدث خطأ أثناء معالجة الملف';
            
            if (apiError.message.includes('404') || apiError.message.includes('not found')) {
                errorMessage = 'نموذج Gemini 2.0 Flash غير متوفر. تأكد من أن مفتاح API يدعم هذا النموذج.';
            } else if (apiError.message.includes('API key not valid')) {
                errorMessage = 'مفتاح API غير صالح. يرجى التحقق من المفتاح وإعادة المحاولة.';
            } else if (apiError.message.includes('quota')) {
                errorMessage = 'تم تجاوز الحد المسموح. جرب لاحقاً أو استخدم مفتاح API آخر.';
            } else {
                errorMessage += ': ' + apiError.message;
            }
            
            showAlert('manageAlert', errorMessage, 'error');
        }

        function displayFormattedResults(text) {
            // تنظيف النص مع الحفاظ على التنسيق
            let formattedText = text
                .replace(/\n/g, '<br>')
                .replace(/\t/g, '&nbsp;&nbsp;&nbsp;&nbsp;')
                .replace(/  /g, '&nbsp;&nbsp;');
            
            // تحويل الجداول إلى HTML
            formattedText = formatTables(formattedText);
            
            // تحويل القوائم إلى HTML
            formattedText = formatLists(formattedText);
            
            elements.extractedResults.innerHTML = formattedText;
            elements.resultContainer.classList.remove('hidden');
        }

        function formatTables(text) {
            // البحث عن الجداول في النص
            const tableRegex = /(\|[^\n]+\|\n)+/g;
            return text.replace(tableRegex, (match) => {
                const rows = match.trim().split('\n').filter(row => row.trim());
                let html = '<div style="overflow-x: auto; margin: 15px 0;">';
                html += '<table class="result-table">';
                
                rows.forEach((row, index) => {
                    const cells = row.split('|').filter(cell => cell.trim() !== '');
                    const isHeader = index === 0;
                    
                    html += '<tr>';
                    cells.forEach(cell => {
                        const content = cell.trim();
                        if (isHeader) {
                            html += `<th>${content}</th>`;
                        } else {
                            html += `<td>${content}</td>`;
                        }
                    });
                    html += '</tr>';
                });
                
                html += '</table></div>';
                return html;
            });
        }

        function formatLists(text) {
            // البحث عن القوائم النقطية
            const listRegex = /(?:^|\n)(?:[-•*]\s+[^\n]+(?:\n(?![-•*]\s+)[^\n]*)*)+/g;
            return text.replace(listRegex, (match) => {
                const items = match.trim().split('\n').filter(item => item.trim());
                let html = '<ul class="result-list">';
                
                items.forEach(item => {
                    const content = item.replace(/^[-•*]\s+/, '').trim();
                    if (content) {
                        html += `<li>${content}</li>`;
                    }
                });
                
                html += '</ul>';
                return html;
            });
        }

        function copyResults() {
            const textToCopy = appState.extractedData;
            navigator.clipboard.writeText(textToCopy).then(() => {
                showAlert('manageAlert', 'تم نسخ النتائج إلى الحافظة', 'success');
            });
        }

        // معالجة البيانات المستخرجة
        function processExtractedData() {
            if (!appState.extractedData) {
                showAlert('manageAlert', 'لا توجد بيانات مستخرجة', 'error');
                return;
            }
            
            const lines = appState.extractedData.split('\n');
            let processedCount = 0;
            
            lines.forEach(line => {
                const studentData = parseStudentLine(line);
                if (studentData) {
                    addStudentToMap(studentData);
                    processedCount++;
                }
            });
            
            if (processedCount > 0) {
                updateStudentsTable();
                updateAnalysis();
                saveToLocalStorage();
                showAlert('manageAlert', `تمت معالجة ${processedCount} طالب بنجاح`, 'success');
            } else {
                showAlert('manageAlert', 'لم يتم العثور على بيانات طلاب في النص المستخرج', 'warning');
            }
        }

        function parseStudentLine(line) {
            const cleanLine = line.trim();
            if (!cleanLine || cleanLine.length < 2) return null;
            
            // أنماط مختلفة للتحليل
            const patterns = [
                // النمط: اسم | مادة | فصل | درجة
                /([^|]+)\s*\|\s*([^|]+)\s*\|\s*([^|]+)\s*\|\s*([\d\.]+)/,
                // النمط: اسم - مادة - فصل - درجة
                /([^-]+)\s*-\s*([^-]+)\s*-\s*([^-]+)\s*-\s*([\d\.]+)/,
                // النمط: اسم، مادة، فصل، درجة
                /([^،]+)\s*،\s*([^،]+)\s*،\s*([^،]+)\s*،\s*([\d\.]+)/,
                // النمط: اسم: درجة
                /([^:]+):\s*([\d\.]+)/,
                // النمط: درجة - اسم
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
                        score = parseFloat(match[4]);
                    } else if (pattern === patterns[3]) {
                        name = match[1].trim();
                        score = parseFloat(match[2]);
                        subject = "عام";
                        className = "غير محدد";
                    } else if (pattern === patterns[4]) {
                        score = parseFloat(match[1]);
                        name = match[2].trim();
                        subject = "عام";
                        className = "غير محدد";
                    }
                    
                    // تحويل النسبة المئوية
                    if (score > 40 && score <= 100) {
                        score = (score * 40) / 100;
                    }
                    
                    // التأكد من صحة الدرجة
                    if (score >= 0 && score <= 40 && name.length > 1) {
                        return {
                            id: generateStudentId(name),
                            name: cleanText(name),
                            subject: cleanText(subject) || "عام",
                            className: cleanText(className) || "غير محدد",
                            score: parseFloat(score.toFixed(1)),
                            level: getLevel(score)
                        };
                    }
                }
            }
            
            return null;
        }

        function generateStudentId(name) {
            // إنشاء ID فريد للطالب باستخدام الاسم والطابع الزمني
            const timestamp = Date.now();
            const nameHash = Array.from(name).reduce((hash, char) => {
                return ((hash << 5) - hash) + char.charCodeAt(0);
            }, 0);
            
            return `student_${Math.abs(nameHash)}_${timestamp}`;
        }

        function cleanText(text) {
            if (!text) return '';
            // إزالة الرموز الخاصة والحفاظ على النص العربي
            return text.replace(/[^\u0600-\u06FF\sa-zA-Z0-9]/g, '')
                      .replace(/\s+/g, ' ')
                      .trim();
        }

        function getLevel(score) {
            if (score >= 36) return {name: 'ممتاز', class: 'excellent'};
            if (score >= 32) return {name: 'جيد جدًا', class: 'verygood'};
            if (score >= 28) return {name: 'جيد', class: 'good'};
            if (score >= 20) return {name: 'مقبول', class: 'pass'};
            return {name: 'ضعيف', class: 'weak'};
        }

        function addStudentToMap(studentData) {
            // استخدام Map لمنع التكرار بناءً على ID
            appState.students.set(studentData.id, studentData);
        }

        // إدارة البيانات
        function addStudent() {
            const name = elements.studentName.value.trim();
            const subject = elements.subject.value;
            const className = elements.className.value;
            const score = parseFloat(elements.score.value);
            
            if (!name || isNaN(score) || score < 0 || score > 40) {
                showAlert('manageAlert', 'يرجى إدخال بيانات صحيحة (الدرجة من 0 إلى 40)', 'error');
                return;
            }
            
            const studentData = {
                id: generateStudentId(name),
                name: cleanText(name),
                subject,
                className,
                score,
                level: getLevel(score)
            };
            
            addStudentToMap(studentData);
            updateStudentsTable();
            
            // تفريغ الحقول
            clearForm();
            
            showAlert('manageAlert', `تم إضافة الطالب ${name} بنجاح`, 'success');
            
            updateAnalysis();
            saveToLocalStorage();
        }

        function updateStudentsTable() {
            const tbody = elements.studentsTableBody;
            
            if (appState.students.size === 0) {
                tbody.innerHTML = `
                    <tr>
                        <td colspan="7" style="text-align:center; padding:30px; color:#666;">
                            <i class="fas fa-users-slash" style="font-size:2rem; display:block; margin-bottom:10px;"></i>
                            لا توجد بيانات، يرجى إضافة طلاب
                        </td>
                    </tr>
                `;
                return;
            }
            
            let html = '';
            let index = 1;
            
            // استخدام Array.from للحفاظ على ترتيب الإدخال
            Array.from(appState.students.values()).forEach(student => {
                html += `
                    <tr>
                        <td>${index++}</td>
                        <td>${student.name}</td>
                        <td>${student.subject}</td>
                        <td>${student.className}</td>
                        <td><strong>${student.score}</strong></td>
                        <td>
                            <span class="level-badge level-${student.level.class}">
                                ${student.level.name}
                            </span>
                        </td>
                        <td>
                            <button onclick="deleteStudent('${student.id}')" class="btn btn-danger" style="padding: 6px 12px; font-size: 0.85rem;">
                                <i class="fas fa-trash"></i>
                            </button>
                        </td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
        }

        function deleteStudent(studentId) {
            if (confirm('هل أنت متأكد من حذف هذا الطالب؟')) {
                appState.students.delete(studentId);
                updateStudentsTable();
                updateAnalysis();
                showAlert('manageAlert', 'تم حذف الطالب بنجاح', 'success');
                saveToLocalStorage();
            }
        }

        function clearForm() {
            elements.studentName.value = '';
            elements.score.value = '';
            elements.studentName.focus();
        }

        function clearAllData() {
            if (confirm('هل أنت متأكد من مسح جميع البيانات؟ هذا الإجراء لا يمكن التراجع عنه.')) {
                appState.students.clear();
                appState.extractedData = '';
                updateStudentsTable();
                updateAnalysis();
                elements.resultContainer.classList.add('hidden');
                showAlert('manageAlert', 'تم مسح جميع البيانات بنجاح', 'success');
                saveToLocalStorage();
            }
        }

        // التحليل والإحصائيات
        function updateAnalysis() {
            if (appState.students.size === 0) {
                elements.analysisAlert.classList.remove('hidden');
                elements.summaryCards.innerHTML = '';
                elements.levelDetailsTable.innerHTML = '';
                return;
            }
            
            elements.analysisAlert.classList.add('hidden');
            
            const students = Array.from(appState.students.values());
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
            students.forEach(student => {
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
                    <div class="subtext">طالب</div>
                </div>
                <div class="summary-card">
                    <h3><i class="fas fa-chart-line"></i> متوسط الدرجات</h3>
                    <div class="value">${avgScore.toFixed(1)}</div>
                    <div class="subtext">من 40</div>
                </div>
                <div class="summary-card">
                    <h3><i class="fas fa-percentage"></i> نسبة النجاح</h3>
                    <div class="value">${passRate}%</div>
                    <div class="subtext">نجحوا</div>
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
            const levelCtx = document.getElementById('levelChart').getContext('2d');
            new Chart(levelCtx, {
                type: 'doughnut',
                data: {
                    labels: Object.keys(levelCounts),
                    datasets: [{
                        data: Object.values(levelCounts),
                        backgroundColor: ['#4caf50', '#009688', '#2196f3', '#ff9800', '#f44336'],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { 
                            position: 'bottom',
                            labels: { padding: 15 }
                        }
                    }
                }
            });
            
            // رسم بياني للمواد
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
                        label: 'المتوسط',
                        data: subjectAverages,
                        backgroundColor: '#1a5c9e'
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
                <div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; text-align: center;">
                    <div style="font-weight: bold; padding: 10px; background: #f8f9fa; border-radius: 8px;">عدد</div>
                    <div style="font-weight: bold; padding: 10px; background: #f8f9fa; border-radius: 8px;">نطاق الدرجات</div>
                    <div style="font-weight: bold; padding: 10px; background: #f8f9fa; border-radius: 8px;">المستوى</div>
            `;
            
            const levels = ['ممتاز', 'جيد جدًا', 'جيد', 'مقبول', 'ضعيف'];
            
            levels.forEach(level => {
                const count = levelCounts[level] || 0;
                const percentage = appState.students.size > 0 ? ((count / appState.students.size) * 100).toFixed(1) : '0';
                tableHTML += `
                    <div style="padding: 12px; border-bottom: 1px solid #eee;"><strong>${count}</strong> <small>(${percentage}%)</small></div>
                    <div style="padding: 12px; border-bottom: 1px solid #eee;">${levelRanges[level]}</div>
                    <div style="padding: 12px; border-bottom: 1px solid #eee;">
                        <span class="level-badge level-${level}">${level}</span>
                    </div>
                `;
            });
            
            tableHTML += '</div>';
            elements.levelDetailsTable.innerHTML = tableHTML;
        }

        // التقرير النهائي
        async function generatePDF() {
            if (appState.students.size === 0) {
                showAlert('reportAlert', 'لا توجد بيانات لإنشاء تقرير', 'error');
                return;
            }
            
            updateReportContent();
            
            try {
                // استخدام html2canvas و jsPDF لإنشاء PDF
                const canvas = await html2canvas(document.getElementById('reportContent'), {
                    scale: 2,
                    backgroundColor: '#ffffff',
                    useCORS: true
                });
                
                const { jsPDF } = window.jspdf;
                const pdf = new jsPDF('p', 'mm', 'a4');
                
                const imgData = canvas.toDataURL('image/jpeg', 1.0);
                pdf.addImage(imgData, 'JPEG', 0, 0, 210, 297);
                pdf.save('تقرير_النتائج.pdf');
                
                showAlert('reportAlert', 'تم حفظ التقرير بنجاح', 'success');
            } catch (error) {
                console.error('خطأ في إنشاء PDF:', error);
                showAlert('reportAlert', 'حدث خطأ أثناء إنشاء التقرير', 'error');
            }
        }

        function updateReportContent() {
            if (appState.students.size === 0) {
                elements.reportAlert.classList.remove('hidden');
                elements.reportContent.innerHTML = '';
                return;
            }
            
            elements.reportAlert.classList.add('hidden');
            
            const students = Array.from(appState.students.values());
            const totalStudents = students.length;
            const totalScore = students.reduce((sum, student) => sum + student.score, 0);
            const avgScore = totalScore / totalStudents;
            
            let reportHTML = `
                <div style="text-align: center; margin-bottom: 25px;">
                    <h1 style="color: #1a5c9e; margin-bottom: 10px; font-size: 1.8rem;">📊 تقرير نتائج الطلاب</h1>
                    <p style="color: #666; margin-bottom: 5px;">التقرير تم إنشاؤه تلقائياً</p>
                    <p style="color: #888; font-size: 0.9rem;">${new Date().toLocaleDateString('ar-SA')}</p>
                </div>
                
                <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-bottom: 30px;">
                    <div style="background: linear-gradient(135deg, #e3f2fd 0%, #bbdefb 100%); padding: 20px; border-radius: 10px; text-align: center;">
                        <h3 style="color: #1a5c9e; margin-bottom: 10px;"><i class="fas fa-users"></i> إجمالي الطلاب</h3>
                        <div style="font-size: 2.5rem; font-weight: bold; color: #0d47a1;">${totalStudents}</div>
                    </div>
                    <div style="background: linear-gradient(135deg, #e8f5e9 0%, #c8e6c9 100%); padding: 20px; border-radius: 10px; text-align: center;">
                        <h3 style="color: #2e7d32; margin-bottom: 10px;"><i class="fas fa-chart-line"></i> متوسط الدرجات</h3>
                        <div style="font-size: 2.5rem; font-weight: bold; color: #1b5e20;">${avgScore.toFixed(1)}</div>
                    </div>
                </div>
                
                <div style="overflow-x: auto;">
                    <table style="width: 100%; border-collapse: collapse; margin: 20px 0;">
                        <thead>
                            <tr style="background: #1a5c9e; color: white;">
                                <th style="padding: 12px; text-align: center;">الاسم</th>
                                <th style="padding: 12px; text-align: center;">المادة</th>
                                <th style="padding: 12px; text-align: center;">الفصل</th>
                                <th style="padding: 12px; text-align: center;">الدرجة</th>
                                <th style="padding: 12px; text-align: center;">المستوى</th>
                            </tr>
                        </thead>
                        <tbody>
            `;
            
            students.forEach(student => {
                reportHTML += `
                    <tr>
                        <td style="padding: 10px; text-align: center; border-bottom: 1px solid #eee;">${student.name}</td>
                        <td style="padding: 10px; text-align: center; border-bottom: 1px solid #eee;">${student.subject}</td>
                        <td style="padding: 10px; text-align: center; border-bottom: 1px solid #eee;">${student.className}</td>
                        <td style="padding: 10px; text-align: center; border-bottom: 1px solid #eee; font-weight: bold;">${student.score}</td>
                        <td style="padding: 10px; text-align: center; border-bottom: 1px solid #eee;">
                            <span style="color: #fff; padding: 5px 10px; border-radius: 4px; background: ${getLevelColor(student.level.name)};">
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
            `;
            
            elements.reportContent.innerHTML = reportHTML;
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

        function printReport() {
            if (appState.students.size === 0) {
                showAlert('reportAlert', 'لا توجد بيانات للطباعة', 'error');
                return;
            }
            
            updateReportContent();
            const printWindow = window.open('', '_blank');
            printWindow.document.write(`
                <!DOCTYPE html>
                <html dir="rtl">
                <head>
                    <meta charset="UTF-8">
                    <title>تقرير النتائج</title>
                    <style>
                        body { font-family: Arial, sans-serif; margin: 20px; direction: rtl; }
                        @media print {
                            body { margin: 0; }
                        }
                    </style>
                </head>
                <body>
                    ${document.getElementById('reportContent').innerHTML}
                </body>
                </html>
            `);
            printWindow.document.close();
            printWindow.print();
        }

        function exportToExcel() {
            if (appState.students.size === 0) {
                showAlert('reportAlert', 'لا توجد بيانات للتصدير', 'error');
                return;
            }
            
            const students = Array.from(appState.students.values());
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            csvContent += "الاسم,المادة,الفصل,الدرجة,المستوى\n";
            
            students.forEach(student => {
                csvContent += `${student.name},${student.subject},${student.className},${student.score},${student.level.name}\n`;
            });
            
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", "نتائج_الطلاب.csv");
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showAlert('reportAlert', 'تم تصدير البيانات إلى ملف Excel', 'success');
        }

        function shareReport() {
            if (appState.students.size === 0) {
                showAlert('reportAlert', 'لا توجد بيانات للمشاركة', 'error');
                return;
            }
            
            const students = Array.from(appState.students.values());
            const totalStudents = students.length;
            const totalScore = students.reduce((sum, student) => sum + student.score, 0);
            const avgScore = totalScore / totalStudents;
            
            const text = `تقرير نتائج الطلاب:
            • عدد الطلاب: ${totalStudents}
            • متوسط الدرجات: ${avgScore.toFixed(1)}
            • تم إنشاء التقرير بواسطة نظام استخراج وتحليل النتائج`;
            
            if (navigator.share) {
                navigator.share({
                    title: 'تقرير نتائج الطلاب',
                    text: text,
                    url: window.location.href
                });
            } else {
                // نسخ إلى الحافظة
                navigator.clipboard.writeText(text).then(() => {
                    showAlert('reportAlert', 'تم نسخ التقرير إلى الحافظة', 'success');
                });
            }
        }

        // وظائف مساعدة
        function switchTab(tabName) {
            // إخفاء جميع المحتويات
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // إلغاء تنشيط جميع التبويبات
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // إظهار المحتوى المطلوب
            const targetTab = document.getElementById(tabName + '-tab');
            if (targetTab) {
                targetTab.classList.add('active');
            }
            
            // تفعيل التبويب المطلوب
            document.querySelectorAll('.tab').forEach(tab => {
                if (tab.textContent.includes(getTabText(tabName))) {
                    tab.classList.add('active');
                }
            });
            
            // تحديث العرض إذا لزم الأمر
            if (tabName === 'analysis') {
                updateAnalysis();
            } else if (tabName === 'report') {
                updateReportContent();
            }
        }

        function getTabText(tabName) {
            switch(tabName) {
                case 'extract': return 'استخراج النصوص';
                case 'manage': return 'إدارة البيانات';
                case 'analysis': return 'تحليل النتائج';
                case 'report': return 'التقرير النهائي';
                default: return '';
            }
        }

        function showAlert(elementId, message, type = 'info') {
            const element = document.getElementById(elementId);
            if (!element) return;
            
            const icon = type === 'success' ? 'fas fa-check-circle' : 
                         type === 'error' ? 'fas fa-times-circle' : 
                         type === 'warning' ? 'fas fa-exclamation-triangle' : 
                         'fas fa-info-circle';
            
            const alertClass = type === 'success' ? 'alert-success' : 
                              type === 'error' ? 'alert-error' : 
                              type === 'warning' ? 'alert-warning' : 
                              'alert-info';
            
            element.className = `alert ${alertClass}`;
            element.innerHTML = `
                <i class="${icon}"></i>
                <span>${message}</span>
            `;
            
            element.classList.remove('hidden');
            
            // إخفاء الرسالة بعد 5 ثواني
            setTimeout(() => {
                element.classList.add('hidden');
            }, 5000);
        }

        function saveToLocalStorage() {
            try {
                const data = {
                    students: Array.from(appState.students.entries()),
                    extractedData: appState.extractedData,
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
                    
                    // تحميل الطلاب
                    if (data.students && Array.isArray(data.students)) {
                        appState.students = new Map(data.students);
                    }
                    
                    // تحميل البيانات المستخرجة
                    if (data.extractedData) {
                        appState.extractedData = data.extractedData;
                    }
                }
            } catch (error) {
                console.error('خطأ في تحميل البيانات:', error);
            }
        }

        // تهيئة النظام
        initApp();

        // جعل الدوال متاحة عالمياً
        window.switchTab = switchTab;
        window.addStudent = addStudent;
        window.deleteStudent = deleteStudent;
        window.clearForm = clearForm;
        window.clearAllData = clearAllData;
        window.processExtractedData = processExtractedData;
        window.generatePDF = generatePDF;
        window.printReport = printReport;
        window.exportToExcel = exportToExcel;
        window.shareReport = shareReport;
        window.copyResults = copyResults;
    </script>

    <!-- مكتبات خارجية -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
</body>
</html>