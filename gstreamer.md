## 安裝gstreamer
```
apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio
```

## 安裝opencv所需library
```
apt-get install libgtk2.0-dev pkg-config
```

## 編譯opencv支援gstreamer
從[官網](https://opencv.org/releases/)下載source code,解壓縮後到目錄中建立build目錄
```
unzip opencv-4.5.4.zip
cd opencv-4.5.4
mkdir build
cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D INSTALL_PYTHON_EXAMPLES=OFF \
-D INSTALL_C_EXAMPLES=OFF \
-D PYTHON_EXECUTABLE=$(which python3) \
-D BUILD_opencv_python2=OFF \
-D CMAKE_INSTALL_PREFIX=$(python3 -c "import sys; print(sys.prefix)") \
-D PYTHON3_EXECUTABLE=$(which python3) \
-D PYTHON3_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
-D PYTHON3_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
-D WITH_GSTREAMER=ON \
-D BUILD_EXAMPLES=OFF ..
```
確定GStreamer & Python3的部分是有被正確設定。接著執行make
```
make -j$(nproc)
```
編譯完成後執行安裝命令
```
sudo make install
sudo ldconfig
```
接著我們執行以下命令確定編譯完成的opencv有支援GStreamer
```
python3 -c "import cv2; print(cv2.getBuildInformation())"
```

## Reference
https://www.twblogs.net/a/5c7b865ebd9eee3399189fc6
https://github.com/jetsonhacks/buildOpenCVTX2
https://github.com/opencv/opencv/issues/14205
https://stackoverflow.com/questions/54095699/install-gstreamer-support-for-opencv-python-package/54108247#54108247
https://galaktyk.medium.com/how-to-build-opencv-with-gstreamer-b11668fa09c
https://qengineering.eu/install-opencv-4.5-on-jetson-nano.html
