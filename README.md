<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام استخراج وتحليل نتائج الطلاب المتكامل</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* أنماط عامة */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #f0f2f5 0%, #f0f8ff 100%);
            padding: 20px;
            margin: 0;
            min-height: 100vh;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
        }
        .main-title {
            text-align: center;
            color: #1a5c9e;
            margin-bottom: 30px;
            font-size: 2.5rem;
        }
        .app-description {
            text-align: center;
            color: #666;
            margin-bottom: 40px;
            font-size: 1.1rem;
            line-height: 1.6;
        }
        
        /* تبويبات النظام */
        .tabs {
            display: flex;
            margin-bottom: 15px;
            border-bottom: 2px solid #ddd;
            overflow-x: auto;
        }
        .tab {
            padding: 12px 20px;
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
        }
        .tab.active {
            background: #1a5c9e;
            color: white;
        }
        .tab-content {
            display: none;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        .tab-content.active {
            display: block;
        }
        
        /* بطاقة استخراج النصوص */
        .card { 
            background: white; 
            width: 100%; 
            max-width: 800px; 
            padding: 30px; 
            border-radius: 16px; 
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15); 
            margin: 20px auto;
        }
        h2 { 
            color: #2c3e50; 
            text-align: center; 
            margin-bottom: 10px;
            font-size: 28px;
        }
        .subtitle {
            text-align: center;
            color: #7f8c8d;
            margin-bottom: 30px;
            font-size: 16px;
            line-height: 1.5;
        }
        .config-section {
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
            padding: 20px;
            border-radius: 12px;
            border: 1px solid #dee2e6;
            margin-bottom: 25px;
        }
        .section-title {
            color: #2c3e50;
            margin-top: 0;
            margin-bottom: 20px;
            font-size: 18px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .api-config {
            display: grid;
            grid-template-columns: 1fr auto;
            gap: 15px;
            margin-bottom: 15px;
        }
        .api-input {
            padding: 12px 15px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            font-family: 'Courier New', monospace;
        }
        .btn-save {
            background: linear-gradient(to right, #27ae60, #219653);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
        }
        .api-status {
            padding: 10px;
            border-radius: 6px;
            font-size: 14px;
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
            margin: 20px 0;
        }
        .model-select {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            background: white;
            cursor: pointer;
        }
        .model-info {
            margin-top: 10px;
            padding: 10px;
            background: #e3f2fd;
            border-radius: 6px;
            color: #1565c0;
            font-size: 14px;
            border: 1px solid #90caf9;
        }
        
        /* رفع الملفات */
        .upload-area { 
            border: 3px dashed #3498db; 
            padding: 40px 20px; 
            text-align: center; 
            cursor: pointer; 
            border-radius: 12px; 
            background: linear-gradient(135deg, #f8fbff 0%, #e6f2ff 100%); 
            margin-bottom: 25px;
            transition: all 0.3s;
        }
        .upload-area:hover {
            background: linear-gradient(135deg, #e6f2ff 0%, #d4e6ff 100%);
            border-color: #2980b9;
        }
        .upload-area.dragover {
            background: linear-gradient(135deg, #d4edda 0%, #c3e6cb 100%);
            border-color: #27ae60;
        }
        .btn-extract { 
            background: linear-gradient(to right, #e74c3c, #c0392b); 
            color: white; 
            border: none; 
            padding: 16px; 
            border-radius: 10px; 
            width: 100%; 
            cursor: pointer; 
            font-size: 18px;
            font-weight: bold;
            margin-top: 10px;
        }
        #result { 
            background: #f8f9fa; 
            padding: 25px; 
            border-radius: 12px; 
            min-height: 250px; 
            border: 1px solid #dee2e6; 
            margin-top: 25px;
        }
        .progress-bar {
            height: 5px;
            background: #3498db;
            border-radius: 3px;
            margin-top: 10px;
            width: 0%;
            transition: width 0.3s;
        }
        
        /* إدخال البيانات */
        .input-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 15px;
            margin-bottom: 15px;
        }
        .input-group {
            display: flex;
            flex-direction: column;
        }
        .input-group label {
            margin-bottom: 8px;
            font-weight: bold;
            color: #555;
        }
        .input-group input, .input-group select {
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
            background: white;
            transition: border-color 0.3s;
            width: 100%;
        }
        .input-group input:focus, .input-group select:focus {
            border-color: #1a5c9e;
            outline: none;
            box-shadow: 0 0 0 2px rgba(26, 92, 158, 0.1);
        }
        
        /* الأزرار */
        .actions {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 20px;
        }
        button {
            background: #25d366;
            color: #fff;
            border: none;
            padding: 14px 24px;
            font-size: 1rem;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            flex: 1;
            min-width: 140px;
            transition: all 0.3s;
        }
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
        }
        button:active {
            transform: translateY(0);
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
        
        /* الجداول */
        .students-table-container {
            overflow-x: auto;
            margin-top: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        .students-table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            min-width: 600px;
        }
        .students-table th {
            background: #1a5c9e;
            color: white;
            padding: 14px;
            text-align: center;
            font-weight: bold;
        }
        .students-table td {
            padding: 12px;
            text-align: center;
            border-bottom: 1px solid #eee;
        }
        .students-table tr:hover {
            background: #f8f9fa;
        }
        .delete-btn {
            background: #dc3545;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.9rem;
            display: flex;
            align-items: center;
            gap: 5px;
            margin: 0 auto;
        }
        .delete-btn:hover {
            background: #c82333;
        }
        
        /* بطاقات التحليل */
        .summary-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }
        .summary-card {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            text-align: center;
            transition: transform 0.3s;
        }
        .summary-card:hover {
            transform: translateY(-3px);
        }
        
        /* الرسوم البيانية */
        .charts-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 20px;
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
        .chart-box h3 {
            margin: 0 0 15px 0;
            color: #1a5c9e;
            font-size: 1.1rem;
            text-align: center;
        }
        .chart-box canvas {
            flex: 1;
            width: 100% !important;
            height: 100% !important;
        }
        
        /* الرسائل */
        .alert {
            padding: 12px 15px;
            border-radius: 8px;
            margin: 10px 0;
            display: flex;
            align-items: center;
            gap: 10px;
            animation: slideIn 0.3s ease;
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
        
        /* المستويات */
        .level-badge {
            color: #fff;
            font-weight: bold;
            padding: 5px 10px;
            border-radius: 4px;
            display: inline-block;
            min-width: 70px;
        }
        .level-excellent { background: #4caf50; }
        .level-verygood { background: #009688; }
        .level-good { background: #2196f3; }
        .level-pass { background: #ff9800; }
        .level-weak { background: #f44336; }
    </style>
</head>
<body>
    <div class="container">
        <h1 class="main-title">🚀 نظام استخراج وتحليل نتائج الطلاب المتكامل</h1>
        <p class="app-description">
            نظام متكامل لاستخراج النصوص من ملفات PDF والصور باستخدام جميع نماذج Gemini، ثم تحليل النتائج تلقائياً
        </p>
        
        <!-- تبويبات النظام -->
        <div class="tabs">
            <div class="tab active" onclick="switchTab('extract')">
                <i class="fas fa-file-import"></i>
                <span>استخراج النصوص</span>
            </div>
            <div class="tab" onclick="switchTab('input')">
                <i class="fas fa-user-plus"></i>
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
            <div class="card">
                <h2><i class="fas fa-file-alt"></i> مستخرج النصوص الذكي</h2>
                <p class="subtitle">استخرج النصوص من ملفات PDF والصور باستخدام جميع نماذج Gemini المتاحة</p>
                
                <div class="config-section">
                    <div class="section-title">
                        <i class="fas fa-key"></i>
                        <span>إعدادات Google Gemini API</span>
                    </div>
                    
                    <div class="api-config">
                        <input type="password" id="apiKeyInput" class="api-input" placeholder="أدخل مفتاح Google Gemini API هنا..." value="">
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
                    <div style="font-size: 48px; color: #3498db; margin-bottom: 15px;">
                        <i class="fas fa-cloud-upload-alt"></i>
                    </div>
                    <div class="upload-text" id="fileLabel">اسحب ملف PDF أو صورة هنا أو انقر للاختيار</div>
                    <div class="upload-info" id="fileInfo">الحد الأقصى: 10MB | المدعوم: PDF, JPG, PNG, GIF, BMP</div>
                    <input type="file" id="fileInput" accept="application/pdf,image/*" style="display:none">
                </div>
                
                <div class="progress-bar" id="progressBar"></div>
                
                <button id="btnExtract" class="btn-extract" disabled>
                    <i class="fas fa-magic"></i> استخراج وتحليل تلقائي
                </button>
                
                <div id="result">
                    <div style="text-align: center; color: #7f8c8d; padding: 40px;">
                        <i class="fas fa-file-alt" style="font-size: 48px; margin-bottom: 15px; color: #bdc3c7;"></i>
                        <h3 style="color: #95a5a6;">النتائج ستظهر هنا</h3>
                        <p style="margin-top: 10px;">بعد استخراج النصوص، سيتم معالجتها وتحليلها تلقائياً.</p>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- تبويب إدارة البيانات -->
        <div id="input-tab" class="tab-content">
            <div class="card">
                <h2><i class="fas fa-user-plus"></i> إدارة البيانات المستخرجة</h2>
                <p class="subtitle">عرض وتعديل البيانات التي تم استخراجها تلقائياً</p>
                
                <div id="alert-message" class="alert hidden"></div>
                
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
                    <h3 style="margin-top: 30px;"><i class="fas fa-database"></i> البيانات المستخرجة</h3>
                    <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin-top: 15px;">
                        <pre id="rawDataPreview" style="white-space: pre-wrap; font-family: monospace; max-height: 300px; overflow-y: auto; direction: ltr;"></pre>
                    </div>
                </div>
                
                <h3 style="margin-top: 30px;"><i class="fas fa-users"></i> الطلاب المستخرجون</h3>
                <div class="students-table-container">
                    <table class="students-table" id="studentsList">
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
                <p class="subtitle">تحليل إحصائي متقدم للبيانات المستخرجة</p>
                
                <div id="analysis-alert" class="alert warning">
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
                
                <div style="margin-top: 30px; background: white; padding: 20px; border-radius: 10px; box-shadow: 0 2px 10px rgba(0,0,0,0.1);">
                    <h3 style="color: #1a5c9e; margin-bottom: 15px;">تفاصيل النتائج حسب المستوى</h3>
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
                <p class="subtitle">تقرير شامل للبيانات المستخرجة والتحليل الإحصائي</p>
                
                <div id="report-alert" class="alert warning">
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
                </div>
                
                <div id="reportContent" style="margin-top: 30px; background: white; padding: 30px; border-radius: 10px; box-shadow: 0 2px 15px rgba(0,0,0,0.1);">
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
            classes: [],
            extractedData: '',
            availableModels: [],
            selectedModel: '',
            fileType: ''
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
            
            // عناصر إدارة البيانات
            alertMessage: document.getElementById('alert-message'),
            extractedDataSection: document.getElementById('extractedDataSection'),
            rawDataPreview: document.getElementById('rawDataPreview'),
            studentsTableBody: document.getElementById('studentsTableBody'),
            processDataBtn: document.getElementById('processDataBtn'),
            
            // عناصر التحليل
            analysisAlert: document.getElementById('analysis-alert'),
            summaryCards: document.getElementById('summaryCards'),
            levelDetailsTable: document.getElementById('levelDetailsTable'),
            
            // عناصر التقرير
            reportAlert: document.getElementById('report-alert'),
            reportContent: document.getElementById('reportContent'),
            pdfBtn: document.getElementById('pdfBtn')
        };

        // تهيئة التطبيق
        function initApp() {
            console.log("جاري تهيئة التطبيق...");
            
            // تهيئة إعدادات API
            if (appState.API_KEY) {
                elements.apiKeyInput.value = "••••••••" + appState.API_KEY.slice(-4);
                updateApiStatus(true);
                loadAvailableModels();
            }
            
            // تهيئة قائمة الصفوف
            initializeClasses();
            
            // تحميل البيانات المحفوظة
            loadFromLocalStorage();
            
            // إعداد معالجات الأحداث
            setupEventHandlers();
            
            console.log("تم تهيئة التطبيق بنجاح");
        }

        // إعداد معالجات الأحداث
        function setupEventHandlers() {
            // استخراج النصوص
            elements.saveApiBtn.addEventListener('click', saveApiKey);
            elements.modelSelect.addEventListener('change', function() {
                appState.selectedModel = this.value;
                updateModelInfo();
            });
            elements.dropZone.addEventListener('click', () => elements.fileInput.click());
            elements.fileInput.addEventListener('change', handleFileSelect);
            elements.btnExtract.addEventListener('click', extractAndAnalyze);
            
            // إدخال البيانات
            elements.processDataBtn.addEventListener('click', processExtractedData);
            
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
                showAlert('تم مسح مفتاح API بنجاح', 'success');
                return;
            }
            
            if (!inputKey.startsWith('AIza')) {
                showAlert('يبدو أن مفتاح API غير صحيح. يجب أن يبدأ المفتاح بـ "AIza"', 'error');
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
                    model.name.includes('gemini') || 
                    model.name.includes('models/gemini')
                );
                
                // تحديث قائمة النماذج
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
            
            // فرز النماذج حسب الأفضلية
            const sortedModels = models.sort((a, b) => {
                // إعطاء الأولوية للنماذج الأحدث
                if (a.name.includes('1.5') && !b.name.includes('1.5')) return -1;
                if (!a.name.includes('1.5') && b.name.includes('1.5')) return 1;
                
                // ثم النماذج التي تدعم الصور
                if (a.name.includes('vision') && !b.name.includes('vision')) return -1;
                if (!a.name.includes('vision') && b.name.includes('vision')) return 1;
                
                // ثم النماذج العامة
                if (a.name.includes('pro') && !b.name.includes('pro')) return -1;
                if (!a.name.includes('pro') && b.name.includes('pro')) return 1;
                
                return 0;
            });
            
            sortedModels.forEach(model => {
                const modelName = model.name.split('/').pop();
                const option = document.createElement('option');
                option.value = model.name;
                
                // تسمية النماذج بشكل مفهوم
                let displayName = modelName;
                if (modelName.includes('gemini-1.5-flash')) displayName = 'Gemini 1.5 Flash (الأسرع)';
                else if (modelName.includes('gemini-1.5-pro')) displayName = 'Gemini 1.5 Pro (الأقوى)';
                else if (modelName.includes('gemini-pro-vision')) displayName = 'Gemini Pro Vision (للصور)';
                else if (modelName.includes('gemini-pro')) displayName = 'Gemini Pro (عام)';
                else if (modelName.includes('gemini-ultra')) displayName = 'Gemini Ultra (المتقدم)';
                
                option.textContent = displayName;
                elements.modelSelect.appendChild(option);
            });
            
            // تحديد النموذج الافتراضي
            const defaultModel = sortedModels.find(m => m.name.includes('gemini-1.5-flash')) ||
                               sortedModels.find(m => m.name.includes('gemini-pro')) ||
                               (sortedModels.length > 0 ? sortedModels[0] : null);
            
            if (defaultModel) {
                elements.modelSelect.value = defaultModel.name;
                appState.selectedModel = defaultModel.name;
                updateModelInfo();
            }
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
                    m.name.includes('1.5') ||
                    m.supportedGenerationMethods?.includes('generateContent')
                );
            } else if (file.type === 'application/pdf') {
                // البحث عن نموذج قوي للنصوص
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
            
            // بدء المعالجة
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
                        
                        // بناء النص التوضيحي المناسب لاستخراج نتائج الطلاب
                        const promptText = buildExtractionPrompt(file.type);
                        
                        elements.progressBar.style.width = '80%';
                        
                        const result = await model.generateContent([
                            promptText,
                            { inlineData: { data: base64Data, mimeType: file.type } }
                        ]);
                        
                        const response = await result.response;
                        const extractedText = response.text();
                        
                        // حفظ البيانات المستخرجة
                        appState.extractedData = extractedText;
                        
                        // عرض البيانات المستخرجة
                        displayExtractedResults(extractedText);
                        
                        elements.progressBar.style.width = '100%';
                        
                        // معالجة البيانات تلقائياً
                        const processedData = await processExtractedDataAutomatically(extractedText);
                        
                        if (processedData.length > 0) {
                            showAlert(`تم استخراج ${processedData.length} طالب بنجاح!`, 'success');
                            switchTab('input');
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
            let prompt = `أنا أريد استخراج نتائج الطلاب من هذا الملف. 
            
المطلوب استخراج البيانات التالية بدقة:
1. أسماء الطلاب الكاملة
2. الدرجات (من 40 أو النسبة المئوية)
3. المواد الدراسية
4. الفصول أو الأقسام

تعليمات مهمة:
- إذا كانت الدرجات بنسبة مئوية، حولها إلى درجة من 40 (الدرجة = (النسبة × 40) ÷ 100)
- إذا لم يتم ذكر المادة، استخدم "عام" كقيمة افتراضية
- إذا لم يتم ذكر الفصل، استخدم "غير محدد"
- تأكد من استخراج جميع الأرقام بدقة
- استخرج حتى الأسماء العربية بكاملها
- تأكد من مطابقة الأسماء مع درجاتها الصحيحة

الشكل المطلوب للبيانات:
اسم الطالب | المادة | الفصل | الدرجة/40

مثال:
أحمد محمد | الرياضيات | 2/أ | 35
سارة علي | اللغة العربية | 2/ب | 28
محمد حسن | العلوم | 2/ج | 32`;

            if (fileType.startsWith('image/')) {
                prompt += `

ملاحظة: هذا ملف صورة، لذا ركز على قراءة النصوص بوضوح والتعرف على الجداول بدقة.`;
            } else if (fileType === 'application/pdf') {
                prompt += `

ملاحظة: هذا ملف PDF، استخرج البيانات من جميع الصفحات وتأكد من الحفاظ على ترتيب الجداول.`;
            }
            
            return prompt;
        }

        async function processExtractedDataAutomatically(extractedText) {
            try {
                console.log("معالجة البيانات المستخرجة تلقائياً...");
                
                const lines = extractedText.split('\n');
                const students = [];
                
                for (const line of lines) {
                    const studentData = parseStudentLine(line);
                    if (studentData) {
                        students.push(studentData);
                    }
                }
                
                // إذا لم نجد بيانات، حاول البحث بأنماط مختلفة
                if (students.length === 0) {
                    const altStudents = alternativeParsing(extractedText);
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
                }
                
                return students;
                
            } catch (error) {
                console.error("خطأ في المعالجة التلقائية:", error);
                return [];
            }
        }

        function parseStudentLine(line) {
            // تنظيف السطر
            const cleanLine = line.trim();
            if (!cleanLine || cleanLine.length < 3) return null;
            
            // أنماط مختلفة للتحليل
            const patterns = [
                // النمط: اسم | مادة | فصل | درجة
                /([^\|]+)\s*\|\s*([^\|]+)\s*\|\s*([^\|]+)\s*\|\s*([\d\.]+)/,
                // النمط: اسم - مادة - فصل - درجة
                /([^\-]+)\s*\-\s*([^\-]+)\s*\-\s*([^\-]+)\s*\-\s*([\d\.]+)/,
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
                    
                    // تحويل النسبة المئوية إلى درجة من 40
                    if (score > 40 && score <= 100) {
                        score = (score * 40) / 100;
                    }
                    
                    // التأكد من صحة الدرجة
                    if (score >= 0 && score <= 40 && name.length > 1) {
                        return {
                            id: Date.now() + Math.random(),
                            name: cleanArabicText(name),
                            subject: cleanArabicText(subject) || "عام",
                            className: cleanArabicText(className) || "غير محدد",
                            score: parseFloat(score.toFixed(1)),
                            level: getLevel(score)
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
                                students.push({
                                    id: Date.now() + Math.random(),
                                    name: cleanArabicText(name),
                                    subject: "عام",
                                    className: "غير محدد",
                                    score: parseFloat(score.toFixed(1)),
                                    level: getLevel(score)
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
            // إزالة الرموز والمسافات الزائدة
            return text.replace(/[^\u0600-\u06FF\s]/g, '')
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
                        <button onclick="retryWithDifferentModel()" style="background: #3498db; color: white; border: none; padding: 10px 20px; border-radius: 6px; cursor: pointer; margin: 5px;">
                            <i class="fas fa-sync-alt"></i> المحاولة بنموذج مختلف
                        </button>
                        <button onclick="loadAvailableModels()" style="background: #6c757d; color: white; border: none; padding: 10px 20px; border-radius: 6px; cursor: pointer; margin: 5px;">
                            <i class="fas fa-redo"></i> تحديث النماذج
                        </button>
                    </div>
                </div>
            `;
        }

        function retryWithDifferentModel() {
            // اختيار نموذج مختلف عشوائياً
            if (appState.availableModels.length > 1) {
                const currentIndex = appState.availableModels.findIndex(m => m.name === appState.selectedModel);
                const nextIndex = (currentIndex + 1) % appState.availableModels.length;
                const newModel = appState.availableModels[nextIndex];
                
                elements.modelSelect.value = newModel.name;
                appState.selectedModel = newModel.name;
                updateModelInfo();
                
                showAlert(`تم التغيير إلى النموذج: ${newModel.name.split('/').pop()}`, 'info');
                
                // إعادة المحاولة تلقائياً
                setTimeout(() => {
                    if (elements.fileInput.files[0]) {
                        extractAndAnalyze();
                    }
                }, 1000);
            }
        }

        function displayExtractedResults(text) {
            elements.result.innerHTML = `
                <div style="background: #f8f9fa; padding: 15px; border-radius: 8px; margin-bottom: 20px; border-right: 4px solid #3498db;">
                    <h3 style="color: #2c3e50; margin-top: 0;">
                        <i class="fas fa-check-circle" style="color: #27ae60;"></i>
                        تم استخراج البيانات بنجاح
                    </h3>
                    <p style="color: #7f8c8d; margin: 0;">
                        ${text.length} حرف مستخرج. جاري معالجة البيانات تلقائياً...
                    </p>
                </div>
                <div style="background: white; padding: 20px; border-radius: 8px; border: 1px solid #e9ecef; max-height: 300px; overflow-y: auto;">
                    <pre style="white-space: pre-wrap; font-family: 'Courier New', monospace; line-height: 1.5; direction: ltr; text-align: left; font-size: 14px;">${text}</pre>
                </div>
            `;
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
                    
                    // تحديث عرض البيانات
                    elements.extractedDataSection.classList.remove('hidden');
                    elements.rawDataPreview.textContent = appState.extractedData;
                    
                    // الانتقال إلى تحليل النتائج
                    setTimeout(() => switchTab('analysis'), 500);
                } else {
                    showAlert('لم يتم العثور على بيانات طلاب في النص المستخرج. يرجى مراجعة تنسيق البيانات.', 'warning');
                    
                    // عرض البيانات للتحرير اليدوي
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
                        <textarea id="manualEdit" style="width: 100%; height: 300px; padding: 15px; border: 1px solid #ddd; border-radius: 8px; font-family: 'Courier New', monospace; direction: ltr;">${appState.extractedData}</textarea>
                        <div style="margin-top: 20px; text-align: center;">
                            <button onclick="processManualEdit()" style="background: #3498db; color: white; border: none; padding: 12px 24px; border-radius: 6px; cursor: pointer; margin: 5px;">
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

        // إدارة البيانات
        function initializeClasses() {
            appState.classes = ['2/أ', '2/ب', '2/ج', '2/د', '2/هـ', '2/و', 'غير محدد'];
        }

        function getLevel(score) {
            if (score >= 36) return {name: 'ممتاز', class: 'excellent'};
            if (score >= 32) return {name: 'جيد جدًا', class: 'verygood'};
            if (score >= 28) return {name: 'جيد', class: 'good'};
            if (score >= 20) return {name: 'مقبول', class: 'pass'};
            return {name: 'ضعيف', class: 'weak'};
        }

        function updateStudentsTable() {
            const tbody = document.getElementById('studentsTableBody');
            
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
                html += `
                    <tr>
                        <td>${index + 1}</td>
                        <td>${student.name}</td>
                        <td>${student.subject}</td>
                        <td>${student.className}</td>
                        <td><strong>${student.score}</strong></td>
                        <td><span class="level-badge level-${student.level.class}">${student.level.name}</span></td>
                        <td><button class="delete-btn" onclick="deleteStudent('${student.id}')">
                            <i class="fas fa-trash"></i> حذف
                        </button></td>
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
                updateStudentsTable();
                updateAnalysis();
                elements.extractedDataSection.classList.add('hidden');
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

        // تحليل البيانات
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
                    <div style="font-size: 2rem; font-weight: bold; margin: 10px 0; color: #333;">${totalStudents}</div>
                    <div style="font-size: 0.9rem; color: #666;">طالب مستخرج</div>
                </div>
                <div class="summary-card">
                    <h3><i class="fas fa-chart-line"></i> متوسط الدرجات</h3>
                    <div style="font-size: 2rem; font-weight: bold; margin: 10px 0; color: #333;">${avgScore.toFixed(1)}</div>
                    <div style="font-size: 0.9rem; color: #666;">من 40</div>
                </div>
                <div class="summary-card">
                    <h3><i class="fas fa-percentage"></i> نسبة النجاح</h3>
                    <div style="font-size: 2rem; font-weight: bold; margin: 10px 0; color: #333;">${passRate}%</div>
                    <div style="font-size: 0.9rem; color: #666;">${passedStudents} طالب</div>
                </div>
                <div class="summary-card">
                    <h3><i class="fas fa-trophy"></i> أعلى مستوى</h3>
                    <div style="font-size: 2rem; font-weight: bold; margin: 10px 0; color: #333;">${highestLevel}</div>
                    <div style="font-size: 0.9rem; color: #666;">${highestCount} طالب</div>
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
                            backgroundColor: ['#4caf50', '#009688', '#2196f3', '#ff9800', '#f44336']
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
                new Chart(subjectCtx, {
                    type: 'bar',
                    data: {
                        labels: subjectLabels,
                        datasets: [{
                            label: 'متوسط الدرجة',
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
                <div style="display: grid; grid-template-columns: 1fr 1fr 1fr; border-bottom: 1px solid #eee; font-weight: bold; background: #f8f9fa;">
                    <div style="padding: 12px; text-align: center;">عدد الطلاب</div>
                    <div style="padding: 12px; text-align: center;">نطاق الدرجات</div>
                    <div style="padding: 12px; text-align: center;">المستوى</div>
                </div>
            `;
            
            const levels = ['ممتاز', 'جيد جدًا', 'جيد', 'مقبول', 'ضعيف'];
            
            levels.forEach(level => {
                const count = levelCounts[level] || 0;
                const percentage = appState.students.length > 0 ? ((count / appState.students.length) * 100).toFixed(1) : '0';
                tableHTML += `
                    <div style="display: grid; grid-template-columns: 1fr 1fr 1fr; border-bottom: 1px solid #eee;">
                        <div style="padding: 12px; text-align: center;"><strong>${count}</strong> <small>(${percentage}%)</small></div>
                        <div style="padding: 12px; text-align: center;">${levelRanges[level]}</div>
                        <div style="padding: 12px; text-align: center;">
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
            }, 500);
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
            
            const levelCounts = {
                'ممتاز': 0, 'جيد جدًا': 0, 'جيد': 0, 'مقبول': 0, 'ضعيف': 0
            };
            
            appState.students.forEach(student => {
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
                <div style="text-align: center; margin-bottom: 30px;">
                    <h1 style="color: #1a5c9e; margin-bottom: 10px;">📊 تقرير نتائج الطلاب المستخرجة</h1>
                    <p style="color: #666; margin-bottom: 5px;">التقرير تم إنشاؤه تلقائياً من البيانات المستخرجة</p>
                    <p style="color: #888; font-size: 0.9rem;">${dateStr}</p>
                </div>
                
                <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-bottom: 30px;">
                    <div style="background: linear-gradient(135deg, #e3f2fd 0%, #bbdefb 100%); padding: 20px; border-radius: 10px; text-align: center; border: 2px solid #1a5c9e;">
                        <h3 style="color: #1a5c9e; margin-bottom: 10px;"><i class="fas fa-users"></i> إجمالي الطلاب</h3>
                        <div style="font-size: 2.5rem; font-weight: bold; color: #0d47a1;">${totalStudents}</div>
                    </div>
                    <div style="background: linear-gradient(135deg, #e8f5e9 0%, #c8e6c9 100%); padding: 20px; border-radius: 10px; text-align: center; border: 2px solid #4caf50;">
                        <h3 style="color: #2e7d32; margin-bottom: 10px;"><i class="fas fa-chart-line"></i> متوسط الدرجات</h3>
                        <div style="font-size: 2.5rem; font-weight: bold; color: #1b5e20;">${avgScore.toFixed(1)}</div>
                    </div>
                    <div style="background: linear-gradient(135deg, #fff3e0 0%, #ffe0b2 100%); padding: 20px; border-radius: 10px; text-align: center; border: 2px solid #ff9800;">
                        <h3 style="color: #ef6c00; margin-bottom: 10px;"><i class="fas fa-percentage"></i> نسبة النجاح</h3>
                        <div style="font-size: 2.5rem; font-weight: bold; color: #e65100;">${passRate}%</div>
                    </div>
                </div>
                
                <div style="background: white; padding: 25px; border-radius: 10px; margin-bottom: 30px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
                    <h2 style="color: #1a5c9e; margin-bottom: 20px; border-bottom: 3px solid #1a5c9e; padding-bottom: 10px;">
                        <i class="fas fa-chart-pie"></i> توزيع الطلاب حسب المستوى
                    </h2>
                    <div style="height: 350px; display: flex; justify-content: center; align-items: center;">
                        <div style="width: 400px; height: 350px;">
                            <canvas id="reportLevelChart"></canvas>
                        </div>
                    </div>
                </div>
                
                <div style="background: white; padding: 25px; border-radius: 10px; margin-bottom: 30px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
                    <h2 style="color: #1a5c9e; margin-bottom: 20px; border-bottom: 3px solid #1a5c9e; padding-bottom: 10px;">
                        <i class="fas fa-list-ol"></i> تفاصيل النتائج
                    </h2>
                    <div style="overflow-x: auto;">
                        <table style="width: 100%; border-collapse: collapse; border: 1px solid #ddd;">
                            <thead>
                                <tr style="background: linear-gradient(135deg, #1a5c9e 0%, #0d47a1 100%); color: white;">
                                    <th style="padding: 15px; text-align: center; border: 1px solid #0d47a1;">#</th>
                                    <th style="padding: 15px; text-align: center; border: 1px solid #0d47a1;">اسم الطالب</th>
                                    <th style="padding: 15px; text-align: center; border: 1px solid #0d47a1;">المادة</th>
                                    <th style="padding: 15px; text-align: center; border: 1px solid #0d47a1;">الفصل</th>
                                    <th style="padding: 15px; text-align: center; border: 1px solid #0d47a1;">الدرجة</th>
                                    <th style="padding: 15px; text-align: center; border: 1px solid #0d47a1;">المستوى</th>
                                </tr>
                            </thead>
                            <tbody>
            `;
            
            appState.students.forEach((student, index) => {
                const rowColor = index % 2 === 0 ? '#f8f9fa' : '#ffffff';
                reportHTML += `
                    <tr style="background: ${rowColor};">
                        <td style="padding: 12px; text-align: center; border: 1px solid #eee; font-weight: bold;">${index + 1}</td>
                        <td style="padding: 12px; text-align: center; border: 1px solid #eee;">${student.name}</td>
                        <td style="padding: 12px; text-align: center; border: 1px solid #eee;">${student.subject}</td>
                        <td style="padding: 12px; text-align: center; border: 1px solid #eee;">${student.className}</td>
                        <td style="padding: 12px; text-align: center; border: 1px solid #eee; font-weight: bold; color: #1a5c9e;">${student.score}</td>
                        <td style="padding: 12px; text-align: center; border: 1px solid #eee;">
                            <span style="color: #fff; padding: 6px 12px; border-radius: 4px; background: ${getLevelColor(student.level.name)}; display: inline-block; min-width: 80px; font-weight: bold;">
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
                
                <div style="background: linear-gradient(135deg, #f5f5f5 0%, #eeeeee 100%); padding: 20px; border-radius: 10px; text-align: center; margin-top: 30px; border: 1px solid #ddd;">
                    <p style="color: #666; margin-bottom: 10px;">
                        <i class="fas fa-robot"></i> تم إنشاء هذا التقرير تلقائياً بواسطة نظام استخراج وتحليل نتائج الطلاب المتكامل
                    </p>
                    <p style="color: #888; font-size: 0.9rem;">
                        <i class="fas fa-clock"></i> ${now.toLocaleString('ar-SA')}
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
                                backgroundColor: ['#4caf50', '#009688', '#2196f3', '#ff9800', '#f44336'],
                                borderWidth: 2,
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
                                        font: { size: 14 }
                                    }
                                }
                            }
                        }
                    });
                } catch (error) {
                    console.error("خطأ في رسم الرسم البياني للتقرير:", error);
                }
            }, 100);
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
            if (appState.students.length === 0) {
                showAlert('لا توجد بيانات للطباعة', 'error');
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
                        <title>تقرير النتائج</title>
                        <style>
                            body { font-family: Arial, sans-serif; margin: 20px; direction: rtl; }
                            h1 { color: #1a5c9e; text-align: center; }
                            table { width: 100%; border-collapse: collapse; margin: 20px 0; }
                            th { background: #1a5c9e; color: white; padding: 10px; }
                            td { padding: 8px; border: 1px solid #ddd; text-align: center; }
                            .summary { display: flex; justify-content: space-around; margin: 20px 0; }
                            .summary-item { text-align: center; padding: 15px; }
                            .footer { text-align: center; margin-top: 30px; color: #666; }
                        </style>
                    </head>
                    <body>
                        ${document.getElementById('reportContent').innerHTML}
                    </body>
                    </html>
                `);
                printWindow.document.close();
                printWindow.print();
            }, 500);
        }

        function exportToExcel() {
            if (appState.students.length === 0) {
                showAlert('لا توجد بيانات للتصدير', 'error');
                return;
            }
            
            // إنشاء بيانات CSV
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            
            // العنوان
            csvContent += "تقرير نتائج الطلاب المستخرجة\r\n";
            csvContent += `تاريخ الإنشاء: ${new Date().toLocaleString('ar-SA')}\r\n\r\n`;
            
            // رأس الجدول
            csvContent += "م,اسم الطالب,المادة,الفصل,الدرجة,المستوى\r\n";
            
            // البيانات
            appState.students.forEach((student, index) => {
                csvContent += `${index + 1},${student.name},${student.subject},${student.className},${student.score},${student.level.name}\r\n`;
            });
            
            // الإحصائيات
            csvContent += "\r\nالإحصائيات:\r\n";
            const totalStudents = appState.students.length;
            const totalScore = appState.students.reduce((sum, student) => sum + student.score, 0);
            const avgScore = totalScore / totalStudents;
            const passedStudents = appState.students.filter(student => student.score >= 20).length;
            const passRate = (passedStudents / totalStudents * 100).toFixed(1);
            
            csvContent += `إجمالي الطلاب,${totalStudents}\r\n`;
            csvContent += `متوسط الدرجات,${avgScore.toFixed(1)}\r\n`;
            csvContent += `نسبة النجاح,${passRate}%\r\n`;
            
            // إنشاء رابط التحميل
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", "نتائج_الطلاب_المستخرجة.csv");
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showAlert('تم تصدير البيانات إلى ملف Excel', 'success');
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
                updateAnalysis();
            }
            
            if (tabName === 'report') {
                updateReportContent();
            }
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
                              type === 'warning' ? 'warning' : 
                              'info';
            
            const alertDiv = document.createElement('div');
            alertDiv.className = `alert ${alertClass}`;
            alertDiv.innerHTML = `
                <i class="${icon}"></i>
                <span>${message}</span>
            `;
            
            // إزالة أي رسائل سابقة
            const existingAlerts = document.querySelectorAll('.alert');
            existingAlerts.forEach(alert => {
                if (alert.parentNode) {
                    setTimeout(() => alert.remove(), 100);
                }
            });
            
            // إضافة الرسالة الجديدة
            document.querySelector('.container').insertBefore(alertDiv, document.querySelector('.tabs'));
            
            // إزالة الرسالة بعد 5 ثواني
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
                    
                    updateStudentsTable();
                    updateAnalysis();
                    
                    // عرض البيانات المستخرجة إذا كانت موجودة
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
        window.retryWithDifferentModel = retryWithDifferentModel;
        window.loadAvailableModels = loadAvailableModels;
        window.generatePDF = generatePDF;
        window.printReport = printReport;
        window.exportToExcel = exportToExcel;
    </script>
</body>
</html>