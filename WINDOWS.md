# Windows x64 빌드 — 완료 (2026-07-07)

GenTube의 `resources/ffmpeg-win/`은 **web-brain/FFmpeg-Builds**(BtbN/FFmpeg-Builds 포크)에서 빌드한
win64-gpl 정적 바이너리로 교체됨. 기존 gyan.dev 빌드(빌드 스크립트 비공개 → 대응 소스 제공 불가)를 대체.

## 빌드 소스 (GPL corresponding source)

- **포크 리포**: https://github.com/web-brain/FFmpeg-Builds (퍼블릭)
  - 전 의존성이 `scripts.d/*.sh`에 커밋 단위로 핀 고정(SCRIPT_COMMIT/SCRIPT_REV) → 완전 재현 가능
  - gyan.dev와 결정적 차이: **빌드 정의가 전부 공개·핀 고정**되어 누구나 동일 소스 세트 재구성 가능
- **릴리스**: autobuild-2026-07-07-06-11 (`ffmpeg-N-125483-g160737cf0d-win64-gpl.zip`)
  - FFmpeg 소스 커밋: `160737cf0d` (바이너리명에 포함)
- 포크에서 win64-gpl/lgpl만 빌드하도록 매트릭스 트림 (win32/arm/linux 제거)

## 검증 (mac에서 실행 없이 정적 검증 — win 바이너리라 로컬 실행 불가)

- ✅ PE32+ x86-64 (`file`)
- ✅ **완전 정적**: PE import table(objdump -p) — import 38개 전부 Windows 시스템 DLL(kernel32/user32/DWrite/d2d1 등), ffmpeg 종속 DLL(libav*/libx264/libmp3lame/zlib 등) **0개**. mac에서 겪은 dylib 누출과 달리 BtbN win 빌드는 설계상 정적.
- ✅ buildconf(strings): `--enable-gpl --enable-libx264 --enable-libmp3lame`, `--enable-nonfree` **없음**(재배포 가능)
- ✅ libass 스택(libass/libfreetype/libharfbuzz/libfribidi/fontconfig) + 필터(subtitles/zoompan/vignette/loudnorm) 존재
- ✅ libunibreak: libass 내부 의존(BtbN 45-libunibreak.sh→50-libass.sh)으로 CJK 줄바꿈 지원 (mac 이슈와 동일하게 해결)
- ⚠️ **실제 렌더 확인은 Windows 기기 필요** (mac에선 .exe 실행 불가)

## 후속 (선택)

- GPL 대응 소스 강화: 포크가 공개·핀 고정이라 재현 가능하나, 실제 소스 타르볼 아카이브(download.sh 산출)를 릴리스에 첨부하면 더 견고
- 기존 win 바이너리 백업: /tmp/ffmpeg-win-old-backup (구 gyan 빌드)
- 앱 고지(T7): win 소스 링크 = https://github.com/web-brain/FFmpeg-Builds
