# 님드라 안녕하세요 ㅋ Jade만듦 귀찮으니까 GPT가 설명함 Jake Jun a Korean Fried chicken Devour End
# 💎 Jade Protocol

Jade는 JSON을 더 간단하고 읽기 쉬운 형태로 표현하기 위한 데이터 포맷입니다.

복잡한 괄호와 따옴표를 줄이고, 사람이 직접 작성하고 관리하기 쉽게 설계되었습니다.

---

# Jade는 어디에 좋은가요?

## 1. 설정 파일

게임, 프로그램의 설정 데이터를 저장할 때 사용할 수 있습니다.

예:

- 게임 설정
- 사용자 설정
- 프로그램 옵션
- 서버 설정


## 2. 데이터 저장

구조가 있는 데이터를 저장할 때 사용할 수 있습니다.

예:

- 플레이어 데이터
- 아이템 목록
- 캐릭터 정보
- 프로젝트 데이터


## 3. JSON 대체 포맷

JSON과 같은 구조를 가지지만 더 짧고 읽기 쉬운 문법을 제공합니다.

JSON:

    {
        "name": "Alice",
        "level": 10
    }


Jade:

    name : Alice
    level : 10


---

# Jade 배우는 방법

Jade를 배우는 방법은 간단합니다.

## Step 1. 기본 값 배우기

가장 먼저 Key-Value 구조를 배웁니다.

형식:

    이름 : 값


예:

    name : Alice
    age : 13


---

## Step 2. 구조 만들기

여러 데이터를 묶으려면 객체를 사용합니다.

예:

    player :
        name : Alice
        hp : 100
    .


---

## Step 3. 목록 만들기

여러 데이터를 저장하려면 배열을 사용합니다.

예:

    items :
        - Sword
        - Shield
    .


---

# Jade 사용 방법

## 1. JSON → Jade 변환

JSON 데이터를 Jade 형식으로 변환합니다.

입력:

    {
        "name":"Jade",
        "version":1
    }


출력:

    name : Jade
    version : 1


---

## 2. Jade → JSON 변환

Jade 데이터를 JSON으로 다시 변환할 수 있습니다.

입력:

    player :
        name : Alice
        hp : 100
    .


출력:

    {
        "player":{
            "name":"Alice",
            "hp":100
        }
    }


---

# 예제 1: 게임 데이터

JSON:

    {
        "player":{
            "name":"Knight",
            "level":20,
            "items":[
                "Sword",
                "Shield"
            ]
        }
    }


Jade:

    player :
        name : Knight
        level : 20

        items :
            - Sword
            - Shield
        .
    .


---

# 예제 2: 프로그램 설정

JSON:

    {
        "window":{
            "width":800,
            "height":600,
            "fullscreen":false
        }
    }


Jade:

    window :
        width : 800
        height : 600
        <fullscreen>
    .


---

# 빠른 문법 정리

| 목적 | 문법 |
|---|---|
| 값 저장 | key : value |
| 객체 만들기 | key : ... . |
| 배열 만들기 | - value |
| True | <key>/ |
| False | <key> |
| Null | <key>; |
| 주석 | # 내용 |

---

# Jade를 배우는 순서

1. `key : value` 배우기
2. 객체 구조 배우기
3. 배열 배우기
4. 특수 값 배우기
5. 실제 데이터 만들어보기

Jade는 작은 데이터부터 시작해서 큰 프로젝트 데이터까지 확장할 수 있습니다.
# 코드:
      <!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Joux Converter - Retro Windows Edition</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Tahoma', 'Segoe UI', Geneva, Verdana, sans-serif;
            font-size: 12px;
        }

        body {
            background-color: #008080; /* 레트로 Windows Teal */
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        /* 윈도우 창 크기 대폭 확대 */
        .window {
            width: 92vw;
            max-width: 1200px;
            background-color: #c0c0c0;
            border: 2px solid;
            border-color: #ffffff #404040 #404040 #ffffff;
            box-shadow: 3px 3px 12px rgba(0, 0, 0, 0.6);
            padding: 3px;
        }

        .title-bar {
            background: linear-gradient(90deg, #000080, #1084d0);
            padding: 4px 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: white;
            font-weight: bold;
        }

        .title-bar-controls button {
            width: 16px;
            height: 14px;
            background: #c0c0c0;
            border: 1px solid;
            border-color: #ffffff #404040 #404040 #ffffff;
            font-size: 9px;
            line-height: 10px;
            padding: 0;
            cursor: pointer;
            font-weight: bold;
        }

        .window-body {
            padding: 16px;
        }

        .converter-grid {
            display: grid;
            grid-template-columns: 1fr auto 1fr;
            gap: 16px;
            align-items: center;
            margin-bottom: 16px;
        }

        .pane {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        label {
            font-weight: bold;
            color: #000;
        }

        /* 글상자 높이 확대 및 포커스 시 노르스름한 색 변경 */
        textarea {
            width: 100%;
            height: 480px;
            background-color: #ffffff;
            border: 2px solid;
            border-color: #404040 #ffffff #ffffff #404040;
            padding: 8px;
            font-family: 'Consolas', 'Courier New', monospace;
            font-size: 13px;
            line-height: 1.4;
            resize: vertical;
            outline: none;
            white-space: pre;
            overflow-x: auto;
            transition: background-color 0.15s ease;
        }

        /* 탭(Got Focus) 시 노르스름한 크림 옐로우 레트로 패스포트 색상 */
        textarea:focus {
            background-color: #ffffcc;
            border-color: #000080 #ffffff #ffffff #000080;
        }

        .arrow-divider {
            font-size: 28px;
            font-weight: bold;
            color: #404040;
            text-align: center;
        }

        .button-bar {
            display: flex;
            gap: 10px;
            justify-content: flex-start;
            border-top: 1px solid #808080;
            padding-top: 12px;
        }

        button.win-btn {
            background-color: #c0c0c0;
            border: 2px solid;
            border-color: #ffffff #404040 #404040 #ffffff;
            padding: 6px 20px;
            font-weight: bold;
            font-size: 12px;
            cursor: pointer;
            min-width: 90px;
            text-align: center;
        }

        button.win-btn:active {
            border-color: #404040 #ffffff #ffffff #404040;
            padding: 7px 19px 5px 21px;
        }

        .status-bar {
            margin-top: 12px;
            border: 2px solid;
            border-color: #808080 #ffffff #ffffff #808080;
            padding: 4px 8px;
            background: #c0c0c0;
            color: #222;
            font-size: 11px;
        }
    </style>
</head>
<body>

<div class="window">
    <div class="title-bar">
        <div class="title-bar-text">
            <span>💎 Joux Converter v1.1 (Jade Protocol Engine)</span>
        </div>
        <div class="title-bar-controls">
            <button>_</button>
            <button>□</button>
            <button>✕</button>
        </div>
    </div>

    <div class="window-body">
        <div class="converter-grid">
            <div class="pane">
                <label for="inputArea">Input (JSON / Jade)</label>
                <textarea id="inputArea" placeholder="JSON 또는 Jade 데이터를 입력하세요..."></textarea>
            </div>
            
            <div class="arrow-divider">→</div>

            <div class="pane">
                <label for="outputArea">Output (결과)</label>
                <textarea id="outputArea" readonly placeholder="변환 결과가 여기에 표시됩니다..."></textarea>
            </div>
        </div>

        <div class="button-bar">
            <button class="win-btn" onclick="convertData()">[Convert]</button>
            <button class="win-btn" onclick="copyOutput()">[Copy]</button>
            <button class="win-btn" onclick="saveOutput()">[Save]</button>
        </div>

        <div class="status-bar" id="statusBar">Ready.</div>
    </div>
</div>

<script>
    // --- 💎 Jade Protocol Parser Engine (버그 및 텍스트 분할 최적화) ---

    function isJSON(str) {
        try {
            const parsed = JSON.parse(str);
            return typeof parsed === 'object' && parsed !== null;
        } catch (e) {
            return false;
        }
    }

    function convertData() {
        const input = document.getElementById('inputArea').value.trim();
        const outputArea = document.getElementById('outputArea');
        const statusBar = document.getElementById('statusBar');

        if (!input) {
            statusBar.innerText = "Error: Input data is empty.";
            return;
        }

        try {
            if (isJSON(input)) {
                // JSON -> Jade 변환
                const jsonObj = JSON.parse(input);
                const jadeResult = jsonToJade(jsonObj);
                outputArea.value = jadeResult;
                statusBar.innerText = "Success: JSON → Jade Protocol Converted.";
            } else {
                // Jade -> JSON 변환
                const jsonObj = jadeToJson(input);
                outputArea.value = JSON.stringify(jsonObj, null, 4);
                statusBar.innerText = "Success: Jade Protocol → JSON Converted.";
            }
        } catch (err) {
            outputArea.value = "";
            statusBar.innerText = "Parsing Error: " + err.message;
        }
    }

    // 1. JSON ➔ Jade Engine (문장 분할 버그 최적화)
    function jsonToJade(obj, indent = "") {
        let result = [];

        for (const [key, val] of Object.entries(obj)) {
            if (val === null) {
                result.push(`${indent}<${key}>;`);
            } else if (typeof val === 'boolean') {
                result.push(val ? `${indent}<${key}>/` : `${indent}<${key}>`);
            } else if (Array.isArray(val)) {
                // 문장이 포함되거나 띄어쓰기가 있는 배열 요소는 하위 리스트로 100% 원본 보존
                const hasSentence = val.some(item => typeof item === 'string' && item.length > 0);
                
                if (hasSentence) {
                    result.push(`${indent}${key} :`);
                    val.forEach(item => {
                        result.push(`${indent}    - ${item}`);
                    });
                    result.push(`${indent}.`);
                } else {
                    result.push(`${indent}${key} : ${val.join(', ')}`);
                }
            } else if (typeof val === 'object') {
                result.push(`${indent}${key} :`);
                result.push(jsonToJade(val, indent + "    "));
                result.push(`${indent}.`);
            } else if (typeof val === 'string' && val.includes('\n')) {
                // 멀티라인 0 선언
                result.push(`${indent}${key} : 0`);
                val.split('\n').forEach(line => result.push(`${indent}    ${line}`));
                result.push(`${indent}.`);
            } else {
                result.push(`${indent}${key} : ${val}`);
            }
        }

        return result.join('\n');
    }

    // 2. Jade ➔ JSON Engine (완벽 복원 파서)
    function jadeToJson(jadeStr) {
        const lines = jadeStr.split('\n');
        let root = {};
        let stack = [{ obj: root, keyName: null, type: 'object' }];
        let i = 0;

        while (i < lines.length) {
            let rawLine = lines[i];
            let trimmed = rawLine.trim();

            if (!trimmed || trimmed.startsWith('#')) {
                i++;
                continue;
            }

            let currentContext = stack[stack.length - 1].obj;

            // 블록 마감 마침표 .
            if (trimmed === '.') {
                if (stack.length > 1) stack.pop();
                i++;
                continue;
            }

            // 하위 리스트 (- 아이템) 파싱 (문장 분할 버그 방지)
            if (trimmed.startsWith('- ')) {
                let itemValue = trimmed.replace(/^- /, '').trim();
                if (Array.isArray(currentContext)) {
                    currentContext.push(itemValue);
                }
                i++;
                continue;
            }

            // <값> 규격 (/ = true, ; = null)
            if (trimmed.startsWith('<')) {
                const match = trimmed.match(/^<([^>]+)>(.*)$/);
                if (match) {
                    const key = match[1];
                    const rest = match[2];

                    if (rest === '/') currentContext[key] = true;
                    else if (rest === ';') currentContext[key] = null;
                    else if (rest === '') currentContext[key] = false;
                    else currentContext[key] = rest.replace(/^"|"$/g, '');
                    i++;
                    continue;
                }
            }

            // 표 구조 (@)
            if (trimmed.startsWith('@')) {
                const tableMatch = trimmed.match(/^@(\w+)\s*:\s*(.+)$/);
                if (tableMatch) {
                    const tableName = tableMatch[1];
                    const cols = tableMatch[2].split(',').map(c => c.trim().replace('?', ''));
                    let list = [];
                    i++;

                    while (i < lines.length) {
                        let rowLine = lines[i].trim();
                        if (rowLine === '.') break;
                        if (rowLine && !rowLine.startsWith('#')) {
                            let vals = rowLine.split(',').map(v => v.trim());
                            let rowObj = {};
                            cols.forEach((col, idx) => {
                                let v = vals[idx] !== undefined ? vals[idx] : "";
                                if (v === '/') rowObj[col] = true;
                                else if (v === '' || v === '?') rowObj[col] = null;
                                else if (!isNaN(v) && v !== '') rowObj[col] = Number(v);
                                else rowObj[col] = v;
                            });
                            list.push(rowObj);
                        }
                        i++;
                    }
                    currentContext[tableName] = list;
                    i++;
                    continue;
                }
            }

            // 멀티라인 텍스트 (0)
            if (trimmed.includes(' : 0')) {
                const key = trimmed.split(' : 0')[0].trim();
                let textLines = [];
                i++;
                while (i < lines.length) {
                    if (lines[i].trim() === '.') break;
                    textLines.push(lines[i].trim());
                    i++;
                }
                currentContext[key] = textLines.join('\n');
                i++;
                continue;
            }

            // 일반 Key : Value 또는 객체/리스트 블록 시작
            if (trimmed.includes(':')) {
                let parts = trimmed.split(':');
                let key = parts[0].trim().replace('?', '');
                let val = parts.slice(1).join(':').trim();

                if (val === '') {
                    // 다음 라인을 살펴보고 배열인지 객체인지 판단
                    let nextLine = (i + 1 < lines.length) ? lines[i + 1].trim() : '';
                    if (nextLine.startsWith('- ')) {
                        let newArr = [];
                        currentContext[key] = newArr;
                        stack.push({ obj: newArr, type: 'array' });
                    } else {
                        let newObj = {};
                        currentContext[key] = newObj;
                        stack.push({ obj: newObj, type: 'object' });
                    }
                } else if (parts[0].includes('::')) {
                    let realKey = parts[0].split('::')[0].trim().replace('?', '');
                    if (val === 'true') currentContext[realKey] = true;
                    else if (val === 'false') currentContext[realKey] = false;
                    else currentContext[realKey] = Number(val);
                } else {
                    currentContext[key] = val;
                }
            }

            i++;
        }

        return root;
    }

    // 3. 버튼 툴킷
    function copyOutput() {
        const outputText = document.getElementById('outputArea').value;
        const statusBar = document.getElementById('statusBar');

        if (!outputText) {
            statusBar.innerText = "Nothing to copy!";
            return;
        }

        navigator.clipboard.writeText(outputText).then(() => {
            statusBar.innerText = "Copied to clipboard successfully!";
        });
    }

    function saveOutput() {
        const outputText = document.getElementById('outputArea').value;
        const statusBar = document.getElementById('statusBar');

        if (!outputText) {
            statusBar.innerText = "Nothing to save!";
            return;
        }

        const isResultJson = outputText.trim().startsWith('{') || outputText.trim().startsWith('[');
        const filename = isResultJson ? "converted_data.json" : "converted_data.jade";
        
        const blob = new Blob([outputText], { type: "text/plain;charset=utf-8" });
        const link = document.createElement("a");
        link.href = URL.createObjectURL(blob);
        link.download = filename;
        link.click();
        
        statusBar.innerText = `Saved file: ${filename}`;
    }
</script>

</body>
</html>      
