import shutil
from pathlib import Path
from datetime import datetime

def create_backup(source):
    source = Path(source).expanduser()

    if not source.exists() or not source.is_dir():
        print("Error: Folder does not exist.")
        return

    timestamp = datetime.now().strftime("%Y-%m-%d_%H-%M-%S")
    backup_folder = Path("backups") / f"backup_{timestamp}"

    backup_folder.mkdir(parents=True, exist_ok=True)

    copied = 0

    for file in source.rglob("*"):
        if file.is_file():
            relative_path = file.relative_to(source)
            destination = backup_folder / relative_path

            destination.parent.mkdir(
                parents=True,
                exist_ok=True
            )

            shutil.copy2(file, destination)
            copied += 1
            print(f"Copied: {relative_path}")

    print("\nBackup completed!")
    print(f"Files copied: {copied}")
    print(f"Backup location: {backup_folder.resolve()}")

def main():
    print("=== PYTHON FILE BACKUP TOOL ===")
    print("Enter the folder path you want to back up.")

    folder = input("Folder path: ").strip().strip('"')

    if not folder:
        print("Please enter a folder path.")
        return

    create_backup(folder)

if __name__ == "__main__":
    main()
