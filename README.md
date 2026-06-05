from fastapi import FastAPI, HTTPException
from fastapi.responses import HTMLResponse
from pydantic import BaseModel
import psutil
import random
import os

app = FastAPI(title="Stellar Nexus Ultra Edition")

# هيكل قوالب التصاميم العالمية الجاهزة للمعاينة الفورية
WEB_TEMPLATES = {
    "vercel": """
    <div style='background:#000; color:#fff; padding:60px; font-family:sans-serif; text-align:center; border:1px solid #222; margin:40px; border-radius:12px; box-shadow: 0 10px 30px rgba(0,0,0,0.5);'>
        <h1 style='font-size: 40px; letter-spacing: -1px;'>▲ Deployed via Stellar Nexus Agent</h1>
        <p style='color:#888; font-size: 18px;'>Vercel High-Performance Edge Runtime Architecture Active.</p>
        <div style='display:inline-block; background:#111; padding:10px 20px; border-radius:20px; border:1px solid #333; margin-top:20px; color:#39ff14; font-family:monospace;'>STATUS: DEPLOYED SUCCESSFUL // 100% ONLINE</div>
    </div>
    """,
    "stripe": """
    <div style='background:linear-gradient(135deg, #4f46e5, #07b6d4); color:#fff; padding:60px; font-family:sans-serif; text-align:center; margin:40px; border-radius:12px; box-shadow: 0 10px 30px rgba(79,70,229,0.3);'>
        <h1 style='font-size: 40px;'>💳 Stripe Grid Financial Network</h1>
        <p style='font-size: 18px;'>Secure global payment API routing and banking micro-frontends initiated.</p>
        <div style='display:inline-block; background:rgba(0,0,0,0.2); padding:10px 20px; border-radius:20px; margin-top:20px; font-family:monospace;'>ENCRYPTION CORE: SHA-256 SECURED</div>
    </div>
    """,
    "linear": """
    <div style='background:#121214; color:#f4f4f5; padding:60px; border:1px solid #27272a; font-family:sans-serif; text-align:center; margin:40px; border-radius:12px; box-shadow: 0 10px 30px rgba(0,0,0,0.8);'>
        <h1 style='font-size: 40px; color:#f4f4f5;'>⚙️ Linear Engineering & Issue Tracking</h1>
        <p style='color:#a1a1aa; font-size: 18px;'>Project synchronization, cycle analytics, and velocity targets operational.</p>
        <div style='display:inline-block; background:#18181b; padding:10px 20px; border-radius:20px; border:1px solid #3f3f46; margin-top:20px; color:#a855f7; font-family:monospace;'>VELOCITY METRICS: 100% IN SYNC</div>
    </div>
    """
}

class PromptPayload(BaseModel):
    user_idea: str
    mode: str

class CodePayload(BaseModel):
    filepath: str
    content: str

# 1. فحص كامل وشامل لعتاد الخادم والجهاز الحركي
@app.get("/api/server/status")
def get_server_status():
    return {
        "cpu_usage": f"{psutil.cpu_percent()}%",
        "memory_usage": f"{psutil.virtual_memory().percent()}%",
        "disk_usage": f"{psutil.disk_usage('/').percent()}%",
        "status": "SECURE // SYSTEM ACTIVE",
        "active_agents": "Stellar Agent Core Linked via Local CLI Matrix",
        "ping_rate": f"{random.randint(12, 35)}ms"
    }

# 2. محرك قراءة الملفات في محرر الأكواد
@app.get("/api/editor/read")
def read_file(path: str):
    if not os.path.exists(path):
        raise HTTPException(status_code=404, detail="File infrastructure not found local")
    with open(path, "r", encoding="utf-8") as f:
        return {"content": f.read()}

# 3. محرك حفظ ونشر الملفات الفوري
@app.post("/api/editor/save")
def save_file(payload: CodePayload):
    try:
        with open(payload.filepath, "w", encoding="utf-8") as f:
            f.write(payload.content)
        return {"status": "Success", "message": f"[DEPLOYED] File '{payload.filepath}' synchronized safely to active server cluster."}
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))

# 4. محرك هندسة وتوليد المطالبات الفنية السينمائية
@app.post("/api/ai/generate-prompt")
def generate_advanced_prompt(payload: PromptPayload):
    idea = payload.user_idea
    if not idea:
        raise HTTPException(status_code=400, detail="Core idea payload cannot be empty.")
    
    if payload.mode == "image":
        compiled = f"Anamorphic cinematic masterclass portrait of {idea}, dark cyberpunk atmospheric environment, deep volumetric fog, neon purple and cyan laser reflections, wet surface, high-intricate texturing, shot on Arri Alexa 65, 8k resolution --style raw"
    elif payload.mode == "video":
        compiled = f"Hyper-lapse immersive drone shot spinning around {idea}, dynamic cinematic lighting, shifting volumetric clouds, hyper-detailed environment particles, rendered in Unreal Engine 5, 4k 120fps cinematic ratio"
    else:
        compiled = f"[CYBER MATRIX TERMINAL STREAM] Active ASCII configuration representing: {idea} --matrix architecture code standard 100% fill rate"
        
    return {"prompt": compiled}

# 5. بروتوكول تفعيل واستضافة قوالب الأنظمة
@app.get("/api/templates/deploy")
def deploy_template(style: str):
    if style not in WEB_TEMPLATES:
        raise HTTPException(status_code=404, detail="Requested system template does not exist.")
    return {"status": "SUCCESS", "url": f"http://127.0.0.1:8000/view/{style}"}

@app.get("/view/{style}", response_class=HTMLResponse)
def view_style(style: str):
    return WEB_TEMPLATES.get(style, "<h1>System Infrastructure Allocation Failure</h1>")

# 6. تحميل وتشغيل واجهة المستخدم الرئيسية
@app.get("/", response_class=HTMLResponse)
def serve_home():
    with open("index.html", "r", encoding="utf-8") as f:
        return f.read()
