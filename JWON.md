# JWON (JWON Object Notation) v4.1
### 님들아 이건 엄청남 ㅋ 이거 JSON 저장해야하는데 용량 없으면 좋음 ㅋ 71븐 동안 만듬 ㅋ(메모장 같은 환경에서 좋음 )

> CUP(Core Ultra Parser) 및 AI 컨텍스트 최적화를 위해 설계된 초경량 ASCII 데이터 직렬화 포맷

## 1. JWON을 만든 이유 (Motivation)

JSON은 범용성과 가독성이 뛰어나지만, AI 토큰 절약, IoT 통신, 게임 세이브 데이터 등 **기계 간(Machine-to-Machine) 초고속/초경량 전송**이 필요한 환경에서는 다음과 같은 한계가 있습니다.

- **키(Key)의 중복**: `"name"`, `"health"` 같은 키가 수천 번 반복되며 불필요한 바이트를 소모합니다.
- **구조 오버헤드**: `{`, `}`, `[`, `]`, `:`, `,` 등 구조 문자가 전체 용량의 상당 부분을 차지합니다.
- **파싱 비용**: 문자열 기반의 키 매칭과 중첩 구조 해석에 CPU 사이클이 낭비됩니다.
- **토큰 비효율**: LLM 컨텍스트 윈도우에서 JSON은 동일한 정보를 더 많은 토큰으로 인코딩합니다.

JWON은 **"사람이 읽기 편함"을 과감히 포기**하고, 다음 목표를 달성하기 위해 설계되었습니다.

| 목표 | 설명 |
|------|------|
| 압축률 | ASCII 기준 JSON 대비 약 50% 용량 절감 |
| 복원성 | 100% 손실 없는 JSON 역변환 보장 |
| 파싱 속도 | O(n) 단일 패스 재귀 커서 파서 |
| 충돌 방지 | 모든 토큰을 1-byte ASCII 특수문자로 제한 |
| 유니코드 | 한글, 이모지, 다국어 100% 지원 |

## 2. 핵심 설계 원리 (Design Principles)

### 2.1 Pure ASCII Special Characters Only

가장 중요한 설계 결정입니다. 알파벳(a-z, A-Z)이나 숫자(0-9)를 토큰으로 사용하면 **사용자 입력 데이터와 토큰이 충돌**하여 복원이 불가능해지는 치명적 문제가 발생합니다.

JWON v4.1은 모든 구조/값/키 토큰을 **키보드 특수문자**로만 구성하여 이 문제를 원천 차단합니다.

### 2.2 Explicit Boundary Tokens

v2.x에서는 닫는 기호 없이 무한히 이어지는 구조였으나, 중첩 객체와 복잡한 문자열에서 **파싱 모호성(Ambiguity)** 이 발생했습니다. v4.1부터는 명시적인 시작/종료 토큰을 도입하여 100% 결정론적 파싱을 보장합니다.

### 2.3 Single-Pass Recursive Cursor Parser

`split()`, `replace()`, 정규식 기반 파싱을 지양하고, **단일 인덱스 포인터(`pos`)** 로 문자열을 스캔하는 재귀 하강 파서를 사용합니다. 메모리 할당을 최소화하여 CUP/AI 환경에서 극한의 성능을 발휘합니다.

## 3. 토큰 스펙 (Token Specification)

모든 토큰은 UTF-8 기준 **정확히 1 byte** 입니다.

### 3.1 Structure & Value Tokens

| Token | Meaning | JSON Equivalent | Size |
|-------|---------|-----------------|------|
| `$` | 객체 시작 | `{` | 1B |
| `%` | 객체 종료 | `}` | 1B |
| `*` | 배열 시작 | `[` | 1B |
| `&` | 배열 종료 | `]` | 1B |
| `` ` `` | 문자열 경계 | `"` | 1B |
| `^` | 숫자 00 압축 | `00` | 1B |
| `@` | true | `true` | 1B |
| `#` | false | `false` | 1B |
| `~` | null | `null` | 1B |
| `\` | 이스케이프 | `\` | 1B |

### 3.2 Default Key Tokens

| Token | JSON Key | Size |
|-------|----------|------|
| `!` | name | 1B |
| `?` | health | 1B |
| `+` | level | 1B |
| `=` | alive | 1B |
| `<` | id | 1B |
| `>` | type | 1B |
| `;` | value | 1B |
| `/` | desc | 1B |

등록되지 않은 키는 `` `key` `` 형태로 문자열 인코딩됩니다. 커스텀 토큰 추가로 확장 가능합니다.

### 3.3 Number Compression

끝에 `0`이 2개 이상 붙은 정수는 `^`로 압축합니다. 소수점은 데이터 손실 방지를 위해 압축하지 않습니다.

| JSON | JWON | Bytes Saved |
|------|------|-------------|
| `100` | `1^` | 1B |
| `10000` | `1^^` | 2B |
| `35000000` | `35^^^` | 4B |
| `-500` | `-5^` | 2B |
| `3.14` | `3.14` | 0B (안전) |

### 3.4 String Escaping

문자열 내부에 JWON 토큰 문자가 포함될 경우 `\`로 이스케이프합니다.

| Original String | JWON Encoded |
|----------------|--------------|
| `Price is $100` | `` `Price is \$100` `` |
| `Use * and &` | `` `Use \* and \&` `` |
| `Back\slash` | `` `Back\\slash` `` |

## 4. 변환 예시 (Examples)

### Basic
JSON: {"name":"Alice","health":100,"alive":true}
JWON: $!Alice?1^=@
Size: 37B -> 16B (57% compression)

### Nested Object + Array
JSON: {"user":{"name":"Bob"},"items":["sword","shield"]}
JWON: 
!
!!Bob%*sword``shield&%
Size: 52B -> 26B (50% compression)

### Unicode + Escape
JSON: {"desc":"안녕하세요 🎉 Price 50","alive":false}
JWON:/안녕하세요 🎉 Price \$50=
Note: 유니코드는 그대로 보존, $만 이스케이프

## 5. API Reference

### Encoder

```javascript
const jwonString = encodeJWON(jsonObject);
Decoder
const jsonObject = decodeJWON(jwonString);
Round-Trip Validation
const original = JSON.parse(jsonString);
const restored = decodeJWON(encodeJWON(original));
const isValid = deepEqual(original, restored); // must be true
## 6. 압축률 참고 사항

| 데이터 유형 | 예상 압축률 | 비고 |
|------------|-----------|------|
| ASCII 영문 + 반복 키 | 50~70% | 최적 시나리오 |
| 한글/한자 혼합 | 30~45% | UTF-8 3B 특성 |
| 이모지 다수 | 20~35% | UTF-8 4B 특성 |
| 짧은 값 + 긴 키 | 40~55% | 키 토큰 효과 큼 |

압축률은 데이터 특성에 따라 달라지지만, **동일 정보 대비 항상 JSON보다 작거나 같습니다.**

## 7. 파서 구현 가이드

### 권장 방식: 재귀 커서 파서

`split()`, `replace()`, 정규식 기반 파싱은 지양하고, 단일 인덱스 포인터(`pos`)로 문자열을 스캔하는 재귀 하강 파서를 사용하십시오.

```javascript
// 핵심 구조 예시
function decodeJWON(str) {
    let pos = 0;
    function peek() { return str[pos]; }
    function consume() { return str[pos++]; }

    function parseValue() {
        const c = peek();
        if (c === '$') return parseObject();
        if (c === '*') return parseArray();
        if (c === '`') return parseString();
        if (c === '@' || c === '#' || c === '~') return parseBooleanOrNull();
        if (/[-0-9]/.test(c)) return parseNumber();
    }
    // ... parseObject, parseArray, parseString, parseNumber
}
이스케이프 처리 주의사항
parseString() 내부에서 \를 만나면, **다음 문자를 무조건 문자열 내용으로 취급**해야 합니다. 다음 문자가 JWON 토큰($, %, ` 등)이더라도 구조 토큰으로 해석하지 않고 그대로 수집합니다.
// 올바른 이스케이프 처리
if (char === '\\') {
    consume();       // \ 소비
    result += consume(); // 다음 문자는 무엇이든 문자열 내용에 포함
    continue;
}
이 처리가 누락되면 \$, \% 등이 포함된 문자열 복원 시 파서가 조기 종료되거나 오류를 발생시킵니다.
8. 커스텀 토큰 확장
기본 키 토큰(!, ?, + 등) 외에 프로젝트 고유의 키를 추가할 수 있습니다.
규칙
반드시 1-byte ASCII 특수문자만 사용
알파벳(a-z, A-Z), 숫자(0-9) 사용 금지 (데이터 충돌 방지)
기존 구조 토큰($ % * & ^ @ # ~ `)과 중복 금지
등록 예시
TOKEN_MAP.keys['inventory'] = '|';
TOKEN_MAP.reverseKeys['|'] = 'inventory';
등록 후 {"inventory":["sword"]} 는 $|*sword&% 로 인코딩됩니다.
## 9. 한계 및 주의사항

| 항목 | 설명 |
|------|------|
| 가독성 | 사람이 직접 읽거나 편집하는 용도가 아님 |
| 소수점 | 데이터 손실 방지를 위해 압축하지 않음 |
| 음수 | `-` 부호는 그대로 유지 (숫자 컨텍스트에서만 사용되므로 모호성 없음) |
| 빈 객체/배열 | `$%`, `*&` 로 정상 인코딩/디코딩됨 |
| 최대 중첩 | 재귀 파서의 스택 제한에 따름 (일반적 사용 범위 내 문제 없음) |

## 10. License

MIT License

---

*JWON is not designed for human readability. It is designed for machines.*
코드:
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JWON Studio v4.1 - Pure ASCII Specials</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Cascadia Code', 'Consolas', monospace; background: #0d1117; color: #c9d1d9; overflow: hidden; }
        .container { display: flex; flex-direction: column; height: 100vh; }
        .header { background: #010409; border-bottom: 2px solid #30363d; padding: 14px 20px; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 10px; }
        .header h1 { font-size: 18px; font-weight: 700; color: #79c0ff; display: flex; align-items: center; gap: 10px; }
        .version-badge { font-size: 10px; background: #238636; color: white; padding: 2px 8px; border-radius: 10px; font-weight: 700; }
        .header-controls { display: flex; gap: 6px; flex-wrap: wrap; }
        .btn { background: #238636; color: #fff; border: 1px solid #2d333b; padding: 7px 12px; border-radius: 6px; cursor: pointer; font-family: inherit; font-size: 11px; font-weight: 500; transition: all 0.2s; }
        .btn:hover { background: #2ea043; border-color: #388bfd; }
        .btn-secondary { background: #1f6feb; }
        .btn-secondary:hover { background: #388bfd; }
        .btn-danger { background: #da3633; }
        .btn-danger:hover { background: #f85149; }
        .btn-live { background: #21262d; border-color: #30363d; }
        .btn-live.active { background: #238636; border-color: #3fb950; box-shadow: 0 0 10px rgba(63,185,80,0.3); }
        .main-content { display: flex; flex: 1; overflow: hidden; }
        .panel { flex: 1; display: flex; flex-direction: column; background: #0d1117; border-right: 1px solid #30363d; overflow: hidden; }
        .panel:last-child { border-right: none; }
        .panel-header { background: #161b22; border-bottom: 1px solid #30363d; padding: 10px 16px; font-size: 11px; font-weight: 700; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; display: flex; justify-content: space-between; align-items: center; }
        .panel-actions { display: flex; gap: 6px; }
        .mini-btn { background: #21262d; color: #8b949e; border: 1px solid #30363d; padding: 3px 8px; border-radius: 4px; cursor: pointer; font-family: inherit; font-size: 10px; transition: all 0.2s; }
        .mini-btn:hover { color: #c9d1d9; border-color: #388bfd; }
        .editor { flex: 1; padding: 16px; background: #0d1117; border: none; color: #c9d1d9; font-family: inherit; font-size: 13px; line-height: 1.6; resize: none; outline: none; overflow-y: auto; overflow-x: auto; }
        .editor::placeholder { color: #6e7681; }
        .center-controls { display: flex; flex-direction: column; justify-content: center; align-items: center; padding: 16px; gap: 10px; min-width: 130px; background: #010409; border-left: 1px solid #30363d; border-right: 1px solid #30363d; }
        .convert-btn { padding: 10px 14px; background: #1f6feb; color: white; border: 2px solid #388bfd; border-radius: 6px; cursor: pointer; font-weight: 700; font-size: 11px; transition: all 0.3s; width: 105px; text-transform: uppercase; letter-spacing: 1px; }
        .convert-btn:hover { background: #388bfd; box-shadow: 0 0 20px rgba(88,166,255,0.5); transform: scale(1.05); }
        .convert-btn:active { transform: scale(0.95); }
        .validate-btn { padding: 8px 14px; background: #21262d; color: #d2a8ff; border: 2px solid #8957e5; border-radius: 6px; cursor: pointer; font-weight: 700; font-size: 11px; transition: all 0.3s; width: 105px; text-transform: uppercase; letter-spacing: 1px; }
        .validate-btn:hover { background: #8957e5; color: white; }
        .shortcut-hint { font-size: 9px; color: #6e7681; text-align: center; }
        .footer { background: #161b22; border-top: 1px solid #30363d; padding: 10px 20px; display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 16px; font-size: 11px; }
        .stat { display: flex; flex-direction: column; gap: 3px; }
        .stat-label { color: #6e7681; font-weight: 700; text-transform: uppercase; letter-spacing: 0.5px; }
        .stat-value { color: #79c0ff; font-size: 14px; font-weight: 700; font-family: 'Courier New', monospace; }
        .stat-value.good { color: #3fb950; text-shadow: 0 0 8px rgba(63,185,80,0.4); }
        .stat-value.warning { color: #ffa657; }
        .stat-value.target { color: #d2a8ff; }
        .progress-container { position: relative; height: 6px; background: #21262d; border-radius: 3px; margin-top: 4px; }
        .progress-bar { height: 100%; width: 0%; background: linear-gradient(90deg, #f85149, #ffa657, #3fb950); border-radius: 3px; transition: width 0.4s ease; }
        .progress-marker { position: absolute; left: 50%; top: -3px; width: 2px; height: 12px; background: #d2a8ff; box-shadow: 0 0 6px rgba(210,168,255,0.8); }
        ::-webkit-scrollbar { width: 8px; height: 8px; }
        ::-webkit-scrollbar-track { background: #0d1117; }
        ::-webkit-scrollbar-thumb { background: #30363d; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #484f58; }
        .modal { display: none; position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.8); z-index: 1000; justify-content: center; align-items: center; }
        .modal.active { display: flex; }
        .modal-content { background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 24px; max-width: 800px; width: 90%; max-height: 85vh; overflow-y: auto; box-shadow: 0 10px 40px rgba(0,0,0,0.5); position: relative; }
        .close-modal { position: absolute; top: 12px; right: 12px; background: none; border: none; color: #8b949e; cursor: pointer; font-size: 24px; transition: color 0.2s; line-height: 1; }
        .close-modal:hover { color: #c9d1d9; }
        .modal-header { font-size: 16px; font-weight: 700; color: #79c0ff; margin-bottom: 16px; padding-bottom: 12px; border-bottom: 1px solid #30363d; margin-right: 24px; }
        .modal-body { margin: 16px 0; color: #c9d1d9; font-size: 13px; line-height: 1.7; }
        .section-title { color: #79c0ff; font-weight: 700; margin-top: 18px; margin-bottom: 10px; font-size: 13px; }
        table { width: 100%; border-collapse: collapse; margin: 12px 0; }
        th, td { padding: 8px 10px; text-align: left; border: 1px solid #30363d; }
        th { background: #161b22; color: #79c0ff; font-weight: 700; text-transform: uppercase; font-size: 11px; }
        td { color: #c9d1d9; font-size: 12px; }
        .token-symbol { color: #ffd700; font-weight: 700; font-size: 16px; }
        .example-box { background: #161b22; border-left: 3px solid #79c0ff; padding: 12px; margin: 10px 0; border-radius: 4px; font-size: 12px; color: #c9d1d9; overflow-x: auto; white-space: pre-wrap; }
        .code-inline { background: #161b22; padding: 2px 6px; border-radius: 3px; color: #79c0ff; font-size: 12px; }
        .byte-badge { font-size: 9px; background: #238636; color: white; padding: 1px 5px; border-radius: 8px; margin-left: 4px; }
        .token-form { display: flex; gap: 8px; margin: 12px 0; flex-wrap: wrap; }
        .token-input { background: #0d1117; border: 1px solid #30363d; color: #c9d1d9; padding: 8px 12px; border-radius: 6px; font-family: inherit; font-size: 13px; outline: none; flex: 1; min-width: 120px; }
        .token-input:focus { border-color: #388bfd; }
        .custom-token-row { display: flex; justify-content: space-between; align-items: center; background: #161b22; padding: 8px 12px; border-radius: 6px; margin: 6px 0; }
        .delete-token { background: none; border: none; color: #f85149; cursor: pointer; font-size: 14px; }
        @media (max-width: 1400px) {
            .main-content { flex-direction: column; }
            .panel { border-right: none; border-bottom: 1px solid #30363d; }
            .center-controls { flex-direction: row; border-left: none; border-right: none; border-bottom: 1px solid #30363d; width: 100%; min-width: unset; }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>⚡ JWON Studio <span class="version-badge">v4.1 Pure ASCII</span></h1>
            <div class="header-controls">
                <button class="btn btn-live active" id="liveToggle" onclick="toggleLive()">🔴 Live</button>
                <button class="btn btn-secondary" onclick="openTokenModal()">📋 토큰</button>
                <button class="btn btn-secondary" onclick="openCustomTokenModal()">➕ 커스텀</button>
                <button class="btn btn-secondary" onclick="openHelpModal()">❓ 가이드</button>
                <button class="btn btn-secondary" onclick="loadSampleData()">📦 샘플</button>
                <button class="btn btn-danger" onclick="clearAll()">🗑️ 초기화</button>
            </div>
        </div>

        <div class="main-content">
            <div class="panel">
                <div class="panel-header">
                    <span>📄 JSON Input</span>
                    <span class="shortcut-hint">Ctrl+Enter = 변환</span>
                </div>
                <textarea id="jsonInput" class="editor" spellcheck="false" placeholder='{"name":"Alice","health":100,"alive":true}'></textarea>
            </div>

            <div class="center-controls">
                <button class="convert-btn" onclick="convertJsonToJwon()">JSON→<br>JWON</button>
                <button class="convert-btn" onclick="convertJwonToJson()">JWON→<br>JSON</button>
                <button class="validate-btn" onclick="validateRoundTrip()">🔄 검증</button>
            </div>

            <div class="panel">
                <div class="panel-header">
                    <span>⚙️ JWON Output</span>
                    <div class="panel-actions">
                        <button class="mini-btn" onclick="copyJwon()">📋 복사</button>
                        <button class="mini-btn" onclick="downloadJwon()">💾 저장</button>
                    </div>
                </div>
                <textarea id="jwonOutput" class="editor" spellcheck="false" placeholder="JWON 출력 (직접 입력 가능)"></textarea>
            </div>
        </div>

        <div class="footer">
            <div class="stat">
                <div class="stat-label">JSON Size</div>
                <div class="stat-value" id="jsonSize">0 B</div>
            </div>
            <div class="stat">
                <div class="stat-label">JWON Size</div>
                <div class="stat-value" id="jwonSize">0 B</div>
            </div>
            <div class="stat">
                <div class="stat-label">Compression</div>
                <div class="stat-value" id="compressionRate">0%</div>
                <div class="progress-container">
                    <div class="progress-bar" id="compressionBar"></div>
                    <div class="progress-marker" title="Target: 50%"></div>
                </div>
            </div>
            <div class="stat">
                <div class="stat-label">Target</div>
                <div class="stat-value target" id="targetStatus">🎯 50%</div>
            </div>
            <div class="stat">
                <div class="stat-label">Saved Bytes</div>
                <div class="stat-value" id="savedBytes">0 B</div>
            </div>
            <div class="stat">
                <div class="stat-label">Tokens Used</div>
                <div class="stat-value" id="tokenCount">0</div>
            </div>
            <div class="stat">
                <div class="stat-label">Status</div>
                <div class="stat-value" id="status">Ready</div>
            </div>
        </div>
    </div>

    <div class="modal" id="tokenModal">
        <div class="modal-content">
            <button class="close-modal" onclick="closeModal('tokenModal')">×</button>
            <div class="modal-header">JWON v4.1 Token Reference (1-byte 특수문자만 사용)</div>
            <div class="modal-body">
                <p style="color:#8b949e; font-size:12px;">💡 알파벳/숫자와의 충돌을 100% 방지하기 위해 키보드 특수문자만 사용합니다.</p>
                
                <div class="section-title">🏗️ Structure & Value Tokens</div>
                <table>
                    <thead><tr><th>Token</th><th>Meaning</th><th>JSON</th><th>Size</th></tr></thead>
                    <tbody>
                        <tr><td class="token-symbol">$</td><td>객체 시작</td><td>{</td><td><span class="byte-badge">1B</span></td></tr>
                        <tr><td class="token-symbol">%</td><td>객체 종료</td><td>}</td><td><span class="byte-badge">1B</span></td></tr>
                        <tr><td class="token-symbol">*</td><td>배열 시작</td><td>[</td><td><span class="byte-badge">1B</span></td></tr>
                        <tr><td class="token-symbol">&</td><td>배열 종료</td><td>]</td><td><span class="byte-badge">1B</span></td></tr>
                        <tr><td class="token-symbol">`</td><td>문자열 경계</td><td>"</td><td><span class="byte-badge">1B</span></td></tr>
                        <tr><td class="token-symbol">^</td><td>숫자 00 압축</td><td>00</td><td><span class="byte-badge">1B</span></td></tr>
                        <tr><td class="token-symbol">@</td><td>true</td><td>true</td><td><span class="byte-badge">1B</span></td></tr>
                        <tr><td class="token-symbol">#</td><td>false</td><td>false</td><td><span class="byte-badge">1B</span></td></tr>
                        <tr><td class="token-symbol">~</td><td>null</td><td>null</td><td><span class="byte-badge">1B</span></td></tr>
                        <tr><td class="token-symbol">\</td><td>이스케이프</td><td>\</td><td><span class="byte-badge">1B</span></td></tr>
                    </tbody>
                </table>

                <div class="section-title">🔑 Key Tokens (알파벳 충돌 방지)</div>
                <table>
                    <thead><tr><th>Token</th><th>JSON Key</th><th>Meaning</th><th>Size</th></tr></thead>
                    <tbody id="keyTokenTableBody"></tbody>
                </table>
            </div>
        </div>
    </div>

    <div class="modal" id="customTokenModal">
        <div class="modal-content">
            <button class="close-modal" onclick="closeModal('customTokenModal')">×</button>
            <div class="modal-header">➕ 커스텀 토큰 (1-byte ASCII)</div>
            <div class="modal-body">
                <p>자주 사용하는 JSON 키를 1-byte 특수문자로 등록하세요. (알파벳/숫자 제외)</p>
                <div class="token-form">
                    <input type="text" id="newKeyInput" class="token-input" placeholder="JSON 키 (예: inventory)">
                    <input type="text" id="newTokenInput" class="token-input" placeholder="특수문자 (1글자)" maxlength="1" style="max-width: 100px;">
                    <button class="btn btn-secondary" onclick="suggestAsciiToken()">🎲 추천</button>
                    <button class="btn" onclick="addCustomToken()">➕ 추가</button>
                </div>
                <div class="section-title">📌 등록된 커스텀 토큰</div>
                <div id="customTokenList"></div>
            </div>
        </div>
    </div>

    <div class="modal" id="helpModal">
        <div class="modal-content">
            <button class="close-modal" onclick="closeModal('helpModal')">×</button>
            <div class="modal-header">JWON v4.1 - 충돌 방지 가이드</div>
            <div class="modal-body">
                <div class="section-title">1. 왜 특수문자만 쓰나요?</div>
                <p>사용자가 입력한 데이터(이름, 설명 등)에 알파벳이나 숫자가 포함될 수 있습니다. 토큰으로 알파벳을 쓰면 데이터와 토큰이 섞여 <strong>복원이 불가능</strong>해집니다. 특수문자만 쓰면 이 충돌을 100% 막을 수 있습니다.</p>

                <div class="section-title">2. 이스케이프 (\)</div>
                <div class="example-box">JSON: {"desc": "Price is $100"}
JWON: $/`Price is \$100`%</div>
                <p>문자열 안에 JWON 토큰($, %, ` 등)이 들어가면 `\`를 붙여 이스케이프합니다.</p>

                <div class="section-title">3. 중첩 구조</div>
                <div class="example-box">JSON: {"user": {"name": "Bob"}}
JWON: $!$!`Bob`%%</div>
            </div>
        </div>
    </div>

    <script>
        const TOKEN_MAP = {
            keys: {
                'name': '!', 'health': '?', 'level': '+',
                'alive': '=', 'id': '<', 'type': '>', 'value': ';', 'desc': '/'
            },
            reverseKeys: {},
            boolean: { 'true': '@', 'false': '#', 'null': '~' },
            reverseBoolean: {},
            special: {
                objectStart: '$',
                objectEnd: '%',
                arrayStart: '*',
                arrayEnd: '&',
                stringQuote: '`',
                numberZero: '^',
                escape: '\\'
            }
        };

        const KEY_MEANINGS = {
            'name': '이름', 'health': '체력', 'level': '레벨',
            'alive': '생존', 'id': 'ID', 'type': '타입', 'value': '값', 'desc': '설명'
        };

        // 사용자가 제시한 10개 특수문자를 포함한 안전한 ASCII_POOL
        const ASCII_POOL = ['@', '#', '$', '%', '^', '&', '*', '`', '~', '/', '!', '?', '+', '=', '<', '>', ';', '|', '_', '-', '.'];

        let customKeys = {};

        for (const [key, token] of Object.entries(TOKEN_MAP.keys)) TOKEN_MAP.reverseKeys[token] = key;
        for (const [key, token] of Object.entries(TOKEN_MAP.boolean)) TOKEN_MAP.reverseBoolean[token] = key === 'null' ? null : (key === 'true');

        function getAllSpecialChars() {
            return [
                ...Object.values(TOKEN_MAP.special),
                ...Object.values(TOKEN_MAP.keys),
                ...Object.values(TOKEN_MAP.boolean)
            ];
        }

        function escapeRegexChar(c) {
            return c.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
        }

        function buildEscapeRegex() {
            const chars = getAllSpecialChars();
            const escaped = chars.map(escapeRegexChar).join('');
            return {
                escape: new RegExp(`[${escaped}]`, 'g'),
                unescape: new RegExp(`\\\\([${escaped}])`, 'g')
            };
        }

        let escapeRegexes = buildEscapeRegex();

        function encodeJWON(obj) {
            if (obj === null) return TOKEN_MAP.boolean['null'];
            if (typeof obj === 'boolean') return TOKEN_MAP.boolean[obj.toString()];
            if (typeof obj === 'number') return compressNumber(obj);
            if (typeof obj === 'string') return TOKEN_MAP.special.stringQuote + escapeString(obj) + TOKEN_MAP.special.stringQuote;

            if (Array.isArray(obj)) {
                let result = TOKEN_MAP.special.arrayStart;
                for (const item of obj) result += encodeJWON(item);
                result += TOKEN_MAP.special.arrayEnd;
                return result;
            }

            if (typeof obj === 'object') {
                let result = TOKEN_MAP.special.objectStart;
                for (const key in obj) {
                    if (Object.hasOwn(obj, key)) {
                        const keyToken = TOKEN_MAP.keys[key] || (TOKEN_MAP.special.stringQuote + escapeString(key) + TOKEN_MAP.special.stringQuote);
                        result += keyToken + encodeJWON(obj[key]);
                    }
                }
                result += TOKEN_MAP.special.objectEnd;
                return result;
            }
            return '';
        }

        function compressNumber(num) {
            if (!Number.isInteger(num)) return String(num);
            const str = String(num);
            const isNegative = num < 0;
            const absStr = isNegative ? str.substring(1) : str;
            let result = absStr;
            if (absStr.endsWith('0') && absStr !== '0') {
                let zeroCount = 0;
                for (let i = absStr.length - 1; i >= 0; i--) {
                    if (absStr[i] === '0') zeroCount++; else break;
                }
                if (zeroCount >= 2 && zeroCount < absStr.length) {
                    const base = absStr.substring(0, absStr.length - zeroCount);
                    const carets = TOKEN_MAP.special.numberZero.repeat(Math.floor(zeroCount / 2));
                    const remainder = zeroCount % 2 === 1 ? '0' : '';
                    result = base + carets + remainder;
                }
            }
            return isNegative ? '-' + result : result;
        }

        function escapeString(str) {
            return str.replace(/\\/g, '\\\\').replace(escapeRegexes.escape, '\\$&');
        }

        function decodeJWON(jwonStr) {
            let pos = 0;
            const len = jwonStr.length;
            function peek() { return pos < len ? jwonStr[pos] : null; }
            function consume() { return jwonStr[pos++]; }

            function parseValue() {
                const char = peek();
                if (char === null) return null;
                if (char === TOKEN_MAP.special.objectStart) return parseObject();
                if (char === TOKEN_MAP.special.arrayStart) return parseArray();
                if (char === TOKEN_MAP.special.stringQuote) return parseString();
                if (TOKEN_MAP.reverseBoolean[char] !== undefined) { consume(); return TOKEN_MAP.reverseBoolean[char]; }
                if (/[-0-9]/.test(char)) return parseNumber();
                throw new Error(`Unexpected character '${char}' at pos ${pos}`);
            }

            function parseObject() {
                consume(); // $
                const obj = {};
                while (pos < len && peek() !== TOKEN_MAP.special.objectEnd) {
                    const keyToken = peek();
                    let key;
                    if (TOKEN_MAP.reverseKeys[keyToken]) { consume(); key = TOKEN_MAP.reverseKeys[keyToken]; }
                    else if (keyToken === TOKEN_MAP.special.stringQuote) { key = parseString(); }
                    else { throw new Error(`Invalid object key at pos ${pos}`); }
                    obj[key] = parseValue();
                }
                if (peek() === TOKEN_MAP.special.objectEnd) consume(); // %
                return obj;
            }

            function parseArray() {
                consume(); // *
                const arr = [];
                while (pos < len && peek() !== TOKEN_MAP.special.arrayEnd) arr.push(parseValue());
                if (peek() === TOKEN_MAP.special.arrayEnd) consume(); // &
                return arr;
            }

            function parseString() {
                consume(); // `
                let result = '';
                while (pos < len) {
                    const char = peek();
                    if (char === TOKEN_MAP.special.escape) {
                        consume();
                        if (pos < len) result += consume();
                        continue;
                    }
                    if (char === TOKEN_MAP.special.stringQuote) { consume(); return result; }
                    result += consume();
                }
                throw new Error('Unterminated string');
            }

            function parseNumber() {
                let result = '';
                while (pos < len) {
                    const char = peek();
                    if (/[-0-9]/.test(char) || char === TOKEN_MAP.special.numberZero) result += consume();
                    else break;
                }
                return decompressNumber(result);
            }

            return parseValue();
        }

        function decompressNumber(str) {
            const caretCount = (str.match(/\^/g) || []).length;
            if (caretCount > 0) {
                const isNeg = str.startsWith('-');
                const base = str.replace(/\^/g, '').replace('-', '');
                const num = Number(base + '00'.repeat(caretCount));
                return isNeg ? -num : num;
            }
            return Number(str);
        }

        function deepEqual(a, b) {
            if (a === b) return true;
            if (typeof a !== typeof b || a === null || b === null) return false;
            if (Array.isArray(a) !== Array.isArray(b)) return false;
            if (typeof a === 'object') {
                const keysA = Object.keys(a), keysB = Object.keys(b);
                if (keysA.length !== keysB.length) return false;
                return keysA.every(k => deepEqual(a[k], b[k]));
            }
            return false;
        }

        function validateRoundTrip() {
            const jsonInput = document.getElementById('jsonInput').value.trim();
            if (!jsonInput) return setStatus('Error: Empty JSON', 'warning');
            try {
                const original = JSON.parse(jsonInput);
                const jwon = encodeJWON(original);
                const restored = decodeJWON(jwon);
                if (deepEqual(original, restored)) {
                    document.getElementById('jwonOutput').value = jwon;
                    updateStats(jsonInput, jwon);
                    setStatus('✓ 100% Round-trip verified', 'good');
                } else { setStatus('✕ Mismatch!', 'warning'); }
            } catch (e) { setStatus(`Error: ${e.message}`, 'warning'); }
        }

        function convertJsonToJwon(silent = false) {
            const jsonInput = document.getElementById('jsonInput').value.trim();
            if (!jsonInput) { if (!silent) setStatus('Error: Empty', 'warning'); return; }
            try {
                const jwon = encodeJWON(JSON.parse(jsonInput));
                document.getElementById('jwonOutput').value = jwon;
                updateStats(jsonInput, jwon);
                if (!silent) setStatus('✓ Success', 'good');
            } catch (e) { if (!silent) setStatus(`Error: ${e.message}`, 'warning'); }
        }

        function convertJwonToJson() {
            const jwonInput = document.getElementById('jwonOutput').value.trim();
            if (!jwonInput) return setStatus('Error: Empty', 'warning');
            try {
                const jsonOutput = JSON.stringify(decodeJWON(jwonInput), null, 2);
                document.getElementById('jsonInput').value = jsonOutput;
                updateStats(jsonOutput, jwonInput);
                setStatus('✓ Restored', 'good');
            } catch (e) { setStatus(`Error: ${e.message}`, 'warning'); }
        }

        function updateStats(jsonStr, jwonStr) {
            const jsonSize = new Blob([jsonStr]).size;
            const jwonSize = new Blob([jwonStr]).size;
            const saved = jsonSize - jwonSize;
            const rate = jsonSize > 0 ? Math.round((saved / jsonSize) * 100) : 0;
            const specialChars = getAllSpecialChars();
            const tokenUsed = specialChars.reduce((sum, ch) => sum + (jwonStr.split(ch).length - 1), 0);

            document.getElementById('jsonSize').textContent = `${jsonSize} B`;
            document.getElementById('jwonSize').textContent = `${jwonSize} B`;
            const rateEl = document.getElementById('compressionRate');
            rateEl.textContent = `${rate}%`;
            rateEl.className = 'stat-value ' + (rate >= 50 ? 'good' : rate >= 30 ? 'warning' : '');
            document.getElementById('compressionBar').style.width = Math.min(Math.max(rate, 0), 100) + '%';
            const targetEl = document.getElementById('targetStatus');
            targetEl.textContent = rate >= 50 ? '🎯 Achieved!' : `🎯 50% (need ${50 - rate}%)`;
            targetEl.className = 'stat-value ' + (rate >= 50 ? 'good' : 'target');
            document.getElementById('savedBytes').textContent = `${saved} B`;
            document.getElementById('tokenCount').textContent = `${tokenUsed}`;
        }

        let liveMode = true, debounceTimer;
        function toggleLive() {
            liveMode = !liveMode;
            const btn = document.getElementById('liveToggle');
            btn.textContent = liveMode ? '🔴 Live' : '⚪ Live';
            btn.classList.toggle('active', liveMode);
        }
        document.getElementById('jsonInput').addEventListener('input', () => {
            if (!liveMode) return;
            clearTimeout(debounceTimer);
            debounceTimer = setTimeout(() => convertJsonToJwon(true), 300);
        });
        document.getElementById('jwonOutput').addEventListener('input', () => {
            if (!liveMode) return;
            clearTimeout(debounceTimer);
            debounceTimer = setTimeout(() => convertJwonToJson(true), 300);
        });
        document.addEventListener('keydown', (e) => {
            if ((e.ctrlKey || e.metaKey) && e.key === 'Enter') {
                e.preventDefault();
                document.activeElement.id === 'jwonOutput' ? convertJwonToJson() : convertJsonToJwon();
            }
        });

        function copyJwon() {
            const jwon = document.getElementById('jwonOutput').value;
            if (!jwon) return setStatus('Empty', 'warning');
            navigator.clipboard.writeText(jwon).then(() => setStatus('✓ Copied', 'good'));
        }
        function downloadJwon() {
            const jwon = document.getElementById('jwonOutput').value;
            if (!jwon) return setStatus('Empty', 'warning');
            const a = document.createElement('a');
            a.href = URL.createObjectURL(new Blob([jwon], { type: 'text/plain' }));
            a.download = 'data.jwon'; a.click();
            setStatus('✓ Downloaded', 'good');
        }

        function registerToken(key, token, save = true) {
            TOKEN_MAP.keys[key] = token;
            TOKEN_MAP.reverseKeys[token] = key;
            escapeRegexes = buildEscapeRegex();
            if (save) { customKeys[key] = token; saveCustomTokens(); }
            renderKeyTokenTable();
        }
        function addCustomToken() {
            const key = document.getElementById('newKeyInput').value.trim();
            const token = document.getElementById('newTokenInput').value.trim();
            if (!key || !token) return setStatus('Error: Required', 'warning');
            if (TOKEN_MAP.reverseKeys[token] || Object.values(TOKEN_MAP.special).includes(token)) return setStatus('Error: Token in use', 'warning');
            if (/^[a-zA-Z0-9]$/.test(token)) return setStatus('Error: No alphanumeric', 'warning');
            registerToken(key, token);
            document.getElementById('newKeyInput').value = '';
            document.getElementById('newTokenInput').value = '';
            renderCustomTokenList();
            setStatus(`✓ Added: ${key} → ${token}`, 'good');
        }
        function removeCustomToken(key) {
            const token = customKeys[key];
            delete customKeys[key]; delete TOKEN_MAP.keys[key]; delete TOKEN_MAP.reverseKeys[token];
            escapeRegexes = buildEscapeRegex();
            saveCustomTokens(); renderKeyTokenTable(); renderCustomTokenList();
        }
        function suggestAsciiToken() {
            const used = new Set([...Object.values(TOKEN_MAP.keys), ...Object.values(TOKEN_MAP.boolean), ...Object.values(TOKEN_MAP.special)]);
            const available = ASCII_POOL.filter(t => !used.has(t));
            document.getElementById('newTokenInput').value = available.length ? available[Math.floor(Math.random() * available.length)] : '!';
        }
        function renderCustomTokenList() {
            const list = document.getElementById('customTokenList');
            const entries = Object.entries(customKeys);
            list.innerHTML = entries.length ? entries.map(([k, t]) => `<div class="custom-token-row"><span><span class="token-symbol">${t}</span> ← ${k}</span><button class="delete-token" onclick="removeCustomToken('${k}')">✕</button></div>`).join('') : '<p style="color:#6e7681;">No custom tokens.</p>';
        }
        function renderKeyTokenTable() {
            document.getElementById('keyTokenTableBody').innerHTML = Object.entries(TOKEN_MAP.keys).map(([k, t]) => `<tr><td class="token-symbol">${t}</td><td>${k}</td><td>${KEY_MEANINGS[k] || 'Custom'}</td><td><span class="byte-badge">1B</span></td></tr>`).join('');
        }
        function saveCustomTokens() { localStorage.setItem('jwon_v41', JSON.stringify(customKeys)); }
        function loadCustomTokens() {
            try {
                const saved = localStorage.getItem('jwon_v41');
                if (saved) {
                    customKeys = JSON.parse(saved);
                    for (const [k, t] of Object.entries(customKeys)) { TOKEN_MAP.keys[k] = t; TOKEN_MAP.reverseKeys[t] = k; }
                    escapeRegexes = buildEscapeRegex();
                }
            } catch (e) {}
        }

        function setStatus(msg, type = 'normal') {
            const el = document.getElementById('status');
            el.textContent = msg;
            el.className = 'stat-value ' + (type === 'warning' ? 'warning' : type === 'good' ? 'good' : '');
        }
        function clearAll() {
            document.getElementById('jsonInput').value = '';
            document.getElementById('jwonOutput').value = '';
            ['jsonSize', 'jwonSize', 'savedBytes', 'tokenCount'].forEach(id => document.getElementById(id).textContent = '0 B' || '0');
            document.getElementById('compressionRate').textContent = '0%';
            document.getElementById('compressionRate').className = 'stat-value';
            document.getElementById('compressionBar').style.width = '0%';
            document.getElementById('targetStatus').textContent = '🎯 50%';
            document.getElementById('targetStatus').className = 'stat-value target';
            setStatus('Cleared');
        }
        function loadSampleData() {
            const sample = {
                name: "Alice",
                health: 10000,
                alive: true,
                desc: "Use $ and % carefully!",
                items: ["sword", "shield"],
                stats: { attack: 1500, defense: 1000 }
            };
            document.getElementById('jsonInput').value = JSON.stringify(sample, null, 2);
            convertJsonToJwon();
        }

        function openTokenModal() { renderKeyTokenTable(); document.getElementById('tokenModal').classList.add('active'); }
        function openCustomTokenModal() { renderCustomTokenList(); document.getElementById('customTokenModal').classList.add('active'); }
        function openHelpModal() { document.getElementById('helpModal').classList.add('active'); }
        function closeModal(id) { document.getElementById(id).classList.remove('active'); }
        window.onclick = (e) => {
            ['tokenModal', 'customTokenModal', 'helpModal'].forEach(id => {
                if (e.target === document.getElementById(id)) document.getElementById(id).classList.remove('active');
            });
        };

        loadCustomTokens();
        renderKeyTokenTable();
    </script>
</body>
</html>
