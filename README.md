#!/bin/bash 
 
read -p "Введіть шлях до каталогу: " dir_path 
 
# Проверка существования директории
if [ ! -d "$dir_path" ]; then 
    echo "Помилка: Каталог не існує!" 
    exit 1 
fi 
 
read -p "Введіть максимальний розмір файлів (наприклад, 10M): " file_size 
 
echo "Пошук файлів більше $file_size у каталозі $dir_path..." 

# Используем find для поиска файлов и обрабатываем их в цикле
find "$dir_path" -type f -size +"$file_size" -print0 | while IFS= read -r -d '' file; do 
    echo "Знайдено файл: $file" 
    
    # Цикл для корректной обработки ответа пользователя
    while true; do
        read -p "Видалити цей файл? (y/n): " confirm 
        case "$confirm" in
            [yY]) # Удаление файла
                rm "$file" 
                echo "Файл видалено: $file" 
                break
                ;;
            [nN]) # Пропуск файла
                echo "Файл залишено: $file" 
                break
                ;;
            *) # Неверный ввод
                echo "Невірний вибір. Введіть 'y' (так) або 'n' (ні)."
                ;;
        esac
    done
done 
 
echo "Операція завершена."
