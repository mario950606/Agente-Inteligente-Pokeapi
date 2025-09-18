# 🧑‍💻 Agente Pokémon con LangGraph + PokéAPI + RAG (Chroma)

Este proyecto implementa un **agente inteligente** usando [LangChain](https://www.langchain.com/) y [LangGraph](https://github.com/langchain-ai/langgraph).  
El agente responde preguntas sobre Pokémon en español, combinando datos de la **PokéAPI** y conocimiento adicional almacenado en una base vectorial **Chroma (RAG)**.  
Además, soporta **memoria por `thread_id`**, lo que permite hilar preguntas en una conversación.

---

## 🚀 Funcionalidades
- 🔎 Consultar información de un Pokémon (tipos, stats, altura y peso).  
- 🔄 Obtener rutas de evolución desde la PokéAPI.  
- ⚔️ Comparar Pokémon en stats específicas (ejemplo: velocidad, ataque).  
- 🌀 Obtener información sobre tipos y relaciones de daño.  
- 📖 Preguntar por movimientos y sus características (poder, precisión, tipo, clase de daño).  
- 📚 Consultar conocimiento extendido gracias a un **RAG con ChromaDB** (ejemplo: “Historia de Charizard”).  
- 🧵 Mantiene **memoria de la conversación** usando `thread_id` (ejemplo: preguntar “compáralo” después de mencionar a Gengar).  

---

## 📂 Estructura del proyecto
- `agent pokemon - Chroma - hilado.ipynb` → notebook principal.  
- **Tools definidas**:
  - `get_pokemon_info`
  - `get_type_info`
  - `get_move_info`
  - `get_evolution_paths`
  - `compare_pokemon`
  - `search_pokemon_knowledge` (usa **Chroma RAG**)  
- **Grafo LangGraph**:
  - Nodo **Agent** (LLM + instrucciones en español).  
  - Nodo **Tools** (PokéAPI + RAG).  
  - **Memoria** con `thread_id` para hilar conversaciones.  

---

## ⚙️ Instalación
1. Clonar este repositorio o copiar el notebook.  
2. Crear un entorno virtual e instalar dependencias:
   ```bash
   pip install langchain langgraph langchain-openai chromadb requests python-dotenv
   ```
3. Crear un archivo `.env` con tu API Key de OpenAI:
   ```env
   OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxx
   ```

---

## ▶️ Uso
Ejecuta el notebook y prueba consultas:

```python
config = {"configurable": {"thread_id": "pokemon-demo"}}

# Ejemplo 1: tipos de un Pokémon (PokéAPI)
app.invoke({"messages": [("user", "¿De qué tipo es Gengar y contra qué tipos es fuerte o débil?")]}, config)

# Ejemplo 2: hilar con memoria
app.invoke({"messages": [("user", "Ahora compáralo con Alakazam en velocidad y ataque")]}, config)

# Ejemplo 3: conocimiento extendido (RAG con Chroma)
app.invoke({"messages": [("user", "Cuéntame la historia de Mewtwo")]}, config)
```

---

## 📌 Requisitos
- Python 3.10+  
- Conexión a internet (para consultas a la PokéAPI).  
- Una API Key de OpenAI.  

---

## 🗺️ Arquitectura
```mermaid
flowchart LR
    Usuario --> AppLangGraph
    AppLangGraph --> Agent
    Agent --> Tools
    Tools --> PokéAPI
    Tools --> ChromaDB
    Tools --> Agent
    Agent --> AppLangGraph
    AppLangGraph --> Usuario
    AppLangGraph --> Memoria
    Memoria --> AppLangGraph
```

---

## ✨ Futuras mejoras
- Ampliar la base de conocimiento RAG con más documentos de lore y guías de Pokémon.  
- Añadir comparaciones múltiples (más de 2 Pokémon).  
- Exportar resultados en JSON o CSV.  
