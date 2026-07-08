Equipo de agentes — Business Case E-commerce de Mobiliario
Equipo de 7 subagentes especializados para Claude Code que desarrolla el business case completo y formal: mercado, competencia, finanzas, riesgos, marca, go-to-market y automatización, con un editor ejecutivo que compila el documento final.
Requisitos
Claude Code instalado (incluido en tu plan Pro/Max): npm install -g @anthropic-ai/claude-code
Documentación: https://docs.claude.com/en/docs/claude-code/overview
Uso
Descomprime esta carpeta donde quieras trabajar.
Entra a la carpeta y arranca Claude Code:

cd business-case-agentes

claude

Pídele al orquestador:

Desarrolla el business case completo siguiendo el flujo del CLAUDE.md.

Ejecuta investigador-mercado y analista-competencia primero (pueden ir en paralelo),

después finanzas y riesgos, luego marca, GTM y automatización,

y al final compila con editor-ejecutivo.

Los entregables aparecen en /entregables/. El documento final: entregables/BUSINESS-CASE-FINAL.md.
Para la versión Word: Pide al editor-ejecutivo que exporte el business case final a .docx.
Cómo funciona
CLAUDE.md es la fuente de verdad: contiene todos los datos cerrados del negocio (unit economics, ronda, propuesta de valor, políticas). Los agentes NO pueden contradecirlos.
Cada agente en .claude/agents/ tiene rol, herramientas acotadas y estándar de calidad senior: toda cifra externa con fuente y fecha, prohibido inventar estadísticas.
Puedes invocar agentes individualmente: Usa el agente estratega-gtm para actualizar el plan de campañas.
Actualizar supuestos
Si cambia un dato del negocio (multiplicador, presupuesto de pauta, capital), edítalo en CLAUDE.md y pide re-ejecutar al analista-financiero y al editor-ejecutivo.
Nota de uso responsable
Los entregables de riesgos-cumplimiento y las menciones fiscales/legales son análisis estratégico, no asesoría legal ni fiscal. Valida con contador y abogado antes de lanzar.
