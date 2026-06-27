import { useState } from "react";
import {
  Home, BookOpen, MessageSquare, Calendar, User, Sun, Moon, Search, Bell,
  ChevronRight, ChevronDown, ArrowLeft, ClipboardList, CheckCircle2, XCircle,
  Clock, FileCheck, RotateCcw, GraduationCap, TrendingUp, Users, Plus, X,
  Check, FileText, Paperclip, Mic, Smile, Send,
} from "lucide-react";

/* ----------------------------- Theme tokens ----------------------------- */
const LIGHT = {
  page: "bg-slate-50 text-slate-900",
  panel: "bg-white",
  border: "border-slate-200",
  divide: "divide-slate-200",
  muted: "text-slate-500",
  hover: "hover:bg-slate-100",
  chip: "bg-slate-100",
  input: "bg-slate-100 border-slate-200 placeholder-slate-400 text-slate-900",
  railBg: "bg-white",
};
const DARK = {
  page: "bg-slate-950 text-slate-100",
  panel: "bg-slate-900",
  border: "border-slate-800",
  divide: "divide-slate-800",
  muted: "text-slate-400",
  hover: "hover:bg-slate-800",
  chip: "bg-slate-800",
  input: "bg-slate-800 border-slate-700 placeholder-slate-500 text-slate-100",
  railBg: "bg-slate-900",
};

const HUE = {
  sky: { dotL: "bg-sky-500", chipL: "bg-sky-100 text-sky-700", chipD: "bg-sky-500/15 text-sky-300", barTop: "bg-sky-500" },
  rose: { dotL: "bg-rose-500", chipL: "bg-rose-100 text-rose-700", chipD: "bg-rose-500/15 text-rose-300", barTop: "bg-rose-500" },
  emerald: { dotL: "bg-emerald-500", chipL: "bg-emerald-100 text-emerald-700", chipD: "bg-emerald-500/15 text-emerald-300", barTop: "bg-emerald-500" },
  violet: { dotL: "bg-violet-500", chipL: "bg-violet-100 text-violet-700", chipD: "bg-violet-500/15 text-violet-300", barTop: "bg-violet-500" },
};

const ESTADOS = {
  pendiente: { label: "Pendiente", chipL: "bg-amber-100 text-amber-700", chipD: "bg-amber-500/15 text-amber-300", Icon: Clock },
  entregado: { label: "Entregado", chipL: "bg-sky-100 text-sky-700", chipD: "bg-sky-500/15 text-sky-300", Icon: CheckCircle2 },
  calificado: { label: "Calificado", chipL: "bg-emerald-100 text-emerald-700", chipD: "bg-emerald-500/15 text-emerald-300", Icon: GraduationCap },
  rehacer: { label: "A rehacer", chipL: "bg-rose-100 text-rose-700", chipD: "bg-rose-500/15 text-rose-300", Icon: RotateCcw },
  vencido: { label: "Vencido", chipL: "bg-slate-200 text-slate-600", chipD: "bg-slate-700 text-slate-300", Icon: XCircle },
};

/* ------------------------------- Mock data ------------------------------- */
const MATERIAS = [
  { id: "mat1", nombre: "Matemática", docente: "Prof. Liliana Gómez", curso: "4° Año B", hue: "sky" },
  { id: "mat2", nombre: "Historia", docente: "Prof. Martín Ríos", curso: "4° Año B", hue: "rose" },
  { id: "mat3", nombre: "Biología", docente: "Prof. Carla Núñez", curso: "4° Año B", hue: "emerald" },
  { id: "mat4", nombre: "Inglés", docente: "Prof. Diego Salas", curso: "4° Año B", hue: "violet" },
];

const TRABAJOS_INIT = {
  mat1: [
    { id: "t1", nombre: "Práctica de ecuaciones cuadráticas", descripcion: "Resolver los ejercicios 1 a 12 de la guía.", publicado: "2026-06-10", limite: "2026-06-28", estado: "pendiente" },
    { id: "t2", nombre: "Trabajo integrador: funciones", descripcion: "Análisis de funciones lineales y cuadráticas aplicado a un caso real.", publicado: "2026-06-01", limite: "2026-06-15", estado: "calificado", nota: 8, comentario: "Buen desarrollo, revisar el punto 3.", archivosEntrega: ["integrador_funciones.pdf"], archivosDocente: ["consigna.pdf"] },
    { id: "t3", nombre: "Guía de repaso trimestral", descripcion: "Repaso general para el examen.", publicado: "2026-05-20", limite: "2026-06-05", estado: "rehacer", comentario: "Falta justificar el desarrollo de los ejercicios 4 y 7." },
    { id: "t4", nombre: "Cuestionario de probabilidad", descripcion: "Responder el cuestionario adjunto.", publicado: "2026-05-15", limite: "2026-05-30", estado: "vencido" },
    { id: "t5", nombre: "Ejercicios de geometría", descripcion: "Calcular área y perímetro de las figuras dadas.", publicado: "2026-06-18", limite: "2026-06-25", estado: "entregado", archivosEntrega: ["geometria_resuelto.pdf"] },
  ],
  mat2: [
    { id: "h1", nombre: "Ensayo: Revolución de Mayo", descripcion: "Escribir un ensayo de 2 páginas sobre las causas de la Revolución de Mayo.", publicado: "2026-06-12", limite: "2026-06-30", estado: "pendiente" },
    { id: "h2", nombre: "Línea de tiempo colonial", descripcion: "Armar una línea de tiempo con los hechos principales del período colonial.", publicado: "2026-05-25", limite: "2026-06-10", estado: "calificado", nota: 9, comentario: "Excelente nivel de detalle.", archivosEntrega: ["linea_tiempo.pdf"] },
  ],
  mat3: [
    { id: "b1", nombre: "Informe de laboratorio: célula", descripcion: "Informe sobre la observación de células al microscopio.", publicado: "2026-06-15", limite: "2026-06-27", estado: "pendiente" },
    { id: "b2", nombre: "Cuestionario: sistema digestivo", descripcion: "Responder el cuestionario sobre el sistema digestivo.", publicado: "2026-06-01", limite: "2026-06-14", estado: "rehacer", comentario: "Revisar las preguntas 3 y 5, faltan referencias." },
  ],
  mat4: [
    { id: "i1", nombre: "Reading comprehension: Unit 5", descripcion: "Leer el texto y responder las preguntas de comprensión.", publicado: "2026-06-20", limite: "2026-07-01", estado: "pendiente" },
    { id: "i2", nombre: "Speaking practice video", descripcion: "Grabar un video de 2 minutos presentándote en inglés.", publicado: "2026-06-05", limite: "2026-06-18", estado: "entregado", archivosEntrega: ["speaking_practice.mp4"] },
  ],
};

const ASISTENCIA_DATA = {
  mat1: { total: 42, presentes: 38, ausentes: 2, tardanzas: 1, justificadas: 1, registros: [
    { fecha: "2026-06-24", estado: "presente" },
    { fecha: "2026-06-22", estado: "presente" },
    { fecha: "2026-06-19", estado: "tardanza", comentario: "Llegó 10 minutos tarde" },
    { fecha: "2026-06-17", estado: "ausente" },
    { fecha: "2026-06-15", estado: "justificada", comentario: "Certificado médico presentado" },
  ]},
  mat2: { total: 40, presentes: 37, ausentes: 1, tardanzas: 2, justificadas: 0, registros: [
    { fecha: "2026-06-23", estado: "presente" }, { fecha: "2026-06-16", estado: "tardanza" }, { fecha: "2026-06-09", estado: "presente" },
  ]},
  mat3: { total: 38, presentes: 35, ausentes: 2, tardanzas: 0, justificadas: 1, registros: [
    { fecha: "2026-06-25", estado: "presente" }, { fecha: "2026-06-18", estado: "ausente" }, { fecha: "2026-06-11", estado: "justificada", comentario: "Aviso previo de la familia" },
  ]},
  mat4: { total: 36, presentes: 34, ausentes: 1, tardanzas: 1, justificadas: 0, registros: [
    { fecha: "2026-06-24", estado: "presente" }, { fecha: "2026-06-17", estado: "tardanza" },
  ]},
};

const MENSAJES_INIT = {
  mat1: [
    { id: 1, autor: "docente", texto: "Hola! Les recuerdo que el viernes vence la entrega del integrador de funciones.", hora: "09:14" },
    { id: 2, autor: "yo", texto: "Gracias profe, ¿podemos entregarlo en grupo?", hora: "09:20" },
    { id: 3, autor: "docente", texto: "Sí, pueden formar grupos de hasta 3 personas desde la pestaña Grupos.", hora: "09:22" },
  ],
  mat2: [{ id: 1, autor: "docente", texto: "Subí material extra sobre la Revolución de Mayo a los archivos del trabajo.", hora: "14:02" }],
  mat3: [
    { id: 1, autor: "yo", texto: "Profe, ¿el informe de laboratorio es individual?", hora: "11:40" },
    { id: 2, autor: "docente", texto: "Sí, individual esta vez.", hora: "12:05" },
  ],
  mat4: [{ id: 1, autor: "docente", texto: "Great job on the speaking practice videos, everyone!", hora: "16:30" }],
};

const GRUPOS_INIT = {
  mat1: [
    { id: "g1", nombre: "Grupo Integrador Funciones", integrantes: ["Vos", "Bruno Ledesma", "Sofía Paz"], estado: "aprobado", expira: "2026-06-29" },
    { id: "g2", nombre: "Grupo repaso trimestral", integrantes: ["Vos", "Ariana Vega"], estado: "pendiente" },
  ],
  mat2: [],
  mat3: [{ id: "g3", nombre: "Grupo informe de laboratorio", integrantes: ["Vos", "Tomás Funes"], estado: "rechazado" }],
  mat4: [],
};

const COMPANEROS = {
  mat1: ["Bruno Ledesma", "Sofía Paz", "Ariana Vega", "Tomás Funes", "Lucía Bianchi", "Nicolás Soto"],
  mat2: ["Bruno Ledesma", "Sofía Paz", "Ariana Vega", "Tomás Funes"],
  mat3: ["Lucía Bianchi", "Nicolás Soto", "Sofía Paz"],
  mat4: ["Bruno Ledesma", "Tomás Funes", "Ariana Vega"],
};

const PENDIENTES_DOCENTE = [
  { alumno: "Bruno Ledesma", trabajo: "Práctica de ecuaciones cuadráticas", materia: "Matemática", materiaId: "mat1", fecha: "2026-06-24" },
  { alumno: "Sofía Paz", trabajo: "Práctica de ecuaciones cuadráticas", materia: "Matemática", materiaId: "mat1", fecha: "2026-06-25" },
  { alumno: "Lucía Bianchi", trabajo: "Informe de laboratorio: célula", materia: "Biología", materiaId: "mat3", fecha: "2026-06-25" },
];

const ROSTER = ["Bruno Ledesma", "Sofía Paz", "Ariana Vega", "Tomás Funes", "Lucía Bianchi", "Nicolás Soto"];

/* -------------------------------- Helpers -------------------------------- */
function formatFecha(iso) {
  const d = new Date(iso + "T00:00:00");
  return d.toLocaleDateString("es-AR", { day: "2-digit", month: "short" });
}
function nowStr() {
  return new Date().toLocaleTimeString("es-AR", { hour: "2-digit", minute: "2-digit" });
}
function headerTitle(page, materia, role) {
  if (page === "materias" && materia) return materia.nombre;
  const map = { inicio: "Inicio", materias: role === "estudiante" ? "Materias" : "Cursos", mensajes: "Mensajes", calendario: "Calendario", perfil: "Perfil" };
  return map[page] || "Aula Connect";
}
function headerSubtitle(page, role) {
  if (page === "inicio") return role === "estudiante" ? "Tu actividad de hoy" : "Panel docente";
  return null;
}

/* ------------------------------ Sub-components ---------------------------- */
function Rail({ page, setPage, dark, setDark, role, setRole, T }) {
  const items = [
    { key: "inicio", label: "Inicio", Icon: Home },
    { key: "materias", label: "Materias", Icon: BookOpen },
    { key: "mensajes", label: "Mensajes", Icon: MessageSquare },
    { key: "calendario", label: "Calendario", Icon: Calendar },
    { key: "perfil", label: "Perfil", Icon: User },
  ];
  return (
    <aside className={`w-20 sm:w-24 flex flex-col items-center py-5 gap-1 border-r ${T.border} ${T.railBg} shrink-0`}>
      <div className="mb-4 flex flex-col items-center gap-1">
        <div className="w-10 h-10 rounded-xl bg-teal-600 flex items-center justify-center text-white font-bold text-lg">A</div>
        <span className="text-xs font-semibold tracking-wide text-teal-600">AULA</span>
      </div>
      <nav className="flex-1 flex flex-col gap-1 w-full px-2">
        {items.map(({ key, label, Icon }) => {
          const active = page === key;
          return (
            <button key={key} onClick={() => setPage(key)}
              className={`flex flex-col items-center gap-1 py-2.5 rounded-xl text-xs font-medium transition-colors ${active ? "bg-teal-600 text-white" : `${T.muted} ${T.hover}`}`}>
              <Icon size={18} />
              {label}
            </button>
          );
        })}
      </nav>
      <div className="w-full px-2 flex flex-col gap-2 mt-2">
        <button onClick={() => setRole((r) => (r === "estudiante" ? "docente" : "estudiante"))}
          className={`text-xs font-semibold py-2 rounded-lg border ${T.border} ${T.hover}`}>
          {role === "estudiante" ? "Alumno" : "Docente"}
        </button>
        <button onClick={() => setDark((d) => !d)} className={`flex items-center justify-center py-2 rounded-lg border ${T.border} ${T.hover}`}>
          {dark ? <Sun size={16} /> : <Moon size={16} />}
        </button>
      </div>
    </aside>
  );
}

function Topbar({ T, title, subtitle }) {
  return (
    <header className={`flex items-center justify-between gap-4 px-6 py-4 border-b ${T.border}`}>
      <div>
        <h1 className="text-xl font-extrabold tracking-tight">{title}</h1>
        {subtitle && <p className={`text-sm ${T.muted}`}>{subtitle}</p>}
      </div>
      <div className="flex items-center gap-3">
        <div className={`hidden md:flex items-center gap-2 px-3 py-2 rounded-lg border ${T.border} ${T.chip} text-sm ${T.muted}`}>
          <Search size={15} />
          <span>Buscar materias, trabajos, personas...</span>
        </div>
        <button className={`p-2 rounded-lg border ${T.border} ${T.hover}`}>
          <Bell size={17} />
        </button>
      </div>
    </header>
  );
}

function NotebookEdge({ T }) {
  return (
    <div className={`flex gap-2.5 px-6 py-2 border-b ${T.border} overflow-hidden`}>
      {Array.from({ length: 22 }).map((_, i) => (
        <span key={i} className={`w-1.5 h-1.5 rounded-full shrink-0 ${T.chip}`} />
      ))}
    </div>
  );
}

function StatCard({ label, value, hint, Icon, accent, T }) {
  return (
    <div className={`rounded-2xl border ${T.border} ${T.panel} p-4 flex items-start gap-3`}>
      <div className={`w-10 h-10 rounded-xl flex items-center justify-center ${accent} shrink-0`}>
        <Icon size={18} className="text-white" />
      </div>
      <div className="min-w-0">
        <p className="text-2xl font-mono font-bold leading-none">{value}</p>
        <p className="text-sm font-medium mt-1">{label}</p>
        {hint && <p className={`text-xs ${T.muted} mt-0.5`}>{hint}</p>}
      </div>
    </div>
  );
}

function MateriaCard({ materia, onClick, T }) {
  const hue = HUE[materia.hue];
  return (
    <button onClick={onClick} className={`text-left rounded-2xl border ${T.border} ${T.panel} p-5 hover:-translate-y-0.5 hover:shadow-md transition-all`}>
      <div className={`h-1.5 w-10 rounded-full ${hue.barTop} mb-4`} />
      <h3 className="font-bold text-lg">{materia.nombre}</h3>
      <p className={`text-sm ${T.muted} mt-1`}>{materia.docente}</p>
      <p className={`text-xs ${T.muted} mt-3`}>{materia.curso}</p>
    </button>
  );
}

function ChatView({ title, subtitle, messages, draft, setDraft, onSend, T }) {
  return (
    <div className={`rounded-2xl border ${T.border} ${T.panel} flex flex-col`} style={{ height: "420px" }}>
      <div className={`flex items-center gap-3 px-5 py-3 border-b ${T.border}`}>
        <div className="w-9 h-9 rounded-full bg-teal-600 text-white flex items-center justify-center font-bold text-sm">{title ? title[0] : "?"}</div>
        <div>
          <p className="font-semibold text-sm">{title}</p>
          {subtitle && <p className={`text-xs ${T.muted}`}>{subtitle}</p>}
        </div>
      </div>
      <div className="flex-1 overflow-y-auto px-5 py-4 space-y-3">
        {messages.map((m) => (
          <div key={m.id} className={`flex ${m.autor === "yo" ? "justify-end" : "justify-start"}`}>
            <div className={`rounded-2xl px-4 py-2.5 text-sm ${m.autor === "yo" ? "bg-teal-600 text-white" : T.chip}`} style={{ maxWidth: "75%" }}>
              <p>{m.texto}</p>
              <p className={`text-xs mt-1 ${m.autor === "yo" ? "text-teal-100" : T.muted}`}>{m.hora}</p>
            </div>
          </div>
        ))}
        {messages.length === 0 && <p className={`text-sm ${T.muted} text-center py-8`}>Todavía no hay mensajes.</p>}
      </div>
      <div className={`flex items-center gap-2 px-4 py-3 border-t ${T.border}`}>
        <button className={`p-2 rounded-lg ${T.hover}`}><Paperclip size={16} /></button>
        <button className={`p-2 rounded-lg ${T.hover}`}><Mic size={16} /></button>
        <input value={draft} onChange={(e) => setDraft(e.target.value)} onKeyDown={(e) => { if (e.key === "Enter") onSend(); }}
          placeholder="Escribí un mensaje..." className={`flex-1 rounded-lg px-3 py-2 text-sm border outline-none ${T.input}`} />
        <button className={`p-2 rounded-lg ${T.hover}`}><Smile size={16} /></button>
        <button onClick={onSend} className="p-2 rounded-lg bg-teal-600 text-white"><Send size={16} /></button>
      </div>
    </div>
  );
}

function TrabajosTab({ trabajos, role, dark, filter, setFilter, expanded, setExpanded, T }) {
  const filters = ["todos", "pendiente", "entregado", "rehacer", "vencido", "calificado"];
  const filterLabels = { todos: "Todos", pendiente: "Pendientes", entregado: "Entregados", rehacer: "Rehacer", vencido: "Sin entregar", calificado: "Calificados" };
  const filtered = filter === "todos" ? trabajos : trabajos.filter((t) => t.estado === filter);
  return (
    <div className="space-y-4">
      <div className="flex items-center justify-between flex-wrap gap-3">
        <div className="flex gap-2 flex-wrap">
          {filters.map((f) => (
            <button key={f} onClick={() => setFilter(f)}
              className={`px-3 py-1.5 rounded-full text-xs font-semibold border ${T.border} ${filter === f ? "bg-teal-600 text-white border-teal-600" : T.hover}`}>
              {filterLabels[f]}
            </button>
          ))}
        </div>
        {role === "docente" && (
          <button className="flex items-center gap-1.5 text-sm font-semibold px-3 py-2 rounded-lg bg-teal-600 text-white">
            <Plus size={15} /> Crear trabajo
          </button>
        )}
      </div>
      <div className="space-y-3">
        {filtered.length === 0 && <p className={`text-sm ${T.muted} py-8 text-center`}>No hay trabajos en este filtro.</p>}
        {filtered.map((t) => {
          const est = ESTADOS[t.estado];
          const isOpen = expanded === t.id;
          return (
            <div key={t.id} className={`rounded-2xl border ${T.border} ${T.panel} overflow-hidden`}>
              <button onClick={() => setExpanded(isOpen ? null : t.id)} className={`w-full flex items-center justify-between gap-3 px-5 py-4 text-left ${T.hover}`}>
                <div className="flex items-center gap-3 min-w-0">
                  <est.Icon size={17} className="shrink-0" />
                  <div className="min-w-0">
                    <p className="font-semibold truncate">{t.nombre}</p>
                    <p className={`text-xs ${T.muted}`}>Vence el {formatFecha(t.limite)}</p>
                  </div>
                </div>
                <div className="flex items-center gap-3 shrink-0">
                  <span className={`text-xs font-semibold px-2.5 py-1 rounded-full ${dark ? est.chipD : est.chipL}`}>{est.label}</span>
                  <ChevronDown size={16} className={`transition-transform ${isOpen ? "rotate-180" : ""}`} />
                </div>
              </button>
              {isOpen && (
                <div className={`px-5 pb-5 pt-1 border-t ${T.border} space-y-3`}>
                  <p className={`text-sm ${T.muted}`}>{t.descripcion}</p>
                  <div className="flex flex-wrap gap-4 text-xs">
                    <span className={T.muted}>Publicado: {formatFecha(t.publicado)}</span>
                    {t.nota != null && <span className="font-mono font-bold text-emerald-600">Nota: {t.nota}/10</span>}
                  </div>
                  {t.comentario && <div className={`rounded-xl px-4 py-3 text-sm ${T.chip}`}>💬 {t.comentario}</div>}
                  <div className="flex flex-wrap gap-2">
                    {(t.archivosDocente || []).map((a, i) => (
                      <span key={"d" + i} className={`flex items-center gap-1.5 text-xs px-3 py-1.5 rounded-lg border ${T.border}`}><FileText size={13} /> {a}</span>
                    ))}
                    {(t.archivosEntrega || []).map((a, i) => (
                      <span key={"e" + i} className={`flex items-center gap-1.5 text-xs px-3 py-1.5 rounded-lg border ${T.border} text-teal-600`}><FileText size={13} /> {a}</span>
                    ))}
                  </div>
                  <div className="flex gap-2 pt-1">
                    {role === "estudiante" && ["pendiente", "rehacer", "vencido"].includes(t.estado) && (
                      <button className="text-sm font-semibold px-4 py-2 rounded-lg bg-teal-600 text-white">Subir entrega</button>
                    )}
                    {role === "docente" && (
                      <>
                        <button className="text-sm font-semibold px-4 py-2 rounded-lg bg-teal-600 text-white">Corregir entrega</button>
                        <button className={`text-sm font-semibold px-4 py-2 rounded-lg border ${T.border}`}>Solicitar rehacer</button>
                      </>
                    )}
                  </div>
                </div>
              )}
            </div>
          );
        })}
      </div>
    </div>
  );
}

function AsistenciaTab({ data, T, role, roster, rosterState, setRosterState }) {
  if (!data) return <p className={T.muted}>Sin datos de asistencia.</p>;
  const pct = Math.round(((data.presentes + data.justificadas) / data.total) * 100);
  const cfgMap = {
    presente: { Icon: CheckCircle2, color: "text-emerald-600", label: "Presente" },
    ausente: { Icon: XCircle, color: "text-rose-600", label: "Ausente" },
    tardanza: { Icon: Clock, color: "text-amber-600", label: "Tardanza" },
    justificada: { Icon: FileCheck, color: "text-sky-600", label: "Justificada" },
  };
  const markOpts = [
    { key: "presente", Icon: CheckCircle2, color: "emerald" },
    { key: "ausente", Icon: XCircle, color: "rose" },
    { key: "tardanza", Icon: Clock, color: "amber" },
    { key: "justificada", Icon: FileCheck, color: "sky" },
  ];
  return (
    <div className="space-y-5">
      {role === "docente" && (
        <div className={`rounded-2xl border ${T.border} ${T.panel} p-5 space-y-3`}>
          <p className="font-semibold">Tomar asistencia de hoy</p>
          <div className="space-y-2">
            {roster.map((name) => {
              const current = rosterState[name] || null;
              return (
                <div key={name} className="flex items-center justify-between gap-3">
                  <span className="text-sm font-medium">{name}</span>
                  <div className="flex gap-1.5">
                    {markOpts.map((o) => (
                      <button key={o.key} onClick={() => setRosterState((prev) => ({ ...prev, [name]: o.key }))}
                        className={`p-1.5 rounded-lg border ${T.border} ${current === o.key ? `bg-${o.color}-500 text-white border-${o.color}-500` : T.hover}`}>
                        <o.Icon size={14} />
                      </button>
                    ))}
                  </div>
                </div>
              );
            })}
          </div>
        </div>
      )}
      <div className="grid grid-cols-2 sm:grid-cols-5 gap-3">
        <StatCard label="Clases totales" value={data.total} Icon={Calendar} accent="bg-slate-500" T={T} />
        <StatCard label="Presentes" value={data.presentes} Icon={CheckCircle2} accent="bg-emerald-500" T={T} />
        <StatCard label="Ausentes" value={data.ausentes} Icon={XCircle} accent="bg-rose-500" T={T} />
        <StatCard label="Tardanzas" value={data.tardanzas} Icon={Clock} accent="bg-amber-500" T={T} />
        <StatCard label="Justificadas" value={data.justificadas} Icon={FileCheck} accent="bg-sky-500" T={T} />
      </div>
      <div className={`rounded-2xl border ${T.border} ${T.panel} p-5`}>
        <div className="flex items-center justify-between mb-2">
          <p className="font-semibold">Asistencia general</p>
          <p className="font-mono font-bold text-lg">{pct}%</p>
        </div>
        <div className={`h-2.5 rounded-full ${T.chip} overflow-hidden`}>
          <div className="h-full bg-emerald-500 rounded-full" style={{ width: pct + "%" }} />
        </div>
      </div>
      <div>
        <p className="font-semibold mb-2">Historial</p>
        <div className={`rounded-2xl border ${T.border} ${T.panel} divide-y ${T.divide}`}>
          {data.registros.map((r, i) => {
            const cfg = cfgMap[r.estado];
            return (
              <div key={i} className="flex items-center justify-between gap-3 px-5 py-3 flex-wrap">
                <div className="flex items-center gap-3">
                  <cfg.Icon size={16} className={cfg.color} />
                  <span className="text-sm font-medium">{formatFecha(r.fecha)}</span>
                  <span className={`text-xs ${cfg.color} font-semibold`}>{cfg.label}</span>
                </div>
                {r.comentario && <span className={`text-xs ${T.muted}`}>{r.comentario}</span>}
              </div>
            );
          })}
        </div>
      </div>
    </div>
  );
}

function GruposTab({ grupos, role, T, onApprove, onReject, onOpenRequest }) {
  return (
    <div className="space-y-4">
      <div className="flex items-center justify-between gap-3 flex-wrap">
        <p className={`text-sm ${T.muted}`}>Los chats grupales se archivan automáticamente al vencer su fecha de expiración.</p>
        {role === "estudiante" && (
          <button onClick={onOpenRequest} className="flex items-center gap-1.5 text-sm font-semibold px-3 py-2 rounded-lg bg-teal-600 text-white shrink-0">
            <Plus size={15} /> Solicitar grupo
          </button>
        )}
        {role === "docente" && (
          <button className="flex items-center gap-1.5 text-sm font-semibold px-3 py-2 rounded-lg bg-teal-600 text-white shrink-0">
            <Plus size={15} /> Crear grupo
          </button>
        )}
      </div>
      <div className="space-y-3">
        {grupos.map((g) => (
          <div key={g.id} className={`rounded-2xl border ${T.border} ${T.panel} p-5 flex items-center justify-between flex-wrap gap-3`}>
            <div>
              <p className="font-semibold">{g.nombre}</p>
              <p className={`text-sm ${T.muted}`}>{g.integrantes.join(", ")}</p>
              {g.estado === "aprobado" && <p className="text-xs text-emerald-600 font-semibold mt-1">Activo · expira el {formatFecha(g.expira)}</p>}
              {g.estado === "pendiente" && <p className="text-xs text-amber-600 font-semibold mt-1">Esperando aprobación del docente</p>}
              {g.estado === "rechazado" && <p className="text-xs text-rose-600 font-semibold mt-1">Rechazado</p>}
            </div>
            {role === "docente" && g.estado === "pendiente" && (
              <div className="flex gap-2">
                <button onClick={() => onApprove(g.id)} className="flex items-center gap-1 text-xs font-semibold px-3 py-1.5 rounded-lg bg-emerald-600 text-white"><Check size={13} /> Aprobar</button>
                <button onClick={() => onReject(g.id)} className={`flex items-center gap-1 text-xs font-semibold px-3 py-1.5 rounded-lg border ${T.border}`}><X size={13} /> Rechazar</button>
              </div>
            )}
            {g.estado === "aprobado" && <button className={`text-xs font-semibold px-3 py-1.5 rounded-lg border ${T.border}`}>Abrir chat</button>}
          </div>
        ))}
        {grupos.length === 0 && <p className={`text-sm ${T.muted} text-center py-8`}>Todavía no hay grupos en esta materia.</p>}
      </div>
    </div>
  );
}

function GroupRequestModal({ open, onClose, companeros, picked, setPicked, onSubmit, T }) {
  if (!open) return null;
  return (
    <div className="fixed inset-0 bg-black/40 flex items-center justify-center z-50 p-4">
      <div className={`w-full max-w-sm rounded-2xl border ${T.border} ${T.panel} p-5 space-y-4`}>
        <div className="flex items-center justify-between">
          <p className="font-bold">Solicitar grupo</p>
          <button onClick={onClose}><X size={18} /></button>
        </div>
        <p className={`text-sm ${T.muted}`}>Elegí compañeros del curso. El docente deberá aprobar la solicitud.</p>
        <div className="space-y-1.5 max-h-48 overflow-y-auto">
          {companeros.map((name) => {
            const checked = picked.includes(name);
            return (
              <label key={name} className={`flex items-center gap-2.5 px-3 py-2 rounded-lg cursor-pointer ${T.hover}`}>
                <input type="checkbox" checked={checked} onChange={() => setPicked((p) => (checked ? p.filter((n) => n !== name) : [...p, name]))} />
                <span className="text-sm">{name}</span>
              </label>
            );
          })}
        </div>
        <button onClick={onSubmit} disabled={picked.length === 0} className="w-full text-sm font-semibold py-2.5 rounded-lg bg-teal-600 text-white disabled:opacity-50">
          Enviar solicitud
        </button>
      </div>
    </div>
  );
}

function MateriaDetail({ materia, T, role, tab, setTab, backToMaterias, trabajosTabProps, asistenciaTabProps, mensajesTabProps, gruposTabProps }) {
  const hue = HUE[materia.hue];
  const tabs = [
    { key: "trabajos", label: "Trabajos", Icon: ClipboardList },
    { key: "asistencia", label: "Asistencia", Icon: CheckCircle2 },
    { key: "mensajes", label: "Mensajes", Icon: MessageSquare },
    { key: "grupos", label: "Grupos", Icon: Users },
  ];
  return (
    <div className="space-y-5">
      <button onClick={backToMaterias} className={`flex items-center gap-1.5 text-sm font-semibold ${T.muted}`}>
        <ArrowLeft size={15} /> Volver a materias
      </button>
      <div className="flex items-center gap-3">
        <div className={`h-10 w-1.5 rounded-full ${hue.barTop}`} />
        <div>
          <h2 className="text-2xl font-extrabold tracking-tight">{materia.nombre}</h2>
          <p className={`text-sm ${T.muted}`}>{materia.docente} · {materia.curso}</p>
        </div>
      </div>
      <div className={`flex gap-1 border-b ${T.border} overflow-x-auto`}>
        {tabs.map(({ key, label, Icon }) => {
          const active = tab === key;
          return (
            <button key={key} onClick={() => setTab(key)}
              className={`flex items-center gap-1.5 px-4 py-2.5 text-sm font-semibold rounded-t-lg border-b-2 -mb-px whitespace-nowrap transition-colors ${active ? `${hue.barTop} text-white border-transparent` : `border-transparent ${T.muted} ${T.hover}`}`}>
              <Icon size={15} /> {label}
            </button>
          );
        })}
      </div>
      <div>
        {tab === "trabajos" && <TrabajosTab {...trabajosTabProps} />}
        {tab === "asistencia" && <AsistenciaTab {...asistenciaTabProps} />}
        {tab === "mensajes" && <ChatView {...mensajesTabProps} />}
        {tab === "grupos" && <GruposTab {...gruposTabProps} />}
      </div>
    </div>
  );
}

function MateriasGrid({ T, role, openMateria }) {
  return (
    <div className="space-y-4">
      <h2 className="text-2xl font-extrabold tracking-tight">{role === "estudiante" ? "Tus materias" : "Tus cursos"}</h2>
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        {MATERIAS.map((m) => <MateriaCard key={m.id} materia={m} onClick={() => openMateria(m.id)} T={T} />)}
      </div>
    </div>
  );
}

function InicioEstudiante({ T, openMateria }) {
  const proximos = Object.entries(TRABAJOS_INIT)
    .flatMap(([mid, list]) => list.filter((t) => t.estado === "pendiente").map((t) => ({ ...t, materiaId: mid })))
    .slice(0, 4);
  return (
    <div className="space-y-6">
      <div>
        <h2 className="text-2xl font-extrabold tracking-tight">Hola, Valentina 👋</h2>
        <p className={T.muted}>Esto es lo que tenés para hoy.</p>
      </div>
      <div className="grid grid-cols-1 sm:grid-cols-3 gap-4">
        <StatCard label="Trabajos pendientes" value={proximos.length} Icon={ClipboardList} accent="bg-amber-500" T={T} hint="Para esta semana" />
        <StatCard label="Asistencia general" value="94%" Icon={TrendingUp} accent="bg-emerald-500" T={T} hint="Últimos 30 días" />
        <StatCard label="Mensajes sin leer" value="2" Icon={MessageSquare} accent="bg-sky-500" T={T} hint="De tus docentes" />
      </div>
      <div>
        <h3 className="font-bold mb-3">Próximos vencimientos</h3>
        <div className={`rounded-2xl border ${T.border} ${T.panel} divide-y ${T.divide}`}>
          {proximos.map((t) => {
            const m = MATERIAS.find((x) => x.id === t.materiaId);
            return (
              <button key={t.id} onClick={() => openMateria(t.materiaId)} className={`w-full flex items-center justify-between gap-3 px-5 py-4 text-left ${T.hover}`}>
                <div>
                  <p className="font-semibold">{t.nombre}</p>
                  <p className={`text-sm ${T.muted}`}>{m ? m.nombre : ""} · vence el {formatFecha(t.limite)}</p>
                </div>
                <ChevronRight size={18} className={T.muted} />
              </button>
            );
          })}
        </div>
      </div>
      <div>
        <h3 className="font-bold mb-3">Tus materias</h3>
        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
          {MATERIAS.map((m) => <MateriaCard key={m.id} materia={m} onClick={() => openMateria(m.id)} T={T} />)}
        </div>
      </div>
    </div>
  );
}

function InicioDocente({ T, openMateria }) {
  return (
    <div className="space-y-6">
      <div>
        <h2 className="text-2xl font-extrabold tracking-tight">Hola, Prof. Liliana 👋</h2>
        <p className={T.muted}>Resumen de tus cursos.</p>
      </div>
      <div className="grid grid-cols-1 sm:grid-cols-4 gap-4">
        <StatCard label="Por corregir" value={PENDIENTES_DOCENTE.length} Icon={ClipboardList} accent="bg-amber-500" T={T} />
        <StatCard label="Estudiantes" value="118" Icon={Users} accent="bg-sky-500" T={T} />
        <StatCard label="Asistencia hoy" value="3 cursos" Icon={CheckCircle2} accent="bg-emerald-500" T={T} hint="Sin registrar" />
        <StatCard label="Mensajes nuevos" value="5" Icon={MessageSquare} accent="bg-violet-500" T={T} />
      </div>
      <div>
        <h3 className="font-bold mb-3">Entregas para corregir</h3>
        <div className={`rounded-2xl border ${T.border} ${T.panel} divide-y ${T.divide}`}>
          {PENDIENTES_DOCENTE.map((p, i) => (
            <div key={i} className="flex items-center justify-between gap-3 px-5 py-4 flex-wrap">
              <div>
                <p className="font-semibold">{p.alumno} — {p.trabajo}</p>
                <p className={`text-sm ${T.muted}`}>{p.materia} · entregado el {formatFecha(p.fecha)}</p>
              </div>
              <button onClick={() => openMateria(p.materiaId)} className="text-sm font-semibold px-3 py-1.5 rounded-lg bg-teal-600 text-white shrink-0">Corregir</button>
            </div>
          ))}
        </div>
      </div>
      <div>
        <h3 className="font-bold mb-3">Tus cursos</h3>
        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
          {MATERIAS.map((m) => <MateriaCard key={m.id} materia={m} onClick={() => openMateria(m.id)} T={T} />)}
        </div>
      </div>
    </div>
  );
}

function MensajesGlobal({ T, globalChatId, setGlobalChatId, chats, draft, setDraft, onSend, role }) {
  const current = MATERIAS.find((m) => m.id === globalChatId);
  return (
    <div className="flex gap-4 flex-wrap">
      <div className={`w-full sm:w-60 shrink-0 rounded-2xl border ${T.border} ${T.panel} overflow-hidden flex flex-col`} style={{ height: "420px" }}>
        <div className={`px-4 py-3 border-b ${T.border} font-semibold text-sm`}>Conversaciones</div>
        <div className="flex-1 overflow-y-auto">
          {MATERIAS.map((m) => {
            const active = m.id === globalChatId;
            const list = chats[m.id] || [];
            const last = list[list.length - 1];
            return (
              <button key={m.id} onClick={() => setGlobalChatId(m.id)} className={`w-full text-left px-4 py-3 flex items-center gap-3 ${active ? "bg-teal-600 text-white" : T.hover}`}>
                <div className={`w-8 h-8 rounded-full ${HUE[m.hue].dotL} text-white flex items-center justify-center text-xs font-bold shrink-0`}>{m.nombre[0]}</div>
                <div className="min-w-0">
                  <p className="text-sm font-semibold truncate">{role === "estudiante" ? m.docente : m.nombre}</p>
                  <p className={`text-xs truncate ${active ? "text-teal-100" : T.muted}`}>{last ? last.texto : "Sin mensajes"}</p>
                </div>
              </button>
            );
          })}
        </div>
      </div>
      <div className="flex-1 min-w-[260px]">
        <ChatView title={role === "estudiante" ? current.docente : current.nombre} subtitle={current.nombre} messages={chats[globalChatId] || []} draft={draft} setDraft={setDraft} onSend={onSend} T={T} />
      </div>
    </div>
  );
}

function CalendarioView({ T }) {
  const year = 2026, month = 5; // junio
  const first = new Date(year, month, 1);
  const startDay = first.getDay();
  const daysInMonth = new Date(year, month + 1, 0).getDate();
  const cells = [];
  for (let i = 0; i < startDay; i++) cells.push(null);
  for (let d = 1; d <= daysInMonth; d++) cells.push(d);
  const eventos = { 25: ["Entrega: Geometría"], 27: ["Entrega: Informe de laboratorio"], 28: ["Vence: Práctica de ecuaciones"], 30: ["Vence: Ensayo de Historia"] };
  const dow = ["D", "L", "M", "M", "J", "V", "S"];
  return (
    <div className="space-y-4">
      <h2 className="text-2xl font-extrabold tracking-tight">Calendario — Junio 2026</h2>
      <div className={`rounded-2xl border ${T.border} ${T.panel} p-5`}>
        <div className="grid grid-cols-7 gap-2 mb-2">
          {dow.map((d, i) => <div key={i} className={`text-center text-xs font-bold ${T.muted}`}>{d}</div>)}
        </div>
        <div className="grid grid-cols-7 gap-2">
          {cells.map((d, i) => (
            <div key={i} className={`aspect-square rounded-xl flex flex-col items-center justify-center text-sm relative ${d ? T.hover : ""} ${d === 26 ? "ring-2 ring-teal-500" : ""}`}>
              {d && <span className="font-medium">{d}</span>}
              {d && eventos[d] && <span className="w-1.5 h-1.5 rounded-full bg-amber-500 absolute bottom-1.5" />}
            </div>
          ))}
        </div>
      </div>
      <div>
        <p className="font-semibold mb-2">Próximos eventos</p>
        <div className={`rounded-2xl border ${T.border} ${T.panel} divide-y ${T.divide}`}>
          {Object.entries(eventos).filter(([d]) => Number(d) >= 26).sort((a, b) => Number(a[0]) - Number(b[0])).map(([d, list]) =>
            list.map((ev, i) => (
              <div key={d + "-" + i} className="flex items-center gap-3 px-5 py-3">
                <span className="font-mono text-sm font-bold w-8">{d}</span>
                <span className="text-sm">{ev}</span>
              </div>
            ))
          )}
        </div>
      </div>
    </div>
  );
}

function PerfilView({ T, role }) {
  const nombre = role === "estudiante" ? "Valentina Acosta" : "Prof. Liliana Gómez";
  const detalle = role === "estudiante" ? "4° Año B · Instituto San Martín" : "Matemática · Instituto San Martín";
  return (
    <div className="max-w-md space-y-5">
      <div className={`rounded-2xl border ${T.border} ${T.panel} p-6 text-center`}>
        <div className="w-16 h-16 rounded-full bg-teal-600 text-white flex items-center justify-center text-xl font-bold mx-auto mb-3">{nombre[0]}</div>
        <p className="font-bold text-lg">{nombre}</p>
        <p className={`text-sm ${T.muted}`}>{detalle}</p>
      </div>
      <div className={`rounded-2xl border ${T.border} ${T.panel} divide-y ${T.divide}`}>
        {[["Materias", MATERIAS.length], ["Notificaciones", "Activadas"], ["Idioma", "Español (AR)"]].map(([k, v], i) => (
          <div key={i} className="flex items-center justify-between px-5 py-3.5 text-sm">
            <span className={T.muted}>{k}</span>
            <span className="font-semibold">{v}</span>
          </div>
        ))}
      </div>
    </div>
  );
}

/* --------------------------------- App ----------------------------------- */
export default function AulaConnect() {
  const [dark, setDark] = useState(false);
  const [role, setRole] = useState("estudiante");
  const [page, setPage] = useState("inicio");
  const [materiaId, setMateriaId] = useState(null);
  const [tab, setTab] = useState("trabajos");
  const [expanded, setExpanded] = useState(null);
  const [filter, setFilter] = useState("todos");
  const [chats, setChats] = useState(MENSAJES_INIT);
  const [draft, setDraft] = useState("");
  const [groupsState, setGroupsState] = useState(GRUPOS_INIT);
  const [showGroupModal, setShowGroupModal] = useState(false);
  const [picked, setPicked] = useState([]);
  const [rosterState, setRosterState] = useState({});
  const [globalChatId, setGlobalChatId] = useState(MATERIAS[0].id);

  const T = dark ? DARK : LIGHT;
  const materia = MATERIAS.find((m) => m.id === materiaId) || null;

  function openMateria(id) {
    setMateriaId(id); setTab("trabajos"); setExpanded(null); setFilter("todos"); setPage("materias");
  }
  function backToMaterias() { setMateriaId(null); }
  function goPage(p) { setPage(p); if (p !== "materias") setMateriaId(null); }

  function sendMessage(mid) {
    if (!draft.trim()) return;
    setChats((prev) => ({ ...prev, [mid]: [...(prev[mid] || []), { id: Date.now(), autor: "yo", texto: draft, hora: nowStr() }] }));
    setDraft("");
  }
  function approveGroup(mid, id) {
    setGroupsState((prev) => ({ ...prev, [mid]: prev[mid].map((g) => (g.id === id ? { ...g, estado: "aprobado", expira: "2026-07-03" } : g)) }));
  }
  function rejectGroup(mid, id) {
    setGroupsState((prev) => ({ ...prev, [mid]: prev[mid].map((g) => (g.id === id ? { ...g, estado: "rechazado" } : g)) }));
  }
  function submitGroupRequest() {
    if (picked.length === 0 || !materiaId) return;
    const nuevo = { id: "g" + Date.now(), nombre: "Grupo nuevo", integrantes: ["Vos", ...picked], estado: "pendiente" };
    setGroupsState((prev) => ({ ...prev, [materiaId]: [...(prev[materiaId] || []), nuevo] }));
    setShowGroupModal(false); setPicked([]);
  }

  return (
    <div className={`w-full flex rounded-2xl overflow-hidden border ${T.border} ${T.page} font-sans`} style={{ height: "780px" }}>
      <Rail page={page} setPage={goPage} dark={dark} setDark={setDark} role={role} setRole={setRole} T={T} />
      <div className="flex-1 flex flex-col min-w-0">
        <Topbar T={T} title={headerTitle(page, materia, role)} subtitle={headerSubtitle(page, role)} />
        <NotebookEdge T={T} />
        <main className="flex-1 overflow-y-auto p-6">
          {page === "inicio" && (role === "estudiante" ? <InicioEstudiante T={T} openMateria={openMateria} /> : <InicioDocente T={T} openMateria={openMateria} />)}
          {page === "materias" && !materia && <MateriasGrid T={T} role={role} openMateria={openMateria} />}
          {page === "materias" && materia && (
            <MateriaDetail
              materia={materia} T={T} role={role} tab={tab} setTab={setTab} backToMaterias={backToMaterias}
              trabajosTabProps={{ trabajos: TRABAJOS_INIT[materia.id] || [], role, dark, filter, setFilter, expanded, setExpanded, T }}
              asistenciaTabProps={{ data: ASISTENCIA_DATA[materia.id], T, role, roster: ROSTER, rosterState, setRosterState }}
              mensajesTabProps={{ title: role === "estudiante" ? materia.docente : "Chat del curso", subtitle: materia.nombre, messages: chats[materia.id] || [], draft, setDraft, onSend: () => sendMessage(materia.id), T }}
              gruposTabProps={{ grupos: groupsState[materia.id] || [], role, T, onApprove: (id) => approveGroup(materia.id, id), onReject: (id) => rejectGroup(materia.id, id), onOpenRequest: () => setShowGroupModal(true) }}
            />
          )}
          {page === "mensajes" && (
            <MensajesGlobal T={T} globalChatId={globalChatId} setGlobalChatId={setGlobalChatId} chats={chats} draft={draft} setDraft={setDraft} onSend={() => sendMessage(globalChatId)} role={role} />
          )}
          {page === "calendario" && <CalendarioView T={T} />}
          {page === "perfil" && <PerfilView T={T} role={role} />}
        </main>
      </div>
      <GroupRequestModal open={showGroupModal} onClose={() => setShowGroupModal(false)} companeros={materia ? COMPANEROS[materia.id] || [] : []} picked={picked} setPicked={setPicked} onSubmit={submitGroupRequest} T={T} />
    </div>
  );
}
