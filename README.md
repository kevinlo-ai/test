<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TIS Phantom Shadow — FLL Team</title>
    <link rel="icon" href="images/Tis Jesters Logo.png" type="image/png">
    <link rel="shortcut icon" href="images/Tis Jesters Logo.png" type="image/png">
    <link rel="apple-touch-icon" href="images/Tis Jesters Logo.png" type="image/png">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Sora:wght@400;600;700;800&display=swap" rel="stylesheet">
    <style>
        /* ═══════════════════════════════════════
           ROOT — WARM PALETTE
           ═══════════════════════════════════════ */
        :root {
            --primary: #e25535;
            --primary-soft: rgba(226, 85, 53, .45);
            --amber: #f0a030;
            --amber-soft: rgba(240, 160, 48, .45);
            --gold: #ffd699;
            --warm-orange: #ff8844;
            --rose: #e0506a;
            --terracotta: #d07040;

            --bg-deep: #060302;
            --bg-warm: #0e0806;
            --bg-card: rgba(30, 18, 12, .55);

            --glass-bg: rgba(255, 190, 140, .025);
            --glass-bg-hover: rgba(255, 190, 140, .06);
            --glass-border: rgba(255, 190, 130, .08);
            --glass-border-hover: rgba(255, 175, 110, .22);
            --glass-highlight: rgba(255, 235, 210, .09);
            --glass-inner: rgba(255, 180, 120, .018);
            --glass-blur: 18px;

            --text-primary: #faf0e6;
            --text-secondary: #b8a898;
            --text-dim: #7a6a5a;

            --radius-sm: 16px;
            --radius-md: 24px;
            --radius-lg: 32px;

            --ease-out-expo: cubic-bezier(.16, 1, .3, 1);
            --ease-spring: cubic-bezier(.34, 1.56, .64, 1);
            --micro: cubic-bezier(.4, 0, .2, 1);
        }

        /* ═══════════════════════════════════════
           RESET
           ═══════════════════════════════════════ */
        *,
        *::before,
        *::after {
            margin: 0;
            padding: 0;
            box-sizing: border-box
        }

        html {
            scroll-behavior: smooth;
            scrollbar-width: thin;
            scrollbar-color: rgba(255, 180, 120, .12) transparent;
        }

        ::-webkit-scrollbar {
            width: 5px
        }

        ::-webkit-scrollbar-track {
            background: transparent
        }

        ::-webkit-scrollbar-thumb {
            background: rgba(255, 180, 120, .12);
            border-radius: 3px
        }

        ::-webkit-scrollbar-thumb:hover {
            background: rgba(255, 180, 120, .25)
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            background: var(--bg-deep);
            color: var(--text-primary);
            overflow-x: hidden;
            min-height: 100vh;
        }

        /* ═══════════════════════════════════════
           WARM BACKGROUND BLOBS  (3 only — perf)
           ═══════════════════════════════════════ */
        .bg-blobs {
            position: fixed;
            inset: 0;
            z-index: 0;
            overflow: hidden;
            pointer-events: none;
            contain: strict;
        }

        .blob {
            position: absolute;
            border-radius: 50%;
            filter: blur(90px);
            opacity: .45;
            will-change: transform;
        }

        .blob-1 {
            width: 560px;
            height: 560px;
            background: radial-gradient(circle, var(--primary) 0%, transparent 70%);
            top: -8%;
            left: -4%;
            animation: drift-a 28s ease-in-out infinite;
        }

        .blob-2 {
            width: 480px;
            height: 480px;
            background: radial-gradient(circle, var(--amber) 0%, transparent 70%);
            top: 35%;
            right: -6%;
            animation: drift-b 24s ease-in-out infinite;
        }

        .blob-3 {
            width: 520px;
            height: 520px;
            background: radial-gradient(circle, var(--terracotta) 0%, transparent 70%);
            bottom: 0;
            left: 18%;
            animation: drift-c 30s ease-in-out infinite;
        }

        @keyframes drift-a {

            0%,
            100% {
                transform: translate(0, 0) scale(1)
            }

            50% {
                transform: translate(60px, -40px) scale(1.06)
            }
        }

        @keyframes drift-b {

            0%,
            100% {
                transform: translate(0, 0) scale(1)
            }

            50% {
                transform: translate(-50px, 45px) scale(1.08)
            }
        }

        @keyframes drift-c {

            0%,
            100% {
                transform: translate(0, 0) scale(1)
            }

            50% {
                transform: translate(40px, 50px) scale(.95)
            }
        }

        /* Warm vignette */
        .vignette {
            position: fixed;
            inset: 0;
            z-index: 1;
            pointer-events: none;
            background: radial-gradient(ellipse at center, transparent 50%, var(--bg-deep) 100%);
            opacity: .6;
        }

        /* ═══════════════════════════════════════
           CURSOR GLOW
           ═══════════════════════════════════════ */
        .cursor-glow {
            position: fixed;
            width: 350px;
            height: 350px;
            background: radial-gradient(circle, rgba(240, 160, 48, .07) 0%, transparent 70%);
            border-radius: 50%;
            pointer-events: none;
            transform: translate(-50%, -50%);
            z-index: 1;
            opacity: 0;
            transition: opacity .4s;
        }

        body:hover .cursor-glow {
            opacity: 1
        }

        /* ═══════════════════════════════════════
           LOADING SCREEN
           ═══════════════════════════════════════ */
        #loading-screen {
            position: fixed;
            inset: 0;
            background: var(--bg-deep);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 10000;
            transition: opacity .7s var(--ease-out-expo), visibility .7s;
        }

        #loading-screen.hidden {
            opacity: 0;
            visibility: hidden
        }

        .glass-loader {
            width: 56px;
            height: 56px;
            border: 2px solid var(--glass-border);
            border-top-color: var(--amber);
            border-radius: 50%;
            background: var(--glass-bg);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            animation: spin .9s linear infinite;
            box-shadow: 0 0 30px rgba(240, 160, 48, .18);
        }

        .loader-text {
            margin-top: 20px;
            font-family: 'Sora', sans-serif;
            font-size: .82rem;
            letter-spacing: 4px;
            text-transform: uppercase;
            color: var(--text-dim);
            animation: pulse-text 2s ease-in-out infinite;
        }

        @keyframes spin {
            to {
                transform: rotate(360deg)
            }
        }

        @keyframes pulse-text {

            0%,
            100% {
                opacity: .35
            }

            50% {
                opacity: 1
            }
        }

        /* ═══════════════════════════════════════
           GLASS COMPONENT
           ═══════════════════════════════════════ */
        .glass {
            background: var(--glass-bg);
            backdrop-filter: blur(var(--glass-blur)) saturate(150%);
            -webkit-backdrop-filter: blur(var(--glass-blur)) saturate(150%);
            border: 1px solid var(--glass-border);
            border-radius: var(--radius-md);
            box-shadow:
                0 8px 32px rgba(0, 0, 0, .45),
                inset 0 1px 0 var(--glass-highlight),
                inset 0 0 40px var(--glass-inner),
                inset 0 -1px 0 rgba(0, 0, 0, .15);
            position: relative;
            overflow: hidden;
            transition: all .5s var(--ease-out-expo);
            contain: layout style;
        }

        /* Shimmer sweep */
        .glass::before {
            content: '';
            position: absolute;
            top: 0;
            left: -110%;
            width: 55%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 220, 170, .05), transparent);
            transition: left .75s var(--ease-out-expo);
            pointer-events: none;
            z-index: 1;
        }

        .glass:hover::before {
            left: 110%
        }

        /* Top-edge warm glow strip */
        .glass::after {
            content: '';
            position: absolute;
            top: 0;
            left: 10%;
            right: 10%;
            height: 1px;
            background: linear-gradient(90deg, transparent, rgba(255, 210, 160, .18), transparent);
            pointer-events: none;
            z-index: 1;
            transition: opacity .4s;
        }

        .glass:hover {
            background: var(--glass-bg-hover);
            border-color: var(--glass-border-hover);
            box-shadow:
                0 20px 56px rgba(0, 0, 0, .55),
                0 0 70px rgba(240, 160, 48, .045),
                inset 0 1px 0 rgba(255, 235, 210, .14),
                inset 0 0 50px rgba(255, 180, 120, .025);
            transform: translateY(-4px);
        }

        .glass:hover::after {
            opacity: 1.5
        }

        /* ═══════════════════════════════════════
           GLASS ACCENT — stronger glass for hero cards
           ═══════════════════════════════════════ */
        .glass-accent {
            background: rgba(255, 190, 140, .04);
            border-color: rgba(255, 190, 130, .12);
            box-shadow:
                0 12px 40px rgba(0, 0, 0, .5),
                0 0 60px rgba(240, 160, 48, .03),
                inset 0 1px 0 rgba(255, 235, 210, .12),
                inset 0 0 60px rgba(255, 180, 120, .02);
        }

        .glass-accent:hover {
            border-color: rgba(255, 175, 100, .28);
            box-shadow:
                0 24px 64px rgba(0, 0, 0, .6),
                0 0 90px rgba(240, 160, 48, .06),
                inset 0 1px 0 rgba(255, 235, 210, .2),
                inset 0 0 80px rgba(255, 180, 120, .03);
        }

        /* ═══════════════════════════════════════
           NAVIGATION
           ═══════════════════════════════════════ */
        nav {
            position: fixed;
            top: 16px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 1000;
            background: rgba(14, 8, 6, .65);
            backdrop-filter: blur(28px) saturate(180%);
            -webkit-backdrop-filter: blur(28px) saturate(180%);
            border: 1px solid rgba(255, 190, 130, .07);
            border-radius: 100px;
            padding: 11px 28px;
            box-shadow:
                0 8px 32px rgba(0, 0, 0, .5),
                0 0 40px rgba(240, 160, 48, .025),
                inset 0 1px 0 rgba(255, 230, 200, .07);
            transition: all .4s var(--ease-out-expo);
            max-width: 92vw;
        }

        nav:hover {
            border-color: rgba(255, 190, 130, .14);
            box-shadow:
                0 12px 40px rgba(0, 0, 0, .6),
                0 0 50px rgba(240, 160, 48, .04),
                inset 0 1px 0 rgba(255, 230, 200, .1);
        }

        .nav-inner {
            display: flex;
            align-items: center;
            gap: 10px
        }

        .logo {
            font-family: 'Sora', sans-serif;
            font-size: 1.05rem;
            font-weight: 700;
            letter-spacing: 1px;
            text-decoration: none;
            background: linear-gradient(135deg, var(--primary), var(--amber));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            white-space: nowrap;
            margin-right: 14px;
            transition: filter .3s var(--micro);
        }

        .logo:hover {
            filter: brightness(1.2)
        }

        .nav-divider {
            width: 1px;
            height: 18px;
            background: rgba(255, 190, 130, .1);
            margin: 0 6px
        }

        .nav-links {
            display: flex;
            list-style: none;
            gap: 2px
        }

        .nav-links a {
            color: var(--text-secondary);
            text-decoration: none;
            font-size: .82rem;
            font-weight: 500;
            padding: 7px 13px;
            border-radius: 100px;
            transition: all .3s var(--micro);
            position: relative;
            white-space: nowrap;
        }

        /* Micro: warm underline on hover */
        .nav-links a::after {
            content: '';
            position: absolute;
            bottom: 4px;
            left: 50%;
            width: 0;
            height: 1.5px;
            background: var(--amber);
            border-radius: 2px;
            transition: all .3s var(--ease-out-expo);
            transform: translateX(-50%);
        }

        .nav-links a:hover::after,
        .nav-links a.active::after {
            width: 55%
        }

        .nav-links a:hover {
            color: var(--text-primary);
            background: rgba(255, 190, 130, .06)
        }

        .nav-links a.active {
            color: var(--amber);
            background: rgba(240, 160, 48, .1);
        }

        /* ═══════════════════════════════════════
           MAIN CONTENT
           ═══════════════════════════════════════ */
        .main-content {
            position: relative;
            z-index: 2;
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 24px;
        }

        /* ═══════════════════════════════════════
           HERO
           ═══════════════════════════════════════ */
        .hero {
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 120px 20px 80px;
            position: relative;
        }

        .hero-badge {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            padding: 8px 20px;
            border-radius: 100px;
            background: rgba(240, 160, 48, .06);
            border: 1px solid rgba(240, 160, 48, .16);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            font-size: .78rem;
            font-weight: 600;
            letter-spacing: 3px;
            text-transform: uppercase;
            color: var(--amber);
            margin-bottom: 32px;
            opacity: 0;
            transform: translateY(20px);
            animation: fadeUp .8s .5s var(--ease-out-expo) forwards, micro-float 4s 1.5s ease-in-out infinite;
        }

        .hero-badge .dot {
            width: 6px;
            height: 6px;
            background: var(--amber);
            border-radius: 50%;
            animation: pulse-dot 2s ease-in-out infinite;
            box-shadow: 0 0 8px rgba(240, 160, 48, .5);
        }

        @keyframes pulse-dot {

            0%,
            100% {
                opacity: 1;
                transform: scale(1)
            }

            50% {
                opacity: .35;
                transform: scale(.55)
            }
        }

        /* Micro: gentle floating badge */
        @keyframes micro-float {

            0%,
            100% {
                transform: translateY(0)
            }

            50% {
                transform: translateY(-4px)
            }
        }

        .hero-title {
            font-family: 'Sora', sans-serif;
            font-size: clamp(3rem, 8vw, 6rem);
            font-weight: 800;
            line-height: 1.05;
            letter-spacing: -1px;
            margin-bottom: 24px;
            text-transform: uppercase;
        }

        .hero-title .char {
            display: inline-block;
            opacity: 0;
            transform: translateY(60px) rotateX(80deg);
            animation: charIn .7s var(--ease-out-expo) forwards;
            background: linear-gradient(135deg, var(--text-primary) 0%, var(--text-secondary) 100%);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            transition: text-shadow .3s;
        }

        .hero-title .char:hover {
            text-shadow: 0 0 20px rgba(240, 160, 48, .25);
        }

        .hero-title .word-phantom .char,
        .hero-title .word-shadow .char {
            background: linear-gradient(135deg, var(--primary), var(--amber), var(--gold));
            background-size: 200% 200%;
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            animation: charIn .7s var(--ease-out-expo) forwards, gradShift 5s ease-in-out infinite;
        }

        @keyframes charIn {
            to {
                opacity: 1;
                transform: translateY(0) rotateX(0)
            }
        }

        @keyframes gradShift {

            0%,
            100% {
                background-position: 0% 50%
            }

            50% {
                background-position: 100% 50%
            }
        }

        .hero-tagline {
            font-size: clamp(.95rem, 2.5vw, 1.25rem);
            color: var(--text-secondary);
            letter-spacing: 4px;
            text-transform: uppercase;
            font-weight: 400;
            margin-bottom: 40px;
            opacity: 0;
            transform: translateY(30px);
            animation: fadeUp .8s 1.2s var(--ease-out-expo) forwards;
        }

        .hero-divider {
            width: 80px;
            height: 3px;
            border-radius: 3px;
            background: linear-gradient(90deg, var(--primary), var(--amber), var(--terracotta));
            background-size: 200% 100%;
            animation: fadeUp .6s 1.4s var(--ease-out-expo) forwards, shimmer 3s ease-in-out infinite;
            opacity: 0;
            transform: translateY(20px);
            margin-bottom: 24px;
        }

        @keyframes shimmer {

            0%,
            100% {
                background-position: 0% 50%
            }

            50% {
                background-position: 100% 50%
            }
        }

        .hero-values {
            font-size: .95rem;
            color: var(--text-dim);
            letter-spacing: 2px;
            opacity: 0;
            transform: translateY(20px);
            animation: fadeUp .6s 1.6s var(--ease-out-expo) forwards;
        }

        .hero-values .sep {
            color: var(--amber);
            margin: 0 12px;
            font-size: .55rem;
            vertical-align: middle
        }

        .hero-scroll-hint {
            position: absolute;
            bottom: 40px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            opacity: 0;
            animation: fadeUp .6s 2s var(--ease-out-expo) forwards;
        }

        .hero-scroll-hint span {
            font-size: .68rem;
            letter-spacing: 3px;
            text-transform: uppercase;
            color: var(--text-dim);
        }

        .scroll-line {
            width: 1px;
            height: 36px;
            background: linear-gradient(to bottom, var(--amber), transparent);
            animation: scrollPulse 2s ease-in-out infinite;
        }

        @keyframes scrollPulse {

            0%,
            100% {
                opacity: .25;
                transform: scaleY(.4);
                transform-origin: top
            }

            50% {
                opacity: 1;
                transform: scaleY(1)
            }
        }

        @keyframes fadeUp {
            to {
                opacity: 1;
                transform: translateY(0)
            }
        }

        /* ═══════════════════════════════════════
           SECTION STYLING
           ═══════════════════════════════════════ */
        section {
            padding: 80px 0
        }

        .section-header {
            text-align: center;
            margin-bottom: 56px
        }

        .section-label {
            display: inline-block;
            font-size: .72rem;
            font-weight: 600;
            letter-spacing: 4px;
            text-transform: uppercase;
            color: var(--amber);
            margin-bottom: 14px;
            padding: 6px 16px;
            border-radius: 100px;
            background: rgba(240, 160, 48, .06);
            border: 1px solid rgba(240, 160, 48, .12);
            transition: all .3s var(--micro);
        }

        .section-label:hover {
            background: rgba(240, 160, 48, .1);
            border-color: rgba(240, 160, 48, .2);
        }

        .section-title {
            font-family: 'Sora', sans-serif;
            font-size: clamp(2rem, 5vw, 3rem);
            font-weight: 700;
            background: linear-gradient(135deg, var(--text-primary), var(--text-secondary));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 14px;
        }

        .section-subtitle {
            font-size: 1rem;
            color: var(--text-dim);
            max-width: 580px;
            margin: 0 auto;
            line-height: 1.7;
        }

        /* ═══════════════════════════════════════
           ABOUT
           ═══════════════════════════════════════ */
        .about-content {
            max-width: 800px;
            margin: 0 auto
        }

        .about-text {
            font-size: 1.02rem;
            line-height: 1.9;
            color: var(--text-secondary);
            padding: 40px;
            position: relative;
            z-index: 1;
        }

        .about-text::after {
            content: '\201C';
            position: absolute;
            top: 10px;
            left: 18px;
            font-family: 'Sora', serif;
            font-size: 5.5rem;
            color: rgba(240, 160, 48, .06);
            line-height: 1;
            pointer-events: none;
            z-index: 0;
        }

        /* ═══════════════════════════════════════
           TEAM
           ═══════════════════════════════════════ */
        .team-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
            gap: 28px;
        }

        .team-card {
            padding: 36px 26px 30px;
            text-align: center;
            border-radius: var(--radius-lg);
        }

        .team-card:hover {
            transform: translateY(-8px) scale(1.008)
        }

        .card-photo {
            width: 150px;
            height: 150px;
            margin: 0 auto 22px;
            border-radius: 50%;
            overflow: hidden;
            position: relative;
            border: 2px solid rgba(255, 190, 130, .1);
            box-shadow: 0 0 40px rgba(240, 160, 48, .08);
            transition: all .5s var(--ease-out-expo);
        }

        /* Micro: warm ring glow on hover */
        .card-photo::after {
            content: '';
            position: absolute;
            inset: -6px;
            border: 2px solid rgba(240, 160, 48, .0);
            border-radius: 50%;
            transition: all .5s var(--ease-out-expo);
        }

        .team-card:hover .card-photo {
            border-color: rgba(240, 160, 48, .35);
            box-shadow: 0 0 50px rgba(240, 160, 48, .15), 0 0 100px rgba(240, 160, 48, .04);
            transform: scale(1.04);
        }

        .team-card:hover .card-photo::after {
            border-color: rgba(240, 160, 48, .15);
            inset: -10px;
        }

        .card-photo img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform .5s var(--ease-out-expo);
        }

        .team-card:hover .card-photo img {
            transform: scale(1.06)
        }

        .card-name {
            font-family: 'Sora', sans-serif;
            font-size: 1.4rem;
            font-weight: 700;
            margin-bottom: 4px;
            color: var(--text-primary);
        }

        .card-role {
            font-size: .82rem;
            font-weight: 600;
            letter-spacing: 1px;
            text-transform: uppercase;
            background: linear-gradient(135deg, var(--primary), var(--amber));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 18px;
        }

        .card-bio {
            font-size: .9rem;
            line-height: 1.7;
            color: var(--text-secondary);
            padding: 18px;
            background: rgba(255, 190, 130, .015);
            border: 1px solid rgba(255, 190, 130, .04);
            border-radius: var(--radius-sm);
            transition: border-color .3s var(--micro);
        }

        .team-card:hover .card-bio {
            border-color: rgba(255, 190, 130, .08)
        }

        /* ═══════════════════════════════════════
           ROBOT
           ═══════════════════════════════════════ */
        .robot-section {
            text-align: center
        }

        .robot-frame {
            max-width: 500px;
            margin: 0 auto 40px;
            border-radius: var(--radius-lg);
            overflow: hidden;
            border: 1px solid rgba(255, 190, 130, .07);
            box-shadow:
                0 20px 60px rgba(0, 0, 0, .5),
                0 0 80px rgba(240, 160, 48, .06);
            transition: all .5s var(--ease-out-expo);
            position: relative;
        }

        .robot-frame::after {
            content: '';
            position: absolute;
            inset: 0;
            background: linear-gradient(180deg, transparent 50%, rgba(6, 3, 2, .45) 100%);
            pointer-events: none;
        }

        .robot-frame:hover {
            transform: scale(1.02) translateY(-4px);
            border-color: rgba(240, 160, 48, .25);
            box-shadow:
                0 30px 80px rgba(0, 0, 0, .6),
                0 0 120px rgba(240, 160, 48, .08);
        }

        .robot-frame img {
            width: 100%;
            display: block;
            transition: transform .5s var(--ease-out-expo);
        }

        .robot-frame:hover img {
            transform: scale(1.04)
        }

        .robot-desc {
            max-width: 700px;
            margin: 0 auto;
            padding: 36px
        }

        .robot-desc p {
            font-size: 1.02rem;
            line-height: 1.8;
            color: var(--text-secondary)
        }

        .robot-name {
            color: var(--amber);
            font-weight: 600;
            text-shadow: 0 0 18px rgba(240, 160, 48, .25);
            transition: text-shadow .3s;
        }

        .robot-name:hover {
            text-shadow: 0 0 28px rgba(240, 160, 48, .4)
        }

        /* ═══════════════════════════════════════
           ACHIEVEMENTS
           ═══════════════════════════════════════ */
        .achievement-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 28px;
        }

        .achievement-card {
            padding: 44px 30px;
            text-align: center;
            border-radius: var(--radius-lg);
        }

        .achievement-card:hover {
            transform: translateY(-8px)
        }

        .achievement-icon {
            width: 68px;
            height: 68px;
            margin: 0 auto 22px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            background: linear-gradient(135deg, rgba(226, 85, 53, .08), rgba(240, 160, 48, .08));
            border: 1px solid rgba(240, 160, 48, .12);
            transition: all .45s var(--ease-spring);
        }

        /* Micro: gentle breathe animation */
        .achievement-icon {
            animation: icon-breathe 3s ease-in-out infinite
        }

        @keyframes icon-breathe {

            0%,
            100% {
                transform: scale(1)
            }

            50% {
                transform: scale(1.04)
            }
        }

        .achievement-card:hover .achievement-icon {
            transform: scale(1.12) rotate(8deg);
            background: linear-gradient(135deg, rgba(226, 85, 53, .14), rgba(240, 160, 48, .14));
            border-color: rgba(240, 160, 48, .25);
            box-shadow: 0 0 35px rgba(240, 160, 48, .12);
            animation: none;
        }

        .achievement-icon i {
            font-size: 1.6rem;
            background: linear-gradient(135deg, var(--primary), var(--amber));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .achievement-card h3 {
            font-family: 'Sora', sans-serif;
            font-size: 1.25rem;
            font-weight: 600;
            margin-bottom: 10px;
            color: var(--text-primary);
            transition: color .3s;
        }

        .achievement-card:hover h3 {
            color: var(--gold)
        }

        .achievement-card p {
            color: var(--text-secondary);
            font-size: .92rem;
            line-height: 1.6
        }

        /* ═══════════════════════════════════════
           TIMELINE
           ═══════════════════════════════════════ */
        .timeline-container {
            position: relative;
            max-width: 900px;
            margin: 0 auto;
            padding: 20px 0;
        }

        .timeline-line {
            position: absolute;
            left: 50%;
            top: 0;
            bottom: 0;
            width: 2px;
            background: linear-gradient(180deg, transparent,
                    rgba(240, 160, 48, .25) 10%,
                    rgba(208, 112, 64, .25) 50%,
                    rgba(226, 85, 53, .2) 90%,
                    transparent);
            transform: translateX(-50%);
        }

        .timeline-item {
            position: relative;
            margin-bottom: 52px;
            display: flex
        }

        .timeline-item:nth-child(odd) {
            justify-content: flex-start;
            padding-right: calc(50% + 36px)
        }

        .timeline-item:nth-child(even) {
            justify-content: flex-end;
            padding-left: calc(50% + 36px)
        }

        .timeline-card {
            padding: 22px 26px;
            border-radius: var(--radius-md);
            max-width: 380px;
            width: 100%;
        }

        .timeline-card:hover {
            transform: translateY(-4px) scale(1.02)
        }

        .timeline-dot {
            position: absolute;
            left: 50%;
            top: 28px;
            width: 14px;
            height: 14px;
            background: var(--amber);
            border-radius: 50%;
            transform: translateX(-50%);
            z-index: 3;
            box-shadow: 0 0 18px rgba(240, 160, 48, .45);
            transition: all .3s;
        }

        .timeline-dot::before {
            content: '';
            position: absolute;
            inset: -5px;
            border: 2px solid rgba(240, 160, 48, .25);
            border-radius: 50%;
            animation: ring-pulse 2.5s ease-in-out infinite;
        }

        @keyframes ring-pulse {

            0%,
            100% {
                transform: scale(1);
                opacity: .8
            }

            50% {
                transform: scale(1.6);
                opacity: 0
            }
        }

        .timeline-date {
            font-size: .78rem;
            font-weight: 600;
            letter-spacing: 2px;
            text-transform: uppercase;
            color: var(--amber);
            margin-bottom: 6px;
        }

        .timeline-title {
            font-family: 'Sora', sans-serif;
            font-size: 1.1rem;
            font-weight: 600;
            color: var(--text-primary);
            margin-bottom: 6px;
        }

        .timeline-desc {
            font-size: .9rem;
            line-height: 1.7;
            color: var(--text-secondary)
        }

        /* ═══════════════════════════════════════
           FOOTER
           ═══════════════════════════════════════ */
        footer {
            position: relative;
            z-index: 2;
            padding: 80px 24px 40px;
            text-align: center;
        }

        .footer-glass {
            max-width: 580px;
            margin: 0 auto;
            padding: 44px 36px;
            border-radius: var(--radius-lg);
        }

        .footer-logo {
            font-family: 'Sora', sans-serif;
            font-size: 1.7rem;
            font-weight: 800;
            letter-spacing: 2px;
            background: linear-gradient(135deg, var(--primary), var(--amber), var(--gold));
            background-size: 200% 200%;
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            animation: gradShift 5s ease-in-out infinite;
            margin-bottom: 10px;
        }

        .footer-tagline {
            color: var(--text-secondary);
            font-size: .92rem;
            margin-bottom: 6px
        }

        .footer-motto {
            font-size: .78rem;
            letter-spacing: 3px;
            text-transform: uppercase;
            color: var(--text-dim);
            margin-bottom: 28px;
        }

        .footer-divider {
            width: 60px;
            height: 1px;
            background: linear-gradient(90deg, transparent, rgba(255, 190, 130, .15), transparent);
            margin: 0 auto 20px;
        }

        .copyright {
            font-size: .78rem;
            color: var(--text-dim)
        }

        /* ═══════════════════════════════════════
           SCROLL REVEAL
           ═══════════════════════════════════════ */
        .reveal {
            opacity: 0;
            transform: translateY(50px);
            transition: all .9s var(--ease-out-expo);
        }

        .reveal.revealed {
            opacity: 1;
            transform: translateY(0)
        }

        .reveal-left {
            opacity: 0;
            transform: translateX(-70px);
            transition: all .9s var(--ease-out-expo)
        }

        .reveal-right {
            opacity: 0;
            transform: translateX(70px);
            transition: all .9s var(--ease-out-expo)
        }

        .reveal-left.revealed,
        .reveal-right.revealed {
            opacity: 1;
            transform: translateX(0)
        }

        .reveal-scale {
            opacity: 0;
            transform: scale(.88);
            transition: all .9s var(--ease-out-expo)
        }

        .reveal-scale.revealed {
            opacity: 1;
            transform: scale(1)
        }

        .stagger-1 {
            transition-delay: .1s
        }

        .stagger-2 {
            transition-delay: .2s
        }

        .stagger-3 {
            transition-delay: .3s
        }

        .stagger-4 {
            transition-delay: .4s
        }

        /* ═══════════════════════════════════════
           SCROLL INDICATOR
           ═══════════════════════════════════════ */
        .scroll-indicator {
            position: fixed;
            bottom: 18px;
            right: 18px;
            z-index: 999;
            padding: 7px 14px;
            border-radius: 100px;
            background: rgba(14, 8, 6, .7);
            backdrop-filter: blur(14px);
            -webkit-backdrop-filter: blur(14px);
            border: 1px solid rgba(255, 190, 130, .06);
            font-size: .68rem;
            letter-spacing: 2px;
            text-transform: uppercase;
            color: var(--text-dim);
            transition: all .3s;
            pointer-events: none;
        }

        .scroll-indicator.active {
            color: var(--amber);
            border-color: rgba(240, 160, 48, .15)
        }

        /* ═══════════════════════════════════════
           BACK-TO-TOP BUTTON
           ═══════════════════════════════════════ */
        .back-to-top {
            position: fixed;
            bottom: 18px;
            left: 18px;
            z-index: 999;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: rgba(14, 8, 6, .7);
            backdrop-filter: blur(14px);
            -webkit-backdrop-filter: blur(14px);
            border: 1px solid rgba(255, 190, 130, .08);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: var(--text-dim);
            font-size: .85rem;
            transition: all .35s var(--ease-spring);
            opacity: 0;
            transform: translateY(12px);
            pointer-events: none;
        }

        .back-to-top.visible {
            opacity: 1;
            transform: translateY(0);
            pointer-events: all
        }

        .back-to-top:hover {
            color: var(--amber);
            border-color: rgba(240, 160, 48, .25);
            transform: translateY(-3px);
            box-shadow: 0 0 25px rgba(240, 160, 48, .1);
        }

        /* ═══════════════════════════════════════
           CLICK BURST
           ═══════════════════════════════════════ */
        .__burst {
            position: fixed;
            pointer-events: none;
            z-index: 2147483647
        }

        .__bline {
            position: absolute;
            left: 0;
            top: 0;
            width: 5px;
            height: 14px;
            background: rgba(255, 210, 170, .7);
            border-radius: 5px;
            transform-origin: center bottom;
            opacity: 1;
        }

        @keyframes __bshoot {
            0% {
                transform: rotate(var(--a)) translateY(0) scaleY(.2);
                opacity: .85
            }

            100% {
                transform: rotate(var(--a)) translateY(-40px) scaleY(1);
                opacity: 0
            }
        }

        /* ═══════════════════════════════════════
           MOBILE TOGGLE
           ═══════════════════════════════════════ */
        .mobile-toggle {
            display: none;
            background: none;
            border: none;
            color: var(--text-primary);
            font-size: 1.15rem;
            cursor: pointer;
            padding: 7px;
            border-radius: 8px;
            transition: background .3s;
        }

        .mobile-toggle:hover {
            background: rgba(255, 190, 130, .08)
        }

        /* ═══════════════════════════════════════
           MOBILE MENU PANEL
           ═══════════════════════════════════════ */
        .mobile-menu {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            z-index: 999;
            padding: 80px 24px 32px;
            background: rgba(10, 6, 4, .92);
            backdrop-filter: blur(30px);
            -webkit-backdrop-filter: blur(30px);
            border-bottom: 1px solid rgba(255, 190, 130, .08);
            transform: translateY(-100%);
            transition: transform .5s var(--ease-out-expo);
        }

        .mobile-menu.open {
            transform: translateY(0)
        }

        .mobile-menu a {
            display: block;
            padding: 14px 0;
            font-size: 1.1rem;
            font-weight: 500;
            color: var(--text-secondary);
            text-decoration: none;
            border-bottom: 1px solid rgba(255, 190, 130, .04);
            transition: color .3s, padding-left .3s var(--ease-spring);
        }

        .mobile-menu a:hover {
            color: var(--amber);
            padding-left: 8px
        }

        /* ═══════════════════════════════════════
           REDUCED MOTION
           ═══════════════════════════════════════ */
        @media(prefers-reduced-motion:reduce) {

            *,
            *::before,
            *::after {
                animation-duration: .01ms !important;
                animation-iteration-count: 1 !important;
                transition-duration: .01ms !important;
            }

            .blob {
                animation: none !important
            }

            .reveal,
            .reveal-left,
            .reveal-right,
            .reveal-scale {
                opacity: 1 !important;
                transform: none !important;
            }
        }

        /* ═══════════════════════════════════════
           RESPONSIVE
           ═══════════════════════════════════════ */
        @media(max-width:768px) {
            nav {
                top: 10px;
                padding: 9px 18px;
                border-radius: var(--radius-sm)
            }

            .nav-divider {
                display: none
            }

            .nav-links {
                display: none
            }

            .mobile-toggle {
                display: block
            }

            .mobile-menu {
                display: block
            }

            .hero {
                padding: 100px 20px 60px;
                min-height: 88vh
            }

            .team-grid {
                grid-template-columns: 1fr
            }

            .achievement-grid {
                grid-template-columns: 1fr
            }

            .timeline-item:nth-child(odd),
            .timeline-item:nth-child(even) {
                padding-left: 44px;
                padding-right: 0;
                justify-content: flex-start;
            }

            .timeline-line {
                left: 14px
            }

            .timeline-dot {
                left: 14px
            }

            .about-text {
                padding: 26px 22px
            }

            .robot-desc {
                padding: 26px 22px
            }

            .footer-glass {
                padding: 32px 22px
            }

            section {
                padding: 56px 0
            }

            .scroll-indicator {
                font-size: .6rem;
                padding: 5px 10px
            }
        }

        @media(max-width:480px) {
            .hero-title {
                font-size: 2.3rem
            }

            .card-photo {
                width: 120px;
                height: 120px
            }
        }
    </style>
</head>

<body>

    <!-- Loading Screen -->
    <div id="loading-screen">
        <div class="glass-loader"></div>
        <div class="loader-text">Loading</div>
    </div>

    <!-- Background Blobs (warm, 3 only for perf) -->
    <div class="bg-blobs">
        <div class="blob blob-1"></div>
        <div class="blob blob-2"></div>
        <div class="blob blob-3"></div>
    </div>

    <!-- Warm Vignette -->
    <div class="vignette"></div>

    <!-- Cursor Glow -->
    <div class="cursor-glow" id="cursorGlow"></div>

    <!-- Click Spark Layer -->
    <div id="sparkLayer" style="position:fixed;inset:0;pointer-events:none;z-index:9999"></div>

    <!-- Navigation -->
    <nav id="mainNav">
        <div class="nav-inner">
            <a href="#home" class="logo">TIS Phantom Shadow</a>
            <div class="nav-divider"></div>
            <ul class="nav-links">
                <li><a href="#home">Home</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#team">Team</a></li>
                <li><a href="#robot">Robot</a></li>
                <li><a href="#achievements">Achievements</a></li>
                <li><a href="#timeline">Timeline</a></li>
            </ul>
            <button class="mobile-toggle" id="mobileToggle" aria-label="Menu">
                <i class="fas fa-bars"></i>
            </button>
        </div>
    </nav>

    <!-- Mobile Menu -->
    <div class="mobile-menu" id="mobileMenu">
        <a href="#home">Home</a>
        <a href="#about">About</a>
        <a href="#team">Team</a>
        <a href="#robot">Robot</a>
        <a href="#achievements">Achievements</a>
        <a href="#timeline">Timeline</a>
    </div>

    <!-- Back to Top -->
    <button class="back-to-top" id="backToTop" aria-label="Back to top">
        <i class="fas fa-chevron-up"></i>
    </button>

    <!-- Auto-Scroll Indicator -->
    <div class="scroll-indicator active" id="scrollIndicator">● Auto-scroll &nbsp;|&nbsp; Press E to toggle</div>

    <!-- Main Content -->
    <div class="main-content">

        <!-- HERO -->
        <section class="hero" id="home">
            <div class="hero-badge">
                <span class="dot"></span>
                FLL Robotics Team 2026
            </div>
            <h1 class="hero-title" id="heroTitle">TIS Phantom Shadow</h1>
            <p class="hero-tagline">"2025 Tested Us, 2026 Defines Us"</p>
            <div class="hero-divider"></div>
            <p class="hero-values">
                Innovation <span class="sep">◆</span> Excellence <span class="sep">◆</span> Teamwork
            </p>
            <div class="hero-scroll-hint">
                <span>Scroll</span>
                <div class="scroll-line"></div>
            </div>
        </section>

        <!-- ABOUT -->
        <section id="about">
            <div class="section-header reveal">
                <span class="section-label">Who We Are</span>
                <h2 class="section-title">About Us</h2>
            </div>
            <div class="about-content glass glass-accent reveal stagger-1">
                <p class="about-text">TIS Phantom Shadow had a demanding and unforgettable 2025 season, earning the opportunity to compete at both Regionals and Nationals through months of hard work, innovation, and teamwork. While reaching these competitions was a major achievement, the journey was far from easy. At both Regionals and Nationals, many failures occurred — robots didn't always perform as planned, pressure was high, and unexpected problems arose at critical moments. These setbacks were frustrating and challenging, but they pushed the team to think deeper, adapt faster, and support one another through tough situations. Rather than seeing these moments as defeats, TIS Phantom Shadow chose to view them as valuable lessons. The experiences from 2025 highlighted areas for improvement in preparation, strategy, and execution. With renewed motivation and a clearer vision, TIS Phantom Shadow have made the decision to return in 2026 stronger than ever, determined to learn from the past, refine their skills, and strive for greater success together.</p>
            </div>
        </section>

        <!-- TEAM -->
        <section id="team">
            <div class="section-header reveal">
                <span class="section-label">The Squad</span>
                <h2 class="section-title">Meet Our Team</h2>
                <p class="section-subtitle">Three minds, one mission — engineering tomorrow's solutions today.</p>
            </div>

            <div class="team-grid">
                <div class="team-card glass reveal stagger-1">
                    <div class="card-photo">
                        <img src="images/Sigma_Nathaniel.png" alt="Nathaniel Person" loading="lazy">
                    </div>
                    <h3 class="card-name">Nathaniel Person</h3>
                    <p class="card-role">Lead Builder · Lead Researcher</p>
                    <p class="card-bio">My name is Nathaniel, and I am a builder, innovation team member, and researcher for the TIS Phantom Shadow FLL team. I help design and build the robot while also researching ideas and contributing to our innovation project. I enjoy learning new things and working with my team to turn ideas into real solutions.</p>
                </div>

                <div class="team-card glass reveal stagger-2">
                    <div class="card-photo">
                        <img src="images/Sigma Kevin.png" alt="Kevin Lo" loading="lazy">
                    </div>
                    <h3 class="card-name">Kevin Lo</h3>
                    <p class="card-role">Team Captain · Co-Coder · Lead Innovation</p>
                    <p class="card-bio">My name is Kevin, and I am the captain of the TIS Phantom Shadow FLL team. I work as a co-coder and co-builder, helping to design, build, and program our robot. I enjoy leading my team, solving challenges together, and pushing our robot to perform at its best.</p>
                </div>

                <div class="team-card glass reveal stagger-3">
                    <div class="card-photo">
                        <img src="images/Sigma Liam.png" alt="Liam Watson" loading="lazy">
                    </div>
                    <h3 class="card-name">Liam Watson</h3>
                    <p class="card-role">Lead Coder · Co-Innovation · Co-Researcher</p>
                    <p class="card-bio">My name is Liam, and I am the co-captain of the TIS Phantom Shadow FLL team. I work as a coder and co-innovation lead, helping to program our robot and develop creative solutions for our innovation project. I enjoy collaborating with my team and contributing ideas that help us succeed.</p>
                </div>
            </div>
        </section>

        <!-- ROBOT -->
        <section id="robot" class="robot-section">
            <div class="section-header reveal">
                <span class="section-label">Engineering</span>
                <h2 class="section-title">Our Robot</h2>
            </div>

            <div class="robot-frame reveal-scale stagger-1">
                <img src="images/Robot.png" alt="Dwayne the Box Johnson" loading="lazy">
            </div>

            <div class="robot-desc glass glass-accent reveal stagger-2">
                <p>Our robot, <span class="robot-name">"Dwayne the Box Johnson"</span>, is the result of countless hours of design, testing, and refinement. Built with precision engineering and innovative attachments, Dwayne the Box Johnson can navigate complex courses and complete challenging missions with remarkable accuracy. Its modular design allows for quick changes between competition rounds.</p>
            </div>
        </section>

        <!-- ACHIEVEMENTS -->
        <section id="achievements">
            <div class="section-header reveal">
                <span class="section-label">Milestones</span>
                <h2 class="section-title">Our Achievements</h2>
            </div>

            <div class="achievement-grid">
                <div class="achievement-card glass reveal stagger-1">
                    <div class="achievement-icon">
                        <i class="fas fa-trophy"></i>
                    </div>
                    <h3>Nationals Achiever</h3>
                    <p>We made it to nationals — competing at the highest level in the country.</p>
                </div>

                <div class="achievement-card glass reveal stagger-2">
                    <div class="achievement-icon">
                        <i class="fas fa-lightbulb"></i>
                    </div>
                    <h3>Innovation Creativity</h3>
                    <p>Recognized for outstanding problem-solving approach and creative solutions.</p>
                </div>

                <div class="achievement-card glass reveal stagger-3">
                    <div class="achievement-icon">
                        <i class="fas fa-users"></i>
                    </div>
                    <h3>Teamwork Excellence</h3>
                    <p>Praised for exceptional collaboration, communication, and team spirit.</p>
                </div>
            </div>
        </section>

        <!-- TIMELINE -->
        <section id="timeline">
            <div class="section-header reveal">
                <span class="section-label">Our Story</span>
                <h2 class="section-title">The Journey</h2>
            </div>

            <div class="timeline-container">
                <div class="timeline-line"></div>

                <div class="timeline-item">
                    <div class="timeline-dot"></div>
                    <div class="timeline-card glass reveal-left">
                        <div class="timeline-date">March 2025</div>
                        <div class="timeline-title">Team Formation</div>
                        <div class="timeline-desc">The TIS Phantom Shadow came together with a shared passion for robotics and innovation.</div>
                    </div>
                </div>

                <div class="timeline-item">
                    <div class="timeline-dot"></div>
                    <div class="timeline-card glass reveal-right">
                        <div class="timeline-date">September 2025</div>
                        <div class="timeline-title">First Competition</div>
                        <div class="timeline-desc">We competed in our first regional tournament, gaining valuable experience and feedback.</div>
                    </div>
                </div>

                <div class="timeline-item">
                    <div class="timeline-dot"></div>
                    <div class="timeline-card glass reveal-left">
                        <div class="timeline-date">November 2025</div>
                        <div class="timeline-title">Regionals Success</div>
                        <div class="timeline-desc">Our team placed in the top 27 at Regionals, qualifying us for the National Championship! Achieved max points of 140.</div>
                    </div>
                </div>

                <div class="timeline-item">
                    <div class="timeline-dot"></div>
                    <div class="timeline-card glass reveal-right">
                        <div class="timeline-date">December 2025</div>
                        <div class="timeline-title">National Championship</div>
                        <div class="timeline-desc">Proud of our achievements — achieved max points of 230, a 90-point increase from Regionals!</div>
                    </div>
                </div>

                <div class="timeline-item">
                    <div class="timeline-dot"></div>
                    <div class="timeline-card glass reveal-left">
                        <div class="timeline-date">January 2026</div>
                        <div class="timeline-title">2026 Comeback</div>
                        <div class="timeline-desc">Preparing for 2026 competitions with our new bot, BOT MOBILE. We are excited to see what we can accomplish this year.</div>
                    </div>
                </div>
            </div>
        </section>

    </div>

    <!-- FOOTER -->
    <footer id="contact">
        <div class="footer-glass glass reveal">
            <div class="footer-logo">TIS PHANTOM SHADOW</div>
            <p class="footer-tagline">First Lego League Team 2026</p>
            <p class="footer-motto">2025 Tested Us · 2026 Defines Us</p>
            <div class="footer-divider"></div>
            <p class="copyright">© 2026 TIS Phantom Shadow. All rights reserved.</p>
        </div>
    </footer>

    <script>
        (() => {
            'use strict';

            /* ─── LOADING SCREEN ─── */
            window.addEventListener('load', () => {
                const ls = document.getElementById('loading-screen');
                setTimeout(() => {
                    ls.classList.add('hidden');
                    setTimeout(() => ls.remove(), 700);
                }, 500);
            });

            /* ─── HERO TITLE CHAR SPLIT ─── */
            const heroTitle = document.getElementById('heroTitle');
            if (heroTitle) {
                const words = heroTitle.textContent.trim().split(' ');
                heroTitle.innerHTML = '';
                let ci = 0;
                words.forEach(word => {
                    const s = document.createElement('span');
                    const wl = word.toLowerCase();
                    if (wl === 'phantom') s.className = 'word-phantom';
                    else if (wl === 'shadow') s.className = 'word-shadow';
                    s.style.display = 'inline-block';
                    s.style.marginRight = '0.25em';
                    word.split('').forEach(ch => {
                        const c = document.createElement('span');
                        c.className = 'char';
                        c.textContent = ch;
                        c.style.animationDelay = `${.6+ci*.04}s`;
                        s.appendChild(c);
                        ci++;
                    });
                    heroTitle.appendChild(s);
                });
            }

            /* ─── CURSOR GLOW (throttled) ─── */
            const glow = document.getElementById('cursorGlow');
            let mx = 0,
                my = 0,
                gx = 0,
                gy = 0;
            document.addEventListener('mousemove', e => {
                mx = e.clientX;
                my = e.clientY
            }, {
                passive: true
            });

            let glowFrame;

            function tickGlow() {
                gx += (mx - gx) * .07;
                gy += (my - gy) * .07;
                glow.style.transform = `translate3d(${gx-175}px,${gy-175}px,0)`;
                glowFrame = requestAnimationFrame(tickGlow);
            }
            tickGlow();

            /* ─── SCROLL REVEAL — IntersectionObserver ─── */
            const revEls = document.querySelectorAll('.reveal,.reveal-left,.reveal-right,.reveal-scale');
            const revObs = new IntersectionObserver(entries => {
                entries.forEach(e => {
                    if (e.isIntersecting) e.target.classList.add('revealed')
                });
            }, {
                threshold: .1,
                rootMargin: '0px 0px -30px 0px'
            });
            revEls.forEach(el => revObs.observe(el));

            /* ─── NAV ACTIVE STATE (throttled) ─── */
            const sections = document.querySelectorAll('section[id]');
            const navLinks = document.querySelectorAll('.nav-links a');
            let navTick = false;

            function updateNav() {
                const sy = window.scrollY + 130;
                let current = '';
                sections.forEach(s => {
                    if (sy >= s.offsetTop && sy < s.offsetTop + s.clientHeight) current = s.id;
                });
                navLinks.forEach(a => a.classList.toggle('active', a.getAttribute('href') === '#' + current));
                navTick = false;
            }
            window.addEventListener('scroll', () => {
                if (!navTick) {
                    navTick = true;
                    requestAnimationFrame(updateNav)
                }
            }, {
                passive: true
            });
            updateNav();

            /* ─── BACK TO TOP ─── */
            const btt = document.getElementById('backToTop');
            window.addEventListener('scroll', () => {
                btt.classList.toggle('visible', window.scrollY > 500);
            }, {
                passive: true
            });
            btt.addEventListener('click', () => {
                window.scrollTo({
                    top: 0,
                    behavior: 'smooth'
                });
            });

            /* ─── SMOOTH NAV SCROLL ─── */
            document.querySelectorAll('a[href^="#"]').forEach(a => {
                a.addEventListener('click', e => {
                    e.preventDefault();
                    const t = document.querySelector(a.getAttribute('href'));
                    if (!t) return;
                    const off = t.getBoundingClientRect().top + window.scrollY - 80;
                    window.scrollTo({
                        top: off,
                        behavior: 'smooth'
                    });
                    // close mobile menu
                    mobileMenu.classList.remove('open');
                });
            });

            /* ─── MOBILE MENU ─── */
            const mobileToggle = document.getElementById('mobileToggle');
            const mobileMenu = document.getElementById('mobileMenu');
            mobileToggle.addEventListener('click', () => {
                mobileMenu.classList.toggle('open');
            });

            /* ─── AUTO-SCROLL ─── */
            let autoDir = 1,
                autoSpeed = 1.6,
                autoRunning = true,
                autoRaf = null;
            const indicator = document.getElementById('scrollIndicator');

            function autoScroll() {
                if (!autoRunning) return;
                window.scrollBy(0, autoSpeed * autoDir);
                const top = window.scrollY;
                const maxS = document.documentElement.scrollHeight - window.innerHeight;
                if (top >= maxS - 1) autoDir = -1;
                if (top <= 0) autoDir = 1;
                autoRaf = requestAnimationFrame(autoScroll);
            }
            autoScroll();

            document.addEventListener('keydown', e => {
                const k = e.key.toLowerCase();
                if (k === 'e') {
                    autoRunning = !autoRunning;
                    if (autoRunning) {
                        autoScroll();
                        indicator.classList.add('active');
                        indicator.textContent = '● Auto-scroll  |  Press E to toggle';
                    } else {
                        if (autoRaf) {
                            cancelAnimationFrame(autoRaf);
                            autoRaf = null
                        }
                        indicator.classList.remove('active');
                        indicator.textContent = '○ Paused  |  Press E to toggle';
                    }
                }
                if (e.key === 'ArrowUp') autoDir = -1;
                if (e.key === 'ArrowDown') autoDir = 1;
                if (e.key === '+' || e.key === '=') autoSpeed = Math.min(7, autoSpeed + .35);
                if (e.key === '-') autoSpeed = Math.max(.3, autoSpeed - .35);
            });

            /* ─── CLICK SPARK (simplified canvas) ─── */
            const sparkLayer = document.getElementById('sparkLayer');
            const sCanvas = document.createElement('canvas');
            sCanvas.style.cssText = 'width:100%;height:100%;display:block;pointer-events:none';
            sparkLayer.appendChild(sCanvas);
            const sCtx = sCanvas.getContext('2d');
            let sparks = [];

            function resizeS() {
                const d = window.devicePixelRatio || 1;
                sCanvas.width = window.innerWidth * d;
                sCanvas.height = window.innerHeight * d;
                sCtx.setTransform(d, 0, 0, d, 0, 0);
            }
            resizeS();
            window.addEventListener('resize', resizeS);

            function drawSparks(ts) {
                sCtx.clearRect(0, 0, sCanvas.width, sCanvas.height);
                sparks = sparks.filter(s => {
                    const t = (ts - s.t) / 450;
                    if (t > 1) return false;
                    const ease = t * (2 - t);
                    const dist = ease * 36 * s.sc;
                    const len = 12 * (1 - ease) * s.sc;
                    const x1 = s.x + dist * Math.cos(s.a);
                    const y1 = s.y + dist * Math.sin(s.a);
                    const x2 = s.x + (dist + len) * Math.cos(s.a);
                    const y2 = s.y + (dist + len) * Math.sin(s.a);
                    sCtx.strokeStyle = `rgba(255,210,170,${1-ease})`;
                    sCtx.lineWidth = 1.5;
                    sCtx.lineCap = 'round';
                    sCtx.beginPath();
                    sCtx.moveTo(x1, y1);
                    sCtx.lineTo(x2, y2);
                    sCtx.stroke();
                    return true;
                });
                requestAnimationFrame(drawSparks);
            }
            requestAnimationFrame(drawSparks);

            document.addEventListener('pointerdown', e => {
                if (e.pointerType === 'mouse' && e.button !== 0) return;
                const now = performance.now();
                for (let i = 0; i < 12; i++) {
                    sparks.push({
                        x: e.clientX,
                        y: e.clientY,
                        a: (Math.PI * 2 * i) / 12 + (Math.random() - .5) * .3,
                        t: now,
                        sc: .65 + Math.random() * .55
                    });
                }
            }, {
                passive: true
            });

            /* ─── CLICK BURST (CSS lines) ─── */
            let lastBurst = 0;
            window.addEventListener('pointerdown', e => {
                const now = performance.now();
                if (now - lastBurst < 60) return;
                lastBurst = now;
                if (e.pointerType === 'mouse' && e.button !== 0) return;

                const c = document.createElement('div');
                c.className = '__burst';
                c.style.cssText = `left:${e.clientX}px;top:${e.clientY}px;transform:translate(-50%,-50%)`;
                document.body.appendChild(c);

                for (let i = 0; i < 8; i++) {
                    const l = document.createElement('div');
                    l.className = '__bline';
                    l.style.setProperty('--a', `${(360/8)*i}deg`);
                    l.style.animation = '__bshoot 380ms ease-out forwards';
                    c.appendChild(l);
                }
                setTimeout(() => c.remove(), 450);
            }, {
                passive: true,
                capture: true
            });

            /* ─── BLOB PARALLAX (throttled) ─── */
            const blobs = document.querySelectorAll('.blob');
            let scrollTick = false;
            window.addEventListener('scroll', () => {
                if (!scrollTick) {
                    scrollTick = true;
                    requestAnimationFrame(() => {
                        const sy = window.scrollY;
                        blobs.forEach((b, i) => {
                            b.style.transform = `translate3d(0,${sy*(.015+i*.012)}px,0)`;
                        });
                        scrollTick = false;
                    });
                }
            }, {
                passive: true
            });

        })();
    </script>
</body>

</html>
