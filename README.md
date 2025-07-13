
````markdown
# quick_draw_dl

Quick, Draw! 데이터셋 기반 분류 모델 학습 & 추론 모듈입니다.  
Python 스크립트로 데이터 전처리, 모델 학습, 예측을 수행합니다.

## 요구 사항

- Python 3.8+  
- 가상환경 권장: `python3 -m venv .venv`

## 설치

```bash
cd quick_draw_dl
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
````

## 파일 & 폴더

```
quick_draw_dl/
├─ data/                   # 전처리된 .npy 파일 저장 디렉터리
├─ best-model.keras        # 학습된 Keras 모델
├─ category.npy            # 클래스 이름 배열
├─ preprocessing.py        # NDJSON → NumPy 변환 스크립트
├─ train.py                # CNN 모델 학습 스크립트
├─ predict.py              # 이미지 단일 추론 스크립트
├─ requirements.txt
└─ sample.jpg              # 테스트용 예제 그림
```

## 워크플로우

1. **데이터 전처리**

   ```bash
   python preprocessing.py \
     --input-dir ./raw_ndjson/ \
     --output-dir ./data/
   ```

   * Quick, Draw! 원본 NDJSON 파일을 `data/` 하위 `.npy`로 변환합니다.

2. **모델 학습**

   ```bash
   python train.py \
     --data-dir ./data/ \
     --model-out ./best-model.keras
   ```

   * CNN 아키텍처 학습 후 `best-model.keras`를 생성합니다.

3. **추론**

   ```bash
   python predict.py \
     --model-path ./best-model.keras \
     --labels ./category.npy \
     --image ./sample.jpg
   ```

   * 상위 5개 클래스와 확률을 JSON으로 콘솔 출력합니다.

---

## 배포(Deployment) 가이드

### 1. 서버 준비

```bash
sudo apt update
sudo apt install -y python3.12-venv python3-pip git
sudo pip3 config set global.break-system-packages true
```

### 2. (선택) 스왑 파일 설정

```bash
sudo dd if=/dev/zero of=/swapfile bs=128M count=32
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

### 3. 가상환경 및 의존성 설치

```bash
cd quick_draw_dl
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 4. 스크립트 실행

```bash
# 데이터 전처리
python preprocessing.py --input-dir ./raw_ndjson/ --output-dir ./data/

# 모델 학습
python train.py --data-dir ./data/ --model-out ./best-model.keras

# 추론
python predict.py --model-path ./best-model.keras --labels ./category.npy --image ./sample.jpg
```

```
```
