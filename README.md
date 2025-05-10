<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>حاسبة زاوية السقوط</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        .container {
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        h1 {
            color: #333;
            text-align: center;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        input {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
        }
        button:hover {
            background-color: #45a049;
        }
        #result {
            margin-top: 20px;
            padding: 10px;
            background-color: #e9f7ef;
            border-radius: 4px;
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>حاسبة زاوية السقوط</h1>
        
        <div class="form-group">
            <label for="n1">معامل الانكسار للوسط الأول (n₁):</label>
            <input type="number" id="n1" step="0.01" min="1" value="1.0">
        </div>
        
        <div class="form-group">
            <label for="n2">معامل الانكسار للوسط الثاني (n₂):</label>
            <input type="number" id="n2" step="0.01" min="1" value="1.33">
        </div>
        
        <div class="form-group">
            <label for="angle2">زاوية الانكسار بالدرجات (θ₂):</label>
            <input type="number" id="angle2" min="0" max="90" value="30">
        </div>
        
        <button onclick="calculateAngle()">حساب زاوية السقوط</button>
        
        <div id="result"></div>
    </div>

    <script>
        function calculateAngle() {
            // الحصول على القيم من المدخلات
            const n1 = parseFloat(document.getElementById('n1').value);
            const n2 = parseFloat(document.getElementById('n2').value);
            const angle2 = parseFloat(document.getElementById('angle2').value);
            
            // التحقق من القيم المدخلة
            if (isNaN(n1) || isNaN(n2) || isNaN(angle2)) {
                alert("الرجاء إدخال قيم صحيحة");
                return;
            }
            
            if (angle2 < 0 || angle2 > 90) {
                alert("زاوية الانكسار يجب أن تكون بين 0 و 90 درجة");
                return;
            }
            
            // تحويل زاوية الانكسار من درجات إلى راديان
            const angle2Rad = angle2 * Math.PI / 180;
            
            // حساب زاوية السقوط باستخدام قانون سنيل
            const sinAngle1 = (n2 * Math.sin(angle2Rad)) / n1;
            
            // التحقق من وجود حل (إذا كانت sinAngle1 > 1 فهذا يعني انكسار كلي)
            if (sinAngle1 > 1) {
                document.getElementById('result').innerHTML = `
                    <p><strong>النتيجة:</strong> لا يوجد حل، يحدث انكسار كلي لأن n₂/n₁ * sin(θ₂) = ${sinAngle1.toFixed(4)} > 1</p>
                `;
                document.getElementById('result').style.display = 'block';
                return;
            }
            
            const angle1Rad = Math.asin(sinAngle1);
            const angle1 = angle1Rad * 180 / Math.PI;
            
            // عرض النتيجة
            document.getElementById('result').innerHTML = `
                <p><strong>زاوية السقوط (θ₁):</strong> ${angle1.toFixed(2)} درجة</p>
                <p><strong>حسب قانون سنيل:</strong> n₁ * sin(θ₁) = n₂ * sin(θ₂)</p>
                <p>${n1.toFixed(2)} * sin(${angle1.toFixed(2)}°) = ${n2.toFixed(2)} * sin(${angle2.toFixed(2)}°)</p>
            `;
            document.getElementById('result').style.display = 'block';
        }
    </script>
</body>
</html>
