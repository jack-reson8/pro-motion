# Pro Motion · Claude Code로 만드는 오디오 퍼스트 모션그래픽 영상

[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | 한국어 | [Tiếng Việt](README.vi.md) | [हिन्दी](README.hi.md)

**Claude에게 제품을 주세요(곡을 함께 주거나, 곡 기획부터 맡겨도 됩니다). 템플릿이 아니라 제대로 설계된 런칭 영상, 브랜드 영상, TVC를 코드로 렌더링해 비트에 맞춰 편집한 결과물을 몇 분 안에 받아볼 수 있습니다.**

<table><tr>
<td align="center"><img src="docs/showcase-launch.gif" width="260"><br><sub><b>런칭 영상</b> · 제품 UI, 연기하는 이펙트, 5개 언어 버전</sub></td>
<td align="center"><img src="docs/showcase-brand.gif" width="260"><br><sub><b>브랜드 영상</b> · 스타일라이즈된 애니메이션, 사물이 악기가 된다</sub></td>
<td align="center"><img src="docs/demo.gif" width="260"><br><sub><b>스타터 템플릿</b> · 설치하면 바로 들어 있는 8초 데모</sub></td>
</tr></table>

<sub>사진 놀이 앱 <b>Tutti</b>를 위해 이 스킬로 만든 영상입니다. 풀 버전은 <a href="https://github.com/jack-reson8/pro-motion/releases">최신 릴리스</a>에 첨부되어 있습니다.</sub>

요즘 대부분의 "AI 영상"은 템플릿(3D 폰 + App Store 배지)이거나, 연출을 통제할 수 없는 생성 클립입니다. Pro Motion은 세 번째 길을 갑니다. **먼저 음악을 마케팅 자산으로 기획하고, 그다음 Claude가 영상을 코드로 작성합니다.** 캔버스 위의 결정론적 `render(t)`입니다. 그래서 모든 히트가 16분음표 단위로 음악에 맞고, 모든 트랜지션이 설계되어 있으며, 어떤 샷이든 타임스탬프로 지정해 수정하고 몇 초 만에 다시 렌더링할 수 있습니다.

이 스킬은 실제 프로젝트에서 "PPT 같다", "소재가 별로다", "글자가 화면 위에 둥둥 떠 있다", "스타일라이즈하라고 했지 팝아트를 하라는 게 아니다"라는 피드백을 약 10라운드 받으며 컷이 승인될 때까지 다듬은 방법을 정리한 것입니다. **결과를 바꾼 것은 방법이므로, 이 스킬이 담고 있는 것도 방법입니다.**

## 하는 일

| 단계 | Claude가 하는 일 |
|---|---|
| 1 · 브리프 | 제품, 타깃, 시장, 음악, 화면비, 영상 유형을 확정합니다. 꼬치꼬치 묻지 않고 기본값을 스스로 정합니다 |
| 1b · 캠페인 + 오디오 | `campaign.md` 워크시트: 12가지 비정통 마케팅 레버(매체가 고장 난다, 옆 광고판과의 대화, 제약을 개그로 바꾼다, 황당한 마스코트, 대규모 개인 맞춤 답글, UGC 군단, FOOH…)에서 6개 아이디어를 내고 점수를 매겨 하나를 고릅니다. 이어서 캠페인 핵심(진실 · 긴장 · 장치 · 채널), 사운드 로고와 훅이 담긴 오디오 계획, 정밀한 Suno/Udio 프롬프트 하나를 만들고, 마지막으로 `audio_post.py`로 정확히 비트에 맞는 SFX, 스터터, 루프, 라우드니스를 다듬습니다 |
| 2 · 음악 | `analyze_music.py` → 템포, 비트 그리드, 온셋, 저역/고역 히트, 그리드를 그려 넣은 스펙트로그램. 스펙트로그램이 곧 편집표입니다 |
| 3 · 리서치 | 공식 업로드를 찾고, yt-dlp(브라우저 불필요)로 24프레임 **컨택트 시트**를 추출하고, 레퍼런스마다 **테크닉 시트**를 작성합니다 |
| 4 · 디렉션 | 이름이 있는 기법들로 **확실히 다른 스타일 프레임 3개**를 만듭니다. 애니메이션을 시작하기 전에 직접 고르시면 됩니다 |
| 5 · 스토리 | 모든 장면을 거치며 변신하는 **주인공** 하나. 모든 기능은 음악 이벤트에 맞춰 스스로 연기하고, 모든 트랜지션은 설계되며, 텍스트도 애니메이션이고, 엔딩은 로고로 자라납니다 |
| 6 · 빌드 | `motion-lib.js`: 이징과 스프링, 스쿼시 & 스트레치, 온셋으로 움직이는 비트 카메라, 12fps 낙서 라인 보일, 손으로 쓰는 텍스트, 메타볼, 하프톤, 스트라이프, 3D 카드 링, 스티커 외곽선, 피사체 누끼(macOS Vision), 다국어 전환. 여기에 AE / Blender / 애니메이션 연출을 결정론적 canvas로 옮긴 40여 가지가 더해집니다: 컬 노이즈 파티클, 로고로 모이는 파티클, Saber 글로우, 샤인, 휩 팬, 아이리스, 잉크 번짐, 산산조각, 줌 스루, 종이 찢기, 모프, 만화 컷 분할, 무한 줌, RGB 분리, 글리치, 디더, ASCII, 리소, 종이 오리기, 툰 셰이딩, 해칭, 수채화, 슬릿 스캔, 에코, 스피드 램프. `fx-demo.html`에서 각 효과를 1초씩 볼 수 있습니다 |
| 7 · 렌더 | `render-fast.js`: 병렬 헤드리스 탭 → JPEG 프레임 → ffmpeg 한 번에 먹싱. 60fps 20초 영상이 약 30초. 컨택트 시트와 오디오 길이를 점검합니다 |
| 8 · 반복 | 피드백은 타임스탬프 단위로 반영합니다. 취향에 대한 피드백은 땜질이 아니라 리서치와 디렉션 단계로 되돌립니다 |

<p align="center"><img src="docs/spectrogram-example.png" width="720" alt="음악 분석: 스펙트로그램, 엔벨로프, 비트 그리드"></p>

## 설치

**Claude Code 플러그인으로 설치(권장)**

세션에서 `/plugin install pro-motion --marketplace jack-reson8/pro-motion`을 실행하거나, 셸에서 다음 명령을 사용하세요.

```bash
claude plugin marketplace add jack-reson8/pro-motion
claude plugin install pro-motion@pro-motion
```

**일반 스킬로 설치**

```bash
git clone https://github.com/jack-reson8/pro-motion
cp -R pro-motion/plugins/pro-motion/skills/pro-motion ~/.claude/skills/
```

요구 사항: Node 18 이상, `numpy scipy matplotlib`이 설치된 Python 3, Chrome/Chromium(자동 감지되며 `CHROME=/path`로 지정할 수도 있습니다). 피사체 누끼에는 macOS 14 이상이 필요합니다. 나머지는 모두 `scripts/setup.sh`가 프로젝트별로 설치합니다.

**Codex** (CLI 또는 ChatGPT 데스크톱 앱):

```bash
codex plugin marketplace add jack-reson8/pro-motion
codex plugin add pro-motion@pro-motion
```

또는 `plugins/pro-motion/skills/pro-motion`을 `~/.codex/skills/`에 복사하세요. 이 스킬은 오픈 [Agent Skills](https://agentskills.io) 표준을 따르므로 Claude Code, Codex, 그리고 SKILL.md를 읽는 모든 에이전트에서 동작합니다.

**기타 에이전트** (Cursor, Gemini CLI, GitHub Copilot 등 skills CLI가 지원하는 에이전트):

```bash
npx skills add jack-reson8/pro-motion
```

## 사용법

곡과 함께 그냥 요청하면 됩니다.

> 내 사진 앱의 20초 런칭 영상을 `~/Downloads/track.mp3`에 맞춰서 만들어줘. 세로 화면, 영어 마스터에 일본어와 한국어 버전도.

> 用这首歌给我的品牌做一支强烈风格化的品牌片，先给我三个风格方向的风格帧。

> 8~10초 구간이 슬라이드쇼 같아. 거기 트랜지션을 다시 설계해줘.

Claude는 스킬의 순서를 따릅니다. 음악을 분석하고, 컨택트 시트로 리서치하고, 스타일 프레임을 보여주고, `shotmap.md`를 쓰고, `film.js`를 만들고, `out/*.mp4`를 렌더링하고, 프레임 시트를 점검한 뒤 결과를 열어 보여줍니다.

## 구성

```
plugins/pro-motion/skills/pro-motion/
├── SKILL.md                    # 방법론 본체(Claude가 읽음)
├── scripts/
│   ├── setup.sh                # 명령 한 줄로 프로젝트 설정
│   ├── analyze_music.py        # 템포, 그리드, 온셋, 스펙트로그램
│   ├── audio_post.py           # 정확히 비트에 맞는 SFX, 스터터, 루프, 라우드니스
│   ├── render-fast.js          # 병렬 렌더, 스틸 모드, 오디오 먹싱
│   ├── contact_sheet.sh        # 지정한 초의 프레임 → 이미지 한 장
│   ├── ref_frames.sh           # 레퍼런스 영상 → 24프레임 시트(yt-dlp)
│   ├── cutout.swift            # 피사체 마스크(macOS Vision)
│   └── faces.swift             # 얼굴 박스(macOS Vision)
├── assets/template/            # campaign.md · audio-plan.md · film.html · film.js (실행 가능한 데모) · motion-lib.js · fx-demo.html · OFL 폰트
└── references/
    ├── unconventional-marketing.md  # 12가지 비정통 마케팅 레버 → 단계 → 사례 → 적용법
    ├── campaign-cases.md       # 실제로 퍼진 런칭 메커니즘 사례
    ├── audio-first.md          # 캠페인 핵심, 오디오 계획, 생성기 프롬프트
    ├── vfx-techniques.md       # 트랜지션, 리듬, AE / Blender / 스타일라이즈 룩: 출처 · 파라미터 · 라이브러리 호출
    ├── sourcing.md             # 레퍼런스 찾고 읽는 법
    ├── research-notes.md       # 테크닉 시트 실전 예시
    ├── aesthetic-checklist.md  # 스토리 · 모션 · 타이포 · 질감 · 언어 · 납품
    └── lessons.md              # 이미 값을 치른 버그와 취향 실수 모음
```

## 원칙(요약)

1. **음악이 곧 편집이다.** 스펙트로그램을 읽고, 모든 움직임을 매핑된 이벤트에 맞춘다.
2. **실제 프레임을 보라. 기억에 의존해 디자인하지 마라.**
3. **스타일 프레임으로 방향을 고른다.** 처음 떠오른 연상이나 코드로 짜기 쉬운 쪽을 고르지 않는다.
4. **주인공 하나, 연기하는 이펙트, 설계된 트랜지션.** 그렇지 않으면 PPT처럼 보인다.
5. **텍스트는 애니메이션이다.** 하단 자막바, 칩, 박스는 쓰지 않는다.
6. **못난 소재는 못난 영상을 만든다.** 실제의, 지금의, 라이선스에 안전한 소재를 쓰거나, 아니면 여백으로 둔다.
7. **몇 초 만에 렌더링하고, 컨택트 시트 한 장으로 점검하고, 납품한다.** 이후에는 타임스탬프 단위로 반복한다.

## 라이선스

코드와 문서: MIT(`LICENSE` 참조). 포함된 폰트: SIL Open Font License 1.1(Patrick Hand, Yomogi, Gaegu, ZCOOL KuaiLe) — `assets/template/assets/fonts/LICENSES.md`를 참고하세요. `ref_frames.sh`로 내려받은 레퍼런스는 개인 학습용으로만 사용하고 재배포하지 마세요.

기여를 환영합니다. 특히 새로운 모션 프리미티브, 트랜지션, 테크닉 시트, 교훈을 환영합니다. `CONTRIBUTING.md`를 참고하세요.
