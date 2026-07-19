# jxo
## Just X Oh no 
### 를 개발했습니다!! 약자는 0.5초만에 만들었습니다.
#### C++로 index.exe로 하세오 저는 그리고 이걸로 어떤일이 생기든 저한테 따지지 마십시오 MIT 조항을 따르십시오.그리고 15분동안 개발했스무니다.
```// 🌟 run 명령어 처리 파트 (최종 안정화 버전)
        if (cmd == "run") {
            string remain;
            getline(ss, remain); 
            if (!remain.empty() && remain[0] == ' ') remain.erase(0, 1);

            if (remain.empty()) {
                cout << "Jxo Run: 파일명을 입력하세요." << endl;
                continue;
            }

            string filename = "";
            string extraArgs = "";

            if (remain[0] == '"') {
                size_t closeQuote = remain.find('"', 1);
                if (closeQuote != string::npos) {
                    filename = remain.substr(1, closeQuote - 1);
                    extraArgs = remain.substr(closeQuote + 1);
                } else {
                    filename = remain; 
                }
            } 
            else {
                vector<string> extPatterns = {".py", ".cpp", ".c", ".cs", ".java", ".js", ".ts", ".html", ".htm", ".css"};
                size_t maxCutPos = string::npos;
                
                // 패턴을 모두 검사하며 '가장 오른쪽에 있는(가장 끝에 있는) 진짜 확장자'를 탐색
                for (const auto& ext : extPatterns) {
                    size_t pos = remain.rfind(ext);
                    if (pos != string::npos) {
                        if (pos + ext.length() == remain.length() || remain[pos + ext.length()] == ' ') {
                            // 더 뒤쪽에 존재하는 확장자 위치가 있다면 갱신
                            if (maxCutPos == string::npos || pos + ext.length() > maxCutPos) {
                                maxCutPos = pos + ext.length();
                            }
                        }
                    }
                }

                if (maxCutPos != string::npos && maxCutPos < remain.length()) {
                    filename = remain.substr(0, maxCutPos);
                    extraArgs = remain.substr(maxCutPos);
                } else {
                    filename = remain; 
                }
            }

            if (!fs::exists(filename)) {
                cout << "Error: 파일을 찾을 수 없습니다 -> [" << filename << "]" << endl;
                continue;
            }

            fs::path absPath = fs::absolute(filename);
            string ext = absPath.extension().string();
            string stem = absPath.stem().string();
            string dirPath = absPath.parent_path().string();

            cout << "[Jxo Active] " << ext << " 스택 구동 중..." << endl;

            // [Python] ~ [Web] 분기 로직은 작성하신 코드와 동일하므로 생략 (그대로 유지)
            // ... (기존 분기문 유지) ...
            
            continue;
        }

        // 🌟 맨 마지막 줄 변경: 일반 포워딩 시스템 명령어 실행 시에도 따옴표 버그 방어
        safeSystemExec(input);
    }
    return 0;
}```
