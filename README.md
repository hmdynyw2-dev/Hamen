import random
import time
import json
import os

WORDS = {
    "حيوانات": [
        {"w": "أسد", "d": "ملك الغابة له لبدة وزئير مرعب"},
        {"w": "فيل", "d": "أكبر حيوان بري له خرطوم طويل"},
        {"w": "زرافة", "d": "أطول حيوان رقبته طويلة جداً"},
        {"w": "قطة", "d": "حيوان أليف يموء ويحب النوم"},
        {"w": "كلب", "d": "أوفى صديق للإنسان"},
        {"w": "حوت", "d": "أكبر مخلوق في البحر"},
        {"w": "نمر", "d": "قطة كبيرة بها بقع صفراء وسوداء"},
        {"w": "دلفين", "d": "حيوان بحري ذكي يحب اللعب"},
        {"w": "أرنب", "d": "حيوان بأذنين طويلتين وذيل صغير"},
        {"w": "ببغاء", "d": "طائر ملوّن يقدر يقلّد الكلام"},
    ],
    "أكل مصري": [
        {"w": "كشري", "d": "أكلة مصرية من العدس والمكرونة والأرز"},
        {"w": "فول مدمس", "d": "أكلة الفطار المصري الشهيرة"},
        {"w": "طعمية", "d": "كفتة خضراء مقلية من الفول"},
        {"w": "ملوخية", "d": "خضرة خضراء لزجة مصرية الأصل"},
        {"w": "كفتة", "d": "لحمة مفرومة مشوية بالتوابل"},
        {"w": "أم علي", "d": "حلو مصري بالعجين والحليب والمكسرات"},
        {"w": "بسبوسة", "d": "حلوى مصرية من السميد والسكر"},
        {"w": "كنافة", "d": "حلو رمضاني من الشعيرية والجبن"},
        {"w": "عيش بلدي", "d": "خبز مصري دائري رقيق"},
        {"w": "عصير قصب", "d": "عصير أخضر من قصب السكر"},
    ],
    "أماكن": [
        {"w": "الأهرامات", "d": "أقدم عجائب الدنيا في مصر"},
        {"w": "برج إيفل", "d": "برج حديدي شهير في باريس"},
        {"w": "الكعبة المشرفة", "d": "أقدس بقعة إسلامية في مكة"},
        {"w": "نهر النيل", "d": "أطول نهر في العالم يمر بمصر"},
        {"w": "دبي", "d": "مدينة إماراتية فيها أطول برج في العالم"},
        {"w": "القاهرة", "d": "عاصمة مصر ومدينة الألف مئذنة"},
        {"w": "الأقصر", "d": "مدينة المعابد الفرعونية في جنوب مصر"},
        {"w": "شرم الشيخ", "d": "مدينة سياحية على البحر الأحمر"},
        {"w": "روما", "d": "عاصمة إيطاليا ومدينة الكولوسيوم"},
        {"w": "لندن", "d": "عاصمة بريطانيا وفيها بيج بن"},
    ],
    "رياضة": [
        {"w": "كرة القدم", "d": "رياضة تلعب فيها 11 لاعب لتسجيل هدف"},
        {"w": "كرة السلة", "d": "رياضة تسجل فيها النقاط في سلة مرتفعة"},
        {"w": "سباحة", "d": "رياضة في الماء بأساليب مختلفة"},
        {"w": "تنس", "d": "رياضة بمضرب وكرة صفراء صغيرة"},
        {"w": "ملاكمة", "d": "رياضة قتالية بالقفازات"},
        {"w": "جمباز", "d": "رياضة تحتاج مرونة وتوازن"},
        {"w": "ركوب خيل", "d": "رياضة تركب فيها الحصان"},
        {"w": "كرة طائرة", "d": "رياضة بشبكة وستة لاعبين من كل جانب"},
        {"w": "رفع أثقال", "d": "رياضة ترفع فيها أوزان حديدية"},
        {"w": "تنس طاولة", "d": "بينج بونج على طاولة خضراء صغيرة"},
    ],
}

def clear():
    os.system('cls' if os.name == 'nt' else 'clear')

def hide_word(word):
    return " ".join(["●" if c != " " else "  " for c in word])

def get_all_words():
    all_words = []
    for cat, words in WORDS.items():
        for w in words:
            all_words.append({**w, "cat": cat})
    return all_words

def play_round(player_name, time_limit, category):
    if category == "كل الفئات":
        words = get_all_words()
    else:
        words = [{**w, "cat": category} for w in WORDS.get(category, [])]
    random.shuffle(words)
    words = words[:10]
    score = 0

    for i, word in enumerate(words):
        clear()
        print(f"👤 {player_name}  |  ⭐ النقاط: {score}")
        print(f"كلمة {i+1} من {len(words)}  |  الفئة: {word['cat']}")
        print("=" * 40)
        print(f"\n📝 الوصف: {word['d']}\n")
        print(f"الكلمة: {hide_word(word['w'])}\n")
        input("اضغط Enter لإظهار الكلمة...")
        start = time.time()
        print(f"\n🔤 الكلمة: {word['w']}\n")
        elapsed = time.time() - start
        remaining = max(0, time_limit - elapsed)
        print(f"⏱ الوقت المتبقي: {int(remaining)} ثانية")
        print("\n1. ✅ صح   2. ❌ غلط   3. ⏭ تخطي")
        choice = input("اختارك: ").strip()
        if choice == "1":
            pts = 10 + int((remaining / time_limit) * 10)
            score += pts
            print(f"✅ +{pts} نقطة!")
            time.sleep(0.8)
        elif choice == "2":
            print("❌ غلط!")
            time.sleep(0.8)
        else:
            print("⏭ تخطي")
            time.sleep(0.5)

    return score

def save_leaderboard(name, score):
    lb = []
    if os.path.exists("leaderboard.json"):
        with open("leaderboard.json", "r", encoding="utf-8") as f:
            lb = json.load(f)
    lb.append({"name": name, "score": score})
    lb.sort(key=lambda x: x["score"], reverse=True)
    with open("leaderboard.json", "w", encoding="utf-8") as f:
        json.dump(lb[:20], f, ensure_ascii=False)

def show_leaderboard():
    if not os.path.exists("leaderboard.json"):
        print("لا يوجد نتائج بعد"); return
    with open("leaderboard.json", "r", encoding="utf-8") as f:
        lb = json.load(f)
    medals = ["🥇","🥈","🥉"]
    for i, e in enumerate(lb[:10]):
        print(f"{medals[i] if i<3 else str(i+1)+'.'} {e['name']} - {e['score']} نقطة")

def choose_settings():
    print("\n⏱ الوقت: 1.30ث  2.45ث  3.60ث")
    t = input("اختار: ").strip()
    time_limit = {"1":30,"2":45,"3":60}.get(t, 45)
    cats = ["كل الفئات"] + list(WORDS.keys())
    print("\n🎲 الفئة:")
    for i,c in enumerate(cats): print(f"{i+1}. {c}")
    c = input("اختار: ").strip()
    try: category = cats[int(c)-1]
    except: category = "كل الفئات"
    return time_limit, category

def main():
    while True:
        clear()
        print("🎯 وصّف وخمّن\n1.لعب فردي  2.دوري  3.متصدرين  4.خروج")
        choice = input("اختارك: ").strip()
        if choice == "1":
            clear()
            name = input("اسمك: ").strip() or "لاعب"
            time_limit, category = choose_settings()
            score = play_round(name, time_limit, category)
            clear()
            print(f"\n🎯 نقاطك: {score}")
            save_leaderboard(name, score)
            input("اضغط Enter للرجوع...")
        elif choice == "2":
            clear()
            players = []
            print("أضف اللاعبين (اكتب 'خلاص' للإنهاء):")
            while len(players) < 8:
                name = input(f"اللاعب {len(players)+1}: ").strip()
                if name in ("خلاص","") and len(players) >= 2: break
                elif name not in ("خلاص",""): players.append({"name":name,"score":0})
            print("جولات: 1.واحدة  2.اثنتين  3.ثلاثة")
            r = input("اختار: ").strip()
            rounds = {"1":1,"2":2,"3":3}.get(r,1)
            time_limit, category = choose_settings()
            for rnd in range(1, rounds+1):
                print(f"\n🔄 الجولة {rnd}")
                for p in players:
                    input(f"دور {p['name']} - Enter للبدء...")
                    p["score"] += play_round(p["name"], time_limit, category)
            clear()
            players.sort(key=lambda x: x["score"], reverse=True)
            print("🏆 النتيجة النهائية")
            medals = ["🥇","🥈","🥉"]
            for i,p in enumerate(players):
                print(f"{medals[i] if i<3 else str(i+1)+'.'} {p['name']} - {p['score']} نقطة")
            print(f"\n🎉 {players[0]['name']} فاز!")
            input("Enter للرجوع...")
        elif choice == "3":
            show_leaderboard()
            input("Enter للرجوع...")
        elif choice == "4":
            print("مع السلامة! 👋"); break

if __name__ == "__main__":
    main()
