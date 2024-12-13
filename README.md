import random
import time
import fractions

def generate_question(level, operation_choice):
    """Генерирует математический вопрос в зависимости от уровня сложности и выбранных операций."""
    if operation_choice == 'all':
        operation = random.choice(['+', '-', '*', '/'])
    else:
        operation = operation_choice

    if operation in ['+', '-', '*']:
        num1 = random.randint(1, 10 + 5 * level)
        num2 = random.randint(1, 10 + 5 * level)
    else:
        num2 = random.randint(1, 10 + 5 * level)
        num1 = num2 * random.randint(1, 10)  # Гарантируем целочисленный результат

    if level == 2:
        if random.choice([True, False]):
            num1 = fractions.Fraction(random.randint(1, 10), random.randint(1, 10))
            num2 = fractions.Fraction(random.randint(1, 10), random.randint(1, 10))
    elif level == 3:
        if random.choice([True, False]):
            num1 = fractions.Fraction(random.randint(1, 10), random.randint(1, 10))
            num2 = random.randint(1, 10 + 5 * level)
        else:
            num1 = random.randint(1, 10 + 5 * level)
            num2 = fractions.Fraction(random.randint(1, 10), random.randint(1, 10))

    if operation == '+':
        answer = num1 + num2
    elif operation == '-':
        answer = num1 - num2
    elif operation == '*':
        answer = num1 * num2
    else:
        answer = num1 / num2

    return num1, operation, num2, answer

def math_game():
    print("Добро пожаловать в математическую игру!")
    
    while True:
        level = int(input("Выберите уровень сложности (1-3): "))
        if level in [1, 2, 3]:
            break
        print("Пожалуйста, выберите уровень от 1 до 3.")

    operation_choice = input("Выберите операции (введите 'all' для всех, '+', '-', '*' или '/'): ").strip().lower()
    if operation_choice not in ['+', '-', '*', '/', 'all']:
        print("Выбрана некорректная операция, будет использован режим 'all'.")

    print(f"Выбранный уровень: {level}.")
    print("Я буду задавать вам вопросы.")
    print("Введите 'exit' для выхода из игры.")
    
    score = 0
    question_count = 0
    rounds = 5  

    for _ in range(rounds):
        num1, operation, num2, correct_answer = generate_question(level, operation_choice)
        question_count += 1
        start_time = time.time()
        
        user_input = input(f"Вопрос {question_count}: {num1} {operation} {num2} = ")
        end_time = time.time()

        if user_input.lower() == 'exit':
            break

        try:
            user_answer = fractions.Fraction(user_input)
            time_taken = end_time - start_time  

            if time_taken > 15:  
                print("Время вышло! ⏰")
                continue
            
            if user_answer == correct_answer:
                print("Правильно! Молодец! 👍")
                score += 2  
            else:
                print(f"Неправильно. Правильный ответ: {correct_answer}.")
                score -= 1  # Штраф за неправильный ответ

            # Вводим случайные события
            if random.random() < 0.2:  # 20% вероятность мошенничества
                print("Попробуйте случайный ответ на следующий вопрос! 🃏")
        except ValueError:
            print("Пожалуйста, введите корректный ответ или 'exit' для выхода.")

    print(f"Вы завершили игру. Ваш счёт: {score}/{question_count * 2}.")

if __name__ == "__main__":
    math_game()

