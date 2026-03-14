# Marketmach-1.0
import React, { useState, useEffect, useRef } from 'react';
import { Star, Send, ChevronLeft, MessageSquare, Trophy, Coffee, Shirt, ShoppingBag, Utensils, Globe, Zap, Building, Search, CheckCircle2, User, Briefcase, Loader2 } from 'lucide-react';

/**
 * MARKETMATCH - CONSULTORÍA INTELIGENTE
 * Este archivo es auto-contenido y está listo para ser publicado.
 * Utiliza Tailwind CSS para los estilos y Lucide React para la iconografía.
 */

// --- CONFIGURACIÓN DE API ---
// Nota: La apiKey se maneja de forma dinámica en el entorno de ejecución.
const apiKey = ""; 

const DATA_GEOGRAPHY = {
  "América": {
    "México": ["CDMX", "Monterrey", "Guadalajara", "Puebla", "Cancún"],
    "Colombia": ["Bogotá", "Medellín", "Cali", "Barranquilla", "Cartagena"],
    "Argentina": ["Buenos Aires", "Córdoba", "Rosario", "Mendoza", "La Plata"],
    "Brasil": ["São Paulo", "Río de Janeiro", "Brasilia", "Curitiba", "Salvador"],
    "USA": ["New York", "Los Angeles", "Miami", "Chicago", "Houston"],
    "Canadá": ["Toronto", "Vancouver", "Montreal", "Ottawa", "Calgary"]
  },
  "Europa": {
    "España": ["Madrid", "Barcelona", "Valencia", "Sevilla", "Bilbao"],
    "Francia": ["París", "Lyon", "Marsella", "Burdeos", "Niza"],
    "Italia": ["Roma", "Milán", "Nápoles", "Turín", "Florencia"],
    "Reino Unido": ["Londres", "Manchester", "Birmingham", "Liverpool", "Edimburgo"],
    "Alemania": ["Berlín", "Múnich", "Hamburgo", "Frankfurt", "Colonia"]
  },
  "Asia": {
    "Japón": ["Tokio", "Osaka", "Kioto", "Yokohama", "Nagoya"],
    "China": ["Pekín", "Shanghái", "Shenzhen", "Cantón", "Chengdu"],
    "Corea del Sur": ["Seúl", "Busan", "Incheon", "Daegu"],
    "India": ["Mumbai", "Delhi", "Bangalore", "Hyderabad", "Chennai"]
  },
  "Oceanía": {
    "Australia": ["Sydney", "Melbourne", "Brisbane", "Perth", "Adelaida"],
    "Nueva Zelanda": ["Auckland", "Wellington", "Christchurch", "Hamilton"]
  },
  "África": {
    "Sudáfrica": ["Ciudad del Cabo", "Johannesburgo", "Pretoria", "Durban"],
    "Egipto": ["El Cairo", "Alejandría", "Giza", "Luxor"],
    "Marruecos": ["Casablanca", "Marrakech", "Rabat", "Tánger"]
  }
};

const CATEGORIES = [
  { id: 'f1', name: 'Fórmula 1' },
  { id: 'sports', name: 'Deportes' },
  { id: 'fashion', name: 'Moda y Ropa' },
  { id: 'gastronomy', name: 'Gastronomía' },
  { id: 'petshop', name: 'Pet Shop' },
  { id: 'tech', name: 'Tecnología' },
  { id: 'crypto', name: 'Cripto & Web3' },
  { id: 'realestate', name: 'Bienes Raíces' },
];

const MALE_NAMES = ["Carlos", "Juan", "Diego", "Andrés", "Ricardo", "Fernando", "Mateo", "Hugo", "Javier", "Luis", "Pablo", "Alberto", "Roberto", "Sergio", "Antonio", "Miguel", "David", "Ángel", "Tomás", "Nicolás"];
const FEMALE_NAMES = ["Mariana", "Elena", "Lucía", "Sofía", "Valeria", "Isabel", "Camila", "Gabriela", "Daniela", "Paola", "Carmen", "Beatriz", "Raquel", "Mónica", "Laura", "Patricia", "Verónica", "Julia", "Natalia", "Silvia"];
const LAST_NAMES = ["García", "Rodríguez", "López", "Martínez", "Pérez", "Sánchez", "González", "Gómez", "Torres", "Ramírez", "Cruz", "Morales", "Ortiz", "Reyes", "Ruiz", "Jiménez", "Silva", "Vázquez", "Castro", "Mendoza"];

const JOB_TEMPLATES = {
  f1: ["Simulación aerodinámica avanzada", "Telemetría para escuderías", "Optimización de neumáticos", "Gestión de logística", "Ingeniería de pista"],
  sports: ["Consultoría para clubes", "Análisis biométrico", "Marketing deportivo", "Gestión de estadios", "Dirección técnica"],
  fashion: ["Diseño de alta costura", "Estrategia retail", "Coolhunting", "Producción de desfiles", "Desarrollo de textiles"],
  gastronomy: ["Creación de conceptos Michelin", "Ingeniería de menú", "Auditoría de calidad", "Gestión de suministro", "Dirección de cocinas"],
  petshop: ["Lanzamiento de e-commerce", "Dirección de retail", "Nutrición animal", "Logística premium", "Marketing veterinario"],
  tech: ["Arquitectura de microservicios", "Liderazgo Fullstack", "Ciberseguridad", "Implementación de IA", "Optimización infra"],
  crypto: ["Auditoría de smart contracts", "Protocolos DeFi", "Arquitectura DAOs", "Gestión de liquidez", "Seguridad multicadena"],
  realestate: ["Valuación de lujo", "Smart buildings", "Gestión de fondos", "Corretaje internacional", "Análisis emergente"],
};

const generateProfiles = (category, isJunior, count = 20) => {
  return Array.from({ length: count }, (_, i) => {
    const isFemale = Math.random() > 0.5;
    const firstName = isFemale 
      ? FEMALE_NAMES[Math.floor(Math.random() * FEMALE_NAMES.length)]
      : MALE_NAMES[Math.floor(Math.random() * MALE_NAMES.length)];
    
    const lastName = LAST_NAMES[Math.floor(Math.random() * LAST_NAMES.length)];
    const name = `${firstName} ${lastName}`;
    const rating = isJunior ? (3.5 + Math.random() * 1.5).toFixed(1) : (4.5 + Math.random() * 0.5).toFixed(1);
    
    const templates = JOB_TEMPLATES[category];
    const selectedJobs = [...templates].sort(() => 0.5 - Math.random()).slice(0, isJunior ? 1 : 3).join(", ");
    const genderSlug = isFemale ? 'women' : 'men';
    const photoId = Math.floor(Math.random() * 95); 

    return {
      id: Math.random(),
      name,
      firstName,
      gender: isFemale ? 'female' : 'male',
      rating: parseFloat(rating),
      jobs: selectedJobs,
      experience: isJunior ? `${Math.floor(Math.random() * 18) + 1} meses` : `${Math.floor(Math.random() * 20) + 5} años`,
      photo: `https://randomuser.me/api/portraits/${genderSlug}/${photoId}.jpg`,
      isJunior
    };
  });
};

const StarRating = ({ rating }) => {
  const fullStars = Math.floor(rating);
  return (
    <div className="flex items-center gap-1">
      {[...Array(5)].map((_, i) => (
        <Star key={i} size={12} className={i < fullStars ? "fill-amber-500 text-amber-500" : "text-white/20"} />
      ))}
      <span className="ml-1 text-[10px] font-black text-amber-500">{rating}</span>
    </div>
  );
};

const App = () => {
  const [step, setStep] = useState('search'); 
  const [filters, setFilters] = useState({ continent: '', country: '', city: '', category: '' });
  const [results, setResults] = useState({ seniors: [], juniors: [] });
  const [selectedSpecialist, setSelectedSpecialist] = useState(null);
  const [chatMessages, setChatMessages] = useState([]);
  const [inputValue, setInputValue] = useState('');
  const [isTyping, setIsTyping] = useState(false);
  const [loadingText, setLoadingText] = useState("");
  const chatEndRef = useRef(null);

  useEffect(() => {
    chatEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [chatMessages]);

  const handleSearch = () => {
    if (filters.country && filters.city && filters.category) {
      setStep('loading');
      const sequence = ["Conectando con red global...", `Escaneando en ${filters.city}...`, "Generando 40 perfiles locales...", "Finalizando sincronización..."];
      sequence.forEach((text, i) => {
        setTimeout(() => {
          setLoadingText(text);
          if (i === sequence.length - 1) {
            setTimeout(() => {
              setResults({ seniors: generateProfiles(filters.category, false, 20), juniors: generateProfiles(filters.category, true, 20) });
              setStep('results');
            }, 400);
          }
        }, i * 500);
      });
    }
  };

  const openChat = (specialist) => {
    setSelectedSpecialist(specialist);
    setChatMessages([
      { 
        role: 'ai', 
        text: `¡Hola! Qué gusto saludarte. He analizado que te interesa el área de ${CATEGORIES.find(c => c.id === filters.category)?.name}. Para brindarte una asesoría precisa, ¿podrías comentarme cuál es el desafío principal que quieres resolver hoy?` 
      }
    ]);
    setStep('chat');
  };

  const sendMessage = async () => {
    if (!inputValue.trim()) return;
    const userMsg = { role: 'user', text: inputValue };
    setChatMessages(prev => [...prev, userMsg]);
    setInputValue('');
    setIsTyping(true);

    try {
      const categoryName = CATEGORIES.find(c => c.id === filters.category)?.name;
      const systemPrompt = `Eres ${selectedSpecialist.name}, ${selectedSpecialist.gender === 'female' ? 'una profesional experta' : 'un profesional experto'} en ${categoryName}.
      
      REGLAS DE CONDUCTA:
      1. NO repitas tu nombre en cada mensaje. El usuario ya sabe quién eres por la interfaz.
      2. Sé MUY BREVE (2-3 líneas máximo).
      3. Responde DIRECTAMENTE a lo que el cliente escribió.
      4. Sé CORDIAL y profesional.
      5. SIEMPRE cierra con una pregunta inteligente para calificar el proyecto (ej. "¿Qué tiempos de entrega manejas?", "¿Tienes un presupuesto definido?", "¿Cuál es el alcance tecnológico?").
      6. Mantén la coherencia de género (${selectedSpecialist.gender === 'female' ? 'Femenina' : 'Masculino'}).`;

      const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          contents: [{ parts: [{ text: `El cliente dice: "${inputValue}". Responde brevemente y pregunta para seguir calificando.` }] }],
          systemInstruction: { parts: [{ text: systemPrompt }] }
        })
      });
      
      const data = await response.json();
      const aiText = data.candidates?.[0]?.content?.parts?.[0]?.text || "Entiendo perfectamente. Para avanzar, ¿podrías comentarme qué presupuesto aproximado tienes contemplado para esta fase del proyecto?";
      setChatMessages(prev => [...prev, { role: 'ai', text: aiText }]);
    } catch (error) {
      setChatMessages(prev => [...prev, { role: 'ai', text: "Entiendo. ¿Me podrías dar más detalles sobre la urgencia y los objetivos específicos de este requerimiento?" }]);
    } finally {
      setIsTyping(false);
    }
  };

  return (
    <div className="min-h-screen bg-[#050505] text-white font-sans selection:bg-amber-500 selection:text-black">
      <header className="border-b border-white/5 bg-black/80 backdrop-blur-xl sticky top-0 z-50 p-6">
        <div className="max-w-6xl mx-auto flex items-center justify-between">
          <div className="flex items-center gap-2 cursor-pointer" onClick={() => setStep('search')}>
            <div className="bg-amber-500 w-8 h-8 rounded-lg flex items-center justify-center font-black text-black">M</div>
            <h1 className="text-xl font-black tracking-tighter uppercase">Market<span className="text-amber-500">Match</span></h1>
          </div>
          {step === 'results' && (
            <button onClick={() => setStep('search')} className="text-[10px] font-black uppercase text-white/40 hover:text-white transition-all">Nueva búsqueda</button>
          )}
        </div>
      </header>

      <main className="max-w-6xl mx-auto p-6 md:py-12">
        {step === 'search' && (
          <div className="py-10 animate-in fade-in duration-700 space-y-12">
            <div className="text-center space-y-4">
              <h2 className="text-5xl md:text-7xl font-black uppercase tracking-tighter">Talento <span className="text-amber-500">Global</span></h2>
              <p className="text-white/40 max-w-lg mx-auto text-sm">Localiza instantáneamente 40 perfiles expertos por ciudad.</p>
            </div>
            <div className="bg-white/5 border border-white/10 p-8 rounded-[3rem] max-w-4xl mx-auto space-y-6">
              <div className="grid md:grid-cols-3 gap-4">
                {['continent', 'country', 'city'].map((f) => (
                  <div key={f} className="space-y-2">
                    <label className="text-[10px] font-black uppercase text-amber-500 ml-2">{f === 'continent' ? 'Continente' : f === 'country' ? 'País' : 'Ciudad'}</label>
                    <select 
                      className="w-full bg-black/40 border border-white/10 p-4 rounded-2xl outline-none focus:border-amber-500 text-sm appearance-none"
                      value={filters[f]}
                      onChange={(e) => setFilters({...filters, [f]: e.target.value})}
                    >
                      <option value="">Seleccionar...</option>
                      {f === 'continent' ? Object.keys(DATA_GEOGRAPHY).map(o => <option key={o} value={o}>{o}</option>) : 
                       f === 'country' ? (filters.continent ? Object.keys(DATA_GEOGRAPHY[filters.continent]).map(o => <option key={o} value={o}>{o}</option>) : []) :
                       (filters.continent && filters.country ? DATA_GEOGRAPHY[filters.continent][filters.country].map(o => <option key={o} value={o}>{o}</option>) : [])}
                    </select>
                  </div>
                ))}
              </div>
              <div className="space-y-2">
                <label className="text-[10px] font-black uppercase text-amber-500 ml-2">Especialidad</label>
                <select 
                  className="w-full bg-black/40 border border-white/10 p-4 rounded-2xl outline-none focus:border-amber-500 text-sm appearance-none"
                  value={filters.category}
                  onChange={(e) => setFilters({...filters, category: e.target.value})}
                >
                  <option value="">Seleccionar área...</option>
                  {CATEGORIES.map(c => <option key={c.id} value={c.id}>{c.name}</option>)}
                </select>
              </div>
              <button onClick={handleSearch} className="w-full bg-amber-500 hover:bg-white text-black font-black py-5 rounded-2xl transition-all uppercase tracking-widest text-xs flex items-center justify-center gap-3">
                <Search size={16} /> Escanear Red Profesional
              </button>
            </div>
          </div>
        )}

        {step === 'loading' && (
          <div className="h-[50vh] flex flex-col items-center justify-center space-y-6 animate-pulse">
            <div className="w-16 h-16 border-4 border-amber-500/20 border-t-amber-500 rounded-full animate-spin" />
            <p className="text-amber-500 font-black uppercase text-[10px] tracking-widest">{loadingText}</p>
          </div>
        )}

        {step === 'results' && (
          <div className="animate-in fade-in slide-in-from-bottom-5 duration-700 space-y-16">
            <div className="border-b border-white/5 pb-8">
              <h2 className="text-4xl font-black uppercase tracking-tighter">Expertos en <span className="text-amber-500">{filters.city}</span></h2>
              <p className="text-[10px] font-bold text-white/30 uppercase tracking-[0.3em] mt-2">40 perfiles encontrados • {CATEGORIES.find(c => c.id === filters.category)?.name}</p>
            </div>

            <section className="space-y-8">
              <h3 className="text-xs font-black uppercase text-amber-500 tracking-widest border-l-2 border-amber-500 pl-4">División Senior (20)</h3>
              <div className="grid gap-4">
                {results.seniors.map(s => (
                  <div key={s.id} className="bg-white/5 border border-white/5 p-6 rounded-3xl flex items-center justify-between group hover:border-amber-500/30 transition-all">
                    <div className="flex items-center gap-6">
                      <img src={s.photo} className="w-20 h-20 rounded-2xl object-cover grayscale group-hover:grayscale-0 transition-all" alt={s.name} />
                      <div className="space-y-1">
                        <h4 className="text-lg font-black">{s.name}</h4>
                        <StarRating rating={s.rating} />
                        <p className="text-[10px] text-white/40 font-medium">Exp: {s.experience} • {s.jobs.split(',')[0]}</p>
                      </div>
                    </div>
                    <button onClick={() => openChat(s)} className="bg-amber-500 text-black font-black px-6 py-3 rounded-xl text-[10px] uppercase hover:bg-white transition-all">Chatear</button>
                  </div>
                ))}
              </div>
            </section>

            <section className="space-y-8 pt-10 border-t border-white/5">
              <h3 className="text-xs font-black uppercase text-white/40 tracking-widest border-l-2 border-white/20 pl-4">Talento Junior (20)</h3>
              <div className="grid md:grid-cols-2 lg:grid-cols-4 gap-4">
                {results.juniors.map(j => (
                  <div key={j.id} className="bg-white/5 border border-white/5 p-6 rounded-3xl flex flex-col items-center text-center space-y-4 hover:border-white/20 transition-all">
                    <img src={j.photo} className="w-16 h-16 rounded-2xl object-cover" alt={j.name} />
                    <div className="space-y-1">
                      <h4 className="text-sm font-black">{j.name}</h4>
                      <StarRating rating={j.rating} />
                    </div>
                    <button onClick={() => openChat(j)} className="w-full bg-white/5 hover:bg-amber-500 hover:text-black py-2.5 rounded-lg text-[9px] font-black uppercase transition-all">Contactar</button>
                  </div>
                ))}
              </div>
            </section>
          </div>
        )}

        {step === 'chat' && (
          <div className="bg-[#080808] border border-white/10 rounded-[2.5rem] h-[700px] flex flex-col animate-in zoom-in-95 duration-500 max-w-3xl mx-auto overflow-hidden shadow-2xl shadow-amber-500/5">
            <div className="bg-white/5 p-6 border-b border-white/5 flex items-center justify-between">
              <div className="flex items-center gap-4">
                <img src={selectedSpecialist.photo} className="w-12 h-12 rounded-xl object-cover border border-amber-500/20" />
                <div>
                  <h3 className="text-md font-black">{selectedSpecialist.name}</h3>
                  <p className="text-[9px] text-amber-500 font-bold uppercase tracking-widest">{selectedSpecialist.isJunior ? 'Junior Specialist' : 'Senior Advisor'}</p>
                </div>
              </div>
              <button onClick={() => setStep('results')} className="text-white/20 hover:text-white transition-all p-2 bg-white/5 rounded-full"><ChevronLeft size={20} /></button>
            </div>
            
            <div className="flex-1 overflow-y-auto p-8 space-y-6 scrollbar-hide">
              {chatMessages.map((msg, i) => (
                <div key={i} className={`flex ${msg.role === 'user' ? 'justify-end' : 'justify-start'}`}>
                  <div className={`max-w-[85%] p-5 rounded-3xl text-sm leading-relaxed ${msg.role === 'user' ? 'bg-amber-500 text-black font-bold shadow-lg shadow-amber-500/10' : 'bg-white/5 text-white/90 border border-white/5'}`}>
                    {msg.text}
                  </div>
                </div>
              ))}
              {isTyping && (
                <div className="flex gap-1 items-center ml-2">
                  <div className="w-1.5 h-1.5 bg-amber-500 rounded-full animate-bounce" />
                  <div className="w-1.5 h-1.5 bg-amber-500 rounded-full animate-bounce [animation-delay:0.2s]" />
                  <div className="w-1.5 h-1.5 bg-amber-500 rounded-full animate-bounce [animation-delay:0.4s]" />
                </div>
              )}
              <div ref={chatEndRef} />
            </div>

            <form onSubmit={(e) => { e.preventDefault(); sendMessage(); }} className="p-6 border-t border-white/5 bg-black/60 backdrop-blur-md flex gap-3">
              <input 
                type="text" 
                placeholder="Escribe tu respuesta aquí..." 
                className="flex-1 bg-white/5 border border-white/10 rounded-2xl px-6 py-4 outline-none focus:border-amber-500 text-sm transition-all focus:ring-1 focus:ring-amber-500/20" 
                value={inputValue} 
                onChange={(e) => setInputValue(e.target.value)} 
                disabled={isTyping} 
              />
              <button 
                type="submit" 
                className="bg-amber-500 text-black px-6 rounded-2xl hover:bg-white transition-all disabled:opacity-50 flex items-center justify-center group" 
                disabled={isTyping || !inputValue.trim()}
              >
                <Send size={20} className="group-hover:translate-x-1 group-hover:-translate-y-1 transition-transform" />
              </button>
            </form>
          </div>
        )}
      </main>
      
      <footer className="py-12 text-center">
        <div className="opacity-20 text-[8px] font-black uppercase tracking-[1em] mb-4">MarketMatch Global Network</div>
        <div className="text-[10px] text-white/10 font-medium">© 2026 Inteligencia de Mercado Descentralizada</div>
      </footer>
    </div>
  );
};

export default App;
