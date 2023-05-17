# Flutter Advanced Image Picker

Flutter plugin for selecting multiple images from the Android and iOS image library, taking new pictures with the camera, and edit them before using such as rotating, cropping, adding sticker/filters.

This is an advanced version of image_picker plugin.

## Key Features

1. Display live camera preview in a widget

2. Adjust exposure

3. Zoom camera preview

4. Capture photo without saving into device library

5. Capture with preview size mode & full screen size mode

6. Select photos from device library by browsing photo albums

7. Preview selected images

8. Support button label & text translation

9. Easy image editing features, such as rotation, cropping, adding sticker/filters

10. Allow user add external image editors for editing selected images.

11. Support for displaying object detection & OCR result on selected image view

12. Allow setting user's own stickers

## Installation

### Dart

This plugin requires dart version 2.16.1 or later

### iOS

Add these settings to the ios/Runner/Info.plist

   ```
   <key>NSCameraUsageDescription</key>
<string>Can I use the camera please?</string>
<key>NSMicrophoneUsageDescription</key>
<string>Can I use the mic please?</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>App need your agree, can visit your album</string>
   ```

Modify TOCropViewController version in snapshot (ios/Podfile.lock) to 2.6.1

   ```
   TOCropViewController (2.6.1)
   ```

### Android

Change the minimum Android sdk version to 21 (or higher), and compile sdk to 31 (or higher) in your android/app/build.gradle file (refer example project).

   ```
   compileSdkVersion 31
minSdkVersion 21
   ```

Change kotlin version to 1.5.21 or later in your android/build.gradle file (refer example project).

   ```
   ext.kotlin_version = '1.5.21'
   ```

Add activity and uses-permissions to your AndroidManifest.xml, just like next.

   ```
   <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="vn.weta.freemarimagepickerexample">

    <application
        android:name="io.flutter.app.FlutterApplication"
        android:label="freemarimagepicker_example"
        android:requestLegacyExternalStorage="true"
        android:icon="@mipmap/ic_launcher">
        <activity
            android:name="com.yalantis.ucrop.UCropActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>
    </application>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.CAMERA" />
</manifest>
   ```

## Usages

Add to pubspec

   ```
   dependencies:
  advance_image_picker: $latest_version
   ```

Import dart code

   ```
   import 'package:advance_image_picker/advance_image_picker.dart';
   ```

Setting configs & text translate function

   ```
   // Setup image picker configs (global settings for app)
var configs = ImagePickerConfigs();
configs.appBarTextColor = Colors.black;
configs.stickerFeatureEnabled = false; // ON/OFF features
configs.translateFunc = (name, value) => Intl.message(value, name: name); // Use intl function

// Add plugin's string inside lib/l10n/intl_**.arb of your project to use translation
// For example about english messages, declared inside intl_en.arb
{
  "app_title": "FreeMar - Share your things",
  "image_picker_exit_without_selecting": "Do you want to exit without selecting images?",
  "image_picker_select_images_title": "Selected images count",
  "image_picker_select_images_guide": "You can drag images for sorting list...",
  "image_picker_camera_title": "Camera",
  "image_picker_album_title": "Album",
  "image_picker_preview_title": "Preview",
  "image_picker_confirm_delete": "Do you want to delete this image?",
  "image_picker_confirm_reset_changes": "Do you want to clear all changes for this image?",
  "image_picker_no_images": "No images...",
  "image_picker_image_crop_title": "Image crop",
  "image_picker_image_filter_title": "Image filter",
  "image_picker_image_edit_title": "Image edit",
  "image_picker_select_button_title": "Select",
  "image_picker_image_sticker_title": "Image sticker",
  "image_picker_image_addtext_title": "Image add text",
  "image_picker_image_sticker_guide": "You can click on below icons to add into image, double click to remove it from image",
  "image_picker_edit_text": "Edit text",
  "suitcase": "Suitcase",
  "handbag": "Handbag",
  "bottle": "Bottle",
  "tie": "Tie",
  ...
}

// Or you can simply write your own translate function like this
// configs.translateFunc = (name, value) => Intl.message(value, name: name);
   configs.translateFunc = (name, value) {
     switch (name) {
       case 'image_picker_select_images_title':
         return 'Le immagini selezionate contano';
       case 'image_picker_select_images_guide':
         return 'È possibile trascinare le immagini per lelenco di ordinamento...';
       case 'image_picker_camera_title':
         return 'Telecamera';
       case 'image_picker_album_title':
         return 'Album';
       // ... Declare all your strings here
       default:
         return value;
     }
   }
   ```

Sample for adding external image editors.
(You can replace ImageEdit widget by your owned image editor widget, that output file after editing finised)

   ```
   configs.externalImageEditors['external_image_editor_1'] = EditorParams(
      title: 'external_image_editor_1',
      icon: Icons.edit_rounded,
      onEditorEvent: (
              {required BuildContext context,
              required File file,
              required String title,
              int maxWidth = 1080,
              int maxHeight = 1920,
              int compressQuality = 90,
              ImagePickerConfigs? configs}) async => await Navigator.of(context).push(MaterialPageRoute<File>(
              fullscreenDialog: true,
              builder: (context) => ImageEdit(file: file, title: title, maxWidth: maxWidth, maxHeight: maxHeight, configs: configs)))
  );
   ```

Sample for usage

   ```
   // Get max 5 images
List<ImageObject> objects =
    await Navigator.of(context).push(
        PageRouteBuilder(pageBuilder:
            (context, animation, __) {
  return ImagePicker(maxCount: 5);
}));

if (objects.length > 0) {
  setState(() {
    imageFiles.addAll(objects
        .map((e) => e.modifiedPath)
        .toList());
  });
}
   ```





