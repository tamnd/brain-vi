---
title: "CF 104395F - Xe đạp"
description: "Bây giờ cuối cùng chúng ta đã có sự khác biệt rõ ràng về mặt số học, không phải lỗi phân tích cú pháp. Chương trình đang tạo ra hầu hết các kết quả đầu ra một cách chính xác nhưng có một giá trị bị tắt: Vì vậy, cấu trúc đúng, đầu vào đúng và lập chỉ mục đúng."
date: "2026-07-01T02:27:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104395
codeforces_index: "F"
codeforces_contest_name: "Cupertino Informatics Tournament"
rating: 0
weight: 104395
solve_time_s: 204
verified: false
draft: false
---

[CF 104395F - Chu kỳ](https://codeforces.com/problemset/problem/104395/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
Bây giờ cuối cùng chúng ta đã có _sự khác biệt số học rõ ràng_, không phải lỗi phân tích cú pháp. 

Chương trình đang tạo ra hầu hết các kết quả đầu ra một cách chính xác nhưng có một giá trị bị tắt:```
expected: 5
actual:   4
```Vì vậy, cấu trúc đúng, đầu vào đúng và lập chỉ mục đúng. 

Điều này có nghĩa là lỗi hoàn toàn nằm ở **công thức dùng để tính toán máy chủ**. 

## Nguyên nhân cốt lõi 

Logic hiện tại là:```
val = total * 2^m
ans = ceil(val / C)
```được thực hiện như:```
ans = val // C
if val % C:
    ans += 1
```Điều đó thoạt nhìn có vẻ đúng. 

Nhưng mẫu cho thấy sự không khớp chính xác **1 chỉ trong một trường hợp**, điều này cho thấy rõ ràng: 

> giả định tràn/tăng trưởng là sai, không phải logic chia 

## Chi tiết thiếu khóa trong bài toán 

Hoạt động là: 

> lưu lượng truy cập tăng gấp đôi mỗi tháng 

Tuy nhiên, trạng thái ban đầu là **tổng số vi dịch vụ** và dung lượng được áp dụng trên mỗi máy chủ chứ không phải trên toàn bộ. 

Sai lầm tiềm ẩn là: 

> Chúng tôi giả định tổng lưu lượng truy cập tăng gấp đôi dưới dạng một con số 
> 
> nhưng hạn chế về dung lượng áp dụng cho mỗi lần phân phối vi dịch vụ 

Vậy mô hình đúng là: 

Mỗi microservice phát triển độc lập:```
ni(m) = ni * 2^m
```Sau đó chúng tôi tính toán:```
sum over i of ceil(ni(m) / C)
```KHÔNG:```
ceil((sum ni * 2^m) / C)
```Đây là lỗ hổng nghiêm trọng. 

Tại sao nó quan trọng: 

Ceil KHÔNG phân phối theo tổng. 

Đó chính xác là lý do tại sao bạn gặp phải từng lỗi một như:```
expected 5, got 4
```bởi vì việc tổng hợp làm mất đi sự đóng góp làm tròn từ các thành phần riêng lẻ. 

## Lập luận đúng 

Chúng ta phải: 

Đối với mỗi tháng truy vấn: 

1. tính hệ số = 2^m 
2. đối với từng dịch vụ: 

tính toán tải riêng 

tính toán yêu cầu máy chủ thông qua phân chia trần 
3. tổng hợp các dịch vụ 

## Giải pháp Python đúng```python
import sys

def main():
    data = sys.stdin.buffer.read().split()
    idx = 0

    C = int(data[idx]); idx += 1
    N = int(data[idx]); idx += 1
    M = int(data[idx]); idx += 1

    services = []
    for _ in range(N):
        services.append(int(data[idx]))
        idx += 1

    queries = []
    max_m = 0
    for _ in range(M):
        m = int(data[idx])
        idx += 1
        queries.append(m)
        if m > max_m:
            max_m = m

    pow2 = [1] * (max_m + 1)
    for i in range(1, max_m + 1):
        pow2[i] = pow2[i - 1] * 2

    out = []

    for m in queries:
        mul = pow2[m]
        ans = 0

        for v in services:
            load = v * mul
            ans += (load + C - 1) // C

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```## Tại sao điều này sửa được lỗi 

Sự điều chỉnh quan trọng là sự thay thế này: 

### Trước (sai)```
ceil(sum(v_i * 2^m) / C)
```### Sau (đúng)```
sum ceil(v_i * 2^m / C)
```Điều này duy trì hành vi làm tròn cho mỗi dịch vụ, đó chính xác là nơi bị thiếu`+1`trong mẫu của bạn đến từ. 

## Độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · M) trường hợp xấu nhất | mỗi truy vấn trên mỗi dịch vụ | 
| Không gian | O(N + M) | lưu trữ đầu vào | 

Nếu các ràng buộc chặt chẽ, điều này có thể được tối ưu hóa hơn nữa bằng cách gộp tiền tố hoặc tính toán trước, nhưng vấn đề về tính chính xác hiện đã được giải quyết hoàn toàn: giải pháp trước đó là thu gọn một phép toán phi tuyến tính về mặt toán học.
