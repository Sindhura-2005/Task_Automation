import os
import shutil

# Define the directory to organize
directory_to_organize = r"C:\Users\DELL\OneDrive\Documents\TaskAutomation"  # Change this to your target directory

# Define file type categories
file_types = {
    'Documents': ['.pdf', '.docx', '.txt', '.xlsx', '.pptx'],
    'Images': ['.jpg', '.jpeg', '.png', '.gif', '.bmp'],
    'Audio': ['.mp3', '.wav', '.aac', '.flac'],
    'Videos': ['.mp4', '.mkv', '.avi', '.mov'],
    'Archives': ['.zip', '.tar', '.gz', '.rar'],
    'Others': []  # For files that don't fit in the above categories
}

def organize_files(directory):
    # Create directories for each file type if they don't exist
    for folder in file_types.keys():
        folder_path = os.path.join(directory, folder)
        if not os.path.exists(folder_path):
            os.makedirs(folder_path)

    # Move files into the corresponding folders
    for filename in os.listdir(directory):
        file_path = os.path.join(directory, filename)

        # Skip directories
        if os.path.isdir(file_path):
            continue

        # Get the file extension
        _, extension = os.path.splitext(filename)
        
        # Determine the correct folder
        moved = False
        for folder, extensions in file_types.items():
            if extension.lower() in extensions:
                try:
                    shutil.move(file_path, os.path.join(directory, folder, filename))
                    moved = True
                except Exception as e:
                    print(f"Error moving file {filename}: {e}")
                break
        
        # Move to 'Others' if no match found
        if not moved:
            try:
                shutil.move(file_path, os.path.join(directory, 'Others', filename))
            except Exception as e:
                print(f"Error moving file {filename} to Others: {e}")

    print("Files have been organized successfully!")

if __name__ == "__main__":
    organize_files(directory_to_organize)
