<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kokum &amp; Co. | Maharashtrian Kitchen, Pune</title>
  <meta name="description" content="Home-style Maharashtrian food in Pune. See the menu and reserve a table.">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&family=Fraunces:opsz,wght@9..144,600;9..144,800&display=swap" rel="stylesheet">
  <style>
/* ---------- Design tokens ---------- */
:root {
  --green: #12332d;
  --green-2: #1d4a41;
  --turmeric: #e9a920;
  --chili: #b8321f;
  --paper: #f7f8f4;
  --ink: #1b1b18;
  --muted: #5c645f;
  --radius: 14px;
  --font-display: "Fraunces", Georgia, serif;
  --font-body: "DM Sans", system-ui, -apple-system, "Segoe UI", sans-serif;
}

* { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; scroll-padding-top: 70px; }
body { font-family: var(--font-body); color: var(--ink); background: var(--paper); line-height: 1.6; }
h1, h2, h3 { font-family: var(--font-display); line-height: 1.15; }
a { color: inherit; text-decoration: none; }
button { font: inherit; cursor: pointer; }
:focus-visible { outline: 3px solid var(--turmeric); outline-offset: 3px; }

/* ---------- Buttons ---------- */
.btn {
  display: inline-block; background: var(--turmeric); color: var(--ink);
  padding: 12px 24px; border: 2px solid var(--turmeric); border-radius: 999px;
  font-weight: 700; transition: transform .2s, background .2s;
}
.btn:hover { transform: translateY(-3px); background: #f3bb3d; }
.btn-ghost { background: transparent; color: #fff; border-color: rgba(255,255,255,.5); }
.btn-ghost:hover { background: rgba(255,255,255,.1); }
.btn-small { padding: 8px 18px; }
.btn-full { width: 100%; font-size: 1rem; }

/* ---------- Navbar ---------- */
.nav {
  position: fixed; inset: 0 0 auto 0; z-index: 50; height: 64px;
  display: flex; align-items: center; justify-content: space-between;
  padding: 0 5%; background: var(--green); color: #fff;
}
.logo { font-family: var(--font-display); font-weight: 800; font-size: 1.35rem; }
.nav-links { display: flex; align-items: center; gap: 28px; }
.nav-links a:not(.btn) { opacity: .85; transition: opacity .2s; }
.nav-links a:not(.btn):hover { opacity: 1; }
.nav-toggle { display: none; background: none; border: 0; color: #fff; font-size: 1.7rem; }

/* ---------- Hero ---------- */
.hero {
  min-height: 100vh; padding: 110px 5% 60px; background: var(--green); color: #fff;
  display: grid; grid-template-columns: 1.2fr 1fr; align-items: center; gap: 30px;
}
.hero-kicker { color: var(--turmeric); font-weight: 500; margin-bottom: 14px; }
.hero h1 { font-size: clamp(2.6rem, 6vw, 4.8rem); font-weight: 800; margin-bottom: 20px; }
.hero-sub { max-width: 46ch; opacity: .85; margin-bottom: 28px; }
.hero-actions { display: flex; gap: 14px; flex-wrap: wrap; }

/* Page-load sequence: the hero text slides in one line after another */
.hero-text > * { opacity: 0; animation: rise .7s ease forwards; }
.hero-text > :nth-child(1) { animation-delay: .1s; }
.hero-text > :nth-child(2) { animation-delay: .25s; }
.hero-text > :nth-child(3) { animation-delay: .4s; }
.hero-text > :nth-child(4) { animation-delay: .55s; }
@keyframes rise { from { opacity: 0; transform: translateY(26px); } to { opacity: 1; transform: none; } }

/* Steaming bowl, built from CSS only */
.bowl-wrap { position: relative; width: min(320px, 70vw); height: 300px; margin: 0 auto; }
.bowl {
  position: absolute; bottom: 20px; left: 0; right: 0; height: 150px;
  background: linear-gradient(180deg, var(--chili) 0 22%, var(--turmeric) 22% 100%);
  border-radius: 0 0 160px 160px / 0 0 150px 150px;
  box-shadow: inset 0 -14px 0 rgba(0,0,0,.15);
  animation: bob 4s ease-in-out infinite;
}
.bowl::before { /* bowl rim */
  content: ""; position: absolute; top: -10px; left: -6px; right: -6px; height: 20px;
  background: #fff; border-radius: 999px;
}
.steam {
  position: absolute; bottom: 150px; width: 14px; height: 90px; border-radius: 999px;
  background: linear-gradient(to top, rgba(255,255,255,.55), transparent);
  opacity: 0; animation: steam 3s ease-in infinite;
}
.s1 { left: 36%; } .s2 { left: 50%; animation-delay: 1s; } .s3 { left: 64%; animation-delay: 2s; }
@keyframes steam {
  0% { opacity: 0; transform: translateY(0) scaleX(1); }
  30% { opacity: .9; }
  100% { opacity: 0; transform: translateY(-70px) scaleX(2.2); }
}
@keyframes bob { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-8px); } }

/* ---------- Sections ---------- */
.section { padding: 80px 5%; max-width: 1150px; margin: 0 auto; }
.section h2 { font-size: clamp(1.8rem, 4vw, 2.6rem); margin-bottom: 16px; }
.about p { max-width: 62ch; color: var(--muted); }
.facts { list-style: none; display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 18px; margin-top: 32px; }
.facts li { border-left: 4px solid var(--turmeric); padding: 4px 0 4px 14px; color: var(--muted); }
.facts strong { display: block; color: var(--ink); font-size: 1.05rem; }
.hint { color: var(--muted); margin-bottom: 18px; }

/* ---------- Menu ---------- */
.filters { display: flex; gap: 10px; flex-wrap: wrap; margin: 20px 0 30px; }
.filter { background: #fff; border: 2px solid var(--green); color: var(--green); padding: 8px 20px; border-radius: 999px; font-weight: 500; transition: .2s; }
.filter:hover { background: #e8eeea; }
.filter.active { background: var(--green); color: #
