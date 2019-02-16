---
id: AndroidLayoutExample1
title: Android Layout Example
---

โปรเจคนี้เป็นโปรเจคที่ทำไว้สำหรับมือใหม่ ที่สงสัยว่าการจัด layouts ของ android นั้นทำอย่างไรและควรเลือก `view` หรือ `viewgroup` แบบไหน แต่... 

เคยไหมครับ เวลาจะแก้ layouts แต่ละตัว ทำไมหาไฟล์ยากเหลือเกิน หรือกว่าจะหาส่วนที่ต้องแก้เจอก็เสียเวลาไล่โค้ดไปนานมากๆๆ

ถ้าเคยเรามาลองแยก res ของ android ให้เป็นส่วนๆเพื่อง่ายต่อการหา นอกจากนี้เรามาลองเขียน layouts ในแบบ components style เพื่อง่ายต่อการเรียกใช้และแก้ไขกันดีกว่า

![Image2](./assets/AndroidLayoutExample1/img2.png)

Clone this project [Github](https://github.com/champunderscore/Android.Layouts)

## Getting Started

 เริ่มต้นก็ไม่มีอะไรมากครับ เราจะแยก folders ให้หน้าตาประมาณนี้ 

![Image1](./assets/AndroidLayoutExample1/img1.png)


โดยแนวคิดก็คือถ้าใน layout นั้นมีการเรียก values อย่างเช่น `@string, @style` หรือค่าอื่นๆที่ใช้ภายใน layout นั้นเท่านั้น เราก็จะเก็บไว้ใน folder values ของแต่ละอัน แต่ถ้ามันเป็น value ที่ต้องใช้หลายๆที่ก็จะเอามาเก็บไว้ใน folder values ปกติ 

> ข้อควรระวังคือการตังชื่อตัวแปร ชื่อต้องไม่ซำ้กันนะครับ 
```
<string name="user_name">Alison Dan</string>
```

#### build.gradle

โดยวิธีการแยก res ก็คือเราสามารถไปตั้งใน gradle แบบนี้ได้เลย (ข้อเสียคือต้องเพิ่ม folders ใหม่เอง)

```js
    sourceSets {
        main {
            res.srcDirs = [
                    'src/main/res/layouts/layout_excel',
                    'src/main/res/layouts/layout_youtube',
                    'src/main/res/layouts/layout_joox_player',
                    'src/main/res/layouts/layout_sound_cloud',
                    'src/main/res/layouts/layout_profile',
                    'src/main/res/layouts/layout_instagram',
                    'src/main/res/layouts/layout_spotify_browse',
                    'src/main/res/layouts/layout_spotify_playlist',
                    'src/main/res/layouts',
                    'src/main/res'
            ]
        }
    }

```

## Layout Types

มาลองนิยามชนิดของ layouts หน่อยดีกว่า

| Options           | Descriptions             |
| ----------------- | ------------------------ |
| Component         | ง่ายๆก็คือ widget ต่างๆ ที่เอามารวมกันให้เป็นอะไรซักอย่างนึง เช่น ปุ่ม Like อาจจะเกิดจาก icon + text อะไรแบบนี้    |
| Container         | ก็คือการเอา Component มารวมกัน เช่นเรามีปุ่ม like, dislike ก็เอามารวมกันเป็น bar หนึ่งอัน  |
| Activity/Fragment | จากนั้นก็เอา container มาประกอบกันให้เป็น Activity/Fragment ก็เป็นอันเสร็จ |


> ความจริงถ้าคิดว่าการทำ component เป็นเรื่องยุ่งยากก็เอาแค่ container ก็ได้นะ

## Example

![Image3](./assets/AndroidLayoutExample1/img3.png)

ตัวอย่างก็ประมาณนี้ครับ โดย component ผมจะตั้งชื่อ + ต่อท้าย เช่น list, text, playlist อะไรแบบนี้ ส่วน container ก็ตั้งซื่อแบบโต้งๆ ไปเลย ส่วน activity/fragment ก็ตั้งโต้งๆเหมือน container ก็ทำให้เข้าใจง่ายดี


ตัวอย่างโค้ด

#### spotify_browser_container.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <include
        android:id="@+id/spotify_browse_toolbar"
        layout="@layout/spotify_browse_toolbar"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <include
        android:id="@+id/spotify_playlist"
        layout="@layout/spotify_playlist"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintStart_toStartOf="@+id/spotify_browse_toolbar"
        app:layout_constraintTop_toBottomOf="@+id/spotify_browse_toolbar" />

</android.support.constraint.ConstraintLayout>

```

#### activity_spotify_browser.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:openDrawer="start">

    <include
        layout="@layout/spotify_browse_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <android.support.design.widget.NavigationView
        android:id="@+id/nav_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:fitsSystemWindows="true"
        app:headerLayout="@layout/nav_header_main"
        app:menu="@menu/activity_sptify_playlist_drawer" />

</android.support.v4.widget.DrawerLayout>

```


## Switch Activity

พอดีผมไม่ได้แยก ใน manifest ไว้ให้ถ้าอยากลอง run ต้องมาสลับเอาเองนะค๊าบบบ

```xml
 <activity android:name=".SpotifyPlaylistActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

ในตอนนี้ก็จบเพียงเท่านี้ หวังว่าจะเป็นประโยชน์