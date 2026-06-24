# Face Recognition System

Ứng dụng nhận diện khuôn mặt dùng webcam để đăng ký người dùng trước và nhận diện trực tiếp từ camera. Khi camera phát hiện khuôn mặt không trùng với dữ liệu đã đăng ký, chương trình hiển thị nhãn `Nguoi_la` và phát file nhạc cảnh báo có sẵn trong project.

## Chức năng chính

- Đăng ký khuôn mặt mới từ webcam.
- Lưu ảnh người đăng ký vào thư mục `Nguoi_dang_ky/<ten_nguoi_dung>/`.
- Tạo và cập nhật dữ liệu embedding trong `embeddings_database.pkl`.
- Nhận diện khuôn mặt trực tiếp qua camera.
- Vẽ khung xanh cho người đã đăng ký và khung đỏ cho người lạ.
- Phát file MP3 cảnh báo khi phát hiện người lạ.

## Cấu trúc project

```text
Face-Recognition-System-Project-master/
├── main.py
├── registration.py
├── recognition.py
├── embeddings_database.pkl
├── dlib_face_recognition_resnet_model_v1.dat
├── shape_predictor_68_face_landmarks.dat
├── Nguoi-La-Oi- Karik x Orange x Superbrothers.mp3
└── Nguoi_dang_ky/
    └── Khang/
        ├── Khang_1.jpg
        ├── Khang_2.jpg
        ├── Khang_3.jpg
        ├── Khang_4.jpg
        └── Khang_5.jpg
```

## Yêu cầu

- Python 3.9 hoặc 3.10 được khuyến nghị, vì `dlib`, `facenet-pytorch` và các thư viện AI thường dễ cài hơn trên các phiên bản này.
- Webcam hoạt động.
- Hệ điều hành Windows, macOS hoặc Linux.
- Các file model `.dat` phải nằm cùng thư mục với `main.py`.
- Lần đầu chạy nhận diện bằng FaceNet có thể cần internet để tải trọng số `vggface2`, trừ khi model đã được cache sẵn.

## Cài đặt

Mở terminal tại thư mục chứa `main.py`, sau đó tạo môi trường ảo:

```powershell
python -m venv .venv
.\.venv\Scripts\activate
python -m pip install --upgrade pip
```

Cài các thư viện cần thiết:

```powershell
pip install opencv-python dlib numpy scipy torch torchvision facenet-pytorch mtcnn tensorflow
```

Nếu cài `dlib` bị lỗi trên Windows, hãy cài thêm CMake và Visual Studio Build Tools, hoặc dùng Anaconda/Miniconda để cài `dlib` dễ hơn.

## Cách chạy

Chạy chương trình chính:

```powershell
python main.py
```

Menu sẽ hiển thị:

```text
1. Đăng ký khuôn mặt mới
2. Nhận diện khuôn mặt
3. Thoát
```

## Đăng ký khuôn mặt mới

Chọn `1`, nhập tên người cần đăng ký, sau đó nhìn vào webcam.

Quy trình đăng ký:

- Chương trình kiểm tra khuôn mặt hiện tại có trùng với dữ liệu đã có hay không.
- Nếu đã tồn tại, chương trình hỏi xác nhận và không đăng ký trùng.
- Nếu chưa tồn tại, chương trình chụp 5 mẫu khuôn mặt.
- Ảnh được lưu vào `Nguoi_dang_ky/<ten>/`.
- Embedding được lưu vào `embeddings_database.pkl`.

Nhấn `q` trong cửa sổ camera để thoát sớm.

## Nhận diện khuôn mặt

Chọn `2` để mở webcam và nhận diện trực tiếp.

Khi nhận diện:

- Người đã đăng ký sẽ có khung xanh và hiển thị tên.
- Người lạ sẽ có khung đỏ và nhãn `Nguoi_la`.
- Khi gặp người lạ, chương trình phát file `Nguoi-La-Oi- Karik x Orange x Superbrothers.mp3`.
- Âm thanh cảnh báo có thời gian chờ 10 giây để tránh mở file nhạc liên tục theo từng frame.

Nhấn `q` để thoát chế độ nhận diện.

## Dữ liệu khuôn mặt

Thư mục `Nguoi_dang_ky` chứa dữ liệu ảnh theo từng người:

```text
Nguoi_dang_ky/
└── TenNguoiDung/
    ├── TenNguoiDung_1.jpg
    ├── TenNguoiDung_2.jpg
    └── ...
```

Không nên đổi tên hoặc xóa thủ công các file trong thư mục này khi chương trình đang chạy.

## Cấu hình quan trọng

Trong `registration.py`:

- `REGISTERED_DIR`: thư mục lưu ảnh người đăng ký.
- `SHAPE_PREDICTOR_PATH`: model landmark 68 điểm của dlib.
- `FACE_RECOGNITION_MODEL_PATH`: model nhận diện khuôn mặt của dlib.
- `DATABASE_PATH`: file lưu embedding.

Trong `recognition.py`:

- `REGISTERED_DIR`: thư mục ảnh người đã đăng ký.
- `ALERT_SOUND_PATH`: file nhạc cảnh báo người lạ.
- `ALERT_COOLDOWN_SECONDS`: thời gian chờ giữa hai lần phát nhạc.
- `THRESHOLD`: ngưỡng khoảng cách để quyết định khuôn mặt có trùng hay không.

## Lỗi thường gặp

### `ModuleNotFoundError`

Thiếu thư viện Python. Hãy kích hoạt môi trường ảo và chạy lại lệnh cài đặt dependency.

### Không truy cập được webcam

Kiểm tra:

- Webcam có đang được ứng dụng khác sử dụng không.
- Quyền camera của hệ điều hành đã bật chưa.
- Camera mặc định có đúng là thiết bị số `0` không.

### File nhạc không phát

Kiểm tra:

- File MP3 còn nằm trong thư mục project.
- Máy có ứng dụng mặc định để mở file `.mp3`.
- Âm lượng hệ thống không bị tắt.

### Nhận diện sai hoặc không ổn định

Có thể thử:

- Đăng ký thêm ảnh với nhiều góc mặt và điều kiện ánh sáng khác nhau.
- Điều chỉnh `THRESHOLD` trong `recognition.py`.
- Đảm bảo camera đủ sáng và khuôn mặt không bị che.

## Ghi chú phát triển

Project đã được chỉnh để dùng đường dẫn tương đối theo vị trí của file script, vì vậy không còn phụ thuộc vào đường dẫn tuyệt đối trên một máy cụ thể. Chỉ cần giữ các file model, file MP3 và thư mục `Nguoi_dang_ky` trong cùng thư mục project là có thể di chuyển project sang máy khác dễ hơn.
