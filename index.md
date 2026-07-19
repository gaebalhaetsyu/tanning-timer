<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>포스트잇 응원 타이머</title>
  <style>
    /* =========================
       1. 기본 색상과 공통 설정
       ========================= */
    :root {
      --page-bg: #f5f1ea;
      --paper: #fff3a8;
      --paper-dark: #ead66f;
      --paper-line: rgba(122, 101, 40, 0.13);
      --ink: #3e372c;
      --muted: #766d5d;
      --button: #fff9cf;
      --button-border: #8d7f62;
      --accent: #f49cae;
      --shadow: rgba(78, 63, 35, 0.18);
    }

    * {
      box-sizing: border-box;
    }

    body {
      min-height: 100vh;
      margin: 0;
      display: grid;
      place-items: center;
      padding: 18px;
      color: var(--ink);
      font-family: Arial, "Malgun Gothic", sans-serif;
      background:
        radial-gradient(circle at 20% 15%, rgba(244, 156, 174, 0.13), transparent 26%),
        var(--page-bg);
    }

    /* =========================
       2. 전체 위젯 (3:4 비율)
       ========================= */
    .widget {
      position: relative;
      width: min(92vw, 420px);
      aspect-ratio: 3 / 4;
      min-height: 520px;
      max-height: 760px;
      overflow: hidden;
      padding: 30px 26px 26px;
      border: 2px solid rgba(108, 90, 51, 0.22);
      border-radius: 12px 12px 24px 12px;
      background:
        repeating-linear-gradient(
          0deg,
          transparent 0 35px,
          var(--paper-line) 35px 36px
        ),
        var(--paper);
      box-shadow:
        10px 14px 0 var(--shadow),
        inset 0 1px 0 rgba(255,255,255,0.55);
      transition:
        background-color 220ms ease,
        transform 220ms ease;
    }

    /* 메모패드 윗부분 접착 영역 */
    .widget::before {
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      height: 20px;
      background: rgba(230, 201, 83, 0.48);
      border-bottom: 1px dashed rgba(116, 93, 38, 0.18);
    }

    .widget.finished {
      animation: finish-pop 520ms ease;
    }

    @keyframes finish-pop {
      0%   { transform: scale(1); }
      45%  { transform: scale(1.025) rotate(-0.6deg); }
      100% { transform: scale(1); }
    }

    /* =========================
       3. 제목과 타이머
       ========================= */
    .title-row {
      display: grid;
      grid-template-columns: minmax(86px, 1fr) auto minmax(86px, 1fr);
      align-items: center;
      gap: 10px;
      margin: 6px 0 14px;
    }

    .title {
      grid-column: 2;
      margin: 0;
      text-align: center;
      font-size: clamp(0.95rem, 3.7vw, 1.25rem);
      font-weight: 900;
      letter-spacing: 0.05em;
      white-space: nowrap;
    }

    .level-box {
      grid-column: 1;
      justify-self: start;
      min-width: 82px;
      text-align: center;
      font-size: 0.72rem;
      font-weight: 900;
      line-height: 1.35;
      color: var(--muted);
    }

    .level-box strong {
      display: block;
      color: var(--ink);
      font-size: 0.9rem;
    }

    .header-record-button {
      grid-column: 3;
      justify-self: start;
      align-self: center;
      min-height: 34px;
      padding: 5px 10px;
      margin-left: 12px;
      font-size: 0.74rem;
      white-space: nowrap;
      background: #fff1b4;
    }

    .timer-panel {
      display: grid;
      place-items: center;
      min-height: 138px;
      padding: 14px 12px;
      border: 3px solid var(--ink);
      border-radius: 14px;
      background: rgba(255,255,255,0.32);
      box-shadow: 5px 6px 0 rgba(81, 67, 39, 0.12);
    }

    .time {
      margin: 0;
      text-align: center;
      font-size: clamp(3.7rem, 18vw, 6.3rem);
      font-weight: 900;
      line-height: 1;
      font-variant-numeric: tabular-nums;
      letter-spacing: -0.04em;
    }

    .status {
      min-height: 1.4em;
      margin: 10px 0 0;
      text-align: center;
      color: var(--muted);
      font-size: 0.9rem;
      font-weight: 800;
    }

    .status.complete {
      color: var(--ink);
    }

    /* =========================
       4. 시간 입력 구역
       ========================= */
    .input-row {
      display: grid;
      grid-template-columns: repeat(2, auto);
      gap: 10px;
      width: max-content;
      margin: 0;
      font-weight: 800;
      text-align: center;
    }

    .time-field {
      display: grid;
