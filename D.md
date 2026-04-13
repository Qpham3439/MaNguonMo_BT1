# D. (Bonus - không bắt buộc)
tạo thư mục ./myapi

tạo file ./myapi/app.py sử dụng Python + Flask để tính chỉ số BMI dựa rheo cân nặng và chiều cao.
<img width="1919" height="1077" alt="image" src="https://github.com/user-attachments/assets/3f244443-c7a2-42d1-ab4d-28c88e3664c2" />

```
from flask import Flask, request

app = Flask(__name__)

@app.route("/")
def home():
    return "API tính BMI 😎"

@app.route("/bmi")
def bmi():
    try:
        # lấy dữ liệu từ URL
        weight = float(request.args.get("weight"))
        height = float(request.args.get("height"))

        # tính BMI
        bmi = weight / (height ** 2)

        # phân loại
        if bmi < 18.5:
            status = "Gầy"
        elif bmi < 25:
            status = "Bình thường"
        elif bmi < 30:
            status = "Thừa cân"
        else:
            status = "Béo"

        return f"BMI = {bmi:.2f} ({status})"

    except:
        return "Lỗi! Dùng dạng: /bmi?weight=60&height=1.7"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=9630)

```

- tạo file ./myapi/requirements.txt chứa các thư viện mà app.py sử dụng (theo như app.py ví dụ thì requirements.txt chỉ cần có nội dung: flask)
- tạo file ./myapi/Dockerfile để khai báo sử dụng Python 3.9 slim
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/3e43344c-6525-45b6-bf81-2a2bc8bf6aaf" />

```
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 9630

CMD ["python", "app.py"]

```

Sửa đổi docker-compose để sử dụng myapp

Thêm phần cấu hình myapi:

<img width="1181" height="786" alt="image" src="https://github.com/user-attachments/assets/d8ed4a3f-f709-40b5-88de-6cfd5da07c68" />

```
  myapi:
    build: ./myapi
    container_name: myapi
    ports:
      - "9630:9630"
    restart: always
```

Sửa đổi nginx/nginx.conf để /api trỏ tới service myapp cổng 9630

<img width="1083" height="743" alt="image" src="https://github.com/user-attachments/assets/58a63b40-81ab-42d1-83c3-c3e0867b04d1" />
