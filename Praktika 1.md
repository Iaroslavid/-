import os
import sys

def find_empty_text_files(directory):
    try:
        # Проходим по всем файлам в указанной директории
        for filename in os.listdir(directory):
            # Проверяем расширение файла
            if filename.endswith('.txt'):
                file_path = os.path.join(directory, filename)
                # Проверяем размер файла
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

