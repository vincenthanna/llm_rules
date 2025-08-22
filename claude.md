# Project Context for Claude

## 프로젝트 개요
이 프로젝트는 Python, C, C++을 활용한 멀티 언어 시스템입니다. 고성능 데이터 처리와 알고리즘 구현을 위해 각 언어의 장점을 활용합니다.

## 기술 스택
- **Python**: 3.9+, FastAPI, NumPy, Pandas, pytest
- **C**: C99 표준, GCC 컴파일러
- **C++**: C++14 표준 및 그 이후, CMake, Google Test
- **Build System**: CMake, Makefile
- **Documentation**: Doxygen (C/C++), Sphinx (Python)
- **Testing**: pytest (Python), CUnit (C), Google Test (C++)



## 공통 코딩 가이드라인
- 코드는 단순하게 작성하라
- 많은 기능보다는 간결함을 추구하라


## Python 코딩 가이드라인

### 스타일 규칙
- **PEP 8** 준수 (black 포매터 사용)
- **Type Hints** 모든 함수와 메서드에 적용
- **Docstring** Google 스타일 사용
- .flake8 파일이 있으면 일단 그것이 우선이다.

```python
def process_data(data: List[Dict[str, Any]], threshold: float = 0.5) -> Dict[str, int]:
    """데이터를 처리하고 결과를 반환합니다.
    
    Args:
        data: 처리할 데이터 리스트
        threshold: 임계값 (기본값: 0.5)
    
    Returns:
        처리 결과를 담은 딕셔너리
        
    Raises:
        ValueError: 잘못된 입력 데이터인 경우
    """
```

### 파일 명명 규칙
- 모듈: `snake_case.py`
- 클래스: `PascalCase`
- 함수/변수: `snake_case`
- 상수: `UPPER_CASE`

### 패키지 구조
- `__init__.py` 파일 필수
- 상대 임포트보다 절대 임포트 선호
- 써드파티 → 표준 라이브러리 → 로컬 임포트 순서

## C 코딩 가이드라인

### 스타일 규칙
- **C99 표준** 준수
- **K&R 스타일** 중괄호 사용
- **4 스페이스** 들여쓰기
- 프로젝트에 .clang-format 파일이 있으면 이것이 우선이다.

```c
int process_array(int *arr, size_t len, int threshold) {
    if (arr == NULL || len == 0) {
        return -1;
    }
    
    for (size_t i = 0; i < len; i++) {
        if (arr[i] > threshold) {
            arr[i] = threshold;
        }
    }
    
    return 0;
}
```

### 명명 규칙
- 함수: `snake_case`
- 변수: `snake_case`
- 매크로/상수: `UPPER_CASE`
- 타입: `snake_case_t`

### 헤더 파일 규칙
```c
#ifndef PROJECT_MODULE_H
#define PROJECT_MODULE_H

#ifdef __cplusplus
extern "C" {
#endif

// 함수 선언
int process_array(int *arr, size_t len, int threshold);

#ifdef __cplusplus
}
#endif

#endif /* PROJECT_MODULE_H */
```

## C++ 코딩 가이드라인

### 스타일 규칙
- **C++17 표준** 사용
- **Google C++ Style Guide** 기반
- **RAII 패턴** 적용

```cpp
class DataProcessor {
public:
    explicit DataProcessor(const std::string& config_path);
    ~DataProcessor() = default;
    
    // 복사 생성자/할당 연산자 금지
    DataProcessor(const DataProcessor&) = delete;
    DataProcessor& operator=(const DataProcessor&) = delete;
    
    // 이동 생성자/할당 연산자 허용
    DataProcessor(DataProcessor&&) = default;
    DataProcessor& operator=(DataProcessor&&) = default;
    
    std::vector<int> ProcessData(const std::vector<int>& input) const;

private:
    std::unique_ptr<ConfigImpl> config_;
};
```

### 명명 규칙
- 클래스: `PascalCase`
- 함수/메서드: `PascalCase`
- 변수: `snake_case`
- 멤버 변수: `snake_case_` (접미사 _ 사용)
- 상수: `kPascalCase`

### 현대적 C++ 기능 사용
- `auto` 키워드 적극 활용
- Range-based for 루프 사용
- `std::unique_ptr`, `std::shared_ptr` 사용
- `nullptr` 사용 (NULL 대신)

## 빌드 시스템

### Python
```bash
# 가상환경 설정
python -m venv venv
source venv/bin/activate  # Linux/Mac
# venv\Scripts\activate   # Windows

# 의존성 설치
pip install -r requirements.txt

# 테스트 실행
pytest python/tests/
```

### C
```bash
# 빌드
cd c/
make clean
make all

# 테스트 실행
make test
```

### C++
```bash
# 빌드
mkdir build && cd build
cmake ..
make -j4

# 테스트 실행
ctest --verbose
```

## 테스트 가이드라인

### Python
- **pytest** 프레임워크 사용
- 테스트 파일명: `test_*.py`
- 커버리지 90% 이상 유지
- Mock 객체 적극 활용

### C
- **CUnit** 프레임워크 사용
- 각 모듈별 테스트 파일 분리
- 메모리 누수 검사 (Valgrind)

### C++
- **Google Test** 프레임워크 사용
- Fixture 클래스 활용
- 테스트 파일명: `*_test.cpp`

## 성능 고려사항
- **Python**: 병목 지점은 C/C++로 구현
- **C**: 최적화 플래그 사용 (`-O2`, `-O3`)
- **C++**: 컴파일러 최적화 및 프로파일링

## 메모리 관리
- **Python**: 가비지 컬렉션 신뢰, 순환 참조 주의
- **C**: `malloc`/`free` 쌍 맞추기, 포인터 NULL 체크
- **C++**: RAII 패턴, 스마트 포인터 사용

## 상호 운용성
- Python-C 연동: **ctypes** 또는 **Cython** 사용
- Python-C++ 연동: **pybind11** 사용
- 공통 데이터 형식: JSON, 바이너리 프로토콜

## 문서화 규칙
- **Python**: Sphinx + Google docstring
- **C/C++**: Doxygen 주석
- README 파일은 각 언어별 디렉토리에 별도 작성

## 디버깅 도구
- **Python**: pdb, PyCharm 디버거
- **C**: GDB, Valgrind
- **C++**: GDB, Address Sanitizer, Clang Static Analyzer

## 금지사항
- **공통**: 하드코딩된 경로, 매직 넘버
- **Python**: `exec()`, `eval()` 사용 금지
- **C**: `gets()`, `strcpy()` 등 안전하지 않은 함수 사용 금지
- **C++**: Raw 포인터 직접 관리, `new`/`delete` 직접 사용 금지

## 코드 리뷰 체크리스트
- [ ] 컴파일 경고 없음
- [ ] 테스트 통과
- [ ] 메모리 누수 없음
- [ ] 문서화 완료
- [ ] 성능 영향 검토

## 버전 관리
- 의미적 버전 관리 (Semantic Versioning) 사용
- 각 언어별 빌드 아티팩트는 `.gitignore`에 추가
- 바이너리 파일은 Git LFS 사용 고려