Важливо: документація по роботі із pre-commits: https://pre-commit.com/

Загальний алгоритм може бути таким у нашому форматі:
1. pip install pre-commit
2. Створюємо .pre-commit-config.yaml файл у нашому репозиторії та налаштовуємо hooks.
3. Виконуємо команду pre-commit install - встановлюємо hooks у цьому репозиторії
4. Тоді алгоримт роботи звичайний:
	- git add .
	- git commit -m "message"
	- Якщо потрібно закомітити без перевірки, тоді:
		git commit --no-verify -m "message"
