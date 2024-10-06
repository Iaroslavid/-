## **Задание №1**
**Вывести отсортированный в алфавитном порядке список имен пользователей в файле passwd (вам понадобится grep).**

'''
grep '.*' /etc/passwd | cut -d: -f1 | sort

'''

grep '.' : Эта команда выводит все строки из файла. Шаблон '.' в grep означает "любой текст", и по сути, это эквивалент просто вывода всего содержимого файла.

cut -d: -f1: Команда cut разделяет строки по указанному разделителю (в данном случае :) и выводит только первую часть строки. Разделителем является двоеточие (:), и первая часть строки — это имя пользователя. Таким образом, cut -d: -f1 выводит список всех имен пользователей.

sort: Эта команда сортирует строки по алфавиту.
![photo_5195152696070497902_y](https://github.com/user-attachments/assets/e408c062-39ae-4d1f-8ea6-46b68bcb5fed)

## **Задание №2**
**Вывести данные /etc/protocols в отформатированном и отсортированном порядке для 5 наибольших портов, как показано в примере ниже:
'''

awk '{print $2, $1}' /etc/protocols | sort -nr | head -n 5

'''

Awk - в данном случае он читает строки из файла и выводит значения из двух полей: второе поле выводится первым, а первое — вторым.
sort -nr сортирует строки по числовому значению (ключ -n) в обратном порядке (ключ -r). Это значит, что строки будут отсортированы по убыванию значений номеров протоколов (то есть второго поля).
head -n 5 выводит первые 5 строк из результата предыдущих команд. Таким образом, из отсортированного списка протоколов будут выбраны только пять первых строк.
![photo_5195152696070497904_x](https://github.com/user-attachments/assets/3a119c8a-a133-45b2-8627-ff5547772796)

## **Задание №3**
**Написать программу banner средствами bash для вывода текстов, как в следующем примере (размер баннера должен меняться!):
'''

#!/bin/bash
text=$*
length=${#text}
for i in $(seq 1 $((length + 2))); do
    line+="-"
done
echo "+${line}+"
echo "| ${text} |"
echo "+${line}+"

'''

text=$*: Эта команда сохраняет все аргументы командной строки в переменную text. Переменная $* объединяет все аргументы в одну строку, разделенную пробелами.
length=${#text}: Здесь вычисляется длина строки, содержащей все аргументы, и сохраняется в переменной length. Это делается с помощью ${#text}, который возвращает количество символов в переменной text.
for i in  (seq1 ((length + 2))); do:
Этот цикл for создаёт линию из символов -. Команда seq 1 $((length + 2)) генерирует последовательность чисел от 1 до (length + 2), где length — это длина текста, а 2 добавляется для учёта пробелов и символов | с обеих сторон текста


![photo_5195152696070497897_x](https://github.com/user-attachments/assets/127eb1ce-81b3-4217-a683-eea4e15bd6f4)

![photo_5195152696070497895_y](https://github.com/user-attachments/assets/e4f61a28-e783-4afc-b7c6-15322c664d67)


## **Задание №4**
**Написать программу для вывода всех идентификаторов (по правилам C/C++ или Java) в файле (без повторений).
'''

#!/bin/bash
file="$1"
id=$(grep -o -E '\b[a-zA-Z]*\b' "$file" | sort -u)

'''

'''

grep -oE '\b[a-zA-Z_][a-zA-Z0-9_]*\b' hello.c | grep -vE '\b(int|void|return|if|else|for|while|include|stdio)\b' | sort | uniq

'''

file="$1": Эта строка присваивает переменной file значение первого аргумента командной строки (путь к файлу). Таким образом, файл, который нужно обработать, передается в скрипт как аргумент.
id= (grep−o−E′\b[a−zA−Z]∗\b′" file" | sort -u): grep -o -E '\b[a-zA-Z]\b' "$file": grep — команда для поиска строк, соответствующих заданному регулярному выражению. Опция -o указывает, что нужно вывести только совпадения, а не всю строку. Опция -E включает использование расширенных регулярных выражений. Регулярное выражение \b[a-zA-Z]\b ищет слова, состоящие из символов латинского алфавита. Вот что это выражение означает: \b — граница слова. [a-zA-Z]* — любое количество (включая ноль) символов от "a" до "z" (как строчные, так и заглавные буквы).
В результате grep находит и выводит все слова (последовательности букв), присутствующие в файле.

## **Задание №5**
**Написать программу для регистрации пользовательской команды (правильные права доступа и копирование в /usr/local/bin).
'''

#!/bin/bash
file=$1
chmod 755 "./$file"
sudo cp "$file" /usr/local/bin/

'''

Команда chmod 755 изменяет права доступа к файлу. В результате файл становится исполняемым для всех пользователей, но записывать его может только владелец.
Путь "./$file" означает, что изменяемые права относятся к файлу, находящемуся в текущей директории.
sudo cp "$file" 

## **Задание №6**
**Написать программу для проверки наличия комментария в первой строке файлов с расширением c, js и py.
'''
import os

def check_comment_in_first_line(file_path):

    try:
    
        with open(file_path, 'r', encoding='utf-8') as file:
        
            first_line = file.readline().strip()
            return is_comment(first_line, file_path)
    except Exception as e:
        print(f"Ошибка при обработке файла {file_path}: {e}")
        return False

def is_comment(line, file_path):

    ext = os.path.splitext(file_path)[1]
    if ext == '.c':
    
        return line.startswith('//')
    elif ext == '.py':
    
        return line.startswith('#')
    elif ext == '.js':
    
        return line.startswith('//')
    else:
    
        return False

def main(directory):

    for root, dirs, files in os.walk(directory):
        for file in files:
        
            if file.endswith(('.c', '.js', '.py')):
            
                file_path = os.path.join(root, file)
                if check_comment_in_first_line(file_path):
                
                    print(f"Комментарий найден в файле: {file_path}")
                else:
                    print(f"Комментарий не найден в файле: {file_path}")

if __name__ == '__main__':

    # Замените '.' на желаемую директорию для проверки
    main('.')
    
'''

1. Запускается функция main, которая принимает директорию для поиска файлов.
2. Используется os.walk для прохождения по всем подкаталогам и файлам в указанной директории.
3. Для каждого файла, который имеет расширение .c, .js или .py, вызывается функция check_comment_in_first_line, чтобы проверить первую строку на наличие комментария.
4. Функция is_comment определяет, является ли первая строка комментарием в зависимости от расширения файла.
5. Результат проверки выводится на экран.

## Задание №7
**Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам).
'''
import os

import hashlib

def hash_file(file_path):

    hash_MD5 = hashlib.md5() 
    try:
    
        with open(file_path, 'rb') as file:
        
            for chunk in iter(lambda: file.read(4096), b""):
            
                hash_MD5.update(chunk)
        return hash_MD5.hexdigest()
    except Exception as e:
    
        print(f"Ошибка чтения файла {file_path}: {e}")
        return None

def find_duplicates(directory):

    files_by_hash = {}  
    for root, _, files in os.walk(directory):
    
        for file in files:
        
            file_path = os.path.join(root, file)
            file_hash = hash_file(file_path)
            if file_hash:
            
                if file_hash in files_by_hash:
                
                    files_by_hash[file_hash].append(file_path)
                else:
                
                    files_by_hash[file_hash] = [file_path]
    return {k: v for k, v in files_by_hash.items() if len(v) > 1}
    
def main():

    directory = input("Введите путь к директории для поиска дубликатов: ")
    duplicates = find_duplicates(directory)
    if duplicates:
    
        print("Найдены дубликаты файлов:")
        for file_hash, files in duplicates.items():
            print(f"Хеш: {file_hash}")
            for file in files:
                print(f"  - {file}")
    else:
        print("Дубликаты файлов не найдены.")

if __name__ == '__main__':
    main()
    
'''

1. Функция hash_file открывает файл, читает его содержимое порциями и вычисляет его хеш-сумму с использованием MD5.
2. Функция find_duplicates проходит по всем файлам в указанной директории и подкаталогах, вычисляет их хеши и заполняет словарь files_by_hash, где ключ — это хеш, а значение — список путей к файлам с этим хешем.
3. В конце программа выводит файлы-дубликаты на экран, если они существуют.

## Задание №8
**Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.
'''
import sys
def replace_spaces_with_tab(input_file, output_file):

    try:
    
        with open(input_file, 'r') as infile:
        
            content = infile.read()
        content = content.replace(' ' * 4, '\t')
        with open(output_file, 'w') as outfile:
        
            outfile.write(content)      
        print(f"Файл '{input_file}' обработан и сохранен в '{output_file}'.")
    except Exception as e:
    
        print(f"Произошла ошибка: {e}")

if __name__ == '__main__':
    if len(sys.argv) != 3:
        print("Использование: python replace_spaces.py <входной_файл> <выходной_файл>")
    else:
        input_file = sys.argv[1]
        output_file = sys.argv[2]
        replace_spaces_with_tab(input_file, output_file)
        
'''
1. Импортируем sys: Для работы с аргументами командной строки.
2. Функция replace_spaces_with_tab: 
   - Читает содержимое входного файла.
   - Заменяет все последовательности из 4 пробелов на символ табуляции.
   - Записывает измененное содержимое в выходной файл.
3. Главный блок:
   - Проверяет количество аргументов.
   - Если аргументов не 3, выводит сообщение о правильном использовании.
   - Если аргументы заданы корректно, вызывает функцию замены.

## Задание №9
**Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файлы задаются аргументами.
'''
import os

import sys

def find_empty_text_files(directory):

    try:
    
        files = os.listdir(directory)
        empty_files = []
        for file in files:
        
            if file.endswith('.txt'):
            
                file_path = os.path.join(directory, file)
                if os.path.isfile(file_path) and os.path.getsize(file_path) == 0:
                    empty_files.append(file)
        if empty_files:
        
            print("Пустые текстовые файлы:")
            for empty_file in empty_files:
            
                print(empty_file)
        else:
            print("Пустые текстовые файлы не найдены.")
    except Exception as e:
    
        print(f"Произошла ошибка: {e}")

if __name__ == '__main__':

    if len(sys.argv) != 2:
    
        print("Использование: python find_empty_text_files.py <директория>")
    else:
    
        directory = sys.argv[1]
        find_empty_text_files(directory)
        
'''
1. Импортируем необходимые модули: os для работы с файловой системой и sys для работы с аргументами командной строки.
2. Функция find_empty_text_files:
   - Получает список файлов в указанной директории.
   - Для каждого файла проверяет, является ли он текстовым (по расширению .txt) и пустым (размер равен 0).
   - Если файл пустой, добавляет его имя в список empty_files.
3. Главный блок:
   - Проверяет количество аргументов.
   - Если аргументов не 2, выводит сообщение о правильном использовании.
   - Если аргументы заданы корректно, вызывает функцию поиска пустых файлов.

## Задание №10
**Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории. Директория передается в программу параметром.
'''
import os
import sys

def find_empty_text_files(directory):

    try:
    
        for filename in os.listdir(directory):
        
            if filename.endswith('.txt'):
            
                file_path = os.path.join(directory, filename)
                if os.path.isfile(file_path) and os.path.getsize(file_path) == 0:
                    print(filename)
    except Exception as e:
    
        print(f"Произошла ошибка: {e}")

if __name__ == "__main__":

    if len(sys.argv) != 2:
        print("Использование: python script.py <путь_к_директории>")
    else:
    
        dir_path = sys.argv[1]
        find_empty_text_files(dir_path)
        
'''
