# Pro Motion · phim motion graphics "âm thanh trước" với Claude Code

[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | Tiếng Việt | [हिन्दी](README.hi.md)

**Đưa cho Claude một sản phẩm (kèm một bản nhạc, hoặc để nó lên kế hoạch luôn phần nhạc). Bạn nhận lại phim ra mắt, phim thương hiệu hay TVC được thiết kế thật sự chứ không dùng template — render bằng code, cắt đúng nhịp, chỉ trong vài phút.**

<table><tr>
<td align="center"><img src="docs/showcase-launch.gif" width="260"><br><sub><b>Phim ra mắt</b> · giao diện sản phẩm, hiệu ứng biết "diễn", bản cho 5 ngôn ngữ</sub></td>
<td align="center"><img src="docs/showcase-brand.gif" width="260"><br><sub><b>Phim thương hiệu</b> · hoạt hình cách điệu, đồ vật hóa thành nhạc cụ</sub></td>
<td align="center"><img src="docs/demo.gif" width="260"><br><sub><b>Template khởi đầu</b> · bản demo 8 giây có sẵn khi cài đặt</sub></td>
</tr></table>

<sub>Các phim được làm bằng skill này cho <b>Tutti</b>, một ứng dụng chơi đùa với ảnh. Bản đầy đủ được đính kèm trong <a href="https://github.com/jack-reson8/pro-motion/releases">bản phát hành mới nhất</a>.</sub>

Phần lớn "video AI" hiện nay hoặc là template (điện thoại 3D + huy hiệu App Store), hoặc là clip do AI sinh ra mà bạn không thể chỉ đạo. Pro Motion đi con đường thứ ba: **nhạc được lên kế hoạch trước như một tài sản marketing, rồi Claude viết phim thành code** — một hàm `render(t)` mang tính tất định trên canvas. Nhờ vậy mỗi cú hit rơi đúng nốt nhạc 1/16, mỗi cú chuyển cảnh đều được thiết kế, và bất kỳ cảnh quay nào cũng có thể sửa theo mốc thời gian rồi render lại trong vài giây.

Đây là phương pháp được chắt lọc từ một dự án thật, đã trải qua khoảng 10 vòng nhận xét kiểu "trông như PowerPoint", "chất liệu xấu", "chữ lơ lửng trên hình", "tôi bảo cách điệu chứ không phải pop art" — cho đến khi bản cắt được chấp nhận. **Chính phương pháp tạo ra khác biệt, nên phương pháp là thứ skill này đóng gói.**

## Skill làm gì

| Bước | Claude làm gì |
|---|---|
| 1 · Brief | Chốt sản phẩm, đối tượng, thị trường, nhạc, tỉ lệ khung hình, loại phim — tự quyết giá trị mặc định thay vì tra hỏi |
| 1b · Chiến dịch + âm thanh | Bảng làm việc `campaign.md`: sáu ý tưởng "chiêu" từ 12 đòn bẩy marketing phi truyền thống (phương tiện bị "hỏng", đối thoại với biển quảng cáo bên cạnh, biến hạn chế thành trò đùa, linh vật phi lý, trả lời cá nhân hóa hàng loạt, đội quân UGC, FOOH…), chấm điểm rồi chọn một. Sau đó là lõi chiến dịch (sự thật · căng thẳng · cơ chế · kênh), kế hoạch âm thanh với sonic logo và hook, một prompt Suno/Udio chính xác, rồi `audio_post.py` để làm SFX khớp nhịp, stutter, loop và loudness |
| 2 · Nhạc | `analyze_music.py` → tempo, lưới nhịp, onset, các cú hit trầm/cao, và spectrogram có vẽ sẵn lưới nhịp. Spectrogram chính là bảng dựng phim |
| 3 · Nghiên cứu | Tìm bản đăng chính thức, dùng yt-dlp (không cần trình duyệt) lấy **contact sheet** 24 khung hình, và viết một **technique sheet** cho từng tham chiếu |
| 4 · Định hướng | **Ba style frame khác nhau rõ rệt** dựng từ các kỹ thuật có tên gọi — bạn chọn trước khi bắt đầu làm chuyển động |
| 5 · Câu chuyện | Một **nhân vật chính** biến hình qua mọi cảnh; mỗi tính năng tự "diễn" theo một sự kiện âm nhạc; mọi chuyển cảnh đều có thiết kế; chữ là một phần của chuyển động; đoạn kết lớn dần thành logo |
| 6 · Dựng | `motion-lib.js`: easing và spring, squash & stretch, camera nhịp điệu điều khiển bằng onset, nét vẽ tay rung 12 fps, chữ như đang được viết bằng bút, metaball, halftone, sọc, vòng thẻ 3D, viền sticker, tách chủ thể (macOS Vision), chuyển đổi đa ngôn ngữ — cùng hơn 40 hiệu ứng canvas tất định tương đương thủ pháp của AE / Blender / anime: hạt curl-noise, hạt tụ lại thành logo, Saber glow, shine, whip pan, iris, mực loang, vỡ vụn, zoom xuyên qua, xé giấy, morph, khung truyện tranh, zoom vô tận, RGB split, glitch, dither, ASCII, riso, giấy cắt, toon shading, hatching, màu nước, slit-scan, echo, speed ramp. `fx-demo.html` chiếu từng hiệu ứng trong một giây |
| 7 · Render | `render-fast.js`: nhiều tab headless chạy song song → khung JPEG → ghép một lần bằng ffmpeg. Khoảng 30 giây cho 20 giây phim ở 60 fps. Kiểm tra contact sheet và độ dài âm thanh |
| 8 · Lặp lại | Góp ý theo mốc thời gian; góp ý về gu thẩm mỹ thì quay lại bước nghiên cứu và định hướng, không vá víu |

<p align="center"><img src="docs/spectrogram-example.png" width="720" alt="Phân tích nhạc: spectrogram, đường bao biên độ và lưới nhịp"></p>

## Cài đặt

**Dưới dạng plugin Claude Code (khuyến nghị)**

Trong một phiên làm việc: `/plugin install pro-motion --marketplace jack-reson8/pro-motion` — hoặc từ shell:

```bash
claude plugin marketplace add jack-reson8/pro-motion
claude plugin install pro-motion@pro-motion
```

**Dưới dạng skill thông thường**

```bash
git clone https://github.com/jack-reson8/pro-motion
cp -R pro-motion/plugins/pro-motion/skills/pro-motion ~/.claude/skills/
```

Yêu cầu: Node 18+, Python 3 với `numpy scipy matplotlib`, Chrome/Chromium (tự động phát hiện, hoặc đặt `CHROME=/path`). Tách chủ thể cần macOS 14+. Mọi thứ còn lại được `scripts/setup.sh` cài theo từng dự án.

**Codex** (CLI hoặc ứng dụng ChatGPT cho máy tính):

```bash
codex plugin marketplace add jack-reson8/pro-motion
codex plugin add pro-motion@pro-motion
```

Hoặc sao chép `plugins/pro-motion/skills/pro-motion` vào `~/.codex/skills/`. Skill tuân theo chuẩn mở [Agent Skills](https://agentskills.io), nên chạy được trong Claude Code, Codex và mọi agent đọc được SKILL.md.

**Các agent khác** (Cursor, Gemini CLI, GitHub Copilot và các agent khác được skills CLI hỗ trợ):

```bash
npx skills add jack-reson8/pro-motion
```

## Cách dùng

Chỉ cần nhờ, kèm một bản nhạc:

> Làm video ra mắt 20 giây cho app chỉnh ảnh của tôi, cắt theo `~/Downloads/track.mp3`. Dọc, bản gốc tiếng Anh, thêm bản tiếng Nhật và tiếng Hàn.

> 用这首歌给我的品牌做一支强烈风格化的品牌片，先给我三个风格方向的风格帧。

> Đoạn giây 8–10 trông như slideshow — thiết kế lại các chuyển cảnh ở đó.

Claude làm theo skill: phân tích nhạc, nghiên cứu bằng contact sheet, đưa bạn xem style frame, viết `shotmap.md`, dựng `film.js`, render ra `out/*.mp4`, kiểm tra bảng khung hình rồi mở kết quả cho bạn.

## Bên trong có gì

```
plugins/pro-motion/skills/pro-motion/
├── SKILL.md                    # phương pháp (Claude đọc)
├── scripts/
│   ├── setup.sh                # thiết lập dự án bằng một lệnh
│   ├── analyze_music.py        # tempo, lưới nhịp, onset, spectrogram
│   ├── audio_post.py           # SFX khớp nhịp, stutter, loop, loudness
│   ├── render-fast.js          # render song song, chế độ ảnh tĩnh, ghép âm thanh
│   ├── contact_sheet.sh        # khung hình tại các giây chỉ định → một ảnh
│   ├── ref_frames.sh           # video tham chiếu → bảng 24 khung hình (yt-dlp)
│   ├── cutout.swift            # mặt nạ chủ thể (macOS Vision)
│   └── faces.swift             # khung khuôn mặt (macOS Vision)
├── assets/template/            # campaign.md · audio-plan.md · film.html · film.js (bản demo chạy được) · motion-lib.js · fx-demo.html · font OFL
└── references/
    ├── unconventional-marketing.md  # 12 đòn bẩy "chiêu" → các bước → ví dụ → cách áp dụng
    ├── campaign-cases.md       # các cơ chế ra mắt đã lan truyền
    ├── audio-first.md          # lõi chiến dịch, kế hoạch âm thanh, prompt cho công cụ sinh nhạc
    ├── vfx-techniques.md       # chuyển cảnh, nhịp điệu, hiệu ứng AE / Blender / cách điệu: nguồn · tham số · lệnh gọi thư viện
    ├── sourcing.md             # cách tìm và đọc tham chiếu
    ├── research-notes.md       # technique sheet mẫu
    ├── aesthetic-checklist.md  # câu chuyện · chuyển động · chữ · chất liệu · ngôn ngữ · bàn giao
    └── lessons.md              # các lỗi kỹ thuật và sai lầm về gu đã phải trả giá
```

## Nguyên tắc (bản ngắn)

1. **Nhạc chính là bản dựng.** Đọc spectrogram; mỗi chuyển động đáp đúng vào một sự kiện đã được đánh dấu.
2. **Nhìn khung hình thật, đừng thiết kế theo trí nhớ.**
3. **Chọn hướng đi từ các style frame,** không phải từ liên tưởng đầu tiên hay thứ dễ code nhất.
4. **Một nhân vật chính, hiệu ứng biết diễn, chuyển cảnh được thiết kế** — nếu không, phim sẽ giống PowerPoint.
5. **Chữ là chuyển động.** Không thanh phụ đề dưới, không chip, không hộp.
6. **Chất liệu xấu thì phim xấu.** Dùng chất liệu thật, mới, an toàn bản quyền — hoặc để trống.
7. **Render trong vài giây, kiểm tra một contact sheet, bàn giao.** Lặp lại theo mốc thời gian.

## Giấy phép

Code và tài liệu: MIT (xem `LICENSE`). Font đi kèm: SIL Open Font License 1.1 (Patrick Hand, Yomogi, Gaegu, ZCOOL KuaiLe) — xem `assets/template/assets/fonts/LICENSES.md`. Các tham chiếu tải về bằng `ref_frames.sh` chỉ dùng để học riêng; đừng phân phối lại.

Hoan nghênh đóng góp — đặc biệt là các thành phần chuyển động mới, chuyển cảnh, technique sheet và bài học kinh nghiệm. Xem `CONTRIBUTING.md`.
