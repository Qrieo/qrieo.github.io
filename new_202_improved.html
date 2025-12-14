
import React, { useState, useEffect, useRef, useMemo } from 'react';
import {
  Eye,
  Globe,
  Zap,
  ShieldCheck,
  Factory,
  Menu,
  ArrowRight,
  ClipboardList,
  Target,
  AlertTriangle,
  Smartphone,
  GanttChart,
  ArrowDown,
  Sparkles,
  Cpu,
  Leaf,
  Wrench,
  Quote,
  ChevronDown,
  Play,
  Pause,
  Gauge
} from 'lucide-react';

// =====================================================
// Helpers
// =====================================================
const usePrefersReducedMotion = () => {
  const [reduced, setReduced] = useState(false);
  useEffect(() => {
    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    const onChange = () => setReduced(!!mq.matches);
    onChange();
    mq.addEventListener?.('change', onChange);
    return () => mq.removeEventListener?.('change', onChange);
  }, []);
  return reduced;
};

const useInView = (options = { threshold: 0.2, rootMargin: '0px 0px -10% 0px' }) => {
  const ref = useRef(null);
  const [inView, setInView] = useState(false);
  useEffect(() => {
    const el = ref.current;
    if (!el) return;
    const obs = new IntersectionObserver((entries) => {
      entries.forEach((e) => e.isIntersecting && setInView(true));
    }, options);
    obs.observe(el);
    return () => obs.disconnect();
  }, [options.threshold, options.rootMargin]);
  return [ref, inView];
};

const clamp = (n, a, b) => Math.max(a, Math.min(b, n));

// =====================================================
// Background + overlays
// =====================================================
const DotGrid = () => (
  <div
    className="absolute inset-0 pointer-events-none opacity-10"
    style={{
      backgroundImage: 'radial-gradient(#ffffff 1px, transparent 1px)',
      backgroundSize: '20px 20px'
    }}
  />
);

const AmbientBackground = () => (
  <>
    <div className="pointer-events-none absolute inset-0 overflow-hidden">
      {/* drifting blobs */}
      <div className="absolute -top-40 -left-40 w-[520px] h-[520px] rounded-full bg-red-500/20 blur-3xl animate-blob" />
      <div className="absolute top-10 -right-40 w-[520px] h-[520px] rounded-full bg-blue-500/15 blur-3xl animate-blob animation-delay-2000" />
      <div className="absolute -bottom-60 left-1/3 w-[620px] h-[620px] rounded-full bg-emerald-500/10 blur-3xl animate-blob animation-delay-4000" />

      {/* subtle moving gradient */}
      <div className="absolute inset-0 opacity-30 bg-[radial-gradient(circle_at_20%_20%,rgba(239,68,68,0.25),transparent_35%),radial-gradient(circle_at_80%_30%,rgba(59,130,246,0.2),transparent_40%),radial-gradient(circle_at_50%_90%,rgba(16,185,129,0.12),transparent_40%)] animate-pan" />

      {/* scanlines */}
      <div className="absolute inset-0 opacity-[0.08] mix-blend-overlay bg-scanlines" />

      {/* film grain */}
      <div className="absolute inset-0 opacity-[0.06] bg-grain" />
    </div>
  </>
);

// =====================================================
// E-Ink Transition (enhanced)
// =====================================================
const EInkRefresh = ({ isActive, onCovered, onComplete }) => {
  const canvasRef = useRef(null);
  const reducedMotion = usePrefersReducedMotion();

  useEffect(() => {
    if (!isActive || reducedMotion) {
      if (isActive && reducedMotion) {
        onCovered?.();
        onComplete?.();
      }
      return;
    }

    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');

    const setDimensions = () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
      ctx.imageSmoothingEnabled = false;
    };

    setDimensions();
    window.addEventListener('resize', setDimensions);

    let frameId;
    let t0 = performance.now();

    // timings (ms)
    const noiseIn = 180;
    const solidBlack = 90;
    const flashWhite = 90;
    const noiseOut = 260;
    const total = noiseIn + solidBlack + flashWhite + noiseOut;

    const pixel = 3; // smaller = higher fidelity

    const cols = () => Math.ceil(canvas.width / pixel);
    const rows = () => Math.ceil(canvas.height / pixel);

    const dither = (x, y) => ((x * 13 + y * 7) % 17) / 17;

    const drawNoise = (density, bias = 0.5) => {
      const c = cols();
      const r = rows();
      const count = Math.floor(c * r * density);
      for (let i = 0; i < count; i++) {
        const cx = (Math.random() * c) | 0;
        const cy = (Math.random() * r) | 0;
        const chance = 0.45 + (dither(cx, cy) - 0.5) * 0.2;
        const v = Math.random() > clamp(bias + (chance - 0.5), 0.15, 0.85);
        ctx.fillStyle = v ? '#000' : '#fff';
        ctx.fillRect(cx * pixel, cy * pixel, pixel, pixel);
      }
    };

    const wipe = (color) => {
      ctx.fillStyle = color;
      ctx.fillRect(0, 0, canvas.width, canvas.height);
    };

    const animate = (now) => {
      const elapsed = now - t0;
      const p = clamp(elapsed / total, 0, 1);

      // subtle vignette
      const vignette = () => {
        const g = ctx.createRadialGradient(
          canvas.width / 2,
          canvas.height / 2,
          Math.min(canvas.width, canvas.height) * 0.2,
          canvas.width / 2,
          canvas.height / 2,
          Math.max(canvas.width, canvas.height) * 0.8
        );
        g.addColorStop(0, 'rgba(0,0,0,0)');
        g.addColorStop(1, 'rgba(0,0,0,0.45)');
        ctx.fillStyle = g;
        ctx.fillRect(0, 0, canvas.width, canvas.height);
      };

      if (elapsed < noiseIn) {
        // build noise
        drawNoise(0.36, 0.55);
        vignette();
      } else if (elapsed < noiseIn + solidBlack) {
        // hold black
        if (elapsed - noiseIn < 16) {
          wipe('#000');
          onCovered?.();
        }
        vignette();
      } else if (elapsed < noiseIn + solidBlack + flashWhite) {
        // flash white
        wipe('#fff');
      } else if (elapsed < total) {
        // noise-out reveal (erode)
        const local = (elapsed - (noiseIn + solidBlack + flashWhite)) / noiseOut;
        wipe('rgba(255,255,255,0.96)');
        ctx.globalCompositeOperation = 'destination-out';
        const c = cols();
        const r = rows();
        const count = Math.floor(c * r * (0.06 + local * 0.28));
        for (let i = 0; i < count; i++) {
          const cx = (Math.random() * c) | 0;
          const cy = (Math.random() * r) | 0;
          const size = 1 + ((Math.random() * 4) | 0);
          ctx.fillRect(cx * pixel, cy * pixel, pixel * size, pixel * size);
        }
        ctx.globalCompositeOperation = 'source-over';
      } else {
        onComplete?.();
        return;
      }

      frameId = requestAnimationFrame(animate);
    };

    frameId = requestAnimationFrame(animate);

    return () => {
      cancelAnimationFrame(frameId);
      window.removeEventListener('resize', setDimensions);
    };
  }, [isActive, onCovered, onComplete, reducedMotion]);

  if (!isActive) return null;
  return <canvas ref={canvasRef} className="fixed inset-0 z-[100] pointer-events-none" />;
};

// =====================================================
// UI components
// =====================================================
const SectionHeader = ({ number, title, subtitle }) => (
  <div className="mb-12 border-b border-white/20 pb-4">
    <div className="flex items-baseline space-x-4 mb-2">
      <span className="text-red-500 font-mono text-xl">({number})</span>
      <h2 className="text-3xl md:text-5xl font-bold uppercase tracking-tighter">{title}</h2>
    </div>
    {subtitle && <p className="text-gray-400 font-mono max-w-2xl mt-2">{subtitle}</p>}
  </div>
);

const Card = ({ title, children, className = '', icon: Icon, kicker }) => (
  <div
    className={
      `border border-white/20 bg-neutral-900/50 backdrop-blur-sm p-6 relative overflow-hidden group hover:border-white/40 transition-colors rounded-lg ${className}`
    }
  >
    {/* hover sheen */}
    <div className="pointer-events-none absolute inset-0 opacity-0 group-hover:opacity-100 transition-opacity duration-700 bg-[radial-gradient(circle_at_30%_20%,rgba(255,255,255,0.12),transparent_50%)]" />

    {kicker && (
      <div className="mb-3 text-[11px] font-mono uppercase tracking-[0.22em] text-gray-500">
        {kicker}
      </div>
    )}

    {title && (
      <h3 className="text-xl font-bold mb-4 uppercase tracking-tight flex items-center gap-3">
        {Icon && <Icon className="w-5 h-5 text-red-500" />}
        {title}
      </h3>
    )}

    <div className="font-mono text-sm leading-relaxed text-gray-300">{children}</div>
  </div>
);

const GlitchText = ({ text }) => (
  <span className="relative inline-block">
    <span className="relative z-10 glitch" data-text={text}>
      {text}
    </span>
  </span>
);

const Button = ({ children, onClick, variant = 'primary', className = '' }) => {
  const base =
    'relative inline-flex items-center justify-center gap-2 px-8 py-4 font-bold uppercase tracking-[0.18em] transition-transform duration-200 active:scale-[0.98]';
  const styles =
    variant === 'primary'
      ? 'bg-white text-black hover:bg-gray-200'
      : 'bg-transparent text-white border border-white/25 hover:border-white/60 hover:bg-white/5';

  return (
    <button onClick={onClick} className={`${base} ${styles} ${className}`}>
      <span className="relative z-10">{children}</span>
      <span className="pointer-events-none absolute inset-0 opacity-0 hover:opacity-100 transition-opacity duration-300 bg-[radial-gradient(circle_at_30%_20%,rgba(255,255,255,0.25),transparent_60%)]" />
    </button>
  );
};

// =====================================================
// Scroll progress + nav highlight
// =====================================================
const ScrollProgress = () => {
  const [p, setP] = useState(0);
  useEffect(() => {
    const onScroll = () => {
      const h = document.documentElement;
      const max = h.scrollHeight - h.clientHeight;
      setP(max > 0 ? h.scrollTop / max : 0);
    };
    onScroll();
    window.addEventListener('scroll', onScroll, { passive: true });
    return () => window.removeEventListener('scroll', onScroll);
  }, []);
  return (
    <div className="fixed top-0 left-0 right-0 z-[60] h-[2px] bg-white/10">
      <div className="h-full bg-red-500" style={{ width: `${Math.round(p * 100)}%` }} />
    </div>
  );
};

// =====================================================
// Animated visuals for ScrollStory (enhanced transitions)
// =====================================================
const PhoneSVG = ({ opacity = 1, active }) => (
  <div
    style={{ opacity }}
    className={`transition-all duration-700 w-full h-full flex items-center justify-center p-8 absolute inset-0 ${active ? 'scale-100 translate-y-0' : 'scale-[0.98] translate-y-2'}`}
  >
    <div className="relative">
      <svg
        viewBox="0 0 300 600"
        className="h-[340px] sm:h-[420px] w-auto text-white/50 stroke-white/80"
        fill="none"
        strokeWidth="6"
        strokeLinecap="round"
        strokeLinejoin="round"
      >
        <rect x="20" y="20" width="260" height="560" rx="40" ry="40" className="fill-black/50" />
        <rect x="35" y="80" width="230" height="440" className="fill-neutral-200 stroke-none" />
        <circle cx="150" cy="50" r="6" fill="#60A5FA" />
        <circle cx="150" cy="550" r="20" className="stroke-white/30" />
      </svg>
      <div className="absolute inset-0 flex items-center justify-center pointer-events-none">
        <span className="text-black font-serif font-bold text-xl bg-white/10 backdrop-blur-sm px-3 py-2 rounded">
          THE EYE
        </span>
      </div>
      <div className="absolute -inset-10 blur-2xl bg-red-500/10 rounded-full animate-pulse" />
    </div>
  </div>
);

const DisplaySVG = ({ opacity = 1, active }) => (
  <div
    style={{ opacity }}
    className={`transition-all duration-700 w-full h-full flex items-center justify-center p-8 absolute inset-0 ${active ? 'scale-100 translate-y-0' : 'scale-[0.98] translate-y-2'}`}
  >
    <div className="w-64 h-96 bg-neutral-200 border-4 border-neutral-400 shadow-[0_0_60px_rgba(255,255,255,0.22)] rounded-sm flex flex-col items-center justify-center p-8 relative overflow-hidden">
      <div className="absolute inset-0 opacity-30 bg-[linear-gradient(120deg,transparent,rgba(0,0,0,0.07),transparent)] animate-shine" />
      <span className="text-3xl font-serif text-black font-bold text-center">E-Paper<br />Module</span>
      <div className="mt-4 w-full h-2 bg-neutral-300 rounded-full overflow-hidden">
        <div className="h-full w-1/3 bg-black animate-pulse" />
      </div>
      <div className="absolute bottom-2 right-2 text-[10px] font-mono text-black uppercase">Refresh: High</div>
    </div>
  </div>
);

const LayersSVG = ({ opacity = 1, active }) => (
  <div
    style={{ opacity }}
    className={`transition-all duration-700 w-full h-full flex flex-col items-center justify-center space-y-4 absolute inset-0 ${active ? 'scale-100 translate-y-0' : 'scale-[0.98] translate-y-2'}`}
  >
    {[
      { label: '03. TOUCH / FRONT LIGHT', color: 'blue', delay: 'delay-0' },
      { label: '02. COLOR FILTER (CFA)', color: 'red', delay: 'delay-100' },
      { label: '01. TFT BACKPLANE', color: 'white', delay: 'delay-200' }
    ].map((l) => (
      <div
        key={l.label}
        className={`w-64 h-24 rounded-lg flex items-center justify-center backdrop-blur-md border-2 transform hover:scale-105 transition-transform animate-float ${l.delay} ` +
          (l.color === 'blue'
            ? 'bg-blue-500/20 border-blue-500'
            : l.color === 'red'
              ? 'bg-red-500/20 border-red-500'
              : 'bg-white/15 border-white')}
      >
        <span className={`font-mono font-bold ${l.color === 'blue' ? 'text-blue-200' : l.color === 'red' ? 'text-red-200' : 'text-white'}`}>{l.label}</span>
      </div>
    ))}
  </div>
);

const BoxSVG = ({ opacity = 1, active }) => (
  <div
    style={{ opacity }}
    className={`transition-all duration-700 w-full h-full flex items-center justify-center absolute inset-0 ${active ? 'scale-100 translate-y-0' : 'scale-[0.98] translate-y-2'}`}
  >
    <div className="w-64 h-64 bg-[#C19A6B] border-4 border-[#8B4513] relative transform rotate-x-12 rotate-y-12 shadow-2xl flex items-center justify-center">
      <div className="absolute inset-0 bg-black/10" />
      <div className="absolute -inset-6 bg-amber-400/10 blur-3xl rounded-full animate-pulse" />
      <div className="relative">
        <Package className="w-20 h-20 text-[#5D4037] opacity-80" />
      </div>
      <div className="absolute bottom-4 left-4 font-mono text-[#5D4037] text-xs font-bold border-2 border-[#5D4037] px-2 py-1 rounded">
        FRAGILE
      </div>
    </div>
  </div>
);

const FactorySVG = ({ opacity = 1, active }) => (
  <div
    style={{ opacity }}
    className={`transition-all duration-700 w-full h-full flex items-center justify-center absolute inset-0 ${active ? 'scale-100 translate-y-0' : 'scale-[0.98] translate-y-2'}`}
  >
    <div className="relative w-80 h-60">
      <div className="absolute bottom-0 w-full h-40 bg-neutral-800 border-t-4 border-red-600 rounded-t-xl" />
      <div className="absolute bottom-4 left-0 w-full h-8 bg-gray-600 border-y-2 border-dashed border-yellow-500 animate-pulse" />
      <div className="absolute bottom-8 left-[10%] w-20 h-20 bg-[#C19A6B] border-2 border-[#8B4513] shadow-lg flex items-center justify-center animate-conveyor">
        <span className="text-xs font-bold text-[#5D4037]">LOCAL</span>
      </div>
      <div className="absolute -top-10 w-full text-center">
        <span className="text-red-500 font-mono bg-black/80 px-2">COSMOLOCAL ASSEMBLY</span>
      </div>
    </div>
  </div>
);

const InkPulse = ({ keyId }) => (
  <div key={keyId} className="pointer-events-none absolute inset-0">
    <div className="absolute inset-0 bg-white/5" />
    <div className="absolute inset-0 bg-[radial-gradient(circle_at_50%_50%,rgba(239,68,68,0.16),transparent_55%)] animate-inkpulse" />
  </div>
);

const ScrollStory = () => {
  const steps = useMemo(
    () => [
      {
        id: 'frame-0',
        visual: PhoneSVG,
        title: 'In Your Hand',
        content:
          'Experience the ultimate digital companion. Designed from the ground up to eliminate eye strain and screen fatigue.'
      },
      {
        id: 'frame-1',
        visual: DisplaySVG,
        title: 'Reflective Tech',
        content:
          'We removed the emission-based display. This is pure E‑Paper technology—paper-like visibility that only uses power on refresh.'
      },
      {
        id: 'frame-2',
        visual: LayersSVG,
        title: 'The Stack',
        content:
          'Three layers: TFT backplane, Color Filter Array (CFA), and Touch/Frontlight. Simple, stable, low-energy.'
      },
      {
        id: 'frame-3',
        visual: BoxSVG,
        title: 'Modular Pack',
        content:
          'The core module ships to local micro-factories. Less waste, tighter quality control, faster repairs.'
      },
      {
        id: 'frame-4',
        visual: FactorySVG,
        title: 'Local Assembly',
        content:
          'Assembled in a certified micro-factory near you. Lower logistics, better repairability, skilled local jobs.'
      }
    ],
    []
  );

  const [currentFrame, setCurrentFrame] = useState(0);
  const [pulseKey, setPulseKey] = useState(0);

  useEffect(() => {
    const observerOptions = {
      root: null,
      rootMargin: '-50% 0px -50% 0px',
      threshold: 0
    };

    const callback = (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const index = steps.findIndex((step) => step.id === entry.target.id);
          if (index !== -1) {
            setCurrentFrame(index);
            setPulseKey((k) => k + 1);
          }
        }
      });
    };

    const observer = new IntersectionObserver(callback, observerOptions);
    steps.forEach((step) => {
      const element = document.getElementById(step.id);
      if (element) observer.observe(element);
    });
    return () => observer.disconnect();
  }, [steps]);

  return (
    <div className="relative w-full border-y border-white/20 bg-neutral-950">
      <div className="lg:grid lg:grid-cols-2">
        {/* STICKY VISUAL */}
        <div className="sticky top-[64px] h-[44vh] lg:h-[calc(100vh-64px)] z-10 bg-black/80 border-b lg:border-b-0 lg:border-r border-white/20 flex flex-col items-center justify-center overflow-hidden">
          <div className="absolute top-4 left-4 z-20 flex items-center gap-2">
            <div className="w-2 h-2 bg-red-500 animate-pulse rounded-full" />
            <span className="text-xs font-mono text-red-500 uppercase tracking-widest">
              Phase {currentFrame + 1}: {steps[currentFrame].title}
            </span>
          </div>

          <div className="absolute top-4 right-4 z-20 hidden sm:flex items-center gap-2 text-xs font-mono text-gray-400">
            <Gauge className="w-4 h-4" />
            <span className="uppercase tracking-widest">Scroll-locked</span>
          </div>

          <div className="relative w-full h-full max-w-md p-8">
            <InkPulse keyId={pulseKey} />
            {steps.map((step, index) => {
              const Visual = step.visual;
              const active = index === currentFrame;
              return <Visual key={step.id} opacity={active ? 1 : 0} active={active} />;
            })}
          </div>

          {/* mini stepper */}
          <div className="absolute bottom-4 left-1/2 -translate-x-1/2 flex gap-2">
            {steps.map((_, i) => (
              <div
                key={i}
                className={`h-[2px] w-8 rounded-full transition-all duration-500 ${i === currentFrame ? 'bg-red-500' : 'bg-white/20'}`}
              />
            ))}
          </div>
        </div>

        {/* SCROLLING CONTENT */}
        <div className="relative z-20 bg-transparent">
          {steps.map((step, index) => (
            <div
              id={step.id}
              key={step.id}
              className="min-h-[100vh] flex items-center justify-center p-6 lg:p-20 border-l border-white/5"
            >
              <Card
                title={step.title}
                icon={index === 0 ? Smartphone : index === 1 ? Eye : index === 2 ? GanttChart : index === 3 ? Package : Factory}
                className={
                  `w-full max-w-md transition-all duration-700 backdrop-blur-xl bg-black/80 ` +
                  (index === currentFrame
                    ? 'border-red-500 shadow-[0_0_40px_rgba(239,68,68,0.18)] translate-y-0 scale-[1.03]'
                    : 'border-white/20 opacity-50 translate-y-2')
                }
                kicker={`Step ${index + 1} / ${steps.length}`}
              >
                <p className="text-base leading-relaxed">{step.content}</p>
                <div className="mt-6 flex items-center gap-2 text-xs font-mono text-gray-500">
                  {index < steps.length - 1 ? (
                    <>
                      <ArrowDown className="w-3 h-3 animate-bounce" />
                      <span>KEEP SCROLLING</span>
                    </>
                  ) : (
                    <span className="text-green-500">COMPLETE</span>
                  )}
                </div>
              </Card>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
};

// =====================================================
// Extra sections (new elements)
// =====================================================
const Stats = () => {
  const [ref, inView] = useInView();
  const reduced = usePrefersReducedMotion();
  const [n, setN] = useState(0);

  useEffect(() => {
    if (!inView) return;
    if (reduced) {
      setN(100);
      return;
    }
    let raf;
    const t0 = performance.now();
    const dur = 900;
    const tick = (t) => {
      const p = clamp((t - t0) / dur, 0, 1);
      const eased = 1 - Math.pow(1 - p, 3);
      setN(Math.round(eased * 100));
      if (p < 1) raf = requestAnimationFrame(tick);
    };
    raf = requestAnimationFrame(tick);
    return () => cancelAnimationFrame(raf);
  }, [inView, reduced]);

  return (
    <section id="stats" className="py-28 px-6 border-b border-white/10 max-w-7xl mx-auto">
      <SectionHeader number="03" title="Signal, Not Hype" subtitle="A few numbers that describe the direction." />
      <div ref={ref} className="grid md:grid-cols-3 gap-6">
        <Card title="Lower Eye Load" icon={Eye} kicker="Comfort">
          <div className="text-4xl font-sans font-extrabold text-white mb-2">{n}%</div>
          <p className="text-gray-400">Design target for reduced perceived strain in long reading sessions.</p>
        </Card>
        <Card title="Less Power (Static)" icon={Zap} kicker="Efficiency">
          <div className="text-4xl font-sans font-extrabold text-white mb-2">~10×</div>
          <p className="text-gray-400">E‑Paper only draws meaningful power when pixels refresh.</p>
        </Card>
        <Card title="Repair-First" icon={Wrench} kicker="Longevity">
          <div className="text-4xl font-sans font-extrabold text-white mb-2">A+</div>
          <p className="text-gray-400">Modular parts, clear access, and local service by default.</p>
        </Card>
      </div>
    </section>
  );
};

const Marquee = () => (
  <div className="border-y border-white/10 bg-black/60 overflow-hidden">
    <div className="flex gap-10 whitespace-nowrap py-4 animate-marquee">
      {Array.from({ length: 14 }).map((_, i) => (
        <div key={i} className="flex items-center gap-2 text-xs font-mono uppercase tracking-[0.3em] text-gray-500">
          <Sparkles className="w-4 h-4 text-red-500" />
          <span>Eye-Friendly</span>
          <span className="text-white/20">•</span>
          <span>Cosmolocal</span>
          <span className="text-white/20">•</span>
          <span>Repairable</span>
          <span className="text-white/20">•</span>
          <span>Low Power</span>
        </div>
      ))}
    </div>
  </div>
);

const FAQItem = ({ q, a }) => {
  const [open, setOpen] = useState(false);
  return (
    <div className="border border-white/10 rounded-lg overflow-hidden">
      <button
        className="w-full flex items-center justify-between px-5 py-4 bg-neutral-900/40 hover:bg-neutral-900/60 transition-colors"
        onClick={() => setOpen((v) => !v)}
      >
        <span className="text-left font-mono text-sm text-white">{q}</span>
        <ChevronDown className={`w-5 h-5 text-gray-400 transition-transform duration-300 ${open ? 'rotate-180' : ''}`} />
      </button>
      <div className={`grid transition-[grid-template-rows] duration-300 ${open ? 'grid-rows-[1fr]' : 'grid-rows-[0fr]'}`}>
        <div className="overflow-hidden px-5">
          <p className="py-4 text-sm font-mono text-gray-400 leading-relaxed">{a}</p>
        </div>
      </div>
    </div>
  );
};

const FAQ = () => (
  <section id="faq" className="py-28 px-6 border-b border-white/10 max-w-7xl mx-auto">
    <SectionHeader number="06" title="FAQ" subtitle="Short answers. No fluff." />
    <div className="grid lg:grid-cols-2 gap-6">
      <div className="space-y-4">
        <FAQItem
          q="Is E‑Paper too slow for a phone?"
          a="For video-heavy usage, yes—this concept prioritizes messaging, reading, navigation, and focused workflows. The goal is smooth scrolling and comfortable text, not cinema." />
        <FAQItem
          q="What about color?"
          a="Modern color e‑paper has improved. We treat color as 'accent', optimized for UI cues and images while keeping text clarity as the core KPI." />
        <FAQItem
          q="How does local assembly work?"
          a="A certified design + centralized compliance, with local micro-factories acting as contracted builders. That keeps quality consistent while enabling repairs and local jobs." />
      </div>
      <div className="space-y-4">
        <FAQItem
          q="Does this replace my smartphone?"
          a="Think of it as a healthier daily driver for text-first life: comms, docs, tasks, transit, reading. Many people will keep a standard phone for cameras/gaming." />
        <FAQItem
          q="What about sustainability?"
          a="Lower use-phase energy + longer device life + easier repairs. The biggest win comes from using phones longer—so we design for longevity and replaceable parts." />
        <FAQItem
          q="Can I contribute?"
          a="Yes. Open design modules, documentation, UX testing, and local maker partnerships. A public roadmap is the next step." />
      </div>
    </div>
  </section>
);

const Testimonials = () => (
  <section id="voices" className="py-28 px-6 border-b border-white/10 max-w-7xl mx-auto">
    <SectionHeader number="07" title="Voices" subtitle="What early testers want (and fear)." />
    <div className="grid md:grid-cols-3 gap-6">
      {[{
        name: 'Knowledge worker',
        quote: 'If it feels like paper and my eyes stop burning at 11pm, I’m in.'
      }, {
        name: 'Parent',
        quote: 'I want a device that’s not designed to hijack attention—and is fixable.'
      }, {
        name: 'Repair tech',
        quote: 'Give me screws, parts, and manuals. I’ll keep it alive for a decade.'
      }].map((t) => (
        <Card key={t.name} icon={Quote} title={t.name}>
          <p className="text-gray-300">“{t.quote}”</p>
        </Card>
      ))}
    </div>
  </section>
);

const CTA = ({ triggerNav }) => (
  <section id="cta" className="py-28 px-6 max-w-7xl mx-auto">
    <div className="relative overflow-hidden rounded-2xl border border-white/15 bg-black/70 p-10 md:p-14">
      <div className="absolute inset-0 opacity-40 bg-[radial-gradient(circle_at_20%_20%,rgba(239,68,68,0.25),transparent_45%),radial-gradient(circle_at_80%_30%,rgba(59,130,246,0.2),transparent_45%)]" />
      <div className="relative z-10">
        <div className="flex items-center gap-3 text-xs font-mono uppercase tracking-[0.3em] text-gray-400">
          <Leaf className="w-4 h-4 text-emerald-400" />
          <span>Build the calmer phone</span>
        </div>
        <h3 className="mt-4 text-3xl md:text-5xl font-bold tracking-tighter">
          Ready to prototype the <span className="text-red-500">eye-first</span> stack?
        </h3>
        <p className="mt-4 text-gray-400 max-w-2xl">
          If you want, I can convert this into a real standalone HTML (no React), or wire it into Next.js/Vite.
          Tell me your target runtime and we’ll ship.
        </p>
        <div className="mt-8 flex flex-col sm:flex-row gap-4">
          <Button onClick={() => triggerNav('animation')}>
            Rewatch Process <ArrowRight className="w-4 h-4" />
          </Button>
          <Button variant="secondary" onClick={() => triggerNav('problem')}>
            Read the Why
          </Button>
        </div>
      </div>
    </div>
  </section>
);

// =====================================================
// Main App
// =====================================================
export default function App() {
  const [activeSection, setActiveSection] = useState('hero');
  const [isMenuOpen, setIsMenuOpen] = useState(false);
  const [isTransitioning, setIsTransitioning] = useState(false);
  const [targetSection, setTargetSection] = useState(null);

  const triggerNav = (id) => {
    if (id === activeSection || isTransitioning) return;
    const element = document.getElementById(id);
    if (element) {
      setTargetSection(id);
      setIsTransitioning(true);
      setIsMenuOpen(false);
    }
  };

  const handleTransitionCovered = () => {
    const element = document.getElementById(targetSection);
    if (element) {
      element.scrollIntoView({ behavior: 'auto' });
      setActiveSection(targetSection);
    }
  };

  const handleTransitionComplete = () => {
    setIsTransitioning(false);
    setTargetSection(null);
  };

  const navItems = [
    { id: 'problem', label: 'Problem' },
    { id: 'solution', label: 'Solution' },
    { id: 'stats', label: 'Signal' },
    { id: 'animation', label: 'Process' },
    { id: 'commitments', label: 'Commitments' },
    { id: 'readiness', label: 'Plan' },
    { id: 'faq', label: 'FAQ' }
  ];

  // active section tracking for nav underline
  useEffect(() => {
    const ids = ['hero', ...navItems.map((n) => n.id), 'cta'];
    const opts = { root: null, threshold: 0.3 };
    const obs = new IntersectionObserver((entries) => {
      const top = entries.filter((e) => e.isIntersecting).sort((a, b) => b.intersectionRatio - a.intersectionRatio)[0];
      if (top?.target?.id) setActiveSection(top.target.id);
    }, opts);
    ids.forEach((id) => {
      const el = document.getElementById(id);
      if (el) obs.observe(el);
    });
    return () => obs.disconnect();
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);

  return (
    <div className="bg-black text-white min-h-screen font-sans selection:bg-red-500 selection:text-white overflow-x-hidden">
      <ScrollProgress />
      <AmbientBackground />
      <DotGrid />
      <EInkRefresh isActive={isTransitioning} onCovered={handleTransitionCovered} onComplete={handleTransitionComplete} />

      {/* Navigation */}
      <nav className="fixed top-0 left-0 w-full z-50 border-b border-white/10 bg-black/70 backdrop-blur-md h-[64px] flex items-center">
        <div className="max-w-7xl mx-auto px-6 w-full flex items-center justify-between">
          <div className="flex items-center gap-2 cursor-pointer" onClick={() => triggerNav('hero')}>
            <Smartphone className="w-5 h-5 text-red-500" />
            <span className="font-bold tracking-widest uppercase text-lg hidden sm:inline">
              Project <span className="font-light">Eye</span>
            </span>
          </div>

          <div className="hidden lg:flex gap-6">
            {navItems.map((item) => (
              <button
                key={item.id}
                onClick={() => triggerNav(item.id)}
                className="text-xs font-mono uppercase tracking-widest hover:text-red-500 transition-colors relative"
              >
                {item.label}
                <span
                  className={
                    `absolute -bottom-2 left-0 h-[2px] bg-red-500 transition-all duration-500 ` +
                    (activeSection === item.id ? 'w-full opacity-100' : 'w-0 opacity-0')
                  }
                />
              </button>
            ))}
          </div>

          <button className="lg:hidden" onClick={() => setIsMenuOpen(!isMenuOpen)}>
            <Menu className="w-6 h-6" />
          </button>
        </div>

        {/* Mobile menu */}
        <div className={`lg:hidden absolute top-[64px] left-0 right-0 border-b border-white/10 bg-black/90 backdrop-blur-md transition-all ${isMenuOpen ? 'max-h-[360px] opacity-100' : 'max-h-0 opacity-0 overflow-hidden'}`}>
          <div className="px-6 py-4 flex flex-col gap-3">
            {navItems.map((item) => (
              <button
                key={item.id}
                onClick={() => triggerNav(item.id)}
                className="text-xs font-mono uppercase tracking-widest text-left hover:text-red-500"
              >
                {item.label}
              </button>
            ))}
          </div>
        </div>
      </nav>

      {/* Hero */}
      <header id="hero" className="relative min-h-screen flex flex-col justify-center items-center pt-20 px-6 border-b border-white/10">
        <div className="max-w-5xl w-full text-center relative z-10">
          <div className="inline-flex items-center gap-2 border border-red-500/50 rounded-full px-4 py-1 mb-8">
            <span className="w-2 h-2 bg-red-500 rounded-full animate-pulse" />
            <span className="text-red-500 font-mono text-xs tracking-[0.2em]">DESIGNED FOR HEALTH</span>
          </div>

          <h1 className="text-5xl md:text-8xl font-bold tracking-tighter mb-6 leading-none">
            RECLAIM <br /> YOUR <GlitchText text="VISION" />
          </h1>

          <p className="text-lg md:text-2xl text-gray-400 max-w-2xl mx-auto mb-10 font-light">
            E‑Ink technology meets local manufacturing—calmer screens, longer life, easier repairs.
          </p>

          <div className="flex flex-col sm:flex-row gap-4 justify-center">
            <Button onClick={() => triggerNav('animation')}>
              See How It Works <ArrowRight className="w-4 h-4" />
            </Button>
            <Button variant="secondary" onClick={() => triggerNav('solution')}>
              Explore the Solution
            </Button>
          </div>

          <div className="mt-12 flex items-center justify-center gap-3 text-xs font-mono text-gray-500">
            <span className="opacity-70">Scroll</span>
            <ArrowDown className="w-4 h-4 animate-bounce" />
            <span className="opacity-70">to explore</span>
          </div>
        </div>
      </header>

      <Marquee />

      <main className="w-full">
        {/* 1. Problem */}
        <section id="problem" className="py-32 px-6 border-b border-white/10 max-w-7xl mx-auto">
          <SectionHeader number="01" title="The Screen Problem" subtitle="OLED and LCD screens are exhausting our eyes." />
          <div className="grid md:grid-cols-2 gap-8">
            <Card title="Health Risk" icon={Eye} kicker="Pain point">
              Current screens cause eye strain and deteriorating comfort due to emission, glare, and flicker.
            </Card>
            <Card title="Failed Fixes" icon={AlertTriangle} kicker="Reality">
              Blue light filters and dark mode are software patches for a hardware problem.
            </Card>
          </div>
        </section>

        {/* 2. Solution */}
        <section id="solution" className="py-32 px-6 border-b border-white/10 max-w-7xl mx-auto">
          <SectionHeader number="02" title="The E‑Ink Solution" subtitle="A triple win: health, sustainability, repairability." />
          <div className="grid md:grid-cols-3 gap-6">
            <Card title="Health" icon={Eye} className="border-emerald-500/40" kicker="Comfort">
              Reflective display means far less harsh light emission—built for long reading sessions.
            </Card>
            <Card title="Green" icon={Globe} className="border-emerald-500/40" kicker="Footprint">
              Ultra‑low power for static content cuts use‑phase energy and charging friction.
            </Card>
            <Card title="Repair" icon={ShieldCheck} className="border-emerald-500/40" kicker="Longevity">
              Modular design: replace parts locally, keep devices alive longer, reduce e‑waste.
            </Card>
          </div>

          <div className="mt-10 grid md:grid-cols-2 gap-6">
            <Card title="Open Ecosystem" icon={Cpu} kicker="Strategy">
              Open modules + shared reference designs unlock faster iteration, more vendors, and less lock‑in.
            </Card>
            <Card title="Cosmolocal Assembly" icon={Factory} kicker="Production">
              Global design, local build—shorter logistics, quicker repairs, and local jobs.
            </Card>
          </div>
        </section>

        <Stats />

        {/* 3. Process */}
        <section id="animation">
          <ScrollStory />
        </section>

        {/* 4. Commitments */}
        <section id="commitments" className="py-32 px-6 border-b border-white/10 max-w-7xl mx-auto">
          <SectionHeader number="04" title="Our Commitments" subtitle="Safety and compliance, not vibes." />
          <div className="grid md:grid-cols-2 gap-6">
            <Card title="Photobiological Safety" icon={Target} kicker="Testing">
              Full IEC/EN 62471 testing on the complete display stack (including any frontlight).
            </Card>
            <Card title="Right to Repair" icon={ClipboardList} kicker="Design rules">
              High repairability scores by design: fasteners, spare parts, documentation, non‑proprietary pairing.
            </Card>
          </div>
        </section>

        {/* 5. Readiness */}
        <section id="readiness" className="py-32 px-6 border-b border-white/10 max-w-7xl mx-auto">
          <SectionHeader number="05" title="Readiness" subtitle="Market and manufacturing path." />
          <div className="grid md:grid-cols-2 gap-8">
            <div className="p-6 border border-white/10 bg-neutral-900/50 rounded-lg">
              <h3 className="text-xl font-bold mb-4 flex items-center gap-2">
                <Sparkles className="w-5 h-5 text-red-500" /> Public Demand
              </h3>
              <p className="text-gray-400">Growing concern for eye strain creates real pull for reflective displays.</p>
            </div>
            <div className="p-6 border border-white/10 bg-neutral-900/50 rounded-lg">
              <h3 className="text-xl font-bold mb-4 flex items-center gap-2">
                <Factory className="w-5 h-5 text-red-500" /> Cosmolocal Model
              </h3>
              <p className="text-gray-400">Global design, local production—lighter logistics and better serviceability.</p>
            </div>
          </div>

          <div className="mt-10 grid md:grid-cols-3 gap-6">
            <Card title="Phase 1" icon={Play} kicker="Prototype">
              Build a minimal, text-first OS shell + validated E‑Paper module demo.
            </Card>
            <Card title="Phase 2" icon={Gauge} kicker="Pilot">
              Limited run with a certified micro‑factory partner + repair program.
            </Card>
            <Card title="Phase 3" icon={Leaf} kicker="Scale">
              Expand local nodes, publish modules, and harden compliance workflows.
            </Card>
          </div>
        </section>

        <Testimonials />
        <FAQ />
        <CTA triggerNav={triggerNav} />
      </main>

      <footer className="border-t border-white/10 bg-neutral-950 py-12 px-6 text-center">
        <p className="text-gray-500 font-mono text-sm">Project Eye — Innovation Policy 2025</p>
      </footer>

      {/* Global CSS for custom animations */}
      <style>{`
        .animation-delay-2000{animation-delay:2s}
        .animation-delay-4000{animation-delay:4s}

        @keyframes blob{
          0%,100%{transform:translate(0,0) scale(1)}
          33%{transform:translate(30px,-40px) scale(1.06)}
          66%{transform:translate(-20px,20px) scale(0.98)}
        }
        .animate-blob{animation:blob 12s ease-in-out infinite;}

        @keyframes pan{
          0%{transform:translate3d(0,0,0) scale(1)}
          50%{transform:translate3d(-2%,1%,0) scale(1.02)}
          100%{transform:translate3d(0,0,0) scale(1)}
        }
        .animate-pan{animation:pan 14s ease-in-out infinite;}

        .bg-scanlines{
          background: repeating-linear-gradient(
            to bottom,
            rgba(255,255,255,0.12),
            rgba(255,255,255,0.12) 1px,
            transparent 1px,
            transparent 4px
          );
        }

        .bg-grain{
          background-image:url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="180" height="180"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="3" stitchTiles="stitch"/></filter><rect width="180" height="180" filter="url(%23n)" opacity="0.35"/></svg>');
        }

        @keyframes glitch{
          0%{clip-path:inset(0 0 0 0); transform:translate(0)}
          10%{clip-path:inset(10% 0 70% 0); transform:translate(-2px)}
          20%{clip-path:inset(60% 0 20% 0); transform:translate(2px)}
          30%{clip-path:inset(30% 0 40% 0); transform:translate(-1px)}
          40%{clip-path:inset(80% 0 5% 0); transform:translate(1px)}
          50%{clip-path:inset(0 0 0 0); transform:translate(0)}
          100%{clip-path:inset(0 0 0 0); transform:translate(0)}
        }

        .glitch{
          position:relative;
          display:inline-block;
        }
        .glitch::before,
        .glitch::after{
          content:attr(data-text);
          position:absolute;
          left:0; top:0;
          width:100%;
          opacity:0.65;
          pointer-events:none;
        }
        .glitch::before{color:#ef4444; transform:translate(2px,0); mix-blend-mode:screen; animation:glitch 2.2s infinite;}
        .glitch::after{color:#3b82f6; transform:translate(-2px,0); mix-blend-mode:screen; animation:glitch 2.2s infinite reverse;}

        @keyframes shine{
          0%{transform:translateX(-120%) rotate(12deg)}
          100%{transform:translateX(120%) rotate(12deg)}
        }
        .animate-shine{animation:shine 2.8s ease-in-out infinite;}

        @keyframes floaty{0%,100%{transform:translateY(0)}50%{transform:translateY(-6px)}}
        .animate-float{animation:floaty 2.6s ease-in-out infinite;}

        @keyframes conveyor{0%{transform:translateX(0)}100%{transform:translateX(260%)} }
        .animate-conveyor{animation:conveyor 3.6s linear infinite;}

        @keyframes inkpulse{0%{opacity:0; transform:scale(0.95)}30%{opacity:1}100%{opacity:0; transform:scale(1.12)}}
        .animate-inkpulse{animation:inkpulse 1.1s ease-out;}

        @keyframes marquee{0%{transform:translateX(0)}100%{transform:translateX(-50%)} }
        .animate-marquee{animation:marquee 18s linear infinite;}

        @media (prefers-reduced-motion: reduce){
          .animate-blob,.animate-pan,.animate-shine,.animate-float,.animate-conveyor,.animate-inkpulse,.animate-marquee{animation:none !important;}
          .glitch::before,.glitch::after{animation:none !important;}
        }
      `}</style>
    </div>
  );
}
