# Windows x64 빌드 — 최소 구성 완료 (2026-07-08)

GenTube의 `resources/ffmpeg-win/`은 **web-brain/gentube-ffmpeg-win**(BtbN/FFmpeg-Builds 포크)의
**최소 구성** win64-gpl 정적 바이너리. 기존 gyan.dev 빌드(대응 소스 제공 불가) → BtbN 풀빌드(임시)
→ **최소 구성 빌드**(최종) 순으로 교체됨.

## 최소 구성 (2026-07-08 확정)

- FFmpeg **n8.1.2** 태그 핀 고정 (`addins/gentube81.sh`)
- scripts.d 81개 → **15개 트림**: x264 + LAME + libass 스택(freetype/harfbuzz/fribidi/fontconfig/libunibreak/libxml2) + zlib/iconv + mingw 툴체인
- `--enable-version3` 제거 → **GPL-2.0-or-later** (mac 빌드와 등급 통일)
- 크기: exe당 144MB(풀빌드) → **39.6MB**

## 빌드 소스 (GPL corresponding source)

- **포크 리포**: https://github.com/web-brain/gentube-ffmpeg-win (퍼블릭, 구 FFmpeg-Builds에서 개명)
  - 전 의존성이 `scripts.d/*.sh`에 커밋 단위로 핀 고정(SCRIPT_COMMIT) → 완전 재현 가능
- **릴리스**: **v8.1.2-1** (`ffmpeg-n8.1.2-win64-gpl-gentube81.zip`)
  - 정적 링크된 **전 의존성 소스 타르볼 11종 + FFmpeg n8.1.2 소스 + BUILDINFO + GPL 전문 + SHA256SUMS 동봉**
  - → GPLv2 §3 대응 소스 제공 의무 이행 완료 (mac v8.1-2와 동일 방식)

## 검증 (mac에서 정적 검증 — win 바이너리라 로컬 실행 불가)

- ✅ **완전 정적**: PE import table(objdump -p) — 전부 Windows 시스템 DLL, 서드파티 DLL **0개**
- ✅ configure(NUL 종단 직접 추출): `--enable-gpl` + x264/lame/libass 스택만, `--enable-version3`/`--enable-nonfree` **없음**
- ✅ 라이선스 배너: "GPL version 2 or later" / zip 동봉 LICENSE.txt = GPLv2 전문
- ✅ 디코더: configure에 disable 계열 없음 → FFmpeg 내장 디코더 전체 유지
- ✅ `npm run setup:ffmpeg`(gentube)로 릴리스 다운로드→체크섬 검증→배치 자동화, 번들과 sha256 일치 확인
- ⚠️ **실제 렌더 확인은 Windows 기기 필요** (mac에선 .exe 실행 불가)

## 이력

- 기존 win 바이너리 백업: /tmp/ffmpeg-win-old-backup (구 gyan), /tmp/ffmpeg-win-fullbuild-backup (BtbN 풀빌드)
- 앱 고지(T7): win 소스 링크 = https://github.com/web-brain/gentube-ffmpeg-win/releases
