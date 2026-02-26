import { useState, useEffect, useRef } from "react";

const SKILLS = [
  { name: "Java", icon: "☕", level: 90, color: "#f89820" },
  { name: "DSA", icon: "🧠", level: 80, color: "#00d4ff" },
  { name: "Spring Boot", icon: "🍃", level: 75, color: "#6db33f" },
  { name: "React", icon: "⚛️", level: 70, color: "#61dafb" },
  { name: "MySQL", icon: "🐬", level: 75, color: "#00758f" },
  { name: "Git", icon: "🔀", level: 85, color: "#f05033" },
];

const FACTS = [
  "Turning ☕ coffee into code since day one",
  "DSA problems? Bring it on 💪",
  "Full-Stack with Java is the dream 🚀",
  "Funny guy who codes seriously 😄",
  "akashmuddapu.netlify.app is live!",
];

function MatrixRain() {
  const canvasRef = useRef(null);
  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext("2d");
    canvas.width = canvas.offsetWidth;
    canvas.height = canvas.offsetHeight;
    const chars = "AKASH01JAVA</>DSA∑∞Ω";
    const fontSize = 13;
    const cols = Math.floor(canvas.width / fontSize);
    const drops = Array(cols).fill(1);

    const draw = () => {
      ctx.fillStyle = "rgba(0,0,0,0.05)";
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = "#0f0";
      ctx.font = `${fontSize}px monospace`;
      drops.forEach((y, i) => {
        const char = chars[Math.floor(Math.random() * chars.length)];
        ctx.fillStyle = `hsl(${120 + i * 2}, 100%, ${40 + Math.random() * 20}%)`;
        ctx.fillText(char, i * fontSize, y * fontSize);
        if (y * fontSize > canvas.height && Math.random() > 0.975) drops[i] = 0;
        drops[i]++;
      });
    };
    const id = setInterval(draw, 40);
    return () => clearInterval(id);
  }, []);
  return <canvas ref={canvasRef} style={{ position: "absolute", inset: 0, width: "100%", height: "100%", opacity: 0.15 }} />;
}

function SkillBar({ skill, delay }) {
  const [width, setWidth] = useState(0);
  useEffect(() => {
    const t = setTimeout(() => setWidth(skill.level), 500 + delay);
    return () => clearTimeout(t);
  }, []);
  return (
    <div style={{ marginBottom: "10px" }}>
      <div style={{ display: "flex", justifyContent: "space-between", marginBottom: "4px", fontSize: "13px", color: "#ccc" }}>
        <span>{skill.icon} {skill.name}</span>
        <span style={{ color: skill.color }}>{skill.level}%</span>
      </div>
      <div style={{ background: "#1a1a2e", borderRadius: "999px", height: "8px", overflow: "hidden", border: "1px solid #333" }}>
        <div style={{
          width: `${width}%`,
          height: "100%",
          background: `linear-gradient(90deg, ${skill.color}aa, ${skill.color})`,
          borderRadius: "999px",
          transition: "width 1.2s cubic-bezier(0.4,0,0.2,1)",
          boxShadow: `0 0 10px ${skill.color}66`,
        }} />
      </div>
    </div>
  );
}

function TypeWriter({ texts }) {
  const [idx, setIdx] = useState(0);
  const [displayed, setDisplayed] = useState("");
  const [deleting, setDeleting] = useState(false);

  useEffect(() => {
    const full = texts[idx];
    let timer;
    if (!deleting && displayed.length < full.length) {
      timer = setTimeout(() => setDisplayed(full.slice(0, displayed.length + 1)), 60);
    } else if (!deleting && displayed.length === full.length) {
      timer = setTimeout(() => setDeleting(true), 2000);
    } else if (deleting && displayed.length > 0) {
      timer = setTimeout(() => setDisplayed(displayed.slice(0, -1)), 30);
    } else if (deleting && displayed.length === 0) {
      setDeleting(false);
      setIdx((i) => (i + 1) % texts.length);
    }
    return () => clearTimeout(timer);
  }, [displayed, deleting, idx]);

  return (
    <span style={{ color: "#00ff88", fontFamily: "monospace", fontSize: "14px" }}>
      {displayed}<span style={{ animation: "blink 1s step-end infinite", color: "#00ff88" }}>|</span>
    </span>
  );
}

export default function AkashProfile() {
  const [activeTab, setActiveTab] = useState("about");
  const [hover, setHover] = useState(null);

  return (
    <div style={{
      minHeight: "100vh",
      background: "#050510",
      display: "flex",
      alignItems: "center",
      justifyContent: "center",
      padding: "20px",
      fontFamily: "'Courier New', monospace",
    }}>
      <style>{`
        @keyframes blink { 50% { opacity: 0; } }
        @keyframes float { 0%,100% { transform: translateY(0); } 50% { transform: translateY(-8px); } }
        @keyframes glow { 0%,100% { text-shadow: 0 0 10px #00ff88, 0 0 20px #00ff88; } 50% { text-shadow: 0 0 20px #00ff88, 0 0 40px #00ff88, 0 0 60px #00ff88; } }
        @keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
        @keyframes slide-in { from { opacity:0; transform:translateX(-20px); } to { opacity:1; transform:translateX(0); } }
        .tab-btn:hover { background: rgba(0,255,136,0.15) !important; }
        .link-btn:hover { transform: scale(1.05); filter: brightness(1.3); }
      `}</style>

      <div style={{
        width: "100%",
        maxWidth: "700px",
        background: "linear-gradient(135deg, rgba(0,255,136,0.04), rgba(0,212,255,0.04))",
        border: "1px solid rgba(0,255,136,0.2)",
        borderRadius: "16px",
        overflow: "hidden",
        position: "relative",
        boxShadow: "0 0 60px rgba(0,255,136,0.08), 0 0 120px rgba(0,0,0,0.8)",
      }}>
        {/* Matrix rain bg */}
        <MatrixRain />

        {/* Header */}
        <div style={{
          padding: "36px 36px 24px",
          background: "linear-gradient(180deg, rgba(0,255,136,0.06) 0%, transparent 100%)",
          borderBottom: "1px solid rgba(0,255,136,0.1)",
          position: "relative",
          zIndex: 1,
        }}>
          <div style={{ display: "flex", alignItems: "center", gap: "20px", flexWrap: "wrap" }}>
            {/* Avatar */}
            <div style={{
              width: "80px", height: "80px",
              borderRadius: "50%",
              background: "linear-gradient(135deg, #00ff88, #00d4ff)",
              display: "flex", alignItems: "center", justifyContent: "center",
              fontSize: "36px",
              animation: "float 4s ease-in-out infinite",
              flexShrink: 0,
              boxShadow: "0 0 30px rgba(0,255,136,0.4)",
            }}>👨‍💻</div>

            <div>
              <h1 style={{
                margin: 0,
                fontSize: "28px",
                fontWeight: "900",
                color: "#fff",
                letterSpacing: "2px",
                animation: "glow 3s ease-in-out infinite",
              }}>MUDDAPU AKASH</h1>
              <div style={{ marginTop: "6px" }}>
                <TypeWriter texts={FACTS} />
              </div>
              <div style={{ marginTop: "8px", display: "flex", gap: "8px", flexWrap: "wrap" }}>
                {["Java Dev", "DSA Enthusiast", "Full-Stack"].map(tag => (
                  <span key={tag} style={{
                    padding: "2px 10px",
                    background: "rgba(0,255,136,0.1)",
                    border: "1px solid rgba(0,255,136,0.3)",
                    borderRadius: "999px",
                    fontSize: "11px",
                    color: "#00ff88",
                  }}>{tag}</span>
                ))}
              </div>
            </div>
          </div>
        </div>

        {/* Tabs */}
        <div style={{
          display: "flex",
          borderBottom: "1px solid rgba(0,255,136,0.1)",
          position: "relative",
          zIndex: 1,
        }}>
          {["about", "skills", "connect"].map(tab => (
            <button key={tab}
              className="tab-btn"
              onClick={() => setActiveTab(tab)}
              style={{
                flex: 1,
                padding: "12px",
                background: activeTab === tab ? "rgba(0,255,136,0.12)" : "transparent",
                border: "none",
                borderBottom: activeTab === tab ? "2px solid #00ff88" : "2px solid transparent",
                color: activeTab === tab ? "#00ff88" : "#666",
                cursor: "pointer",
                fontSize: "12px",
                letterSpacing: "1px",
                textTransform: "uppercase",
                fontFamily: "monospace",
                transition: "all 0.2s",
              }}
            >{`> ${tab}`}</button>
          ))}
        </div>

        {/* Content */}
        <div style={{ padding: "28px 36px", position: "relative", zIndex: 1, minHeight: "260px" }}>
          {activeTab === "about" && (
            <div style={{ animation: "slide-in 0.4s ease" }}>
              <div style={{ color: "#888", fontSize: "12px", marginBottom: "16px" }}>// whoami.md</div>
              <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: "12px" }}>
                {[
                  { icon: "🌱", label: "Learning", value: "DSA · Frameworks · Full-Stack Java" },
                  { icon: "⚡", label: "Fun Fact", value: "I am genuinely funny 😄" },
                  { icon: "📫", label: "Email", value: "akashmuddapu@gmail.com" },
                  { icon: "🌐", label: "Portfolio", value: "akashmuddapu.netlify.app" },
                ].map(item => (
                  <div key={item.label} style={{
                    padding: "14px",
                    background: "rgba(255,255,255,0.02)",
                    border: "1px solid rgba(0,255,136,0.1)",
                    borderRadius: "10px",
                    transition: "border-color 0.2s",
                  }}>
                    <div style={{ fontSize: "18px", marginBottom: "4px" }}>{item.icon}</div>
                    <div style={{ fontSize: "10px", color: "#555", textTransform: "uppercase", letterSpacing: "1px" }}>{item.label}</div>
                    <div style={{ fontSize: "12px", color: "#ccc", marginTop: "2px" }}>{item.value}</div>
                  </div>
                ))}
              </div>

              {/* GitHub streak placeholder */}
              <div style={{
                marginTop: "16px",
                padding: "14px",
                background: "rgba(0,255,136,0.03)",
                border: "1px solid rgba(0,255,136,0.1)",
                borderRadius: "10px",
                textAlign: "center",
              }}>
                <div style={{ fontSize: "11px", color: "#555", marginBottom: "8px" }}>// CONTRIBUTION HEATMAP</div>
                <div style={{ display: "flex", gap: "3px", justifyContent: "center", flexWrap: "wrap" }}>
                  {Array.from({ length: 52 }, (_, w) =>
                    Array.from({ length: 7 }, (_, d) => {
                      const intensity = Math.random();
                      const bg = intensity > 0.85 ? "#00ff88" : intensity > 0.65 ? "#00cc6a" : intensity > 0.4 ? "#008844" : intensity > 0.2 ? "#004422" : "#111";
                      return <div key={`${w}-${d}`} style={{ width: "9px", height: "9px", background: bg, borderRadius: "2px" }} />;
                    })
                  )}
                </div>
              </div>
            </div>
          )}

          {activeTab === "skills" && (
            <div style={{ animation: "slide-in 0.4s ease" }}>
              <div style={{ color: "#888", fontSize: "12px", marginBottom: "16px" }}>// skills.json</div>
              {SKILLS.map((skill, i) => <SkillBar key={skill.name} skill={skill} delay={i * 100} />)}
            </div>
          )}

          {activeTab === "connect" && (
            <div style={{ animation: "slide-in 0.4s ease" }}>
              <div style={{ color: "#888", fontSize: "12px", marginBottom: "20px" }}>// reach_me.sh</div>
              <div style={{ display: "flex", flexDirection: "column", gap: "12px" }}>
                {[
                  { icon: "📧", label: "Email", value: "akashmuddapu@gmail.com", href: "mailto:akashmuddapu@gmail.com", color: "#ea4335" },
                  { icon: "🌐", label: "Portfolio", value: "akashmuddapu.netlify.app", href: "https://akashmuddapu.netlify.app", color: "#00d4ff" },
                  { icon: "📄", label: "Resume", value: "View on Google Drive", href: "https://drive.google.com/file/d/1yaWAa1GyjLKNjTHbEC4DtbDRsPHhAGKY/view?usp=sharing", color: "#00ff88" },
                ].map(item => (
                  <a key={item.label}
                    href={item.href}
                    target="_blank"
                    rel="noopener noreferrer"
                    className="link-btn"
                    style={{
                      display: "flex",
                      alignItems: "center",
                      gap: "16px",
                      padding: "14px 18px",
                      background: "rgba(255,255,255,0.02)",
                      border: `1px solid ${item.color}33`,
                      borderRadius: "10px",
                      textDecoration: "none",
                      transition: "all 0.2s",
                    }}
                  >
                    <span style={{ fontSize: "22px" }}>{item.icon}</span>
                    <div>
                      <div style={{ fontSize: "10px", color: "#555", textTransform: "uppercase", letterSpacing: "1px" }}>{item.label}</div>
                      <div style={{ fontSize: "13px", color: item.color }}>{item.value}</div>
                    </div>
                    <span style={{ marginLeft: "auto", color: item.color, fontSize: "16px" }}>→</span>
                  </a>
                ))}
              </div>
            </div>
          )}
        </div>

        {/* Footer */}
        <div style={{
          padding: "12px 36px",
          borderTop: "1px solid rgba(0,255,136,0.08)",
          display: "flex",
          justifyContent: "space-between",
          alignItems: "center",
          position: "relative",
          zIndex: 1,
        }}>
          <span style={{ fontSize: "10px", color: "#333" }}>© 2025 Muddapu Akash</span>
          <span style={{ fontSize: "10px", color: "#00ff88", animation: "glow 2s ease-in-out infinite" }}>{"</ code with passion >"}</span>
        </div>
      </div>
    </div>
  );
}
