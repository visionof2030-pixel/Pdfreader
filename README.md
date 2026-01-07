<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>مستخرج النصوص الذكي | إصلاح الخطأ 404</title>
    <style>
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            background: linear-gradient(135deg, #f0f2f5 0%, #e6f7ff 100%);
            display: flex; 
            justify-content: center; 
            align-items: center;
            min-height: 100vh;
            padding: 20px;
            margin: 0;
        }
        .card { 
            background: white; 
            width: 100%; 
            max-width: 700px; 
            padding: 30px; 
            border-radius: 16px; 
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.12); 
        }
        h2 { 
            color: #007bff; 
            text-align: center; 
            margin-bottom: 10px;
            font-size: 28px;
        }
        .subtitle {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
            font-size: 16px;
        }
        .api-section {
            background: #f8fbff;
            padding: 20px;
            border-radius: 10px;
            border: 1px solid #d1e9ff;
            margin-bottom: 25px;
        }
        .api-section h3 {
            color: #007bff;
            margin-top: 0;
            font-size: 18px;
        }
        .api-input-group {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }
        .api-input {
            flex: 1;
            padding: 12px 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            font-family: monospace;
        }
        .api-btn {
            background: #28a745;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            white-space: nowrap;
        }
        .api-btn:hover {
            background: #218838;
        }
        .api-status {
            margin-top: 10px;
            font-size: 14px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .api-valid {
            color: #28a745;
            font-weight: bold;
        }
        .api-invalid {
            color: #dc3545;
        }
        .model-selector {
            margin-top: 15px;
            padding: 15px;
            background: #fff8e1;
            border-radius: 8px;
            border: 1px solid #ffd54f;
        }
        .model-selector label {
            display: block;
            margin-bottom: 10px;
            color: #333;
            font-weight: bold;
        }
        .model-select {
            width: 100%;
            padding: 10px;
            border-radius: 8px;
            border: 1px solid #ddd;
            font-size: 16px;
            background: white;
        }
        .model-info {
            margin-top: 10px;
            font-size: 14px;
            color: #666;
            padding: 8px;
            background: #f9f9f9;
            border-radius: 5px;
        }
        .upload-area { 
            border: 2px dashed #007bff; 
            padding: 35px 20px; 
            text-align: center; 
            cursor: pointer; 
            border-radius: 12px; 
            background: #f8fbff; 
            margin-bottom: 25px;
            transition: all 0.3s;
        }
        .upload-area:hover {
            background: #e6f2ff;
            border-color: #0056b3;
        }
        .upload-area.dragover {
            background: #d4edda;
            border-color: #28a745;
        }
        #fileLabel { 
            font-size: 18px;
            color: #333;
            display: block;
            margin-bottom: 10px;
        }
        .file-info {
            color: #666;
            font-size: 14px;
            margin-top: 10px;
        }
        #result { 
            white-space: pre-wrap; 
            background: #f8f9fa; 
            padding: 20px; 
            border-radius: 10px; 
            min-height: 200px; 
            border: 1px solid #dee2e6; 
            margin-top: 25px;
            font-size: 16px;
            line-height: 1.6;
            max-height: 400px;
            overflow-y: auto;
        }
        .btn-primary { 
            background: linear-gradient(to right, #007bff, #0056b3); 
            color: white; 
            border: none; 
            padding: 15px; 
            border-radius: 10px; 
            width: 100%; 
            cursor: pointer; 
            font-size: 18px;
            font-weight: bold;
            transition: all 0.3s;
            margin-top: 10px;
        }
        .btn-primary:hover:not(:disabled) {
            background: linear-gradient(to right, #0069d9, #004085);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 123, 255, 0.3);
        }
        .btn-primary:disabled { 
            background: #aaa; 
            cursor: not-allowed;
            transform: none;
        }
        .action-buttons {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }
        .btn-secondary {
            background: #6c757d;
            color: white;
            border: none;
            padding: 12px;
            border-radius: 8px;
            cursor: pointer;
            flex: 1;
        }
        .btn-secondary:hover {
            background: #5a6268;
        }
        .footer {
            text-align: center;
            margin-top: 25px;
            color: #888;
            font-size: 14px;
            border-top: 1px solid #eee;
            padding-top: 15px;
        }
        .hidden {
            display: none !important;
        }
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(0, 123, 255, 0.3);
            border-radius: 50%;
            border-top-color: #007bff;
            animation: spin 1s ease-in-out infinite;
        }
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        .char-count {
            text-align: left;
            font-size: 14px;
            color: #666;
            margin-top: 5px;
        }
        .test-connection-btn {
            background: #17a2b8;
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 6px;
            cursor: pointer;
            margin-top: 10px;
            font-size: 14px;
        }
        .test-connection-btn:hover {
            background: #138496;
        }
        .models-list {
            max-height: 150px;
            overflow-y: auto;
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 10px;
            margin-top: 10px;
            background: #f9f9f9;
            font-size: 12px;
            display: none;
        }
        .models-list.show {
            display: block;
        }
    </style>
</head>
<body>

<div class="card">
    <h2>مستخرج النصوص الذكي ✨</h2>
    <p class="subtitle">استخرج النصوص من ملفات PDF والصور بدقة عالية باستخدام الذكاء الاصطناعي</p>
    
    <div class="api-section">
        <h3>🔑 إعداد مفتاح API</h3>
        <p>للاستخدام، تحتاج إلى مفتاح Google Gemini API. يمكنك الحصول عليه من <a href="https://makersuite.google.com/app/apikey" target="_blank" style="color: #007bff;">هذا الرابط</a>.</p>
        
        <div class="api-input-group">
            <input type="password" id="apiKeyInput" class="api-input" placeholder="أدخل مفتاح API الخاص بك هنا..." value="">
            <button id="saveApiBtn" class="api-btn">حفظ المفتاح</button>
        </div>
        
        <div id="apiStatus" class="api-status">
            <span id="apiStatusText">❌ لم يتم إضافة مفتاح API بعد</span>
        </div>
        
        <button id="testConnectionBtn" class="test-connection-btn hidden">اختبار الاتصال وعرض النماذج المتاحة</button>
        
        <div id="modelsList" class="models-list"></div>
        
        <div class="model-selector" id="modelSelector" style="display: none;">
            <label for="modelSelect">اختر النموذج المناسب:</label>
            <select id="modelSelect" class="model-select">
                <option value="gemini-1.5-flash">gemini-1.5-flash (الأسرع والأقل تكلفة)</option>
                <option value="gemini-1.5-pro">gemini-1.5-pro (الأكثر دقة)</option>
                <option value="gemini-1.0-pro">gemini-1.0-pro (الإصدار السابق)</option>
                <option value="gemini-pro">gemini-pro (اسم بديل)</option>
                <option value="gemini-pro-vision">gemini-pro-vision (للصور والنصوص)</option>
            </select>
            <div class="model-info" id="modelInfo">النموذج المختار: gemini-1.5-flash - مناسب لاستخراج النصوص من الصور وملفات PDF</div>
        </div>
        
        <div id="apiKeyDisplay" class="hidden">
            <p style="margin-top: 10px; font-size: 14px; color: #666;">
                المفتاح محفوظ في متصفحك. لإزالته، اترك الحقل فارغًا واضغط على "حفظ المفتاح".
            </p>
        </div>
    </div>
    
    <div class="upload-area" id="dropZone">
        <span id="fileLabel">📁 اسحب ملف الـ PDF أو الصورة هنا أو اضغط للاختيار</span>
        <input type="file" id="fileInput" accept="application/pdf,image/*" style="display:none">
        <div class="file-info" id="fileInfo">الحد الأقصى لحجم الملف: 10MB. الأنواع المدعومة: PDF, JPG, PNG, GIF</div>
    </div>
    
    <button id="btn" class="btn-primary" disabled>بدء الاستخراج</button>
    
    <div class="action-buttons">
        <button id="copyBtn" class="btn-secondary" disabled>نسخ النص</button>
        <button id="clearBtn" class="btn-secondary">مسح النتائج</button>
    </div>
    
    <div class="char-count" id="charCount"></div>
    
    <div id="result">📝 النتائج المستخرجة ستظهر هنا...</div>
    
    <div class="footer">
        مستخرج النصوص الذكي | يعمل على نموذج Gemini | v2.1
    </div>
</div>

<script type="module">
    // استخدام إصدار محدد من المكتبة لضمان التوافق
    import { GoogleGenerativeAI } from "https://esm.run/@google/generative-ai@0.1.0";

    // عناصر واجهة المستخدم
    const btn = document.getElementById('btn');
    const resultDiv = document.getElementById('result');
    const fileInput = document.getElementById('fileInput');
    const dropZone = document.getElementById('dropZone');
    const fileLabel = document.getElementById('fileLabel');
    const apiKeyInput = document.getElementById('apiKeyInput');
    const saveApiBtn = document.getElementById('saveApiBtn');
    const apiStatusText = document.getElementById('apiStatusText');
    const apiKeyDisplay = document.getElementById('apiKeyDisplay');
    const copyBtn = document.getElementById('copyBtn');
    const clearBtn = document.getElementById('clearBtn');
    const charCount = document.getElementById('charCount');
    const fileInfo = document.getElementById('fileInfo');
    const modelSelector = document.getElementById('modelSelector');
    const modelSelect = document.getElementById('modelSelect');
    const modelInfo = document.getElementById('modelInfo');
    const testConnectionBtn = document.getElementById('testConnectionBtn');
    const modelsList = document.getElementById('modelsList');

    // النماذج المتاحة مع معلومات عنها
    const availableModels = [
        { id: "gemini-1.5-flash", name: "Gemini 1.5 Flash", desc: "الأسرع والأقل تكلفة، مناسب لاستخراج النصوص" },
        { id: "gemini-1.5-pro", name: "Gemini 1.5 Pro", desc: "متوازن بين السرعة والدقة" },
        { id: "gemini-1.0-pro", name: "Gemini 1.0 Pro", desc: "الإصدار السابق، موثوق" },
        { id: "gemini-pro", name: "Gemini Pro", desc: "اسم بديل لـ Gemini 1.0 Pro" },
        { id: "gemini-pro-vision", name: "Gemini Pro Vision", desc: "مخصص للصور والفيديو" }
    ];

    // تحميل مفتاح API المحفوظ من الذاكرة المحلية
    let API_KEY = localStorage.getItem('gemini_api_key') || '';
    let SELECTED_MODEL = localStorage.getItem('selected_model') || 'gemini-1.5-flash';
    
    // تهيئة القيم
    if (API_KEY) {
        apiKeyInput.value = "••••••••" + API_KEY.slice(-4);
        modelSelect.value = SELECTED_MODEL;
        updateModelInfo();
        updateApiStatus(true);
        modelSelector.style.display = 'block';
        testConnectionBtn.classList.remove('hidden');
    } else {
        updateApiStatus(false);
    }

    // تحديث معلومات النموذج
    function updateModelInfo() {
        const model = availableModels.find(m => m.id === modelSelect.value);
        if (model) {
            modelInfo.textContent = `النموذج المختار: ${model.name} - ${model.desc}`;
        }
    }

    // تحديث حالة مفتاح API
    function updateApiStatus(isValid) {
        if (isValid && API_KEY) {
            apiStatusText.innerHTML = '<span class="api-valid">✅ مفتاح API صالح ومحفوظ</span>';
            apiKeyDisplay.classList.remove('hidden');
            btn.disabled = false;
            modelSelector.style.display = 'block';
            testConnectionBtn.classList.remove('hidden');
        } else {
            apiStatusText.innerHTML = '<span class="api-invalid">❌ يرجى إضافة مفتاح API صالح للاستمرار</span>';
            apiKeyDisplay.classList.add('hidden');
            btn.disabled = true;
            modelSelector.style.display = 'none';
            testConnectionBtn.classList.add('hidden');
        }
    }

    // حفظ مفتاح API
    saveApiBtn.addEventListener('click', () => {
        const inputKey = apiKeyInput.value.trim();
        
        // إذا كان الحقل يحتوي على نقاط (مفتاح مخفي) أو فارغ
        if (inputKey.includes('••••')) {
            // نحتفظ بالمفتاح الحالي
            updateApiStatus(true);
            return;
        }
        
        if (inputKey === '') {
            // مسح المفتاح
            localStorage.removeItem('gemini_api_key');
            API_KEY = '';
            apiKeyInput.value = '';
            updateApiStatus(false);
            alert('تم مسح مفتاح API بنجاح');
            return;
        }
        
        // التحقق من شكل مفتاح API (يبدأ عادة بـ AIza)
        if (!inputKey.startsWith('AIza')) {
            alert('يبدو أن مفتاح API غير صحيح. يجب أن يبدأ المفتاح بـ "AIza"');
            return;
        }
        
        // حفظ المفتاح الجديد
        API_KEY = inputKey;
        localStorage.setItem('gemini_api_key', API_KEY);
        apiKeyInput.value = "••••••••" + API_KEY.slice(-4);
        updateApiStatus(true);
        
        // إظهار اختيار النموذج بعد حفظ المفتاح
        modelSelector.style.display = 'block';
        testConnectionBtn.classList.remove('hidden');
        
        alert('تم حفظ مفتاح API بنجاح! يمكنك الآن استخدام التطبيق.');
    });

    // حفظ النموذج المختار
    modelSelect.addEventListener('change', () => {
        SELECTED_MODEL = modelSelect.value;
        localStorage.setItem('selected_model', SELECTED_MODEL);
        updateModelInfo();
    });

    // اختبار الاتصال وعرض النماذج المتاحة
    testConnectionBtn.addEventListener('click', async () => {
        if (!API_KEY) {
            alert("يرجى إضافة مفتاح API أولاً");
            return;
        }
        
        testConnectionBtn.innerHTML = '<span class="loading"></span> جاري الاختبار...';
        testConnectionBtn.disabled = true;
        
        try {
            const genAI = new GoogleGenerativeAI(API_KEY);
            
            // محاولة الحصول على قائمة النماذج
            const result = await fetch(`https://generativelanguage.googleapis.com/v1/models?key=${API_KEY}`);
            
            if (!result.ok) {
                throw new Error(`خطأ في الاتصال: ${result.status} ${result.statusText}`);
            }
            
            const data = await result.json();
            
            // عرض النماذج المتاحة
            let modelsHtml = '<h4>النماذج المتاحة في حسابك:</h4>';
            
            if (data.models && data.models.length > 0) {
                data.models.forEach(model => {
                    modelsHtml += `<div style="margin: 5px 0; padding: 5px; border-bottom: 1px solid #eee;">
                        <strong>${model.name}</strong><br>
                        <small>يدعم generateContent: ${model.supportedGenerationMethods?.includes('generateContent') ? 'نعم' : 'لا'}</small>
                    </div>`;
                });
                
                modelsList.innerHTML = modelsHtml;
                modelsList.classList.add('show');
                
                // البحث عن النماذج المنصوح بها
                const recommendedModels = data.models.filter(m => 
                    m.name.includes('gemini') && 
                    m.supportedGenerationMethods?.includes('generateContent')
                );
                
                if (recommendedModels.length > 0) {
                    // تحديث قائمة النماذج المتاحة
                    const modelNames = recommendedModels.map(m => m.name.split('/').pop());
                    
                    // تحديث select بالنماذج المتاحة فعلياً
                    modelSelect.innerHTML = '';
                    
                    modelNames.forEach(modelName => {
                        const model = availableModels.find(m => m.id === modelName) || 
                                     { id: modelName, name: modelName, desc: "نموذج متاح في حسابك" };
                        
                        const option = document.createElement('option');
                        option.value = model.id;
                        option.textContent = `${model.name} - ${model.desc}`;
                        modelSelect.appendChild(option);
                    });
                    
                    // اختيار أول نموذج إذا لم يكن المختار موجوداً
                    if (!modelNames.includes(SELECTED_MODEL) && modelNames.length > 0) {
                        SELECTED_MODEL = modelNames[0];
                        modelSelect.value = SELECTED_MODEL;
                        localStorage.setItem('selected_model', SELECTED_MODEL);
                        updateModelInfo();
                    }
                    
                    alert(`✅ الاتصال ناجح! تم العثور على ${recommendedModels.length} نموذج متاح.`);
                } else {
                    alert('⚠️ لم يتم العثور على نماذج Gemini تدعم generateContent في حسابك.');
                }
            } else {
                alert('⚠️ لا توجد نماذج متاحة في حسابك.');
            }
            
        } catch (error) {
            console.error("Connection test error:", error);
            alert(`❌ فشل اختبار الاتصال: ${error.message}`);
            
            // استخدام النماذج الافتراضية في حالة الفشل
            modelSelect.innerHTML = '';
            availableModels.forEach(model => {
                const option = document.createElement('option');
                option.value = model.id;
                option.textContent = `${model.name} - ${model.desc}`;
                modelSelect.appendChild(option);
            });
            modelSelect.value = SELECTED_MODEL;
            updateModelInfo();
            
        } finally {
            testConnectionBtn.innerHTML = 'اختبار الاتصال وعرض النماذج المتاحة';
            testConnectionBtn.disabled = false;
        }
    });

    // عرض/إخفاء مفتاح API عند النقر
    apiKeyInput.addEventListener('focus', () => {
        if (API_KEY && apiKeyInput.value.includes('••••')) {
            apiKeyInput.value = API_KEY;
        }
    });
    
    apiKeyInput.addEventListener('blur', () => {
        if (API_KEY && !apiKeyInput.value.includes('••••')) {
            apiKeyInput.value = "••••••••" + API_KEY.slice(-4);
        }
    });

    // التعامل مع تحميل الملفات
    dropZone.addEventListener('click', () => fileInput.click());
    
    fileInput.addEventListener('change', () => {
        if (fileInput.files[0]) {
            const file = fileInput.files[0];
            const fileSize = (file.size / 1024 / 1024).toFixed(2); // بالـ MB
            fileLabel.innerHTML = `📄 ${file.name}`;
            fileInfo.innerHTML = `حجم الملف: ${fileSize} MB | النوع: ${file.type}`;
        }
    });

    // سحب وإفلات الملفات
    dropZone.addEventListener('dragover', (e) => {
        e.preventDefault();
        dropZone.classList.add('dragover');
    });

    dropZone.addEventListener('dragleave', () => {
        dropZone.classList.remove('dragover');
    });

    dropZone.addEventListener('drop', (e) => {
        e.preventDefault();
        dropZone.classList.remove('dragover');
        
        if (e.dataTransfer.files.length) {
            fileInput.files = e.dataTransfer.files;
            const file = fileInput.files[0];
            const fileSize = (file.size / 1024 / 1024).toFixed(2);
            fileLabel.innerHTML = `📄 ${file.name}`;
            fileInfo.innerHTML = `حجم الملف: ${fileSize} MB | النوع: ${file.type}`;
        }
    });

    // نسخ النص
    copyBtn.addEventListener('click', () => {
        const textToCopy = resultDiv.innerText;
        navigator.clipboard.writeText(textToCopy).then(() => {
            const originalText = copyBtn.textContent;
            copyBtn.textContent = '✓ تم النسخ!';
            setTimeout(() => {
                copyBtn.textContent = originalText;
            }, 2000);
        });
    });

    // مسح النتائج
    clearBtn.addEventListener('click', () => {
        resultDiv.textContent = '📝 النتائج المستخرجة ستظهر هنا...';
        charCount.textContent = '';
        copyBtn.disabled = true;
        fileInput.value = '';
        fileLabel.innerHTML = '📁 اسحب ملف الـ PDF أو الصورة هنا أو اضغط للاختيار';
        fileInfo.innerHTML = 'الحد الأقصى لحجم الملف: 10MB. الأنواع المدعومة: PDF, JPG, PNG, GIF';
        modelsList.classList.remove('show');
    });

    // زر الاستخراج الرئيسي
    btn.addEventListener('click', async () => {
        if (!API_KEY) {
            alert("يرجى إضافة مفتاح API أولاً");
            return;
        }
        
        if (!fileInput.files[0]) {
            alert("الرجاء اختيار ملف أولاً");
            return;
        }
        
        const file = fileInput.files[0];
        const maxSize = 10 * 1024 * 1024; // 10MB
        
        if (file.size > maxSize) {
            alert("حجم الملف كبير جداً. الحد الأقصى هو 10MB");
            return;
        }
        
        btn.disabled = true;
        btn.innerHTML = '<span class="loading"></span> جاري المعالجة...';
        resultDiv.innerText = "⌛ جاري معالجة الملف... قد يستغرق الأمر بضع ثوانٍ حسب حجم الملف وتعقيده.";
        copyBtn.disabled = true;
        
        try {
            const genAI = new GoogleGenerativeAI(API_KEY);
            
            // استخدام النموذج المختار من قبل المستخدم
            const modelName = SELECTED_MODEL;
            console.log(`محاولة استخدام النموذج: ${modelName}`);
            
            const model = genAI.getGenerativeModel({ model: modelName });
            
            const reader = new FileReader();
            
            reader.onloadend = async () => {
                const base64Data = reader.result.split(',')[1];
                
                const prompt = `اقرأ الملف المرفق واستخرج النص منه بالكامل وبدقة عالية جداً. حافظ على تنسيق الفقرات والنقاط والجداول إن وجدت. لا تضيف أي تعليقات إضافية، فقط استخرج النص كما هو.`;
                
                try {
                    const result = await model.generateContent([
                        prompt,
                        { inlineData: { data: base64Data, mimeType: file.type } }
                    ]);
                    
                    const response = await result.response;
                    const extractedText = response.text();
                    
                    resultDiv.innerText = extractedText;
                    
                    // تحديث عدد الأحرف
                    const textLength = extractedText.length;
                    charCount.textContent = `عدد الأحرف: ${textLength}`;
                    
                    // تمكين زر النسخ
                    copyBtn.disabled = false;
                    
                    // إخفاء قائمة النماذج
                    modelsList.classList.remove('show');
                    
                } catch (apiError) {
                    console.error("API Error:", apiError);
                    
                    // رسائل خطأ أكثر وضوحاً
                    if (apiError.message.includes("API key not valid") || apiError.message.includes("403")) {
                        resultDiv.innerText = "❌ مفتاح API غير صالح أو غير مصرح. يرجى التأكد من المفتاح وإعادة محاولة.";
                        updateApiStatus(false);
                    } else if (apiError.message.includes("404") || apiError.message.includes("not found")) {
                        resultDiv.innerText = `❌ النموذج "${modelName}" غير متوفر. يرجى اختيار نموذج آخر من القائمة أو استخدام زر "اختبار الاتصال" للعثور على النماذج المتاحة.`;
                        testConnectionBtn.click();
                    } else if (apiError.message.includes("quota")) {
                        resultDiv.innerText = "❌ تم تجاوز حصة الاستخدام لمفتاح API هذا. يرجى التحقق من حسابك أو استخدام مفتاح آخر.";
                    } else {
                        resultDiv.innerText = "❌ حدث خطأ أثناء معالجة الملف: " + apiError.message;
                    }
                }
                
                btn.disabled = false;
                btn.innerHTML = 'بدء الاستخراج';
            };
            
            reader.onerror = () => {
                resultDiv.innerText = "❌ حدث خطأ أثناء قراءة الملف. يرجى المحاولة مرة أخرى.";
                btn.disabled = false;
                btn.innerHTML = 'بدء الاستخراج';
            };
            
            reader.readAsDataURL(file);
            
        } catch (e) {
            console.error("General Error:", e);
            resultDiv.innerText = "❌ حدث خطأ غير متوقع: " + e.message;
            btn.disabled = false;
            btn.innerHTML = 'بدء الاستخراج';
        }
    });
    
    // تحديث حالة زر النسخ بناءً على وجود نص
    resultDiv.addEventListener('input', () => {
        const hasText = resultDiv.textContent.trim() !== '' && 
                       resultDiv.textContent !== '📝 النتائج المستخرجة ستظهر هنا...' &&
                       resultDiv.textContent !== '⌛ جاري معالجة الملف... قد يستغرق الأمر بضع ثوانٍ حسب حجم الملف وتعقيده.';
        
        copyBtn.disabled = !hasText;
    });
    
    // تحديث معلومات النموذج عند التحميل
    updateModelInfo();
</script>
</body>
</html>
