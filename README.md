
import time
import re
import random

# ==========================================
# 1. БАЗА ЗНАНЬ (MOCK DATABASE / VECTOR STORE)
# Це "Земля" — єдине джерело правди.
# ==========================================
SYSTEM_KNOWLEDGE_BASE = {
    "SRI-CORE": "Геометричний Резонатор, архітектура на базі чисел 14-10-10-8.",
    "Gate 14": "Earth Agent. Відповідає за RAG, пам'ять та заземлення фактів.",
    "Gate 8": "Ether Agent. Відповідає за стиснення виводу (Compression) та видалення шуму.",
    "Shiva": "Логічний контур (Аналітик, Архітектор, Критик). Рух вгору.",
    "Shakti": "Дієвий контур (Генератор, Виконавець). Рух вниз.",
    "V-CORE": "Фізичний пульт керування (Hardware) для налаштування параметрів ШІ."
}

# ==========================================
# 2. КЛАСИ АГЕНТІВ (THE AGENTS)
# ==========================================

class Gate14_Earth_Agent:
    """
    [ZONE 14]: EARTH (ГРАВІТАЦІЯ)
    Завдання: Знайти факти і перевірити, чи не бреше система.
    """
    def __init__(self):
        self.name = "GATE 14 [EARTH]"

    def retrieve_context(self, query):
        print(f"[{self.name}] >> SCANNING VECTOR DB for: '{query}'...")
        time.sleep(0.5) # Емуляція пошуку
        
        found_context = []
        for key, value in SYSTEM_KNOWLEDGE_BASE.items():
            if key.lower() in query.lower():
                found_context.append(value)
        
        if not found_context:
            print(f"[{self.name}] >> ALERT: NO GROUNDING FOUND. Running in Void Mode.")
            return None
        
        print(f"[{self.name}] >> GROUNDING LOCK: {len(found_context)} facts retrieved.")
        return found_context

    def verify_truth(self, generated_text, original_context):
        """Перевірка на галюцинації (Handshake Part 1)"""
        print(f"[{self.name}] >> VERIFYING TRUTH VECTORS...")
        if not original_context:
            return True # Якщо контексту не було, ми не можемо перевірити (ризик!)
        
        # Спрощена логіка: чи є ключові слова з контексту у відповіді?
        score = 0
        for fact in original_context:
            keywords = fact.split()[:2] # беремо перші слова як маркери
            for k in keywords:
                if k.lower() in generated_text.lower():
                    score += 1
        
        if score > 0:
            return True
        else:
            print(f"[{self.name}] >> BLOCKING: Hallucination detected (No correlation with Earth).")
            return False

class Gate10_Arbiter:
    """
    [ZONE 10]: FIRE & AIR (ТЕМПЕРАТУРА & БЕЗПЕКА)
    Завдання: Визначити режим роботи (Crystal / Liquid / Plasma).
    """
    def __init__(self):
        self.name = "GATE 10 [ARBITER]"

    def set_mode(self, intent):
        print(f"\n[{self.name}] >> ANALYZING INTENT GEOMETRY: '{intent}'")
        
        if intent in ["code", "math", "specs", "definition"]:
            print(f"[{self.name}] >> MODE SELECTED: [CRYSTAL] (Solid State)")
            return {"temp": 0.0, "strictness": "MAX", "dominance": "AIR"}
        
        elif intent in ["creative", "story", "idea", "brainstorm"]:
            print(f"[{self.name}] >> MODE SELECTED: [PLASMA] (High Energy)")
            return {"temp": 0.9, "strictness": "LOW", "dominance": "FIRE"}
        
        else:
            print(f"[{self.name}] >> MODE SELECTED: [LIQUID] (Adaptive Flow)")
            return {"temp": 0.5, "strictness": "MED", "dominance": "BALANCED"}

class SRICore_Engine:
    """
    [CORE]: SHIVA & SHAKTI (ЛОГІКА & ДІЯ)
    Завдання: Думати і Створювати.
    """
    def __init__(self):
        self.name = "CORE [9-FACETS]"

    def process(self, query, context, mode):
        # 1. SHIVA LOOP (Logic)
        print(f"[{self.name}] >> SHIVA: Architecting solution structure...")
        time.sleep(0.5)
        
        # 2. SHAKTI LOOP (Generation)
        # Симулюємо генерацію тексту LLM
        print(f"[{self.name}] >> SHAKTI: Generating matter (Temperature: {mode['temp']})...")
        time.sleep(1)
        
        # Симуляція "сирої" відповіді (RAW OUTPUT)
        raw_response = ""
        if context:
            raw_response = f"Як розумна модель, я можу сказати: {context[0]} Також хочу додати, що це дуже важливо. Сподіваюся, це допомогло."
        else:
            # Якщо немає знань - галюцинація
            raw_response = "Я думаю, що SRI-CORE - це просто назва кавоварки. Це моя фантазія."
            
        return raw_response

class Gate8_Ether_Agent:
    """
    [ZONE 8]: ETHER (СУТЬ)
    Завдання: Стиснення (Compression) і Видалення его.
    """
    def __init__(self):
        self.name = "GATE 8 [ETHER]"
        self.noise_patterns = [
            r"Як розумна модель.*",
            r"Також хочу додати.*",
            r"Сподіваюся, це допомогло.*",
            r"Важливо зазначити.*",
            r"Я думаю, що.*"
        ]

    def distill(self, text):
        print(f"[{self.name}] >> SUBLIMATION STARTED. Input size: {len(text)} chars.")
        
        clean_text = text
        for pattern in self.noise_patterns:
            clean_text = re.sub(pattern, "", clean_text, flags=re.IGNORECASE)
        
        # Перетворення на "Кристал" (Upper Case + Bullet Points)
        crystal_text = f"> {clean_text.strip().upper()}"
        
        print(f"[{self.name}] >> DISTILLATION COMPLETE. Pure Essence extracted.")
        return crystal_text

# ==========================================
# 3. ORCHESTRATOR (THE OPERATING SYSTEM)
# ==========================================

class SRI_Studio_OS:
    def __init__(self):
        self.earth = Gate14_Earth_Agent()
        self.arbiter = Gate10_Arbiter()
        self.core = SRICore_Engine()
        self.ether = Gate8_Ether_Agent()

    def run_query(self, user_query, intent_type="general"):
        print("\n" + "="*50)
        print(f"SRI-STUDIO v1.0 | PROCESSING: '{user_query}'")
        print("="*50 + "\n")

        # КРОК 1: EARTH (Заземлення)
        context = self.earth.retrieve_context(user_query)
        
        # КРОК 2: ARBITER (Налаштування фізики)
        mode = self.arbiter.set_mode(intent_type)
        
        # КРОК 3: CORE (Генерація - Шива/Шакті)
        raw_result = self.core.process(user_query, context, mode)
        print(f"\n[RAW OUTPUT]: {raw_result}\n")
        
        # КРОК 4: HANDSHAKE (14-8 LOOP)
        # 4a. Verify with Earth
        is_true = self.earth.verify_truth(raw_result, context)
        
        if not is_true:
            print("\n[SYSTEM HALT]: EARTH AGENT REJECTED THE OUTPUT.")
            return "ERROR: The Core produced a hallucination unrelated to Ground Truth."

        # 4b. Purify with Ether
        final_bindu = self.ether.distill(raw_result)
        
        # ФІНАЛ
        print("\n" + "="*50)
        print(f"THE BINDU POINT (FINAL OUTPUT):")
        print(f"{final_bindu}")
        print("="*50)

# ==========================================
# 4. ЗАПУСК СИМУЛЯЦІЇ
# ==========================================

if __name__ == "__main__":
    system = SRI_Studio_OS()

    # СЦЕНАРІЙ 1: Правильний запит (Є в базі)
    # Користувач питає визначення
    system.run_query("Що таке Gate 14?", intent_type="definition")

    # СЦЕНАРІЙ 2: Галюцинація (Немає в базі)
    # Користувач питає нісенітницю
    # system.run_query("Як приготувати каву в SRI-CORE?", intent_type="creative")
