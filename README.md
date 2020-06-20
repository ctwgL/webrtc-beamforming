# webrtc-beamforming
整理出来的webrtc波束模块

# 步骤

mkdir build

cd build & cmake .. & make
# 注意：CMakeList中的目录路径可能需要自己替换，/build目录中已经有了编译好的可执行文件
# 使用说明
# 若输入的语音是几通道，那么麦克风的坐标就写几个
./webrtc-bf -i input.wav -mic_positions "x1 y1 z1 x2 y2 z2" -o output.wav
