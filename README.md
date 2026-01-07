<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0, user-scalable=yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="mobile-web-app-capable" content="yes">
    <title>نظام استخراج وتحليل نتائج الطلاب المتكامل</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* إعادة تعيين وتصحيحات للأجهزة المختلفة */
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
            font-size: 16px;
            height: 100%;
            -webkit-overflow-scrolling: touch;
        }

        @media (max-width: 480px) {
            html { font-size: 14px; }
        }
        @media (min-width: 481px) and (max-width: 768px) {
            html { font-size: 15px; }
        }
        @media (min-width: 769px) and (max-width: 1024px) {
            html { font-size: 16px; }
        }
        @media (min-width: 1025px) {
            html { font-size: 17px; }
        }

        /* تحسينات خاصة للأجهزة */
        @supports (-webkit-touch-callout: none) {
            /* خاص للأجهزة التي تدعم WebKit مثل iOS */
            body {
                -webkit-font-smoothing: antialiased;
                -moz-osx-font-smoothing: grayscale;
            }
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Helvetica Neue', Arial, sans-serif;
            background: linear-gradient(135deg, #f0f2f5 0%, #e6f7ff 100%);
            min-height: 100vh;
            padding: 15px;
            margin: 0;
            line-height: 1.5;
            overflow-x: hidden;
            position: relative;
        }

        /* تحسينات للهواتف */
        @media (max-width: 480px) {
            body {
                padding: 10px;
                padding-bottom: 80px; /* مساحة للأزرار في الأسفل */
            }
        }

        /* حاوية رئيسية */
        .container {
            max-width: 1200px;
            margin: 0 auto;
            width: 100%;
        }

        /* العنوان الرئيسي */
        .main-title {
            text-align: center;
            color: #1a5c9e;
            margin-bottom: 20px;
            font-size: clamp(1.5rem, 4vw, 2.5rem);
            font-weight: 700;
            padding: 0 10px;
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .app-description {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
            font-size: clamp(0.9rem, 2vw, 1.1rem);
            line-height: 1.6;
            padding: 0 10px;
        }

        /* تبويبات النظام - متجاوبة */
        .tabs {
            display: flex;
            margin-bottom: 15px;
            border-bottom: 2px solid #ddd;
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
            scrollbar-width: none;
            position: sticky;
            top: 0;
            background: linear-gradient(135deg, #f0f2f5 0%, #e6f7ff 100%);
            z-index: 100;
            padding: 5px 0;
        }

        .tabs::-webkit-scrollbar {
            display: none;
        }

        .tab {
            padding: 14px 18px;
            cursor: pointer;
            font-weight: 600;
            border-radius: 12px 12px 0 0;
            background: #e9ecef;
            margin-left: 8px;
            white-space: nowrap;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            min-height: 50px;
            transition: all 0.3s ease;
            border: 2px solid transparent;
            font-size: clamp(0.85rem, 2vw, 1rem);
            min-width: 100px;
        }

        @media (max-width: 480px) {
            .tab {
                padding: 12px 14px;
                min-width: 90px;
                min-height: 45px;
            }
        }

        .tab i {
            font-size: 1.1rem;
        }

        .tab.active {
            background: linear-gradient(135deg, #1a5c9e 0%, #0d47a1 100%);
            color: white;
            border-color: #1a5c9e;
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(26, 92, 158, 0.3);
        }

        .tab:hover:not(.active) {
            background: #d0d7e0;
            transform: translateY(-1px);
        }

        .tab-content {
            display: none;
            background: white;
            padding: clamp(15px, 3vw, 25px);
            border-radius: 16px;
            box-shadow: 0 8px 30px rgba(0,0,0,0.12);
            margin-bottom: 25px;
            animation: fadeIn 0.4s ease;
            position: relative;
            min-height: 300px;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .tab-content.active {
            display: block;
        }

        /* بطاقة استخراج النصوص */
        .card {
            background: white;
            width: 100%;
            padding: clamp(20px, 4vw, 30px);
            border-radius: 20px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.15);
            margin: 0 auto 25px;
            position: relative;
            overflow: hidden;
            border: 1px solid rgba(0,0,0,0.08);
        }

        .card::before {
            content: '';
            position: absolute;
            top: 0;
            right: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, #1a5c9e, #27ae60, #e74c3c);
        }

        h2 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 15px;
            font-size: clamp(1.4rem, 3vw, 1.8rem);
            font-weight: 700;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            flex-wrap: wrap;
        }

        .subtitle {
            text-align: center;
            color: #7f8c8d;
            margin-bottom: 25