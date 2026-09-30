# -Wall--.-damage--dataset--preprocessing.-ipynb
 A wall  damage image dataset for classification of different types of Wall damage 


from google.colab import drive
drive.mount('/content/drive')


# ============================================================
# 2. IMPORTS
# ============================================================

import os


# ============================================================
# 3. DATASET PATH
# ============================================================

dataset_path = "/content/drive/MyDrive/Wall_Damage_Dataset"


# ============================================================
# 4. IMAGE EXTENSIONS
# ============================================================

image_extensions = (
    ".jpg",
    ".jpeg",
    ".png",
    ".webp",
    ".bmp",
    ".heic",
    ".heif"
)


# ============================================================
# 5. RENAME IMAGES INSIDE THE SAME DATASET
# ============================================================

total_images = 0

for class_name in sorted(os.listdir(dataset_path)):

    class_path = os.path.join(
        dataset_path,
        class_name
    )

    # Skip files
    if not os.path.isdir(class_path):
        continue

    # Convert class name to filename format
    # Crack -> crack
    # Peeling_Paint -> peeling_paint
    # Structural Damage -> structural_damage

    label = class_name.lower().replace(" ", "_")

    # Find images
    images = [
        file
        for file in os.listdir(class_path)
        if file.lower().endswith(image_extensions)
    ]

    images.sort()

    print(f"\n{class_name}: {len(images)} images")

    # --------------------------------------------------------
    # STEP 1:
    # Temporarily rename everything first.
    #
    # This prevents filename conflicts.
    # --------------------------------------------------------

    temporary_files = []

    for number, filename in enumerate(images, start=1):

        old_path = os.path.join(
            class_path,
            filename
        )

        temp_name = f"__temp_{number:06d}__{filename}"

        temp_path = os.path.join(
            class_path,
            temp_name
        )

        os.rename(old_path, temp_path)

        temporary_files.append(
            (temp_path, number, filename)
        )

    # --------------------------------------------------------
    # STEP 2:
    # Give final names
    # --------------------------------------------------------

    for temp_path, number, old_filename in temporary_files:

        extension = os.path.splitext(
            old_filename
        )[1].lower()

        new_filename = (
            f"{label}_{number:03d}{extension}"
        )

        new_path = os.path.join(
            class_path,
            new_filename
        )

        os.rename(
            temp_path,
            new_path
        )

        total_images += 1


# ============================================================
# 6. FINAL RESULT
# ============================================================

print("\n" + "=" * 50)
print("RENAMING COMPLETE")
print("=" * 50)

print("Total images renamed:", total_images)
print("Dataset:", dataset_path)

print("=" * 50)
 