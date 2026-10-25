# appservice
clearence home
import React, { useState, useEffect, useRef } from 'react';
import { 
  Globe, Shield, Award, Calendar, Clock, MapPin, DollarSign, 
  FileText, Camera, Star, AlertTriangle, CheckCircle, User, 
  Settings, LogOut, ChevronRight, Sparkles, Send, Lock, Briefcase,
  HelpCircle, Info, Menu, X, Check, HeartHandshake, Eye, Upload
} from 'lucide-react';

// --- TRANSLATIONS DICTIONARY ---
const translations = {
  en: {
    nav: {
      brand: "Squadron Service LLC",
      bookNow: "Book Cleaning",
      cleanerPortal: "Cleaner Portal (1099)",
      antiDiscrimination: "Equal Opportunity & Anti-Discrimination Policy",
      aiAssistant: "AI Room Estimator",
      language: "Language",
      login: "Portal Login",
      logout: "Log Out"
    },
    hero: {
      title: "Premium Eco-Friendly Cleaning in Charlotte & Surrounding Areas",
      subtitle: "Empowering independent cleaning professionals with fair pay, equal respect, and zero discrimination.",
      ctaBook: "Book Your Cleaning",
      ctaContractor: "Join as Independent Contractor",
      badge: "5-Star Rated Professional Cleaners"
    },
    policy: {
      title: "Anti-Discrimination & Equal Opportunity Policy",
      subtitle: "Squadron Service LLC Non-Discrimination Statute & Protections",
      intro: "Squadron Service LLC enforces a strict Zero-Tolerance Policy against discrimination, racial harassment, language bias, or cultural prejudice towards all independent contractors (cleaners) and clients.",
      rulesTitle: "Core Legal Protections & Directives",
      rule1: "Protects independent cleaners from mistreatment based on race, color, native language, ethnicity, national origin, immigration status, religion, or cultural background.",
      rule2: "Strict prohibition of linguistic discrimination. Cleaners have the right to communicate in Spanish, English, or their native language without fear of penalty, disrespect, or reduced compensation.",
      rule3: "Client Code of Conduct: Clients engaging in abusive, racist, or discriminatory behavior toward cleaning professionals will be permanently banned from the platform and billed for lost time.",
      rule4: "Contractor Safety & Right to Refuse: Cleaners retain the full legal right to immediately exit any property where they feel unsafe, harassed, or subjected to discriminatory treatment.",
      reportTitle: "Report a Discrimination or Misconduct Incident",
      reportDesc: "Submit an anonymous or confidential report directly to our legal and ethics department.",
      formName: "Your Name (Optional for anonymous)",
      formRole: "I am a...",
      roleClient: "Client",
      roleCleaner: "Independent Contractor / Cleaner",
      formDetails: "Incident Description (Date, Location, Details)",
      formSubmit: "Submit Confidential Report",
      submittedMsg: "Report submitted securely to Ethics Committee. Incident Reference ID: #SQ-"
    },
    booking: {
      title: "Book Professional Cleaning",
      step1: "1. Select Home Details",
      bedrooms: "Bedrooms",
      bathrooms: "Bathrooms",
      sqft: "Estimated Sq Ft",
      serviceType: "Service Type",
      standardClean: "Standard Maintenance Clean",
      deepClean: "Deep Cleaning & Sanitize",
      moveInOut: "Move-In / Move-Out Clean",
      step2: "2. Choose Your Professional",
      step3: "3. Schedule & Address",
      address: "Property Address (Charlotte Area)",
      zipCode: "Zip Code",
      date: "Preferred Date",
      time: "Time Slot",
      estimatedTotal: "Estimated Total",
      hourlyRate: "$35 - $45 / hr base",
      cleanerSelect: "Select Preferred Cleaner",
      payButton: "Proceed to Secure Payment (Stripe Connect)",
      paymentSim: "Stripe Connect Simulated Processing...",
      successTitle: "Booking Confirmed!",
      successDesc: "Your cleaner has been notified. You will receive SMS updates."
    },
    cleanerPortal: {
      title: "Independent Contractor (1099) Portal",
      subtitle: "Set your own rates, choose your coverage zones, and sign your 1099 agreement.",
      welcome: "Welcome back, Professional Contractor",
      statusActive: "Account Status: Active & Verified",
      ratesTitle: "Configure Your Hourly Rate",
      currentRate: "Your Current Rate",
      zonesTitle: "Coverage Zones in Charlotte Metro",
      zonesDesc: "Select areas where you are willing to accept jobs:",
      agreementTitle: "Squadron Service LLC - 1099 Independent Contractor Agreement",
      agreementNotice: "By signing below, you acknowledge that you operate as an independent business contractor, set your own schedule, and are protected under the Squadron Service Non-Discrimination Statute.",
      signContract: "Digital Signature (Full Legal Name)",
      signBtn: "Sign & Accept 1099 Agreement",
      signedStatus: "Agreement Signed & Valid for Current Tax Year",
      availableJobs: "Available Job Requests Near You",
      acceptJob: "Accept Job",
      declineJob: "Decline"
    },
    aiAssistant: {
      title: "Squadron AI Room Inspector & Estimator",
      subtitle: "Powered by Gemini Vision AI - Upload a room photo (bunk beds, hardwood floors, living areas) for instant time & product estimation.",
      uploadPrompt: "Drop or upload room photo here...",
      analyzeBtn: "Analyze Room with Gemini AI",
      analyzing: "Scanning room layout, surface types, and clutter level...",
      resultsTitle: "AI Analysis Results",
      estHours: "Estimated Cleaning Time",
      recommendedProducts: "Recommended Eco-Friendly Products",
      surfaceNotes: "Surface & Layout Inspection Notes"
    },
    footer: {
      rights: "Squadron Service LLC. All Rights Reserved. Charlotte, NC.",
      legalStatute: "Operates under strict Equal Opportunity & Anti-Discrimination Federal & State laws.",
      contact: "Support & Legal: support@squadronservice.com | Charlotte, NC"
    }
  },
  es: {
    nav: {
      brand: "Squadron Service LLC",
      bookNow: "Reservar Limpieza",
      cleanerPortal: "Portal Limpiadoras (1099)",
      antiDiscrimination: "Política de No Discriminación e Igualdad",
      aiAssistant: "Estimador IA de Cuartos",
      language: "Idioma",
      login: "Iniciar Sesión",
      logout: "Cerrar Sesión"
    },
    hero: {
      title: "Limpieza Ecológica Premium en Charlotte y Áreas Circundantes",
      subtitle: "Empoderando a profesionales independientes de la limpieza con pago justo, respeto equitativo y cero discriminación.",
      ctaBook: "Reservar Limpieza",
      ctaContractor: "Unirse como Contratista Independiente",
      badge: "Limpiadoras Profesionales Calificadas con 5 Estrellas"
    },
    policy: {
      title: "Política de No Discriminación e Igualdad de Oportunidades",
      subtitle: "Estatuto Legal y Protecciones de Squadron Service LLC",
      intro: "Squadron Service LLC aplica una estricta Política de Cero Tolerancia contra la discriminación, el acoso racial, el sesgo de idioma o el prejuicio cultural hacia todos los contratistas independientes (limpiadoras) y clientes.",
      rulesTitle: "Protecciones Legales Directivas y Fundamentales",
      rule1: "Protege activamente a las limpiadoras independientes del maltrato por motivo de raza, color, idioma materno, etnia, origen nacional, estatus migratorio, religión o antecedentes culturales.",
      rule2: "Prohibición estricta de discriminación lingüística. Las limpiadoras tienen el derecho de comunicarse en español, inglés o su idioma nativo sin temor a sanciones, faltas de respeto o reducción de compensación.",
      rule3: "Código de Conducta del Cliente: Los clientes que incurran en comportamiento abusivo, racista o discriminatorio hacia las trabajadoras serán suspendidos permanentemente de la plataforma y se les cobrará el tiempo perdido.",
      rule4: "Seguridad y Derecho de Retiro: Las limpiadoras conservan el derecho legal pleno de abandonar inmediatamente cualquier propiedad si se sienten inseguras, acosadas o sujetas a trato discriminatorio.",
      reportTitle: "Reportar un Incidente de Discriminación o Mal Comportamiento",
      reportDesc: "Envíe un reporte anónimo o confidencial directamente a nuestro departamento legal y de ética.",
      formName: "Su Nombre (Opcional para reporte anónimo)",
      formRole: "Yo soy...",
      roleClient: "Cliente",
      roleCleaner: "Contratista Independiente / Limpiadora",
      formDetails: "Descripción del Incidente (Fecha, Lugar, Detalles)",
      formSubmit: "Enviar Reporte Confidencial",
      submittedMsg: "Reporte enviado de forma segura al Comité de Ética. ID de Referencia: #SQ-"
    },
    booking: {
      title: "Reservar Servicio de Limpieza",
      step1: "1. Detalles de la Propiedad",
      bedrooms: "Habitaciones",
      bathrooms: "Baños",
      sqft: "Pies Cuadrados Estimados",
      serviceType: "Tipo de Servicio",
      standardClean: "Limpieza Mantenimiento Estándar",
      deepClean: "Limpieza Profunda y Desinfección",
      moveInOut: "Limpieza Mudanza (Entrada/Salida)",
      step2: "2. Elija a su Profesional",
      step3: "3. Agenda y Dirección",
      address: "Dirección de Propiedad (Área Charlotte)",
      zipCode: "Código Postal",
      date: "Fecha Preferida",
      time: "Horario",
      estimatedTotal: "Total Estimado",
      hourlyRate: "Base $35 - $45 / hr",
      cleanerSelect: "Seleccionar Limpiadora Preferida",
      payButton: "Proceder al Pago Seguro (Stripe Connect)",
      paymentSim: "Procesamiento Simulado Stripe Connect...",
      successTitle: "¡Reserva Confirmada!",
      successDesc: "Su limpiadora ha sido notificada. Recibirá actualizaciones por SMS."
    },
    cleanerPortal: {
      title: "Portal de Contratistas Independientes (1099)",
      subtitle: "Ajuste sus tarifas, seleccione sus zonas de cobertura y firme su acuerdo 1099.",
      welcome: "Bienvenida de nuevo, Contratista Profesional",
      statusActive: "Estado de Cuenta: Activo y Verificado",
      ratesTitle: "Configurar su Tarifa por Hora",
      currentRate: "Su Tarifa Actual",
      zonesTitle: "Zonas de Cobertura en Charlotte Metro",
      zonesDesc: "Seleccione las zonas donde desea aceptar trabajos:",
      agreementTitle: "Squadron Service LLC - Acuerdo de Contratista Independiente 1099",
      agreementNotice: "Al firmar a continuación, reconoce que opera como contratista independiente, gestiona su propio horario y está protegida bajo la Norma de No Discriminación de Squadron Service LLC.",
      signContract: "Firma Digital (Nombre Legal Completo)",
      signBtn: "Firmar y Aceptar Acuerdo 1099",
      signedStatus: "Acuerdo Firmado y Válido para el Año Fiscal Actual",
      availableJobs: "Solicitudes de Trabajo Disponibles en su Zona",
      acceptJob: "Aceptar Trabajo",
      declineJob: "Rechazar"
    },
    aiAssistant: {
      title: "Asistente IA Inspector de Espacios Squadron",
      subtitle: "Impulsado por Gemini Vision AI - Suba una foto (literas, pisos flotantes, salas) para estimar tiempo y productos adecuados.",
      uploadPrompt: "Arrastre o suba la foto de la habitación aquí...",
      analyzeBtn: "Analizar Habitación con IA Gemini",
      analyzing: "Escaneando distribución del espacio, tipos de superficie y nivel de desorden...",
      resultsTitle: "Resultados del Análisis IA",
      estHours: "Tiempo Estimado de Limpieza",
      recommendedProducts: "Productos Ecológicos Recomendados",
      surfaceNotes: "Notas de Inspección de Superficies y Distribución"
    },
    footer: {
      rights: "Squadron Service LLC. Todos los derechos reservados. Charlotte, NC.",
      legalStatute: "Opera bajo estrictas leyes federales y estatales de Igualdad de Oportunidades y No Discriminación.",
      contact: "Soporte y Legal: soporte@squadronservice.com | Charlotte, NC"
    }
  }
};

// --- MOCK DATA FOR CLEANERS IN CHARLOTTE METRO AREA ---
const mockCleaners = [
  {
    id: 1,
    name: "Maria Santos",
    rating: 4.98,
    reviewsCount: 142,
    languages: ["Español", "English (Basic)"],
    zones: ["Matthews", "Stallings", "South Charlotte"],
    ratePerHour: 40,
    photo: "https://images.unsplash.com/photo-1573496359142-b8d87734a5a2?auto=format&fit=crop&w=300&q=80",
    bio: "Especialista en limpieza profunda ecológica con 6 años de experiencia en el área de Charlotte."
  },
  {
    id: 2,
    name: "Elena Rostova",
    rating: 4.95,
    reviewsCount: 98,
    languages: ["English", "Español"],
    zones: ["Indian Trail", "Ballantyne", "Pineville"],
    ratePerHour: 42,
    photo: "https://images.unsplash.com/photo-1580489944761-15a19d654956?auto=format&fit=crop&w=300&q=80",
    bio: "Detail-oriented professional specializing in move-in/move-out cleans and pet-friendly solutions."
  },
  {
    id: 3,
    name: "Carmen & Sofia Diaz",
    rating: 5.00,
    reviewsCount: 215,
    languages: ["Español", "English"],
    zones: ["Mount Holly", "Uptown Charlotte", "Huntersville"],
    ratePerHour: 45,
    photo: "https://images.unsplash.com/photo-1567532939604-b6b5b0db2604?auto=format&fit=crop&w=300&q=80",
    bio: "Equipo dúo madre e hija para limpiezas expresas de alta calidad con productos biodegradables."
  }
];

const charlotteZonesList = [
  "Uptown Charlotte", "Matthews", "Stallings", "Indian Trail", 
  "Ballantyne", "Mount Holly", "Huntersville", "Pineville", "Concord"
];

export default function App() {
  const [lang, setLang] = useState('es'); // Default bilingual language is Spanish/English switch
  const [activeTab, setActiveTab] = useState('booking'); // 'booking', 'cleanerPortal', 'policy', 'aiAssistant'
  
  // Translation Helper
  const t = translations[lang];

  // --- BOOKING STATE ---
  const [booking, setBooking] = useState({
    bedrooms: 2,
    bathrooms: 2,
    sqft: 1500,
    serviceType: 'standardClean',
    selectedCleaner: mockCleaners[0],
    address: '',
    zipCode: '28202',
    date: '',
    time: '09:00',
    isProcessing: false,
    isSuccess: false
  });

  // Calculate estimated total price
  const baseRate = booking.selectedCleaner ? booking.selectedCleaner.ratePerHour : 40;
  const totalHours = Math.max(2, Math.round((booking.bedrooms * 0.75) + (booking.bathrooms * 0.8) + (booking.sqft / 1000)));
  const estimatedCost = totalHours * baseRate;

  // --- CLEANER 1099 PORTAL STATE ---
  const [cleanerProfile, setCleanerProfile] = useState({
    hourlyRate: 40,
    selectedZones: ["Matthews", "Stallings", "Uptown Charlotte"],
    signatureName: '',
    isSigned: false,
    availableJobsList: [
      { id: 101, area: "Matthews, NC", desc: "3 Bed / 2 Bath - Deep Clean", payout: "$130", date: "Tomorrow, 10:00 AM" },
      { id: 102, area: "Stallings, NC", desc: "2 Bed / 1 Bath - Standard Clean", payout: "$80", date: "Friday, 2:00 PM" }
    ]
  });

  // --- DISCRIMINATION REPORT FORM STATE ---
  const [report, setReport] = useState({
    name: '',
    role: 'roleCleaner',
    details: '',
    submitted: false,
    reportId: ''
  });

  const handleReportSubmit = (e) => {
    e.preventDefault();
    const randomId = Math.floor(100000 + Math.random() * 900000);
    setReport({ ...report, submitted: true, reportId: randomId.toString() });
  };

  // --- GEMINI AI ASSISTANT STATE ---
  const [aiState, setAiState] = useState({
    imageFile: null,
    imagePreview: null,
    analyzing: false,
    analysisResult: null,
    apiKey: "" // Left blank as Canvas automatically injects or handles execution safely
  });

  const handleImageUpload = (e) => {
    const file = e.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onloadend = () => {
        setAiState(prev => ({
          ...prev,
          imageFile: file,
          imagePreview: reader.result,
          analysisResult: null
        }));
      };
      reader.readAsDataURL(file);
    }
  };

  const runGeminiVisionAnalysis = async () => {
    if (!aiState.imagePreview) return;
    setAiState(prev => ({ ...prev, analyzing: true }));

    try {
      const base64Data = aiState.imagePreview.split(',')[1];
      const mimeType = aiState.imagePreview.split(';')[0].split(':')[1] || 'image/jpeg';

      const promptText = lang === 'es' 
        ? "Analiza esta imagen de una habitación (mira si hay literas, tipo de piso como flotante/madera/alfombra, grado de desorden). Proporciona: 1. Tiempo estimado de limpieza en horas. 2. Productos de limpieza recomendados (especialmente para este piso o muebles). 3. Consejos específicos para la limpiadora."
        : "Analyze this room photo (check for bunk beds, flooring type like hardwood/laminate/carpet, clutter level). Provide: 1. Estimated cleaning hours. 2. Recommended eco products. 3. Specific cleaner instructions.";

      const payload = {
        contents: [
          {
            role: "user",
            parts: [
              { text: promptText },
              { inlineData: { mimeType: mimeType, data: base64Data } }
            ]
          }
        ]
      };

      const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${aiState.apiKey}`;

      const res = await fetch(apiUrl, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });

      const data = await res.json();
      const textOutput = data?.candidates?.[0]?.content?.parts?.[0]?.text;

      if (textOutput) {
        setAiState(prev => ({
          ...prev,
          analyzing: false,
          analysisResult: textOutput
        }));
      } else {
        throw new Error("No output text from Gemini API");
      }
    } catch (err) {
      // Fallback simulated response if network or API key limitation occurs
      setTimeout(() => {
        const fallbackText = lang === 'es' 
          ? "📋 **Análisis Inteligente Finalizado (Demostración)**:\n\n⏱️ **Tiempo Estimado:** 2.5 a 3 Horas.\n🧹 **Piso detectado:** Madera flotante / Laminado. Se recomienda mopa húmeda de microfibra con limpiador pH neutro especial para madera (evitar exceso de agua).\n🛏️ **Mobiliario/Distribución:** Litera / Muebles altos. Requiere plumero extensible e inspección de esquinas superiores.\n🌱 **Productos Recomendados:** Limpiador ecológico multiusos biodegradable, desinfectante no abrasivo."
          : "📋 **Smart Analysis Completed (Demo Mode)**:\n\n⏱️ **Estimated Time:** 2.5 to 3 Hours.\n🧹 **Flooring Detected:** Hardwood / Laminate. Microfiber mop with neutral pH wood cleaner recommended (avoid excess water).\n🛏️ **Furniture/Layout:** Bunk beds / High shelving detected. Requires extendable duster and high corner inspection.\n🌱 **Recommended Products:** Biodegradable multi-purpose eco cleaner, non-abrasive disinfectant.";
        setAiState(prev => ({
          ...prev,
          analyzing: false,
          analysisResult: fallbackText
        }));
      }, 1500);
    }
  };

  return (
    <div className="min-h-screen bg-slate-50 text-slate-900 flex flex-col font-sans">
      
      {/* --- TOP BILINGUAL NAVBAR --- */}
      <header className="sticky top-0 z-50 bg-slate-900 text-white shadow-md border-b border-slate-800">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
          
          {/* Brand Logo */}
          <div className="flex items-center space-x-3 cursor-pointer" onClick={() => setActiveTab('booking')}>
            <div className="w-10 h-10 rounded-lg bg-gradient-to-br from-emerald-500 to-teal-700 flex items-center justify-center text-white font-bold text-xl shadow-inner">
              SQ
            </div>
            <div>
              <span className="font-extrabold text-lg sm:text-xl tracking-tight text-white block">
                SQUADRON <span className="text-emerald-400 font-normal">SERVICE</span>
              </span>
              <span className="text-xs text-slate-400 block -mt-1">LLC • Charlotte, NC</span>
            </div>
          </div>

          {/* Navigation Links (Desktop) */}
          <nav className="hidden md:flex items-center space-x-1 lg:space-x-4 text-sm font-medium">
            <button 
              onClick={() => setActiveTab('booking')}
              className={`px-3 py-2 rounded-md transition ${activeTab === 'booking' ? 'bg-emerald-600 text-white' : 'text-slate-300 hover:text-white hover:bg-slate-800'}`}
            >
              {t.nav.bookNow}
            </button>
            <button 
              onClick={() => setActiveTab('cleanerPortal')}
              className={`px-3 py-2 rounded-md transition ${activeTab === 'cleanerPortal' ? 'bg-emerald-600 text-white' : 'text-slate-300 hover:text-white hover:bg-slate-800'}`}
            >
              {t.nav.cleanerPortal}
            </button>
            <button 
              onClick={() => setActiveTab('aiAssistant')}
              className={`px-3 py-2 rounded-md transition flex items-center space-x-1 ${activeTab === 'aiAssistant' ? 'bg-emerald-600 text-white' : 'text-slate-300 hover:text-white hover:bg-slate-800'}`}
            >
              <Sparkles className="w-4 h-4 text-amber-300" />
              <span>{t.nav.aiAssistant}</span>
            </button>
            <button 
              onClick={() => setActiveTab('policy')}
              className={`px-3 py-2 rounded-md transition flex items-center space-x-1 ${activeTab === 'policy' ? 'bg-emerald-600 text-white' : 'text-slate-300 hover:text-white hover:bg-slate-800'}`}
            >
              <Shield className="w-4 h-4 text-emerald-400" />
              <span>{t.nav.antiDiscrimination}</span>
            </button>
          </nav>

          {/* Language Switcher Selector */}
          <div className="flex items-center space-x-3">
            <div className="flex items-center bg-slate-800 border border-slate-700 rounded-lg p-1 text-xs">
              <Globe className="w-4 h-4 text-emerald-400 ml-1 mr-1" />
              <button 
                onClick={() => setLang('es')}
                className={`px-2 py-1 rounded font-semibold transition ${lang === 'es' ? 'bg-emerald-500 text-slate-950' : 'text-slate-400 hover:text-white'}`}
              >
                ES
              </button>
              <button 
                onClick={() => setLang('en')}
                className={`px-2 py-1 rounded font-semibold transition ${lang === 'en' ? 'bg-emerald-500 text-slate-950' : 'text-slate-400 hover:text-white'}`}
              >
                EN
              </button>
            </div>
          </div>
        </div>

        {/* Mobile Navigation Toolbar */}
        <div className="md:hidden flex justify-around bg-slate-950 border-t border-slate-800 py-2 text-xs text-slate-300">
          <button onClick={() => setActiveTab('booking')} className={`flex flex-col items-center ${activeTab === 'booking' ? 'text-emerald-400 font-bold' : ''}`}>
            <Calendar className="w-4 h-4 mb-0.5" />
            <span>{t.nav.bookNow}</span>
          </button>
          <button onClick={() => setActiveTab('cleanerPortal')} className={`flex flex-col items-center ${activeTab === 'cleanerPortal' ? 'text-emerald-400 font-bold' : ''}`}>
            <Briefcase className="w-4 h-4 mb-0.5" />
            <span>1099 Portal</span>
          </button>
          <button onClick={() => setActiveTab('aiAssistant')} className={`flex flex-col items-center ${activeTab === 'aiAssistant' ? 'text-emerald-400 font-bold' : ''}`}>
            <Sparkles className="w-4 h-4 mb-0.5 text-amber-300" />
            <span>AI Estimator</span>
          </button>
          <button onClick={() => setActiveTab('policy')} className={`flex flex-col items-center ${activeTab === 'policy' ? 'text-emerald-400 font-bold' : ''}`}>
            <Shield className="w-4 h-4 mb-0.5 text-emerald-400" />
            <span>Policy</span>
          </button>
        </div>
      </header>

      {/* --- HERO BANNER --- */}
      <section className="bg-gradient-to-r from-slate-900 via-slate-800 to-teal-950 text-white py-10 px-4 sm:px-6 lg:px-8 border-b border-teal-900/40">
        <div className="max-w-7xl mx-auto flex flex-col md:flex-row items-center justify-between gap-8">
          <div className="max-w-2xl">
            <div className="inline-flex items-center space-x-2 bg-emerald-950/80 border border-emerald-500/30 text-emerald-300 text-xs px-3 py-1.5 rounded-full mb-4">
              <Star className="w-3.5 h-3.5 fill-amber-400 text-amber-400" />
              <span>{t.hero.badge}</span>
            </div>
            <h1 className="text-2xl sm:text-4xl font-extrabold tracking-tight leading-tight text-white mb-3">
              {t.hero.title}
            </h1>
            <p className="text-slate-300 text-sm sm:text-base mb-6 leading-relaxed">
              {t.hero.subtitle}
            </p>
            <div className="flex flex-wrap gap-3">
              <button 
                onClick={() => setActiveTab('booking')}
                className="bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold px-5 py-2.5 rounded-lg shadow-lg transition text-sm flex items-center space-x-2"
              >
                <Calendar className="w-4 h-4" />
                <span>{t.hero.ctaBook}</span>
              </button>
              <button 
                onClick={() => setActiveTab('cleanerPortal')}
                className="bg-slate-800 hover:bg-slate-700 text-white border border-slate-600 font-semibold px-5 py-2.5 rounded-lg transition text-sm flex items-center space-x-2"
              >
                <User className="w-4 h-4 text-emerald-400" />
                <span>{t.hero.ctaContractor}</span>
              </button>
            </div>
          </div>

          {/* Quick Stats / Guarantee Card */}
          <div className="w-full md:w-80 bg-slate-800/80 backdrop-blur-md p-5 rounded-xl border border-slate-700/80 shadow-xl text-xs space-y-3">
            <div className="flex items-center space-x-3 text-emerald-400 font-bold text-sm border-b border-slate-700 pb-2">
              <HeartHandshake className="w-5 h-5 shrink-0" />
              <span>Equal Opportunity & Respect</span>
            </div>
            <p className="text-slate-300">
              {lang === 'es' 
                ? "Garantizamos un entorno de respeto mutuo. Tarifas transparentes para clientes y pago justo directo a las limpiadoras."
                : "We guarantee mutual respect. Transparent rates for clients and fair direct payouts for independent cleaners."}
            </p>
            <div className="bg-slate-900/60 p-2.5 rounded border border-slate-700/50 flex justify-between text-slate-300">
              <span>Charlotte, NC Coverage</span>
              <span className="text-emerald-400 font-bold">100% Verified</span>
            </div>
          </div>
        </div>
      </section>

      {/* --- MAIN INTERACTIVE CONTENT AREA --- */}
      <main className="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8">

        {/* ==================== TAB 1: CLIENT BOOKING SYSTEM ==================== */}
        {activeTab === 'booking' && (
          <div className="space-y-8">
            <div className="border-b border-slate-200 pb-4">
              <h2 className="text-2xl font-extrabold text-slate-900">{t.booking.title}</h2>
              <p className="text-slate-600 text-sm">Calculate your price, pick your top-rated local cleaner, and schedule safely.</p>
            </div>

            {booking.isSuccess ? (
              <div className="bg-emerald-50 border-2 border-emerald-500 rounded-xl p-8 text-center max-w-xl mx-auto my-8">
                <CheckCircle className="w-16 h-16 text-emerald-600 mx-auto mb-4" />
                <h3 className="text-2xl font-bold text-slate-900 mb-2">{t.booking.successTitle}</h3>
                <p className="text-slate-700 mb-6">{t.booking.successDesc}</p>
                <div className="bg-white p-4 rounded-lg border border-slate-200 text-left text-sm space-y-2 mb-6">
                  <p><strong>Cleaner:</strong> {booking.selectedCleaner.name}</p>
                  <p><strong>Address:</strong> {booking.address || 'Charlotte Metro Area'}</p>
                  <p><strong>Total Charged:</strong> ${estimatedCost} USD</p>
                </div>
                <button 
                  onClick={() => setBooking({ ...booking, isSuccess: false })}
                  className="bg-emerald-600 hover:bg-emerald-500 text-white font-bold px-6 py-2.5 rounded-lg transition"
                >
                  Make Another Booking
                </button>
              </div>
            ) : (
              <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
                
                {/* Left Column: Property Specs & Details */}
                <div className="lg:col-span-2 space-y-6">
                  
                  {/* Step 1: Specs */}
                  <div className="bg-white p-6 rounded-xl border border-slate-200 shadow-sm space-y-4">
                    <h3 className="text-lg font-bold text-slate-900 text-emerald-800 flex items-center space-x-2">
                      <span>{t.booking.step1}</span>
                    </h3>

                    <div className="grid grid-cols-1 sm:grid-cols-3 gap-4">
                      <div>
                        <label className="block text-xs font-semibold text-slate-600 mb-1">{t.booking.bedrooms}</label>
                        <select 
                          value={booking.bedrooms}
                          onChange={(e) => setBooking({ ...booking, bedrooms: parseInt(e.target.value) })}
                          className="w-full border border-slate-300 rounded-lg p-2 text-sm focus:ring-2 focus:ring-emerald-500 outline-none"
                        >
                          {[1,2,3,4,5,6].map(num => (
                            <option key={num} value={num}>{num} {lang === 'es' ? 'Habitación(es)' : 'Bedrooms'}</option>
                          ))}
                        </select>
                      </div>

                      <div>
                        <label className="block text-xs font-semibold text-slate-600 mb-1">{t.booking.bathrooms}</label>
                        <select 
                          value={booking.bathrooms}
                          onChange={(e) => setBooking({ ...booking, bathrooms: parseInt(e.target.value) })}
                          className="w-full border border-slate-300 rounded-lg p-2 text-sm focus:ring-2 focus:ring-emerald-500 outline-none"
                        >
                          {[1,1.5,2,2.5,3,3.5,4,5].map(num => (
                            <option key={num} value={num}>{num} {lang === 'es' ? 'Baño(s)' : 'Baths'}</option>
                          ))}
                        </select>
                      </div>

                      <div>
                        <label className="block text-xs font-semibold text-slate-600 mb-1">{t.booking.sqft}</label>
                        <input 
                          type="number"
                          value={booking.sqft}
                          onChange={(e) => setBooking({ ...booking, sqft: parseInt(e.target.value) || 500 })}
                          className="w-full border border-slate-300 rounded-lg p-2 text-sm focus:ring-2 focus:ring-emerald-500 outline-none"
                          step="100"
                        />
                      </div>
                    </div>

                    <div>
                      <label className="block text-xs font-semibold text-slate-600 mb-1">{t.booking.serviceType}</label>
                      <div className="grid grid-cols-1 sm:grid-cols-3 gap-3">
                        {[
                          { id: 'standardClean', label: t.booking.standardClean },
                          { id: 'deepClean', label: t.booking.deepClean },
                          { id: 'moveInOut', label: t.booking.moveInOut }
                        ].map(st => (
                          <button
                            key={st.id}
                            type="button"
                            onClick={() => setBooking({ ...booking, serviceType: st.id })}
                            className={`p-3 text-xs font-medium rounded-lg border text-left transition ${
                              booking.serviceType === st.id 
                                ? 'bg-emerald-50 border-emerald-500 text-emerald-900 font-bold shadow-sm' 
                                : 'border-slate-200 text-slate-700 hover:bg-slate-50'
                            }`}
                          >
                            {st.label}
                          </button>
                        ))}
                      </div>
                    </div>
                  </div>

                  {/* Step 2: Choose Cleaner */}
                  <div className="bg-white p-6 rounded-xl border border-slate-200 shadow-sm space-y-4">
                    <h3 className="text-lg font-bold text-slate-900 text-emerald-800">
                      {t.booking.step2}
                    </h3>

                    <div className="grid grid-cols-1 gap-4">
                      {mockCleaners.map(cleaner => (
                        <div 
                          key={cleaner.id}
                          onClick={() => setBooking({ ...booking, selectedCleaner: cleaner })}
                          className={`p-4 rounded-xl border transition cursor-pointer flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 ${
                            booking.selectedCleaner?.id === cleaner.id 
                              ? 'border-emerald-500 bg-emerald-50/40 ring-2 ring-emerald-500/20' 
                              : 'border-slate-200 hover:border-slate-300 bg-white'
                          }`}
                        >
                          <div className="flex items-center space-x-4">
                            <img 
                              src={cleaner.photo} 
                              alt={cleaner.name} 
                              className="w-14 h-14 rounded-full object-cover border-2 border-emerald-500/40"
                            />
                            <div>
                              <div className="flex items-center space-x-2">
                                <h4 className="font-bold text-slate-900 text-base">{cleaner.name}</h4>
                                <span className="bg-amber-100 text-amber-900 text-xs font-bold px-2 py-0.5 rounded flex items-center">
                                  <Star className="w-3 h-3 fill-amber-400 text-amber-500 mr-1" />
                                  {cleaner.rating} ({cleaner.reviewsCount})
                                </span>
                              </div>
                              <p className="text-xs text-slate-600 mt-0.5">{cleaner.bio}</p>
                              <div className="flex flex-wrap gap-1 mt-2">
                                {cleaner.languages.map((l, i) => (
                                  <span key={i} className="text-[10px] bg-slate-100 text-slate-600 px-2 py-0.5 rounded">
                                    {l}
                                  </span>
                                ))}
                                {cleaner.zones.map((z, i) => (
                                  <span key={i} className="text-[10px] bg-teal-50 text-teal-700 px-2 py-0.5 rounded border border-teal-100">
                                    📍 {z}
                                  </span>
                                ))}
                              </div>
                            </div>
                          </div>

                          <div className="shrink-0 text-right">
                            <span className="text-lg font-extrabold text-slate-900">${cleaner.ratePerHour}</span>
                            <span className="text-xs text-slate-500 block">/ hour</span>
                          </div>
                        </div>
                      ))}
                    </div>
                  </div>

                  {/* Step 3: Address & Schedule */}
                  <div className="bg-white p-6 rounded-xl border border-slate-200 shadow-sm space-y-4">
                    <h3 className="text-lg font-bold text-slate-900 text-emerald-800">
                      {t.booking.step3}
                    </h3>

                    <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
                      <div>
                        <label className="block text-xs font-semibold text-slate-600 mb-1">{t.booking.address}</label>
                        <input 
                          type="text"
                          placeholder="123 Main St, Charlotte"
                          value={booking.address}
                          onChange={(e) => setBooking({ ...booking, address: e.target.value })}
                          className="w-full border border-slate-300 rounded-lg p-2 text-sm focus:ring-2 focus:ring-emerald-500 outline-none"
                        />
                      </div>
                      <div>
                        <label className="block text-xs font-semibold text-slate-600 mb-1">{t.booking.zipCode}</label>
                        <input 
                          type="text"
                          value={booking.zipCode}
                          onChange={(e) => setBooking({ ...booking, zipCode: e.target.value })}
                          className="w-full border border-slate-300 rounded-lg p-2 text-sm focus:ring-2 focus:ring-emerald-500 outline-none"
                        />
                      </div>
                      <div>
                        <label className="block text-xs font-semibold text-slate-600 mb-1">{t.booking.date}</label>
                        <input 
                          type="date"
                          value={booking.date}
                          onChange={(e) => setBooking({ ...booking, date: e.target.value })}
                          className="w-full border border-slate-300 rounded-lg p-2 text-sm focus:ring-2 focus:ring-emerald-500 outline-none"
                        />
                      </div>
                      <div>
                        <label className="block text-xs font-semibold text-slate-600 mb-1">{t.booking.time}</label>
                        <input 
                          type="time"
                          value={booking.time}
                          onChange={(e) => setBooking({ ...booking, time: e.target.value })}
                          className="w-full border border-slate-300 rounded-lg p-2 text-sm focus:ring-2 focus:ring-emerald-500 outline-none"
                        />
                      </div>
                    </div>
                  </div>

                </div>

                {/* Right Column: Checkout Summary & Stripe Connect Simulator */}
                <div className="space-y-6">
                  <div className="bg-slate-900 text-white p-6 rounded-xl shadow-xl sticky top-20 border border-slate-800">
                    <h3 className="text-lg font-bold mb-4 border-b border-slate-800 pb-3 flex items-center justify-between">
                      <span>Order Summary</span>
                      <Lock className="w-4 h-4 text-emerald-400" />
                    </h3>

                    <div className="space-y-3 text-sm text-slate-300">
                      <div className="flex justify-between">
                        <span>Selected Cleaner:</span>
                        <span className="font-semibold text-white">{booking.selectedCleaner?.name}</span>
                      </div>
                      <div className="flex justify-between">
                        <span>Hourly Rate:</span>
                        <span className="font-semibold text-white">${baseRate}/hr</span>
                      </div>
                      <div className="flex justify-between">
                        <span>Estimated Hours:</span>
                        <span className="font-semibold text-white">{totalHours} hrs</span>
                      </div>
                      <div className="flex justify-between border-t border-slate-800 pt-3 text-base font-bold text-white">
                        <span>{t.booking.estimatedTotal}:</span>
                        <span className="text-emerald-400 text-xl">${estimatedCost} USD</span>
                      </div>
                    </div>

                    <div className="mt-6 pt-4 border-t border-slate-800">
                      <div className="bg-slate-800 p-3 rounded-lg mb-4 text-xs text-slate-400 space-y-1">
                        <p className="flex items-center text-emerald-400 font-semibold">
                          <Shield className="w-3.5 h-3.5 mr-1" />
                          Squadron Zero-Harassment Protection
                        </p>
                        <p>100% Direct Stripe Connect payout to contractor. Equal treatment protected.</p>
                      </div>

                      <button
                        disabled={booking.isProcessing}
                        onClick={() => {
                          setBooking({ ...booking, isProcessing: true });
                          setTimeout(() => {
                            setBooking({ ...booking, isProcessing: false, isSuccess: true });
                          }, 1800);
                        }}
                        className="w-full bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-extrabold py-3 rounded-lg transition shadow-lg flex items-center justify-center space-x-2 disabled:opacity-50"
                      >
                        {booking.isProcessing ? (
                          <span>{t.booking.paymentSim}</span>
                        ) : (
                          <>
                            <DollarSign className="w-5 h-5" />
                            <span>{t.booking.payButton}</span>
                          </>
                        )}
                      </button>
                    </div>
                  </div>
                </div>

              </div>
            )}
          </div>
        )}

        {/* ==================== TAB 2: CLEANER 1099 PORTAL ==================== */}
        {activeTab === 'cleanerPortal' && (
          <div className="space-y-8">
            <div className="border-b border-slate-200 pb-4 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
              <div>
                <h2 className="text-2xl font-extrabold text-slate-900">{t.cleanerPortal.title}</h2>
                <p className="text-slate-600 text-sm">{t.cleanerPortal.subtitle}</p>
              </div>
              <span className="bg-emerald-100 text-emerald-800 text-xs font-bold px-3 py-1.5 rounded-full flex items-center">
                <CheckCircle className="w-4 h-4 mr-1 text-emerald-600" />
                {t.cleanerPortal.statusActive}
              </span>
            </div>

            <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
              
              {/* Left Column: Rate & Zone Controls */}
              <div className="space-y-6">
                
                {/* Rate Config Card */}
                <div className="bg-white p-6 rounded-xl border border-slate-200 shadow-sm space-y-4">
                  <h3 className="font-bold text-slate-900 text-base flex items-center space-x-2">
                    <DollarSign className="w-5 h-5 text-emerald-600" />
                    <span>{t.cleanerPortal.ratesTitle}</span>
                  </h3>
                  <p className="text-xs text-slate-500">
                    {lang === 'es' 
                      ? "Usted es una contratista independiente y tiene la libertad de establecer su tarifa por hora."
                      : "You are an independent contractor and have full control to set your desired hourly rate."}
                  </p>
                  <div className="flex items-center space-x-3">
                    <span className="text-2xl font-extrabold text-slate-900">${cleanerProfile.hourlyRate}</span>
                    <span className="text-xs text-slate-500">/ hr</span>
                    <input 
                      type="range" 
                      min="30" 
                      max="75" 
                      step="1"
                      value={cleanerProfile.hourlyRate}
                      onChange={(e) => setCleanerProfile({ ...cleanerProfile, hourlyRate: parseInt(e.target.value) })}
                      className="w-full accent-emerald-600"
                    />
                  </div>
                </div>

                {/* Coverage Zones Card */}
                <div className="bg-white p-6 rounded-xl border border-slate-200 shadow-sm space-y-4">
                  <h3 className="font-bold text-slate-900 text-base flex items-center space-x-2">
                    <MapPin className="w-5 h-5 text-emerald-600" />
                    <span>{t.cleanerPortal.zonesTitle}</span>
                  </h3>
                  <p className="text-xs text-slate-500">{t.cleanerPortal.zonesDesc}</p>
                  
                  <div className="flex flex-wrap gap-2">
                    {charlotteZonesList.map((zone) => {
                      const isSelected = cleanerProfile.selectedZones.includes(zone);
                      return (
                        <button
                          key={zone}
                          onClick={() => {
                            if (isSelected) {
                              setCleanerProfile({
                                ...cleanerProfile,
                                selectedZones: cleanerProfile.selectedZones.filter(z => z !== zone)
                              });
                            } else {
                              setCleanerProfile({
                                ...cleanerProfile,
                                selectedZones: [...cleanerProfile.selectedZones, zone]
                              });
                            }
                          }}
                          className={`text-xs px-3 py-1.5 rounded-full border transition ${
                            isSelected 
                              ? 'bg-emerald-600 text-white border-emerald-600 font-semibold' 
                              : 'bg-slate-50 text-slate-700 border-slate-200 hover:border-slate-300'
                          }`}
                        >
                          {zone} {isSelected && '✓'}
                        </button>
                      );
                    })}
                  </div>
                </div>

              </div>

              {/* Right Column: 1099 Legal Agreement & Job Requests */}
              <div className="lg:col-span-2 space-y-6">
                
                {/* 1099 Contract Agreement Card */}
                <div className="bg-white p-6 rounded-xl border border-slate-200 shadow-sm space-y-4">
                  <div className="flex items-center justify-between border-b border-slate-200 pb-3">
                    <h3 className="font-bold text-slate-900 text-base flex items-center space-x-2">
                      <FileText className="w-5 h-5 text-emerald-600" />
                      <span>{t.cleanerPortal.agreementTitle}</span>
                    </h3>
                    <span className="text-xs bg-slate-100 text-slate-700 px-2 py-1 rounded font-mono">Form 1099-NEC</span>
                  </div>

                  <div className="h-40 overflow-y-auto bg-slate-50 p-4 rounded-lg border border-slate-200 text-xs text-slate-600 leading-relaxed space-y-2">
                    <p className="font-bold text-slate-800">1. INDEPENDENT CONTRACTOR STATUS</p>
                    <p>The Cleaner operates as an independent contractor under IRS Form 1099-NEC rules. Squadron Service LLC does not exercise direct employer oversight over cleaning techniques or work hours. The contractor maintains full autonomy over schedule, pricing, and job acceptance.</p>
                    
                    <p className="font-bold text-slate-800">2. ANTI-DISCRIMINATION & SAFETY PROTECTION</p>
                    <p>Squadron Service LLC protects contractors against racial harassment, language discrimination, or xenophobic mistreatment. Cleaners have the absolute legal right to leave any property where they experience discriminatory conduct and will receive full compensation for the scheduled time.</p>
                    
                    <p className="font-bold text-slate-800">3. DIRECT STRIPE CONNECT PAYOUTS</p>
                    <p>Fees earned are disbursed directly to the contractor’s bank account via Stripe Connect within 24 hours of job completion.</p>
                  </div>

                  {cleanerProfile.isSigned ? (
                    <div className="bg-emerald-50 border border-emerald-300 text-emerald-900 p-4 rounded-lg text-xs flex items-center space-x-3">
                      <CheckCircle className="w-5 h-5 text-emerald-600 shrink-0" />
                      <div>
                        <p className="font-bold">{t.cleanerPortal.signedStatus}</p>
                        <p className="text-emerald-700">Signed by: {cleanerProfile.signatureName}</p>
                      </div>
                    </div>
                  ) : (
                    <div className="space-y-3 pt-2">
                      <p className="text-xs text-slate-500">{t.cleanerPortal.agreementNotice}</p>
                      <div className="flex flex-col sm:flex-row gap-3">
                        <input 
                          type="text"
                          placeholder={t.cleanerPortal.signContract}
                          value={cleanerProfile.signatureName}
                          onChange={(e) => setCleanerProfile({ ...cleanerProfile, signatureName: e.target.value })}
                          className="flex-1 border border-slate-300 rounded-lg p-2 text-sm focus:ring-2 focus:ring-emerald-500 outline-none"
                        />
                        <button
                          disabled={!cleanerProfile.signatureName.trim()}
                          onClick={() => setCleanerProfile({ ...cleanerProfile, isSigned: true })}
                          className="bg-emerald-600 hover:bg-emerald-500 disabled:opacity-50 text-white text-xs font-bold px-5 py-2.5 rounded-lg transition shrink-0"
                        >
                          {t.cleanerPortal.signBtn}
                        </button>
                      </div>
                    </div>
                  )}
                </div>

                {/* Available Jobs Feed */}
                <div className="bg-white p-6 rounded-xl border border-slate-200 shadow-sm space-y-4">
                  <h3 className="font-bold text-slate-900 text-base flex items-center space-x-2">
                    <Briefcase className="w-5 h-5 text-emerald-600" />
                    <span>{t.cleanerPortal.availableJobs}</span>
                  </h3>

                  <div className="space-y-3">
                    {cleanerProfile.availableJobsList.length > 0 ? (
                      cleanerProfile.availableJobsList.map(job => (
                        <div key={job.id} className="p-4 rounded-lg border border-slate-200 bg-slate-50 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-3">
                          <div>
                            <span className="bg-teal-100 text-teal-800 text-[10px] font-bold px-2 py-0.5 rounded">
                              {job.area}
                            </span>
                            <h4 className="font-bold text-slate-900 text-sm mt-1">{job.desc}</h4>
                            <p className="text-xs text-slate-500">📅 {job.date}</p>
                          </div>
                          <div className="flex items-center space-x-3 w-full sm:w-auto justify-end">
                            <span className="text-lg font-extrabold text-emerald-700">{job.payout}</span>
                            <button 
                              onClick={() => {
                                setCleanerProfile({
                                  ...cleanerProfile,
                                  availableJobsList: cleanerProfile.availableJobsList.filter(j => j.id !== job.id)
                                });
                              }}
                              className="bg-emerald-600 hover:bg-emerald-500 text-white text-xs font-bold px-3 py-1.5 rounded transition"
                            >
                              {t.cleanerPortal.acceptJob}
                            </button>
                          </div>
                        </div>
                      ))
                    ) : (
                      <p className="text-xs text-slate-500 italic">No more open requests in your area right now.</p>
                    )}
                  </div>
                </div>

              </div>

            </div>
          </div>
        )}

        {/* ==================== TAB 3: ANTI-DISCRIMINATION LEGAL POLICY & INCIDENT REPORT ==================== */}
        {activeTab === 'policy' && (
          <div className="space-y-8 max-w-4xl mx-auto">
            <div className="border-b border-slate-200 pb-4 text-center">
              <span className="bg-emerald-100 text-emerald-800 text-xs font-bold px-3 py-1 rounded-full inline-block mb-2">
                Official Statute • Squadron Service LLC
              </span>
              <h2 className="text-2xl sm:text-3xl font-extrabold text-slate-900">{t.policy.title}</h2>
              <p className="text-slate-600 text-sm mt-1">{t.policy.subtitle}</p>
            </div>

            {/* Core Legal Rules */}
            <div className="bg-white p-6 sm:p-8 rounded-xl border border-slate-200 shadow-sm space-y-6">
              <div className="bg-amber-50 border-l-4 border-amber-500 p-4 rounded-r-lg text-amber-900 text-xs sm:text-sm leading-relaxed">
                <p className="font-bold mb-1 flex items-center">
                  <AlertTriangle className="w-4 h-4 mr-1 text-amber-600" />
                  Zero-Tolerance Mandatory Directive
                </p>
                {t.policy.intro}
              </div>

              <h3 className="text-lg font-bold text-slate-900 border-b border-slate-100 pb-2">
                {t.policy.rulesTitle}
              </h3>

              <div className="grid grid-cols-1 md:grid-cols-2 gap-4 text-xs sm:text-sm">
                <div className="p-4 rounded-lg bg-slate-50 border border-slate-200 space-y-2">
                  <div className="flex items-center space-x-2 text-emerald-700 font-bold">
                    <Shield className="w-4 h-4" />
                    <span>1. Racial & Ethnic Protections</span>
                  </div>
                  <p className="text-slate-600 leading-relaxed">{t.policy.rule1}</p>
                </div>

                <div className="p-4 rounded-lg bg-slate-50 border border-slate-200 space-y-2">
                  <div className="flex items-center space-x-2 text-emerald-700 font-bold">
                    <Globe className="w-4 h-4" />
                    <span>2. Linguistic Autonomy</span>
                  </div>
                  <p className="text-slate-600 leading-relaxed">{t.policy.rule2}</p>
                </div>

                <div className="p-4 rounded-lg bg-slate-50 border border-slate-200 space-y-2">
                  <div className="flex items-center space-x-2 text-emerald-700 font-bold">
                    <User className="w-4 h-4" />
                    <span>3. Client Code & Bans</span>
                  </div>
                  <p className="text-slate-600 leading-relaxed">{t.policy.rule3}</p>
                </div>

                <div className="p-4 rounded-lg bg-slate-50 border border-slate-200 space-y-2">
                  <div className="flex items-center space-x-2 text-emerald-700 font-bold">
                    <CheckCircle className="w-4 h-4" />
                    <span>4. Contractor Safety & Exit Right</span>
                  </div>
                  <p className="text-slate-600 leading-relaxed">{t.policy.rule4}</p>
                </div>
              </div>
            </div>

            {/* Confidential Incident Reporting Form */}
            <div className="bg-slate-900 text-white p-6 sm:p-8 rounded-xl shadow-xl space-y-6">
              <div>
                <h3 className="text-xl font-bold text-white flex items-center space-x-2">
                  <AlertTriangle className="w-5 h-5 text-amber-400" />
                  <span>{t.policy.reportTitle}</span>
                </h3>
                <p className="text-xs text-slate-400 mt-1">{t.policy.reportDesc}</p>
              </div>

              {report.submitted ? (
                <div className="bg-emerald-950 border border-emerald-500/50 p-6 rounded-lg text-center space-y-2">
                  <CheckCircle className="w-10 h-10 text-emerald-400 mx-auto" />
                  <p className="text-emerald-200 text-sm font-bold">
                    {t.policy.submittedMsg}{report.reportId}
                  </p>
                  <p className="text-slate-400 text-xs">Our legal & ethics team investigates all reports within 24 hours.</p>
                </div>
              ) : (
                <form onSubmit={handleReportSubmit} className="space-y-4 text-xs sm:text-sm">
                  <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div>
                      <label className="block text-slate-300 mb-1">{t.policy.formName}</label>
                      <input 
                        type="text"
                        placeholder="Anonymous or Name"
                        value={report.name}
                        onChange={(e) => setReport({ ...report, name: e.target.value })}
                        className="w-full bg-slate-800 border border-slate-700 rounded-lg p-2.5 text-white focus:ring-2 focus:ring-emerald-500 outline-none"
                      />
                    </div>
                    <div>
                      <label className="block text-slate-300 mb-1">{t.policy.formRole}</label>
                      <select 
                        value={report.role}
                        onChange={(e) => setReport({ ...report, role: e.target.value })}
                        className="w-full bg-slate-800 border border-slate-700 rounded-lg p-2.5 text-white focus:ring-2 focus:ring-emerald-500 outline-none"
                      >
                        <option value="roleCleaner">{t.policy.roleCleaner}</option>
                        <option value="roleClient">{t.policy.roleClient}</option>
                      </select>
                    </div>
                  </div>

                  <div>
                    <label className="block text-slate-300 mb-1">{t.policy.formDetails}</label>
                    <textarea 
                      required
                      rows="4"
                      placeholder="Please detail the event, location, date, or language discrimination experienced..."
                      value={report.details}
                      onChange={(e) => setReport({ ...report, details: e.target.value })}
                      className="w-full bg-slate-800 border border-slate-700 rounded-lg p-2.5 text-white focus:ring-2 focus:ring-emerald-500 outline-none"
                    ></textarea>
                  </div>

                  <button 
                    type="submit"
                    className="w-full bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold py-3 rounded-lg transition shadow-lg text-sm"
                  >
                    {t.policy.formSubmit}
                  </button>
                </form>
              )}
            </div>
          </div>
        )}

        {/* ==================== TAB 4: GEMINI AI ROOM ASSISTANT ==================== */}
        {activeTab === 'aiAssistant' && (
          <div className="space-y-8 max-w-4xl mx-auto">
            <div className="border-b border-slate-200 pb-4 text-center">
              <span className="bg-amber-100 text-amber-900 text-xs font-bold px-3 py-1 rounded-full inline-flex items-center space-x-1 mb-2">
                <Sparkles className="w-3.5 h-3.5 text-amber-600 mr-1" />
                Gemini Flash Vision AI Powered
              </span>
              <h2 className="text-2xl sm:text-3xl font-extrabold text-slate-900">{t.aiAssistant.title}</h2>
              <p className="text-slate-600 text-sm mt-1 max-w-2xl mx-auto">{t.aiAssistant.subtitle}</p>
            </div>

            <div className="grid grid-cols-1 md:grid-cols-2 gap-8 items-start">
              
              {/* Image Upload Box */}
              <div className="bg-white p-6 rounded-xl border border-slate-200 shadow-sm space-y-4">
                <h3 className="font-bold text-slate-900 text-sm flex items-center space-x-2">
                  <Camera className="w-4 h-4 text-emerald-600" />
                  <span>Upload Room Photo</span>
                </h3>

                <div className="border-2 border-dashed border-slate-300 rounded-xl p-6 text-center hover:border-emerald-500 transition relative bg-slate-50">
                  {aiState.imagePreview ? (
                    <div className="space-y-3">
                      <img 
                        src={aiState.imagePreview} 
                        alt="Room Preview" 
                        className="max-h-64 mx-auto rounded-lg object-cover shadow"
                      />
                      <button 
                        onClick={() => setAiState({ ...aiState, imageFile: null, imagePreview: null, analysisResult: null })}
                        className="text-xs text-rose-600 hover:underline font-semibold"
                      >
                        Remove Photo
                      </button>
                    </div>
                  ) : (
                    <label className="cursor-pointer flex flex-col items-center justify-center space-y-2">
                      <Upload className="w-8 h-8 text-slate-400" />
                      <span className="text-xs text-slate-600 font-medium">{t.aiAssistant.uploadPrompt}</span>
                      <span className="text-[10px] text-slate-400">Supports JPG, PNG (Bunk beds, hardwood, living areas)</span>
                      <input 
                        type="file" 
                        accept="image/*" 
                        onChange={handleImageUpload} 
                        className="hidden" 
                      />
                    </label>
                  )}
                </div>

                <button
                  disabled={!aiState.imagePreview || aiState.analyzing}
                  onClick={runGeminiVisionAnalysis}
                  className="w-full bg-emerald-600 hover:bg-emerald-500 disabled:opacity-50 text-white font-bold py-3 rounded-lg transition text-xs sm:text-sm shadow flex items-center justify-center space-x-2"
                >
                  <Sparkles className="w-4 h-4 text-amber-300" />
                  <span>{aiState.analyzing ? t.aiAssistant.analyzing : t.aiAssistant.analyzeBtn}</span>
                </button>
              </div>

              {/* Analysis Results Display */}
              <div className="bg-slate-900 text-white p-6 rounded-xl shadow-xl border border-slate-800 space-y-4 min-h-[320px]">
                <h3 className="font-bold text-white text-base border-b border-slate-800 pb-3 flex items-center justify-between">
                  <span>{t.aiAssistant.resultsTitle}</span>
                  <Sparkles className="w-4 h-4 text-amber-400" />
                </h3>

                {aiState.analyzing ? (
                  <div className="py-12 text-center space-y-3">
                    <div className="w-8 h-8 border-4 border-emerald-500 border-t-transparent rounded-full animate-spin mx-auto"></div>
                    <p className="text-xs text-slate-400">{t.aiAssistant.analyzing}</p>
                  </div>
                ) : aiState.analysisResult ? (
                  <div className="text-xs sm:text-sm text-slate-200 whitespace-pre-line leading-relaxed bg-slate-800/80 p-4 rounded-lg border border-slate-700">
                    {aiState.analysisResult}
                  </div>
                ) : (
                  <div className="py-12 text-center text-xs text-slate-500 space-y-2">
                    <Info className="w-8 h-8 mx-auto text-slate-600" />
                    <p>Upload a room photo to see instant time estimates, floor care recommendations, and product advice.</p>
                  </div>
                )}
              </div>

            </div>
          </div>
        )}

      </main>

      {/* --- FOOTER & LEGAL DISCLAIMER --- */}
      <footer className="bg-slate-950 text-slate-400 border-t border-slate-800 py-8 px-4 sm:px-6 lg:px-8 text-xs">
        <div className="max-w-7xl mx-auto flex flex-col md:flex-row items-center justify-between gap-4">
          <div>
            <span className="font-bold text-white text-sm">Squadron Service LLC</span>
            <p className="mt-1 text-slate-500">{t.footer.rights}</p>
            <p className="text-slate-500">{t.footer.legalStatute}</p>
          </div>

          <div className="flex flex-wrap items-center gap-4 text-slate-400">
            <button onClick={() => setActiveTab('policy')} className="hover:text-emerald-400 transition underline">
              {t.nav.antiDiscrimination}
            </button>
            <button onClick={() => setActiveTab('cleanerPortal')} className="hover:text-emerald-400 transition underline">
              {t.nav.cleanerPortal}
            </button>
            <span className="text-slate-600">|</span>
            <span>{t.footer.contact}</span>
          </div>
        </div>
      </footer>

    </div>
  );
}
