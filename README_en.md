
# Image upload with order

## Description

This Vue.js application provides a user-friendly interface for uploading, previewing, and managing images. It utilizes the Vue 3 composition API and follows the Atomic Design principles to ensure a modular and scalable architecture. Key features include image reordering, file type and size validation, Save and error handling.

## Features

- **Image Upload**: Users can upload multiple images at once with instant previews.
- **Reordering**: Images can be reordered using a settings menu for each image or simple drag-and-drop interface.
- **File Validation**: The app validates the file type and size (up to 20MB per image) to ensure proper image uploads.
- **Error Handling**: Interactive modals provide feedback for invalid file types or sizes.
- **Save image**: Uploaded images can be saved via API with rearranged order.

## Installation

To set up the project locally, follow these steps:

1. **Clone the Repository**

   ```sh
   git clone https://github.com/shafiulshaon/vue3_image_upload_order.git
   cd vue3_image_upload_order
   ```

2. **Install Dependencies**

   ```sh
   npm install
   ```

3. **Run the Application**

   ```sh
   npm run serve
   ```

   The application will be available at `http://localhost:8080`.

## Usage

- **Uploading Images**: Click the "ファイルを選択する" button and select one or more images.
- **Removing Images**: Click the "設定" button on an image and select 左へ移動 to move the to the left, 右へ移動 to move the image to the right and "画像を削除" to remove the image.
- **Drag and Drop**: Drag and drop the images to rearrange the order.

## Contact Information

- 名前: [Shafiul Alam](shafiulshaon@gmail.com)
- プロジェクトリンク: [GitHub Repository](https://github.com/shafiulshaon/vue3_image_upload_order)
