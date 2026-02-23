<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Lilly – Biennial Benefits Q&A</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.21.4/babel.min.js"></script>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: 'Segoe UI', Arial, sans-serif; background: #f5f5f5; }
  </style>
</head>
<body>
  <div id="root"></div>
  <script type="text/babel">
    const ADMIN_PASSWORD = "admin123";
    const LILLY_RED = "#CC0000";

    function App() {
      const [view, setView] = React.useState("submit");
      const [questions, setQuestions] = React.useState(() => {
        try { return JSON.parse(localStorage.getItem("lilly-qa") || "[]"); } catch { return []; }
      });
      const [name, setName] = React.useState("");
      const [email, setEmail] = React.useState("");
      const [question, setQuestion] = React.useState("");
      const [submitted, setSubmitted] = React.useState(false);
      const [emailError, setEmailError] = React.useState("");
      const [password, setPassword] = React.useState("");
      const [loginError, setLoginError] = React.useState("");
      const [filter, setFilter] = React.useState("all");
      const [answers, setAnswers] = React.useState({});
      const [editingId, setEditingId] = React.useState(null);

      function save(qs) {
        setQuestions(qs);
        localStorage.setItem("lilly-qa", JSON.stringify(qs));
      }

      function validEmail(v) { return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v); }

      function handleSubmit() {
        if (!question.trim()) return;
        if (!email.trim() || !validEmail(email)) { setEmailError("Please enter a valid email address."); return; }
        setEmailError("");
        const q = { id: Date.now(), name: name.trim() || "Anonymous", email: email.trim(), question: question.trim(), answer: null, timestamp: new Date().toISOString() };
        save([q, ...questions]);
        setSubmitted(true); setName(""); setEmail(""); setQuestion("");
      }

      function handleLogin() {
        if (password === ADMIN_PASSWORD) { setView("dashboard"); setLoginError(""); setPassword(""); }
        else setLoginError("Incorrect password.");
      }

      function handleAnswer(id) {
        const ans = answers[id];
        if (!ans?.trim()) return;
        save(questions.map(q => q.id === id ? { ...q, answer: ans.trim() } : q));
        setAnswers(p => { const n = {...p}; delete n[id]; return n; });
        setEditingId(null);
      }

      function handleDelete(id) { save(questions.filter(q => q.id !== id)); }
      function clearAnswer(id) { save(questions.map(q => q.id === id ? { ...q, answer: null } : q)); }

      const fmt = iso => new Date(iso).toLocaleString();
      const unanswered = questions.filter(q => !q.answer).length;
      const filtered = questions.filter(q =>
        filter === "answered" ? q.answer !== null :
        filter === "unanswered" ? q.answer === null : true
      );

      const labelStyle = { display: "block", fontSize: 13, fontWeight: 600, color: "#444", marginBottom: 4 };
      const inputStyle = { width: "100%", padding: "10px 12px", borderRadius: 8, border: "1px solid #ddd", fontSize: 14, marginBottom: 16, outline: "none", fontFamily: "inherit" };
      const btn = (bg, w="auto") => ({ background: bg, color: "#fff", border: "none", padding: "10px 20px", borderRadius: 8, cursor: bg==="#bbb"?"not-allowed":"pointer", fontWeight: 600, fontSize: 14, width: w });
      const iconBtn = (bg, color) => ({ background: bg, border: "none", borderRadius: 8, padding: "6px 10px", cursor: "pointer", color, fontSize: 14 });

      // SUBMIT
      if (view === "submit") return (
        <div style={{ minHeight: "100vh", background: "#f5f5f5" }}>
          <div style={{ background: LILLY_RED, color: "#fff", padding: "20px 28px" }}>
            <div style={{ maxWidth: 560, margin: "0 auto", display: "flex", alignItems: "center", gap: 12 }}>
              <div style={{ fontSize: 26, fontWeight: 900, letterSpacing: -1 }}>Lilly</div>
              <div style={{ width: 1, height: 28, background: "rgba(255,255,255,0.4)" }}/>
              <div style={{ fontSize: 13 }}>
                <div style={{ fontWeight: 600 }}>Biennial Benefits Inventory</div>
                <div style={{ opacity: 0.85 }}>Review &amp; Attestation</div>
              </div>
            </div>
          </div>
          <div style={{ background: "#fff3f3", borderBottom: "1px solid #f0cccc", padding: "11px 28px" }}>
            <div style={{ maxWidth: 560, margin: "0 auto", fontSize: 13, color: "#8b0000" }}>
              📋 <strong>Submitting your question here helps organize and track all inquiries</strong> for the biennial benefits review — ensuring nothing is missed and your answer is sent directly to your email.
            </div>
          </div>
          <div style={{ display: "flex", justifyContent: "center", padding: "32px 16px" }}>
            <div style={{ background: "#fff", borderRadius: 12, padding: 36, width: "100%", maxWidth: 500, boxShadow: "0 2px 16px rgba(0,0,0,0.08)", border: "1px solid #e8e8e8" }}>
              {submitted ? (
                <div style={{ textAlign: "center", padding: "24px 0" }}>
                  <div style={{ fontSize: 52 }}>✅</div>
                  <h2 style={{ color: "#1a6b3a", margin: "14px 0 8px" }}>Question Submitted!</h2>
                  <p style={{ color: "#555", marginBottom: 24, fontSize: 14 }}>Thank you! Your question has been received. We'll follow up at the email you provided.</p>
                  <button onClick={() => setSubmitted(false)} style={btn(LILLY_RED)}>Submit Another Question</button>
                </div>
              ) : (
                <div>
                  <h3 style={{ margin: "0 0 4px", color: "#1a1a1a", fontSize: 17 }}>Submit a Question</h3>
                  <p style={{ margin: "0 0 22px", fontSize: 13, color: "#777" }}>Use this form for any questions related to the Mercer Gold Plus biennial benefits inventory process.</p>
                  <label style={labelStyle}>Your Name <span style={{ color: "#aaa", fontWeight: 400 }}>(optional)</span></label>
                  <input value={name} onChange={e => setName(e.target.value)} placeholder="e.g. Jane Doe" style={inputStyle}/>
                  <label style={labelStyle}>Email Address <span style={{ color: LILLY_RED }}>*</span></label>
                  <input type="email" value={email} onChange={e => { setEmail(e.target.value); if (emailError) setEmailError(""); }} placeholder="e.g. jane.doe@lilly.com" style={{ ...inputStyle, borderColor: emailError ? LILLY_RED : "#ddd" }}/>
                  {emailError && <p style={{ color: LILLY_RED, fontSize: 12, margin: "-12px 0 12px" }}>{emailError}</p>}
                  <p style={{ fontSize: 12, color: "#888", margin: "-8px 0 16px" }}>Your answer will be sent to this email address.</p>
                  <label style={labelStyle}>Your Question <span style={{ color: LILLY_RED }}>*</span></label>
                  <textarea value={question} onChange={e => setQuestion(e.target.value)} placeholder="Type your question about the biennial benefits review..." rows={4} style={{ ...inputStyle, resize: "vertical" }}/>
                  <button onClick={handleSubmit} disabled={!question.trim()} style={btn(question.trim() ? LILLY_RED : "#bbb", "100%")}>Submit Question</button>
                </div>
              )}
              <div style={{ marginTop: 20, textAlign: "center" }}>
                <span onClick={() => setView("login")} style={{ color: LILLY_RED, cursor: "pointer", fontSize: 12 }}>Admin Dashboard →</span>
              </div>
            </div>
          </div>
          <div style={{ textAlign: "center", paddingBottom: 24, fontSize: 11, color: "#bbb" }}>Eli Lilly and Company · Biennial Benefits Review</div>
        </div>
      );

      // LOGIN
      if (view === "login") return (
        <div style={{ minHeight: "100vh", background: "#f5f5f5", display: "flex", alignItems: "center", justifyContent: "center" }}>
          <div style={{ background: "#fff", borderRadius: 12, padding: 36, width: "100%", maxWidth: 360, boxShadow: "0 2px 16px rgba(0,0,0,0.08)", border: "1px solid #e8e8e8" }}>
            <div style={{ textAlign: "center", marginBottom: 20 }}>
              <div style={{ fontSize: 28, fontWeight: 900, color: LILLY_RED, letterSpacing: -1 }}>Lilly</div>
              <h2 style={{ margin: "4px 0 0", color: "#1a1a1a", fontSize: 17 }}>Admin Login</h2>
            </div>
            <p style={{ margin: "0 0 16px", color: "#888", fontSize: 13, textAlign: "center" }}>Default password: <code>admin123</code></p>
            <label style={labelStyle}>Password</label>
            <input type="password" value={password} onChange={e => setPassword(e.target.value)} onKeyDown={e => e.key === "Enter" && handleLogin()} placeholder="Enter password" style={inputStyle}/>
            {loginError && <p style={{ color: "red", fontSize: 13, margin: "-8px 0 12px" }}>{loginError}</p>}
            <button onClick={handleLogin} style={btn(LILLY_RED, "100%")}>Login</button>
            <div style={{ marginTop: 16, textAlign: "center" }}>
              <span onClick={() => setView("submit")} style={{ color: LILLY_RED, cursor: "pointer", fontSize: 13 }}>← Back to Submit Form</span>
            </div>
          </div>
        </div>
      );

      // DASHBOARD
      return (
        <div style={{ minHeight: "100vh", background: "#f5f5f5" }}>
          <div style={{ background: LILLY_RED, color: "#fff", padding: "14px 28px", display: "flex", justifyContent: "space-between", alignItems: "center" }}>
            <div>
              <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
                <span style={{ fontSize: 22, fontWeight: 900, letterSpacing: -1 }}>Lilly</span>
                <div style={{ width: 1, height: 22, background: "rgba(255,255,255,0.4)" }}/>
                <span style={{ fontSize: 13, opacity: 0.9 }}>Biennial Benefits Review · Admin Dashboard</span>
              </div>
              <p style={{ margin: "2px 0 0", fontSize: 12, opacity: 0.8 }}>{questions.length} total · {unanswered} unanswered</p>
            </div>
            <button onClick={() => setView("submit")} style={{ background: "rgba(255,255,255,0.15)", border: "1px solid rgba(255,255,255,0.35)", color: "#fff", padding: "7px 14px", borderRadius: 8, cursor: "pointer", fontSize: 13 }}>← Public Form</button>
          </div>
          <div style={{ padding: "20px 28px" }}>
            <div style={{ display: "flex", gap: 8, marginBottom: 20 }}>
              {["all","unanswered","answered"].map(f => (
                <button key={f} onClick={() => setFilter(f)} style={{ padding: "6px 16px", borderRadius: 20, border: "none", cursor: "pointer", fontSize: 13, fontWeight: 600, background: filter===f ? LILLY_RED : "#e8e8e8", color: filter===f ? "#fff" : "#555" }}>
                  {f.charAt(0).toUpperCase()+f.slice(1)}
                </button>
              ))}
            </div>
            {filtered.length === 0 && (
              <div style={{ textAlign: "center", padding: "60px 0", color: "#aaa" }}>
                <div style={{ fontSize: 40 }}>📭</div>
                <p style={{ marginTop: 8 }}>No questions here yet.</p>
              </div>
            )}
            {filtered.map(q => (
              <div key={q.id} style={{ background: "#fff", borderRadius: 10, padding: 20, marginBottom: 14, boxShadow: "0 1px 6px rgba(0,0,0,0.06)", border: "1px solid #eee", borderLeft: `4px solid ${q.answer ? "#1a6b3a" : LILLY_RED}` }}>
                <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start" }}>
                  <div style={{ flex: 1 }}>
                    <div style={{ display: "flex", gap: 8, alignItems: "center", flexWrap: "wrap", marginBottom: 6 }}>
                      <span style={{ fontWeight: 700, color: "#1a1a1a" }}>{q.name}</span>
                      <span style={{ fontSize: 12, background: "#fff0f0", color: LILLY_RED, padding: "2px 8px", borderRadius: 10 }}>✉️ {q.email}</span>
                      <span style={{ fontSize: 11, color: "#aaa" }}>{fmt(q.timestamp)}</span>
                      <span style={{ fontSize: 11, padding: "2px 8px", borderRadius: 10, background: q.answer ? "#d4edda" : "#fde8e8", color: q.answer ? "#1a6b3a" : LILLY_RED, fontWeight: 600 }}>
                        {q.answer ? "Answered" : "Pending"}
                      </span>
                    </div>
                    <p style={{ margin: "0 0 12px", color: "#333", fontSize: 15 }}>{q.question}</p>
                    {q.answer && editingId !== q.id && (
                      <div style={{ background: "#f0fff4", border: "1px solid #b7e4c7", borderRadius: 8, padding: "10px 14px", marginBottom: 8 }}>
                        <strong style={{ fontSize: 11, color: "#1a6b3a", textTransform: "uppercase", letterSpacing: 1 }}>Your Answer</strong>
                        <p style={{ margin: "4px 0 0", color: "#1b4332", fontSize: 14 }}>{q.answer}</p>
                        <p style={{ margin: "6px 0 0", fontSize: 12, color: "#888" }}>📧 Send your reply to: <strong>{q.email}</strong></p>
                      </div>
                    )}
                    {(!q.answer || editingId === q.id) && (
                      <div style={{ display: "flex", gap: 8 }}>
                        <textarea placeholder="Type your answer..." rows={2} value={answers[q.id] ?? (editingId===q.id ? q.answer : "")} onChange={e => setAnswers(p => ({ ...p, [q.id]: e.target.value }))} style={{ ...inputStyle, flex: 1, margin: 0, fontSize: 13 }}/>
                        <button onClick={() => handleAnswer(q.id)} style={btn("#1a6b3a")}>Save</button>
                      </div>
                    )}
                  </div>
                  <div style={{ display: "flex", gap: 6, marginLeft: 12, flexShrink: 0 }}>
                    {q.answer && editingId !== q.id && (
                      <button onClick={() => { setEditingId(q.id); setAnswers(p => ({ ...p, [q.id]: q.answer })); }} style={iconBtn("#fdf0f0", LILLY_RED)} title="Edit">✏️</button>
                    )}
                    {q.answer && <button onClick={() => clearAnswer(q.id)} style={iconBtn("#fff3e0","#e07b39")} title="Clear">↩</button>}
                    <button onClick={() => handleDelete(q.id)} style={iconBtn("#ffe4e4","#c0392b")} title="Delete">🗑</button>
                  </div>
                </div>
              </div>
            ))}
          </div>
        </div>
      );
    }

    ReactDOM.render(<App/>, document.getElementById("root"));
  </script>
</body>
</html>
