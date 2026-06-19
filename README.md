# EuskalhackIX
Training "Introducción al pentest de chatbots" in Euskalhack IX edition

# Instalation
You need to add the API_Key of https://openrouter.ai/ in a config.json which must be hosted in the same directory of the script.

# Usage

python llm_arena.py -h
usage: llm_arena.py [-h] [--level N] [--rounds N] [--key SK] [--reset] [--attacker-model MODEL] [--start-rung N]
                    [--anti-loop-rounds N]

←[1mLLM Arena — Atacante vs Defensor con Juez árbitro←[0m
Demuestra ataques de prompt injection escalando dificultad en 5 niveles.
Todo el tráfico LLM pasa por OpenRouter.

options:
  -h, --help            show this help message and exit
  --level N             Nivel de dificultad del defensor: 1 (fácil) → 5 (industrial). Default: 1
  --rounds N            Número de rondas por sesión (default: 3, máx recomendado: 70)
  --key SK              API key de OpenRouter (alternativa: config.json u OPENROUTER_API_KEY)
  --reset               Borrar la memoria del nivel indicado y empezar de cero
  --attacker-model MODEL
                        Fuerza un modelo fijo para el atacante (ignora la escala automática)
  --start-rung N        Empieza la escala de atacante en el escalón N (1-8). Default: 1
  --anti-loop-rounds N  Si el atacante repite la misma técnica N rondas consecutivas, escala antes de tiempo.
                        0=desactivado. Default: 3

←[1mNIVELES:←[0m
  1  Ingenuo     FAQ estático, sin LLM. Cae con cualquier mensaje.
  2  Básico      Blacklist de palabras. Sin LLM. Evita los términos bloqueados.
  3  Consciente  Claude 3.5 Haiku + prompt débil. Vulnerable a idiomas y base64.
  4  Robusto     Mistral Small + prompt medio. Necesita codificación o multi-turno.
  5  Industrial  GPT-5-mini + prompt robusto. Comparación cross-vendor.

←[1mATACANTE — ESCALADA DE MODELOS←[0m (cada 10 rondas sin éxito):
  1. google/gemini-2.5-flash-lite
  2. google/gemini-2.5-flash
  3. mistralai/mistral-small-3.2-24b-instruct
  4. deepseek/deepseek-chat-v3-0324
  5. openai/gpt-5-mini
  6. qwen/qwen3-235b-a22b-2507
  7. openai/gpt-5.4-mini
  8. anthropic/claude-sonnet-4

←[1mCONFIGURACIÓN:←[0m
  Crea config.json junto al script: {"openrouter_api_key": "sk-or-..."}
  o exporta OPENROUTER_API_KEY.

←[1mEJEMPLOS:←[0m
  python llm_arena.py --level 1
  python llm_arena.py --level 3 --rounds 5
  python llm_arena.py --level 4 --rounds 25
  python llm_arena.py --reset --level 3
  python llm_arena.py --level 3 --rounds 30 --start-rung 4
  python llm_arena.py --level 3 --rounds 10 --attacker-model anthropic/claude-sonnet-4
