import { useState, useRef, useCallback } from "react";

// ── Whop checkout URLs ─────────────────────────────────────────────────────────
// Replace each value with your real Whop product checkout links from your Whop dashboard
const WHOP_LINKS = {
  trial:    "https://whop.com/checkout/plan_guCs4iPVzWhXU",
  pro:      "https://whop.com/checkout/plan_if3dszNlKEUWj",
  unlimited:"https://whop.com/checkout/plan_gcpXkeR3nrEnp",
};

// ── Plans ──────────────────────────────────────────────────────────────────────
const PLANS = [
  {
    id: "trial",
    name: "Starter Trial",
    price: "$1",
    period: "one-time",
    generations: 1,
    badge: null,
    color: "#6C63FF",
    gradTo: "#A78BFA",
    desc: "Try it once. No commitment.",
    features: [
      "1 product generation",
      "All 4 product types",
      "Custom branding & logo",
      "Professional PDF export",
      "Trending topic suggestions",
    ],
  },
  {
    id: "pro",
    name: "Pro",
    price: "$19.99",
    period: "/month",
    generations: 50,
    badge: "Most Popular",
    color: "#FF6584",
    gradTo: "#6C63FF",
    desc: "For active creators building a library.",
    features: [
      "50 generations per month",
      "All 4 product types",
      "Custom branding & logo",
      "Professional PDF export",
      "Trending topic suggestions",
      "Priority support",
    ],
  },
  {
    id: "unlimited",
    name: "Unlimited",
    price: "$249",
    period: "one-time",
    generations: Infinity,
    badge: "Best Value",
    color: "#F59E0B",
    gradTo: "#F97316",
    desc: "Build your empire without limits.",
    features: [
      "Unlimited generations",
      "All 4 product types",
      "Custom branding & logo",
      "Professional PDF export",
      "Trending topic suggestions",
      "Priority support",
      "Early access to new features",
    ],
  },
];

// ── Trending niches ────────────────────────────────────────────────────────────
const NICHE_TOPICS = [
  "morning routine optimization","stoic mindset","dopamine detox","anxiety relief habits",
  "manifestation journal","deep work focus","self-discipline blueprint","toxic relationship recovery",
  "financial freedom mindset","identity-based habits","emotional intelligence","sleep optimization",
  "confidence building","social anxiety mastery","digital minimalism","boundary setting",
  "imposter syndrome","overthinking cure","motivation science","burnout recovery",
];

// ── Color schemes ──────────────────────────────────────────────────────────────
const COLOR_SCHEMES = [
  { id:"midnight", label:"Midnight",  primary:"#6C63FF", secondary:"#FF6584", bg:"#0F0F1A" },
  { id:"forest",   label:"Forest",    primary:"#2D6A4F", secondary:"#74C69D", bg:"#F0FDF4" },
  { id:"ember",    label:"Ember",     primary:"#C0392B", secondary:"#F39C12", bg:"#FFF8F0" },
  { id:"ocean",    label:"Ocean",     primary:"#0077B6", secondary:"#90E0EF", bg:"#F0F8FF" },
  { id:"rose",     label:"Rose Gold", primary:"#B76E79", secondary:"#F4A261", bg:"#FFF5F5" },
  { id:"obsidian", label:"Obsidian",  primary:"#CBD5E1", secondary:"#94A3B8", bg:"#0D1117" },
];

const LENGTH_CONFIG = { short: 3, medium: 5, long: 8 };

const TESTIMONIALS = [
  { name:"Sarah M.",  role:"Life Coach",       avatar:"SM", text:"I created my entire ebook library in a single afternoon. The quality is incredible — my clients can't believe I made it so fast.", stars:5 },
  { name:"Jordan K.", role:"Online Educator",  avatar:"JK", text:"The branding feature is a game-changer. Every product looks like it came from a professional design studio.", stars:5 },
  { name:"Priya N.",  role:"Wellness Creator", avatar:"PN", text:"I've tried every content tool out there. Nothing comes close to this for speed, quality, and polish.", stars:5 },
];

// ── Helpers ────────────────────────────────────────────────────────────────────
const hexToRgb = h => {
  const r = parseInt(h.slice(1,3),16), g = parseInt(h.slice(3,5),16), b = parseInt(h.slice(5,7),16);
  return `${r},${g},${b}`;
};
function Stars({ n }) {
  return <span style={{color:"#FFD700",letterSpacing:2}}>{Array(n).fill("★").join("")}</span>;
}
function Card({ title, children }) {
  return (
    <div style={{background:"#111",border:"1px solid #1E1E2E",borderRadius:14,padding:24}}>
      <div style={{fontSize:11,letterSpacing:3,textTransform:"uppercase",color:"#6C63FF",marginBottom:18,fontWeight:700}}>{title}</div>
      {children}
    </div>
  );
}
function Label({ children, mt }) {
  return <div style={{fontSize:11,color:"#64748B",marginBottom:8,marginTop:mt?18:0,fontWeight:600,letterSpacing:.5,textTransform:"uppercase"}}>{children}</div>;
}
function Input({ value, onChange, placeholder }) {
  return (
    <input value={value} onChange={e=>onChange(e.target.value)} placeholder={placeholder}
      style={{width:"100%",background:"#0D0D12",border:"1px solid #1E1E2E",borderRadius:8,padding:"10px 14px",color:"#E2E8F0",fontSize:14,outline:"none",boxSizing:"border-box",fontFamily:"inherit"}}
      onFocus={e=>e.target.style.borderColor="#6C63FF"}
      onBlur={e=>e.target.style.borderColor="#1E1E2E"}/>
  );
}
function ToggleBtn({ active, onClick, children, color }) {
  return (
    <button onClick={onClick}
      style={{padding:"10px 12px",borderRadius:8,border:active?`2px solid ${color}`:"2px solid #1E1E2E",background:active?`rgba(${hexToRgb(color)},.15)`:"#0D0D12",color:active?"#fff":"#555",cursor:"pointer",fontSize:13,fontWeight:active?700:400,transition:"all .15s",fontFamily:"inherit"}}>
      {children}
    </button>
  );
}
const navBtn = {background:"#1E1E2E",border:"none",color:"#94A3B8",padding:"8px 16px",borderRadius:8,cursor:"pointer",fontSize:13,fontWeight:600,fontFamily:"inherit"};

// ═══════════════════════════════════════════════════════════════════════════════
//  MAIN APP
// ═══════════════════════════════════════════════════════════════════════════════
export default function App() {
  const [page, setPage]         = useState("landing"); // landing | pricing | build | download
  const [selectedPlan, setPlan] = useState(null);
  const [branding, setBranding] = useState({ name:"", scheme:COLOR_SCHEMES[0], logo:null });
  const [form, setForm]         = useState({ topic:"", type:"ebook", length:"medium" });
  const [generated, setGen]     = useState(null);
  const [loading, setLoading]   = useState(false);
  const [activeSection, setActiveSection] = useState(null);
  const logoRef = useRef();

  // ── Generate via Claude API ──────────────────────────────────────────────────
  const generate = useCallback(async () => {
    if (!form.topic.trim()) return;
    setLoading(true);
    setGen(null);
    const sectionCount = LENGTH_CONFIG[form.length];
    const relatedTopics = NICHE_TOPICS.filter(t => t !== form.topic).slice(0,5).join(", ");
    const prompt = `You are a bestselling self-help author creating a premium digital product for the Whop platform.

Topic: "${form.topic}"
Product type: ${form.type}
Sections: ${sectionCount}
Related trending topics for inspiration: ${relatedTopics}

Create a complete, fully-formatted ${form.type} with:
1. A compelling, marketable title
2. A one-sentence hook/subtitle
3. A short intro paragraph (2-3 sentences) that sells the transformation
4. Exactly ${sectionCount} sections, each with:
   - A numbered title
   - 3-5 substantive paragraphs of actionable, high-quality content
   - 3 key takeaway bullet points

Respond ONLY in this exact JSON format (no markdown fences):
{
  "title": "...",
  "subtitle": "...",
  "intro": "...",
  "sections": [
    { "title": "...", "content": "...", "takeaways": ["...","...","..."] }
  ]
}`;

    try {
      const res = await fetch("/.netlify/functions/generate", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ prompt }),
      });
      if (!res.ok) throw new Error(`Server error: ${res.status}`);
      const data = await res.json();
      const textBlock = data.content?.find(b => b.type === "text");
      const raw = textBlock?.text || "{}";
      const clean = raw.replace(/```json|```/g,"").trim();
      const parsed = JSON.parse(clean);
      setGen(parsed);
      setActiveSection(0);
    } catch {
      setGen({
        title: `The Ultimate ${form.topic.split(" ").map(w=>w[0]?.toUpperCase()+w.slice(1)).join(" ")} ${form.type==="checklist"?"Checklist":"Guide"}`,
        subtitle: "Transform your life with proven, science-backed strategies",
        intro: `This ${form.type} distills the most powerful insights on ${form.topic} into an actionable roadmap. Whether you're just starting out or looking to level up, every page moves you closer to the breakthrough you've been seeking.`,
        sections: Array.from({length: sectionCount}, (_,i) => ({
          title: `Chapter ${i+1}: ${["Foundation","Mindset","Systems","Action","Mastery","Identity","Momentum","Legacy"][i]||`Step ${i+1}`}`,
          content: `This section covers the essential principles of ${form.topic}. Understanding these fundamentals will unlock every other strategy in this ${form.type}. Real transformation starts with awareness — knowing exactly where you are and where you need to go.\n\nThe research is clear: people who succeed with ${form.topic} share a common set of beliefs and behaviors. They don't rely on motivation; they build systems. They don't wait for perfect conditions; they act with what they have.\n\nAs you work through this section, take notes. Mark the ideas that resonate most. Your job is to apply at least one insight before moving to the next chapter.`,
          takeaways:[`Master the core principles of ${form.topic}`,"Build systems that outlast motivation","Take immediate, imperfect action"]
        }))
      });
      setActiveSection(0);
    }
    setLoading(false);
  }, [form]);

  // ── PDF download (print-to-PDF) ──────────────────────────────────────────────
  const downloadPDF = () => {
    if (!generated) return;
    const w = window.open("","_blank");
    const s = branding.scheme;
    const brandName = branding.name || "CreatorPro";
    const logoSrc = branding.logo || "";

    const sectionsHTML = generated.sections.map((sec,i) => `
      <div class="section-block">
        <div class="section-header" style="background:${s.primary}">
          <span class="section-num">${String(i+1).padStart(2,"0")}</span>
          <h2>${sec.title}</h2>
        </div>
        <div class="section-body">
          ${sec.content.split("\n\n").map(p=>`<p>${p}</p>`).join("")}
          <div class="takeaways" style="border-left:4px solid ${s.primary}">
            <h4 style="color:${s.primary}">Key Takeaways</h4>
            <ul>${sec.takeaways.map(t=>`<li>${t}</li>`).join("")}</ul>
          </div>
        </div>
        <div class="section-divider" style="background:${s.secondary}"></div>
      </div>`).join("");

    const tocHTML = generated.sections.map((sec,i)=>`
      <div class="toc-item">
        <span class="toc-num" style="color:${s.secondary}">${String(i+1).padStart(2,"0")}</span>
        <span class="toc-title">${sec.title}</span>
      </div>`).join("");

    w.document.write(`<!DOCTYPE html><html><head><meta charset="UTF-8">
    <style>
      @import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700;900&family=Inter:wght@300;400;500&display=swap');
      *{margin:0;padding:0;box-sizing:border-box}
      body{font-family:'Inter',sans-serif;color:#1a1a1a;background:#fff}
      .cover{min-height:100vh;background:${s.primary};display:flex;flex-direction:column;justify-content:center;align-items:center;text-align:center;padding:60px;page-break-after:always;position:relative}
      .cover::after{content:'';position:absolute;bottom:0;left:0;right:0;height:6px;background:${s.secondary}}
      .cover-logo{width:80px;height:80px;object-fit:contain;margin-bottom:24px;border-radius:50%;background:rgba(255,255,255,.15);padding:10px}
      .cover-brand{font-size:13px;letter-spacing:4px;text-transform:uppercase;color:rgba(255,255,255,.7);margin-bottom:48px}
      .cover-tag{background:rgba(255,255,255,.2);color:#fff;font-size:11px;letter-spacing:3px;text-transform:uppercase;padding:8px 20px;border-radius:20px;margin-bottom:32px;display:inline-block}
      .cover-title{font-family:'Playfair Display',serif;font-size:52px;font-weight:900;color:#fff;line-height:1.15;margin-bottom:20px;max-width:700px}
      .cover-subtitle{font-size:18px;color:rgba(255,255,255,.85);max-width:500px;line-height:1.6}
      .toc-page{padding:80px 60px;page-break-after:always}
      .toc-heading{font-family:'Playfair Display',serif;font-size:36px;color:${s.primary};margin-bottom:8px}
      .toc-rule{height:3px;background:${s.secondary};width:60px;margin-bottom:40px}
      .toc-item{display:flex;align-items:center;gap:16px;padding:16px 0;border-bottom:1px solid #f0f0f0}
      .toc-num{font-family:'Playfair Display',serif;font-size:22px;font-weight:700;min-width:36px}
      .toc-title{font-size:15px;color:#333;font-weight:500}
      .intro-page{padding:80px 60px;page-break-after:always}
      .intro-heading{font-family:'Playfair Display',serif;font-size:28px;color:${s.primary};margin-bottom:24px}
      .intro-bar{height:3px;width:50px;background:${s.secondary};margin-bottom:32px}
      .intro-text{font-size:16px;line-height:1.8;color:#444}
      .section-block{padding:60px;page-break-after:always}
      .section-header{padding:32px 40px;border-radius:12px;margin-bottom:36px;display:flex;align-items:center;gap:24px}
      .section-num{font-family:'Playfair Display',serif;font-size:48px;font-weight:900;opacity:.4;line-height:1;color:#fff}
      .section-header h2{font-family:'Playfair Display',serif;font-size:28px;font-weight:700;color:#fff;line-height:1.3}
      .section-body p{font-size:15px;line-height:1.85;color:#444;margin-bottom:20px}
      .takeaways{padding:24px 28px;margin-top:32px;background:#fafafa;border-radius:0 8px 8px 0}
      .takeaways h4{font-size:12px;letter-spacing:3px;text-transform:uppercase;margin-bottom:14px}
      .takeaways ul{list-style:none;padding:0}
      .takeaways li{font-size:14px;padding:6px 0;padding-left:20px;position:relative;color:#555}
      .takeaways li::before{content:'→';position:absolute;left:0;color:${s.primary}}
      .section-divider{height:3px;border-radius:2px;margin-top:40px;opacity:.3}
      .footer{text-align:center;padding:40px;background:#fafafa;font-size:12px;color:#999;letter-spacing:1px}
      @media print{body{-webkit-print-color-adjust:exact;print-color-adjust:exact}}
    </style><title>${generated.title}</title></head><body>
    <div class="cover">
      ${logoSrc?`<img class="cover-logo" src="${logoSrc}" alt="logo"/>`:""}
      <div class="cover-brand">${brandName}</div>
      <div class="cover-tag">${form.type.toUpperCase()} · ${form.length.toUpperCase()}</div>
      <h1 class="cover-title">${generated.title}</h1>
      <p class="cover-subtitle">${generated.subtitle}</p>
    </div>
    <div class="intro-page">
      <h2 class="intro-heading">Welcome</h2>
      <div class="intro-bar"></div>
      <p class="intro-text">${generated.intro}</p>
    </div>
    <div class="toc-page">
      <h2 class="toc-heading">Contents</h2>
      <div class="toc-rule"></div>
      ${tocHTML}
    </div>
    ${sectionsHTML}
    <div class="footer">© ${new Date().getFullYear()} ${brandName} · All Rights Reserved</div>
    </body></html>`);
    w.document.close();
    setTimeout(()=>{ w.print(); }, 600);
  };

  // ── Routing ──────────────────────────────────────────────────────────────────
  if (page === "landing")  return <LandingPage  onStart={()=>setPage("pricing")} />;
  if (page === "pricing")  return <PricingPage  onSelect={plan=>{ setPlan(plan); setPage("build"); }} onBack={()=>setPage("landing")} />;
  if (page === "download") return <DownloadPage product={generated} branding={branding} form={form} plan={selectedPlan} onDownload={downloadPDF} onBack={()=>setPage("build")} />;

  // ── Build page ───────────────────────────────────────────────────────────────
  return (
    <div style={{minHeight:"100vh",background:"#09090B",color:"#E2E8F0",fontFamily:"'Inter',sans-serif"}}>

      {/* Topbar */}
      <div style={{borderBottom:"1px solid #1E1E2E",padding:"14px 32px",display:"flex",alignItems:"center",justifyContent:"space-between",background:"rgba(9,9,11,.95)",backdropFilter:"blur(10px)",position:"sticky",top:0,zIndex:100}}>
        <div style={{display:"flex",alignItems:"center",gap:10}}>
          <div style={{width:32,height:32,borderRadius:8,background:"linear-gradient(135deg,#6C63FF,#FF6584)",display:"flex",alignItems:"center",justifyContent:"center",fontWeight:900,fontSize:16,color:"#fff"}}>W</div>
          <span style={{fontWeight:700,fontSize:17,letterSpacing:"-0.5px"}}>Whop Creator</span>
        </div>
        <div style={{display:"flex",alignItems:"center",gap:12}}>
          {selectedPlan && (
            <div style={{background:"rgba(108,99,255,.15)",border:"1px solid rgba(108,99,255,.3)",borderRadius:20,padding:"4px 14px",fontSize:12,color:"#A78BFA"}}>
              {selectedPlan.name}
            </div>
          )}
          <button onClick={()=>setPage("landing")} style={{background:"transparent",border:"1px solid #333",color:"#888",padding:"6px 16px",borderRadius:6,cursor:"pointer",fontSize:13,fontFamily:"inherit"}}>← Back</button>
        </div>
      </div>

      <div style={{maxWidth:1200,margin:"0 auto",padding:"36px 24px",display:"grid",gridTemplateColumns:"370px 1fr",gap:28}}>

        {/* ── LEFT PANEL ──────────────────────────────────────────────── */}
        <div style={{display:"flex",flexDirection:"column",gap:18}}>

          {/* Branding */}
          <Card title="① Brand Identity">
            <Label>Brand Name</Label>
            <Input value={branding.name} onChange={v=>setBranding(p=>({...p,name:v}))} placeholder="e.g. MindShift Press"/>

            <Label mt>Color Scheme</Label>
            <div style={{display:"grid",gridTemplateColumns:"repeat(3,1fr)",gap:8}}>
              {COLOR_SCHEMES.map(s=>(
                <button key={s.id} onClick={()=>setBranding(p=>({...p,scheme:s}))}
                  style={{padding:"10px 6px",borderRadius:8,border:branding.scheme.id===s.id?`2px solid ${s.primary}`:"2px solid #222",background:branding.scheme.id===s.id?"#1a1a2e":"#111",cursor:"pointer",display:"flex",flexDirection:"column",alignItems:"center",gap:6,transition:"all .2s",fontFamily:"inherit"}}>
                  <div style={{width:32,height:14,borderRadius:4,background:`linear-gradient(90deg,${s.primary},${s.secondary})`}}/>
                  <span style={{fontSize:10,color:branding.scheme.id===s.id?"#fff":"#555"}}>{s.label}</span>
                </button>
              ))}
            </div>

            <Label mt>Logo (optional)</Label>
            <div onClick={()=>logoRef.current?.click()}
              style={{border:"2px dashed #333",borderRadius:8,padding:"18px",textAlign:"center",cursor:"pointer",background:"#0D0D12",transition:"border-color .2s"}}
              onMouseEnter={e=>e.currentTarget.style.borderColor=branding.scheme.primary}
              onMouseLeave={e=>e.currentTarget.style.borderColor="#333"}>
              {branding.logo
                ? <img src={branding.logo} style={{maxHeight:56,maxWidth:"100%",objectFit:"contain"}} alt="logo"/>
                : <span style={{fontSize:13,color:"#444"}}>Click to upload logo</span>}
            </div>
            <input ref={logoRef} type="file" accept="image/*" style={{display:"none"}} onChange={e=>{
              const f=e.target.files[0]; if(!f) return;
              const r=new FileReader(); r.onload=ev=>setBranding(p=>({...p,logo:ev.target.result})); r.readAsDataURL(f);
            }}/>
          </Card>

          {/* Product config */}
          <Card title="② Product Setup">
            <Label>Topic</Label>
            <Input value={form.topic} onChange={v=>setForm(p=>({...p,topic:v}))} placeholder="e.g. morning routine optimization"/>
            <div style={{marginTop:8}}>
              <div style={{fontSize:11,color:"#444",marginBottom:6}}>Trending:</div>
              <div style={{display:"flex",flexWrap:"wrap",gap:4}}>
                {NICHE_TOPICS.slice(0,9).map(t=>(
                  <button key={t} onClick={()=>setForm(p=>({...p,topic:t}))}
                    style={{fontSize:10,padding:"3px 9px",borderRadius:20,border:"1px solid #2a2a2a",background:form.topic===t?branding.scheme.primary:"#111",color:form.topic===t?"#fff":"#555",cursor:"pointer",fontFamily:"inherit",transition:"all .15s"}}>{t}</button>
                ))}
              </div>
            </div>

            <Label mt>Product Type</Label>
            <div style={{display:"grid",gridTemplateColumns:"repeat(2,1fr)",gap:8}}>
              {["ebook","guide","checklist","worksheet"].map(t=>(
                <ToggleBtn key={t} active={form.type===t} color={branding.scheme.primary} onClick={()=>setForm(p=>({...p,type:t}))}>
                  {{"ebook":"📖 Ebook","guide":"🗺️ Guide","checklist":"✅ Checklist","worksheet":"📝 Worksheet"}[t]}
                </ToggleBtn>
              ))}
            </div>

            <Label mt>Length</Label>
            <div style={{display:"grid",gridTemplateColumns:"repeat(3,1fr)",gap:8}}>
              {["short","medium","long"].map(l=>(
                <ToggleBtn key={l} active={form.length===l} color={branding.scheme.primary} onClick={()=>setForm(p=>({...p,length:l}))}>
                  {l.charAt(0).toUpperCase()+l.slice(1)}&nbsp;<span style={{opacity:.45,fontSize:10}}>({LENGTH_CONFIG[l]})</span>
                </ToggleBtn>
              ))}
            </div>
          </Card>

          {/* Actions */}
          <button onClick={generate} disabled={loading||!form.topic.trim()}
            style={{padding:"15px",borderRadius:12,border:"none",background:!form.topic.trim()?"#1a1a1a":`linear-gradient(135deg,${branding.scheme.primary},${branding.scheme.secondary})`,color:!form.topic.trim()?"#444":"#fff",fontWeight:700,fontSize:15,cursor:!form.topic.trim()?"not-allowed":"pointer",transition:"all .2s",boxShadow:!form.topic.trim()?"none":`0 8px 28px rgba(${hexToRgb(branding.scheme.primary)},.4)`,fontFamily:"inherit"}}
            onMouseEnter={e=>{if(form.topic.trim()&&!loading)e.currentTarget.style.transform="translateY(-2px)"}}
            onMouseLeave={e=>e.currentTarget.style.transform="translateY(0)"}>
            {loading ? "✨ Generating…" : "✨ Generate Product"}
          </button>

          {generated && !loading && (
            <button onClick={()=>setPage("download")}
              style={{padding:"13px",borderRadius:12,border:"none",background:"linear-gradient(135deg,#22c55e,#16a34a)",color:"#fff",fontWeight:700,fontSize:15,cursor:"pointer",boxShadow:"0 8px 24px rgba(34,197,94,.3)",fontFamily:"inherit"}}>
              ⬇ Download PDF
            </button>
          )}
        </div>

        {/* ── RIGHT PANEL — Preview ────────────────────────────────────── */}
        <div>
          {loading && (
            <div style={{display:"flex",flexDirection:"column",alignItems:"center",justifyContent:"center",minHeight:500,gap:16}}>
              <div style={{width:52,height:52,borderRadius:"50%",border:`3px solid ${branding.scheme.primary}`,borderTopColor:"transparent",animation:"spin 1s linear infinite"}}/>
              <p style={{color:"#555",fontSize:14}}>Researching trends & crafting your product…</p>
              <style>{`@keyframes spin{to{transform:rotate(360deg)}}`}</style>
            </div>
          )}

          {!loading && !generated && (
            <div style={{display:"flex",flexDirection:"column",alignItems:"center",justifyContent:"center",minHeight:500,gap:12,border:"1px dashed #1E1E2E",borderRadius:16,background:"#0D0D12"}}>
              <div style={{fontSize:44}}>📄</div>
              <p style={{color:"#3a3a3a",fontSize:14,textAlign:"center",maxWidth:260,lineHeight:1.6}}>Fill in the details on the left and click Generate to create your digital product.</p>
            </div>
          )}

          {!loading && generated && (
            <div style={{background:"#111",borderRadius:16,overflow:"hidden",border:"1px solid #1E1E2E"}}>
              {/* Cover preview */}
              <div style={{background:branding.scheme.primary,padding:"44px 40px",textAlign:"center",position:"relative"}}>
                <div style={{position:"absolute",bottom:0,left:0,right:0,height:4,background:branding.scheme.secondary}}/>
                {branding.logo && <img src={branding.logo} style={{width:50,height:50,objectFit:"contain",borderRadius:"50%",background:"rgba(255,255,255,.15)",padding:8,marginBottom:14,display:"block",margin:"0 auto 14px"}} alt="logo"/>}
                {branding.name && <div style={{fontSize:10,letterSpacing:4,textTransform:"uppercase",color:"rgba(255,255,255,.55)",marginBottom:18}}>{branding.name}</div>}
                <div style={{display:"inline-block",background:"rgba(255,255,255,.18)",color:"#fff",fontSize:10,letterSpacing:3,textTransform:"uppercase",padding:"5px 14px",borderRadius:20,marginBottom:18}}>{form.type} · {form.length}</div>
                <h1 style={{fontFamily:"Georgia,serif",fontSize:26,fontWeight:900,color:"#fff",lineHeight:1.2,marginBottom:10,maxWidth:440,margin:"0 auto 10px"}}>{generated.title}</h1>
                <p style={{fontSize:13,color:"rgba(255,255,255,.75)"}}>{generated.subtitle}</p>
              </div>

              {/* TOC */}
              <div style={{padding:"24px 28px",borderBottom:"1px solid #161616"}}>
                <div style={{fontSize:10,letterSpacing:3,textTransform:"uppercase",color:branding.scheme.secondary,marginBottom:14}}>Table of Contents</div>
                {generated.sections.map((sec,i)=>(
                  <button key={i} onClick={()=>setActiveSection(i)}
                    style={{width:"100%",display:"flex",alignItems:"center",gap:12,padding:"9px 0",background:"transparent",border:"none",borderBottom:"1px solid #161616",cursor:"pointer",textAlign:"left",fontFamily:"inherit"}}>
                    <span style={{fontFamily:"Georgia,serif",fontSize:16,color:branding.scheme.secondary,minWidth:26,fontWeight:700}}>{String(i+1).padStart(2,"0")}</span>
                    <span style={{fontSize:13,color:activeSection===i?"#fff":"#666",fontWeight:activeSection===i?600:400}}>{sec.title}</span>
                    {activeSection===i && <span style={{marginLeft:"auto",color:branding.scheme.primary,fontSize:12}}>▶</span>}
                  </button>
                ))}
              </div>

              {/* Section content */}
              {activeSection !== null && generated.sections[activeSection] && (
                <div style={{padding:"28px"}}>
                  <div style={{display:"flex",alignItems:"center",gap:14,marginBottom:22}}>
                    <div style={{width:44,height:44,borderRadius:10,background:branding.scheme.primary,display:"flex",alignItems:"center",justifyContent:"center",fontFamily:"Georgia,serif",fontSize:18,fontWeight:900,color:"#fff",flexShrink:0}}>{activeSection+1}</div>
                    <h2 style={{fontFamily:"Georgia,serif",fontSize:19,color:"#E2E8F0",lineHeight:1.3}}>{generated.sections[activeSection].title}</h2>
                  </div>
                  {generated.sections[activeSection].content.split("\n\n").map((p,i)=>(
                    <p key={i} style={{fontSize:14,lineHeight:1.85,color:"#7A8899",marginBottom:14}}>{p}</p>
                  ))}
                  <div style={{borderLeft:`3px solid ${branding.scheme.primary}`,paddingLeft:18,marginTop:22,paddingTop:4}}>
                    <div style={{fontSize:10,letterSpacing:3,textTransform:"uppercase",color:branding.scheme.primary,marginBottom:10}}>Key Takeaways</div>
                    {generated.sections[activeSection].takeaways.map((t,i)=>(
                      <div key={i} style={{fontSize:13,color:"#7A8899",padding:"5px 0",display:"flex",gap:10,alignItems:"flex-start"}}>
                        <span style={{color:branding.scheme.secondary,flexShrink:0}}>→</span>{t}
                      </div>
                    ))}
                  </div>
                  <div style={{display:"flex",gap:8,marginTop:24}}>
                    {activeSection>0 && <button onClick={()=>setActiveSection(p=>p-1)} style={navBtn}>← Prev</button>}
                    {activeSection<generated.sections.length-1 && <button onClick={()=>setActiveSection(p=>p+1)} style={{...navBtn,marginLeft:"auto"}}>Next →</button>}
                  </div>
                </div>
              )}
            </div>
          )}
        </div>
      </div>
    </div>
  );
}

// ═══════════════════════════════════════════════════════════════════════════════
//  LANDING PAGE
// ═══════════════════════════════════════════════════════════════════════════════
function LandingPage({ onStart }) {
  return (
    <div style={{minHeight:"100vh",background:"#09090B",color:"#E2E8F0",fontFamily:"'Inter',sans-serif",overflowX:"hidden"}}>

      {/* Nav */}
      <nav style={{padding:"18px 40px",display:"flex",justifyContent:"space-between",alignItems:"center",borderBottom:"1px solid #141414",position:"sticky",top:0,background:"rgba(9,9,11,.96)",backdropFilter:"blur(12px)",zIndex:100}}>
        <div style={{display:"flex",alignItems:"center",gap:10}}>
          <div style={{width:34,height:34,borderRadius:9,background:"linear-gradient(135deg,#6C63FF,#FF6584)",display:"flex",alignItems:"center",justifyContent:"center",fontWeight:900,fontSize:17,color:"#fff"}}>W</div>
          <span style={{fontWeight:800,fontSize:19,letterSpacing:"-0.5px"}}>Whop Creator</span>
        </div>
        <button onClick={onStart} style={{background:"linear-gradient(135deg,#6C63FF,#FF6584)",border:"none",color:"#fff",padding:"10px 22px",borderRadius:8,fontWeight:700,cursor:"pointer",fontSize:14,fontFamily:"inherit"}}>Get Started</button>
      </nav>

      {/* Hero */}
      <section style={{textAlign:"center",padding:"96px 24px 72px",maxWidth:820,margin:"0 auto",position:"relative"}}>
        <div style={{position:"absolute",top:"40%",left:"50%",transform:"translate(-50%,-50%)",width:640,height:640,borderRadius:"50%",background:"radial-gradient(circle,rgba(108,99,255,.1) 0%,transparent 68%)",pointerEvents:"none"}}/>
        <div style={{display:"inline-flex",alignItems:"center",gap:8,background:"rgba(108,99,255,.1)",border:"1px solid rgba(108,99,255,.25)",borderRadius:20,padding:"6px 16px",marginBottom:30,fontSize:13,color:"#A78BFA"}}>
          ✨ The #1 Digital Product Creator for Whop Sellers
        </div>
        <h1 style={{fontFamily:"Georgia,serif",fontSize:"clamp(38px,6.5vw,66px)",fontWeight:900,lineHeight:1.1,marginBottom:22,background:"linear-gradient(140deg,#fff 35%,#6C63FF 80%)",WebkitBackgroundClip:"text",WebkitTextFillColor:"transparent"}}>
          Launch Your Digital<br/>Empire in Minutes
        </h1>
        <p style={{fontSize:19,color:"#52616F",lineHeight:1.75,marginBottom:38,maxWidth:540,margin:"0 auto 38px"}}>
          Turn any topic into a polished, market-ready ebook, guide, checklist, or worksheet — with your branding baked in. No writing. No design. No waiting.
        </p>
        <button onClick={onStart}
          style={{background:"linear-gradient(135deg,#6C63FF,#FF6584)",border:"none",color:"#fff",padding:"17px 46px",borderRadius:12,fontWeight:800,fontSize:17,cursor:"pointer",boxShadow:"0 18px 56px rgba(108,99,255,.38)",letterSpacing:"-0.2px",transition:"transform .2s",fontFamily:"inherit"}}
          onMouseEnter={e=>e.currentTarget.style.transform="translateY(-3px) scale(1.02)"}
          onMouseLeave={e=>e.currentTarget.style.transform="none"}>
          Get Started →
        </button>
        <p style={{marginTop:14,fontSize:13,color:"#2D3748"}}>Powered by Whop · Plans from $1</p>
      </section>

      {/* Features */}
      <section style={{padding:"72px 24px",maxWidth:1080,margin:"0 auto"}}>
        <h2 style={{textAlign:"center",fontFamily:"Georgia,serif",fontSize:36,fontWeight:900,marginBottom:52}}>Everything you need to sell digital products</h2>
        <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fit,minmax(250px,1fr))",gap:20}}>
          {[
            {icon:"🔥",title:"Trending Topics",desc:"AI researches the most-searched self-help topics in real time so your products always land."},
            {icon:"🎨",title:"Custom Branding",desc:"Logo, brand name, and color scheme baked into every PDF — looks like a design studio made it."},
            {icon:"⚡",title:"60-Second Generation",desc:"Full table of contents, rich section content, and key takeaways — faster than making coffee."},
            {icon:"📄",title:"PDF Download",desc:"Print-ready PDFs formatted for professional delivery directly to your Whop customers."},
            {icon:"💼",title:"4 Product Types",desc:"Ebooks, guides, checklists, and worksheets — all fully formatted for your niche."},
            {icon:"🚀",title:"Built for Whop",desc:"Designed around Whop's platform. Upload and start selling the moment you download."},
          ].map(f=>(
            <div key={f.title} style={{background:"#0F0F0F",border:"1px solid #1a1a1a",borderRadius:14,padding:26,transition:"border-color .2s,transform .2s"}}
              onMouseEnter={e=>{e.currentTarget.style.borderColor="#6C63FF";e.currentTarget.style.transform="translateY(-4px)"}}
              onMouseLeave={e=>{e.currentTarget.style.borderColor="#1a1a1a";e.currentTarget.style.transform="none"}}>
              <div style={{fontSize:30,marginBottom:14}}>{f.icon}</div>
              <h3 style={{fontWeight:700,fontSize:16,marginBottom:8}}>{f.title}</h3>
              <p style={{fontSize:13,color:"#52616F",lineHeight:1.7}}>{f.desc}</p>
            </div>
          ))}
        </div>
      </section>

      {/* Sample product */}
      <section style={{padding:"72px 24px",maxWidth:860,margin:"0 auto",textAlign:"center"}}>
        <h2 style={{fontFamily:"Georgia,serif",fontSize:36,fontWeight:900,marginBottom:10}}>See what gets created</h2>
        <p style={{color:"#52616F",marginBottom:44,fontSize:15}}>A real sample from the generator — ready to sell on Whop.</p>
        <div style={{background:"#0F0F0F",border:"1px solid #1a1a1a",borderRadius:18,overflow:"hidden",textAlign:"left"}}>
          <div style={{background:"linear-gradient(135deg,#6C63FF,#FF6584)",padding:"44px 40px",textAlign:"center"}}>
            <div style={{display:"inline-block",background:"rgba(255,255,255,.2)",color:"#fff",fontSize:10,letterSpacing:3,textTransform:"uppercase",padding:"5px 14px",borderRadius:20,marginBottom:18}}>EBOOK · MEDIUM</div>
            <h3 style={{fontFamily:"Georgia,serif",fontSize:26,color:"#fff",fontWeight:900,marginBottom:10}}>The Morning Routine Blueprint</h3>
            <p style={{fontSize:13,color:"rgba(255,255,255,.78)"}}>Build an unshakeable morning that sets you up for daily wins</p>
          </div>
          <div style={{padding:"28px 36px"}}>
            {["The Science of Morning Momentum","Designing Your Ideal First Hour","Habit Stacking for Peak Performance","Overcoming Resistance & Sleep Inertia","Sustaining Your Routine Long-Term"].map((s,i)=>(
              <div key={i} style={{padding:"13px 0",borderBottom:"1px solid #161616",display:"flex",gap:14,alignItems:"center"}}>
                <span style={{fontFamily:"Georgia,serif",fontSize:16,color:"#FF6584",fontWeight:700,minWidth:28}}>{String(i+1).padStart(2,"0")}</span>
                <span style={{fontSize:14,color:"#7A8899"}}>{s}</span>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* Testimonials */}
      <section style={{padding:"72px 24px",maxWidth:980,margin:"0 auto"}}>
        <h2 style={{textAlign:"center",fontFamily:"Georgia,serif",fontSize:36,fontWeight:900,marginBottom:48}}>Loved by creators worldwide</h2>
        <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fit,minmax(270px,1fr))",gap:20}}>
          {TESTIMONIALS.map(t=>(
            <div key={t.name} style={{background:"#0F0F0F",border:"1px solid #1a1a1a",borderRadius:14,padding:26}}>
              <Stars n={t.stars}/>
              <p style={{fontSize:14,color:"#7A8899",lineHeight:1.8,margin:"14px 0 22px"}}>"{t.text}"</p>
              <div style={{display:"flex",alignItems:"center",gap:12}}>
                <div style={{width:38,height:38,borderRadius:"50%",background:"linear-gradient(135deg,#6C63FF,#FF6584)",display:"flex",alignItems:"center",justifyContent:"center",fontWeight:700,fontSize:13,color:"#fff",flexShrink:0}}>{t.avatar}</div>
                <div>
                  <div style={{fontWeight:600,fontSize:14}}>{t.name}</div>
                  <div style={{fontSize:12,color:"#52616F"}}>{t.role}</div>
                </div>
              </div>
            </div>
          ))}
        </div>
      </section>

      {/* Pricing teaser → links to full pricing page */}
      <section style={{padding:"72px 24px",maxWidth:960,margin:"0 auto",textAlign:"center"}}>
        <h2 style={{fontFamily:"Georgia,serif",fontSize:36,fontWeight:900,marginBottom:10}}>Plans for every creator</h2>
        <p style={{color:"#52616F",marginBottom:52,fontSize:15}}>Start for $1. Scale when you're ready. No hidden fees — payments handled by Whop.</p>
        <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fit,minmax(260px,1fr))",gap:20,marginBottom:44}}>
          {PLANS.map(plan=>(
            <div key={plan.id} style={{background:plan.badge==="Most Popular"?"linear-gradient(145deg,#12101e,#0e0c1a)":"#0F0F0F",border:plan.badge==="Most Popular"?`2px solid ${plan.color}`:"1px solid #1a1a1a",borderRadius:16,padding:"28px 24px",position:"relative",textAlign:"left"}}>
              {plan.badge && (
                <div style={{position:"absolute",top:-13,left:"50%",transform:"translateX(-50%)",background:`linear-gradient(135deg,${plan.color},${plan.gradTo})`,color:"#fff",fontSize:10,fontWeight:700,letterSpacing:2,textTransform:"uppercase",padding:"4px 16px",borderRadius:20,whiteSpace:"nowrap"}}>{plan.badge}</div>
              )}
              <div style={{fontSize:11,color:"#52616F",letterSpacing:2,textTransform:"uppercase",marginBottom:10}}>{plan.name}</div>
              <div style={{display:"flex",alignItems:"baseline",gap:3,marginBottom:6}}>
                <span style={{fontFamily:"Georgia,serif",fontSize:40,fontWeight:900,color:"#fff"}}>{plan.price}</span>
                <span style={{fontSize:13,color:"#52616F"}}>{plan.period}</span>
              </div>
              <p style={{fontSize:13,color:"#52616F",marginBottom:20,lineHeight:1.6}}>{plan.desc}</p>
              <div style={{fontSize:12,color:plan.color,fontWeight:600}}>
                {plan.generations === Infinity ? "Unlimited generations" : `${plan.generations} generation${plan.generations>1?"s":""}`}
              </div>
            </div>
          ))}
        </div>
        <button onClick={onStart} style={{background:"linear-gradient(135deg,#6C63FF,#FF6584)",border:"none",color:"#fff",padding:"17px 46px",borderRadius:12,fontWeight:800,fontSize:17,cursor:"pointer",boxShadow:"0 16px 48px rgba(108,99,255,.36)",fontFamily:"inherit"}}>
          Choose a Plan →
        </button>
      </section>

      {/* Footer */}
      <footer style={{borderTop:"1px solid #141414",padding:"28px",textAlign:"center",color:"#2D3748",fontSize:13,marginTop:48}}>
        © {new Date().getFullYear()} Whop Creator · Payments handled securely by Whop
      </footer>
    </div>
  );
}

// ═══════════════════════════════════════════════════════════════════════════════
//  PRICING PAGE
// ═══════════════════════════════════════════════════════════════════════════════
function PricingPage({ onSelect, onBack }) {
  return (
    <div style={{minHeight:"100vh",background:"#09090B",color:"#E2E8F0",fontFamily:"'Inter',sans-serif",padding:"40px 24px"}}>
      <div style={{maxWidth:1020,margin:"0 auto"}}>

        <button onClick={onBack} style={{background:"transparent",border:"none",color:"#52616F",cursor:"pointer",fontSize:13,marginBottom:36,fontFamily:"inherit"}}>← Back</button>

        <div style={{textAlign:"center",marginBottom:60}}>
          <h1 style={{fontFamily:"Georgia,serif",fontSize:"clamp(30px,5vw,50px)",fontWeight:900,marginBottom:14}}>Choose Your Plan</h1>
          <p style={{color:"#52616F",fontSize:16,maxWidth:460,margin:"0 auto",lineHeight:1.7}}>All plans include branding, PDF export, and Whop-optimised formatting.<br/>Payments processed securely through Whop.</p>
        </div>

        <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fit,minmax(290px,1fr))",gap:22,alignItems:"start"}}>
          {PLANS.map(plan=>(
            <div key={plan.id}
              style={{background:plan.badge==="Most Popular"?"linear-gradient(145deg,#131120,#0e0c1a)":"#0F0F0F",border:plan.badge==="Most Popular"?`2px solid ${plan.color}`:"1px solid #1a1a1a",borderRadius:20,padding:"36px 28px",position:"relative",transition:"transform .2s"}}
              onMouseEnter={e=>e.currentTarget.style.transform="translateY(-5px)"}
              onMouseLeave={e=>e.currentTarget.style.transform="none"}>

              {plan.badge && (
                <div style={{position:"absolute",top:-13,left:"50%",transform:"translateX(-50%)",background:`linear-gradient(135deg,${plan.color},${plan.gradTo})`,color:"#fff",fontSize:10,fontWeight:700,letterSpacing:2,textTransform:"uppercase",padding:"4px 18px",borderRadius:20,whiteSpace:"nowrap"}}>
                  {plan.badge}
                </div>
              )}

              <div style={{display:"flex",alignItems:"center",gap:8,marginBottom:18}}>
                <div style={{width:9,height:9,borderRadius:"50%",background:plan.color,flexShrink:0}}/>
                <span style={{fontSize:11,fontWeight:700,color:"#7A8899",textTransform:"uppercase",letterSpacing:2}}>{plan.name}</span>
              </div>

              <div style={{display:"flex",alignItems:"baseline",gap:4,marginBottom:6}}>
                <span style={{fontFamily:"Georgia,serif",fontSize:50,fontWeight:900,color:"#fff",lineHeight:1}}>{plan.price}</span>
                <span style={{fontSize:14,color:"#52616F"}}>{plan.period}</span>
              </div>

              <p style={{fontSize:14,color:"#52616F",marginBottom:24,lineHeight:1.65}}>{plan.desc}</p>

              <div style={{height:1,background:"#1a1a1a",marginBottom:22}}/>

              {plan.features.map(f=>(
                <div key={f} style={{display:"flex",alignItems:"flex-start",gap:10,padding:"6px 0"}}>
                  <span style={{color:plan.color,fontSize:15,flexShrink:0,marginTop:1}}>✓</span>
                  <span style={{fontSize:14,color:"#CBD5E1",lineHeight:1.5}}>{f}</span>
                </div>
              ))}

              {/* Primary CTA → Whop checkout */}
              <a href={WHOP_LINKS[plan.id]} target="_blank" rel="noopener noreferrer"
                style={{display:"block",marginTop:28,padding:"15px",borderRadius:10,border:"none",background:`linear-gradient(135deg,${plan.color},${plan.gradTo})`,color:"#fff",fontWeight:800,fontSize:15,cursor:"pointer",textAlign:"center",textDecoration:"none",boxShadow:`0 10px 28px rgba(${hexToRgb(plan.color)},.32)`,transition:"opacity .15s",fontFamily:"inherit"}}
                onMouseEnter={e=>e.currentTarget.style.opacity=".85"}
                onMouseLeave={e=>e.currentTarget.style.opacity="1"}>
                {plan.id==="trial"   ? "Try for $1 →"
                : plan.id==="pro"    ? "Subscribe — $19.99/mo →"
                :                      "Get Unlimited Access →"}
              </a>

              {/* Secondary: preview without paying */}
              <button onClick={()=>onSelect(plan)}
                style={{display:"block",width:"100%",marginTop:10,padding:"10px",borderRadius:10,border:"1px solid #1a1a1a",background:"transparent",color:"#444",fontSize:13,cursor:"pointer",transition:"color .15s",fontFamily:"inherit"}}
                onMouseEnter={e=>e.currentTarget.style.color="#E2E8F0"}
                onMouseLeave={e=>e.currentTarget.style.color="#444"}>
                Preview the builder first
              </button>
            </div>
          ))}
        </div>

        <p style={{textAlign:"center",marginTop:36,fontSize:13,color:"#2D3748"}}>
          🔒 All payments processed securely by Whop · Cancel anytime
        </p>
      </div>
    </div>
  );
}

// ═══════════════════════════════════════════════════════════════════════════════
//  DOWNLOAD PAGE
// ═══════════════════════════════════════════════════════════════════════════════
function DownloadPage({ product, branding, form, plan, onDownload, onBack }) {
  return (
    <div style={{minHeight:"100vh",background:"#09090B",color:"#E2E8F0",fontFamily:"'Inter',sans-serif",display:"flex",flexDirection:"column",alignItems:"center",justifyContent:"center",padding:24}}>
      <div style={{maxWidth:500,width:"100%",textAlign:"center"}}>
        <div style={{width:76,height:76,borderRadius:"50%",background:"linear-gradient(135deg,#22c55e,#16a34a)",display:"flex",alignItems:"center",justifyContent:"center",fontSize:32,margin:"0 auto 22px"}}>✓</div>
        <h2 style={{fontFamily:"Georgia,serif",fontSize:30,marginBottom:10}}>Your Product is Ready!</h2>
        <p style={{color:"#52616F",marginBottom:36,fontSize:15,lineHeight:1.7}}>Click below to download your fully formatted, branded PDF.</p>

        <div style={{background:"#0F0F0F",border:"1px solid #1a1a1a",borderRadius:18,padding:28,marginBottom:20}}>
          <div style={{fontWeight:700,fontSize:17,marginBottom:6}}>{product?.title}</div>
          <div style={{fontSize:13,color:"#52616F",marginBottom:22,display:"flex",alignItems:"center",justifyContent:"center",gap:8,flexWrap:"wrap"}}>
            <span>{form.type}</span><span style={{color:"#2D3748"}}>·</span>
            <span>{form.length}</span><span style={{color:"#2D3748"}}>·</span>
            <span>{product?.sections?.length} sections</span>
            {plan && <><span style={{color:"#2D3748"}}>·</span><span style={{color:"#A78BFA"}}>{plan.name}</span></>}
          </div>
          <button onClick={onDownload}
            style={{width:"100%",padding:"15px",borderRadius:12,border:"none",background:`linear-gradient(135deg,${branding.scheme.primary},${branding.scheme.secondary})`,color:"#fff",fontWeight:800,fontSize:16,cursor:"pointer",boxShadow:`0 10px 36px rgba(${hexToRgb(branding.scheme.primary)},.38)`,fontFamily:"inherit"}}>
            ⬇ Download PDF
          </button>
        </div>

        <button onClick={onBack} style={{background:"transparent",border:"none",color:"#52616F",cursor:"pointer",fontSize:13,fontFamily:"inherit"}}>← Back to editor</button>
        <p style={{marginTop:14,fontSize:12,color:"#2D3748"}}>Browser will open a print dialog — choose "Save as PDF".</p>
      </div>
    </div>
  );
}
