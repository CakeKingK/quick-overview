# with GPT
# byte array 변환 관련 참고용

0) 예시 byte[] 두 종류
A. “원래 텍스트였던 bytes”
byte[] textBytes = "Hello, 한글 😀".getBytes(StandardCharsets.UTF_8);
B. “임의의 바이너리 bytes” (암호키/암호문/파일조각 등)
byte[] binBytes = new byte[] {(byte)0xFF, (byte)0x00, (byte)0xA1, (byte)0x80, (byte)0xE2};

1) byte[] → Base64로 인코딩 (바이너리 전달/저장 시 일반적으로 사용)
의미: 임의의 byte[]를 손실 없이 ASCII 문자열로 바꿔서 전송/로그/JSON 저장 가능하게 함
결과: 항상 복원 가능(역변환 decode)
예시
String b64Text = Base64.getEncoder().encodeToString(textBytes);
String b64Bin  = Base64.getEncoder().encodeToString(binBytes);
b64Text: "Hello, 한글 😀"의 UTF-8 바이트를 안전한 문자열로 포장한 것
b64Bin: 0xFF 같은 “텍스트로 해석 불가한” 바이트도 안전하게 포장됨

✅ 장점
어떤 byte[]든 절대 손실 없음
JSON/HTTP/DB/로그에 안전
재현성(항상 동일 input → 동일 output)
❗ 단점
길이가 약 4/3(≈33%) 증가

2) new String(byte[], UTF_8) 로 변환 (텍스트일 때만 정답)
의미: “이 byte[]는 UTF-8로 인코딩된 텍스트야”라는 가정 하에 텍스트로 디코딩
결과: byte[]가 UTF-8 텍스트가 아니면 **깨지거나 대체문자(�)**가 들어가며 원복이 불가능해질 수 있음

예시 A(텍스트 bytes에 대해)
String s1 = new String(textBytes, StandardCharsets.UTF_8);
// 결과: "Hello, 한글 😀"  (정상)
예시 B(바이너리 bytes에 대해)
String s2 = new String(binBytes, StandardCharsets.UTF_8);
// 결과: 보통 "�\u0000��â" 처럼 깨짐/대체문자 포함 가능
여기서 중요한 포인트:
binBytes는 UTF-8 규칙에 맞지 않는 바이트(예: 0xFF)가 섞여 있어서
디코딩 과정에서 �(U+FFFD) 같은 “대체문자”로 바뀌며
다시 s2.getBytes(UTF_8)를 해도 원본 byte[]로 되돌릴 수 없는 경우가 많습니다.

✅ 장점
원래가 텍스트면 사람이 읽기 쉬움
길이 증가 없음(그냥 텍스트)
❗ 단점
바이너리에 쓰면 데이터 손실/깨짐 위험 매우 큼
“표현”이 목적이라면 Base64/Hex를 쓰는 게 맞음


3) “Base64 문자열을 byte[]로 디코딩”
3-1) Base64 문자열 → byte[] (decode)
byte[] bytes = Base64.getDecoder().decode(base64String);
의미: Base64로 포장된 문자열을 원본 byte[]로 복원
3-2) byte[] → Base64 문자열 (encode)
String base64String = Base64.getEncoder().encodeToString(bytes);
의미: 원본 byte[]를 Base64 문자열로 포장
>> 즉, 3은 “Base64 변환의 반대 방향(복원)”까지 포함한 개념

✅ 장점
encode/decode가 가역적(손실 없음)
한 번에 비교: “같은 byte[]로 1과 2를 하면 결과가 얼마나 다른가”
케이스 A: 텍스트 bytes
textBytes = "Hello, 한글 😀"의 UTF-8 바이트
Base64 인코딩 → "SGVsbG8sIO2VnOq4gCDwn5iA" 같은 형태(대충 이런 느낌의 안전 문자열)
new String(UTF-8) → "Hello, 한글 😀" 그대로 사람이 읽음
Base64 디코딩 → 다시 textBytes로 100% 복원
결론: 텍스트라면 2는 “내용을 읽는 용도”, 1/3은 “안전하게 운반/저장하는 용도”
케이스 B: 바이너리 bytes
binBytes = {FF, 00, A1, 80, E2}
Base64 인코딩 → 사람이 못 읽어도 상관없이 “정확히 복구 가능한 문자열” 생성
new String(UTF-8) → 깨지거나 � 포함 가능, 손실 발생 가능
Base64 디코딩 → 원래 {FF, 00, A1, 80, E2}로 100% 복원


** 결론: 바이너리라면 2는 거의 금지에 가깝고, 1/3이 정답

* 언제 뭘 쓰는 게 맞나
✅ byte[]가 “텍스트의 UTF-8 바이트”인 게 확실
화면 표시/로그에서 읽고 싶다 → 2
JSON/HTTP로 안전하게 보내고 싶다 → 1 (Base64)

✅ byte[]가 “임의의 바이너리” (암호키/암호문/파일/해시 등)
무조건 1(Base64) 또는 Hex 사용
2는 위험 (손실/깨짐)

* 추가로 매우 흔한 착각 1개
new String(bytes, UTF_8)는 “바이트를 문자열로 바꾸는 만능”이 아니라
“이 바이트는 UTF-8로 인코딩된 텍스트”일 때만 복원 가능
