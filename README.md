#!/bin/bash 
 
read -p "Введіть шлях до каталогу: " dir_path 
 
if [ ! -d "$dir_path" ]; then 
    echo "Помилка: Каталог не існує!" 
    exit 1 
fi 
 
read -p "Введіть максимальний розмір файлів (наприклад, 10M): " file_size 
 
echo "Пошук файлів більше $file_size у каталозі $dir_path..." 
find "$dir_path" -type f -size +"$file_size" -print0 | while IFS= read -r -d '' file; do 
    echo "Знайдено файл: $file" 
     
    read -p "Видалити цей файл? (y/n): " confirm 
    if [[ "$confirm" == "y" || "$confirm" == "Y" ]]; then 
        rm "$file" 
        echo "Файл видалено: $file" 
    else 
        echo "Файл залишено: $file" 
    fi 
done 
 
echo "Операція завершена."
