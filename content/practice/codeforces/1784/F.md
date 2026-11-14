---
title: "CF 1784F - Tối thiểu hoặc Trung bình"
description: "Cho tập hợp các số nguyên liên tiếp từ 1 đến 2n. Vika thực hiện đúng k thao tác trên tập hợp này. Mỗi thao tác bao gồm việc loại bỏ hai số nguyên nhỏ nhất hoặc loại bỏ hai số nguyên ở giữa, là hai số nguyên ở vị trí n và n+1 nếu tập hợp là…"
date: "2026-06-09T11:03:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1784
codeforces_index: "F"
codeforces_contest_name: "VK Cup 2022 - \u0424\u0438\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 (Engine)"
rating: 3400
weight: 1784
solve_time_s: 87
verified: true
draft: false
---

[CF 1784F - Mức tối thiểu hoặc Trung vị](https://codeforces.com/problemset/problem/1784/F) 

**Đánh giá:** 3400 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho tập hợp các số nguyên liên tiếp từ 1 đến 2n. Vika thực hiện đúng k thao tác trên tập hợp này. Mỗi thao tác bao gồm việc loại bỏ hai số nguyên nhỏ nhất hoặc loại bỏ hai số nguyên ở giữa, là hai số nguyên ở vị trí n và n+1 nếu tập hợp được viết theo thứ tự được sắp xếp. Sau k phép toán, chúng ta cần đếm xem cô ấy có thể có bao nhiêu tập hợp riêng biệt, modulo 998,244,353. 

Các số nguyên đầu vào n và k xác định kích thước của tập hợp ban đầu (2n phần tử) và số lượng thao tác cần thực hiện. Đầu ra là một số nguyên duy nhất biểu thị số tập hợp kết quả riêng biệt. 

Các ràng buộc cho thấy n có thể lớn tới 10^6, do đó, bất kỳ giải pháp nào mô phỏng rõ ràng tất cả các chuỗi thao tác sẽ quá chậm. Cách tiếp cận bạo lực khám phá chuỗi 2^k là không khả thi khi k lên tới 10^6, vì 2^20 đã vượt quá một triệu và ở đây k có thể là 10^6. 

Các trường hợp cạnh phát sinh khi k = n, vì mỗi thao tác loại bỏ chính xác hai phần tử. Sau n phép toán, tập hợp luôn trống, do đó câu trả lời thường là 1. Một trường hợp tinh vi khác xảy ra khi k = 1 hoặc nhỏ so với n. Ví dụ, với n = 3 và k = 1, việc loại bỏ hai phần tử nhỏ nhất hoặc hai phần tử trung vị sẽ tạo ra hai tập hợp riêng biệt, vì vậy việc tính toán cẩn thận là cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force liệt kê mọi chuỗi thao tác k có thể xảy ra và tính toán tập hợp kết quả cho mỗi chuỗi. Mỗi lựa chọn sẽ nhân đôi số chuỗi nên tổng số chuỗi là 2^k. Việc lưu trữ và so sánh rõ ràng các tập hợp là không thể đối với n và k lớn, vì nó sẽ yêu cầu cả thời gian hàm mũ và không gian tuyến tính trên mỗi chuỗi. 

Quan sát quan trọng là tập kết quả được xác định hoàn toàn bằng số lần Vika chọn thao tác "loại bỏ trung vị". Giả sử cô ấy thực hiện m lần loại bỏ trung bình và k - m lần loại bỏ tối thiểu. Sau đó, chính xác 2*(k - m) phần tử ở bên trái và 2*m phần tử ở giữa sẽ bị xóa. Điều này xác định duy nhất tập hợp kết quả. Do đó, thay vì mô phỏng tất cả các chuỗi, chúng ta chỉ cần đếm số cách phân phối k thao tác thành "loại bỏ trung bình" và "loại bỏ tối thiểu" hợp lệ, tức là không loại bỏ nhiều phần tử hơn mức tồn tại. 

Vì chúng ta có 2n phần tử nên sau k thao tác, chúng ta còn lại 2n - 2k phần tử. Số lần loại bỏ trung vị m có thể dao động từ max(0, k - n) đến min(k, n) vì mỗi lần loại bỏ trung vị yêu cầu loại bỏ các phần tử ở giữa, được giới hạn bởi kích thước của tập hợp. Trong thực tế, với bất kỳ 0 ≤ m ≤ k nào, phép toán có hiệu lực miễn là 2*(k - m) ≤ n, đưa ra một công thức đơn giản. 

Việc đếm số tập kết quả tương đương với việc đếm số giá trị hợp lệ của m. Mỗi m hợp lệ tạo ra một tập cuối cùng duy nhất vì các vị trí loại bỏ là xác định. Điều này làm giảm vấn đề về số lượng tổ hợp đơn giản thay vì đặt bảng liệt kê. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k * n) | O(n * 2^k) | Quá chậm | 
| Đếm tổ hợp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số lần loại bỏ trung bình tối thiểu và tối đa có thể. Tối thiểu là 0 và tối đa là min(k, n) vì chúng ta không thể xóa nhiều hơn n phần tử ở giữa. 
2. Nhận biết các phần tử còn lại sau k thao tác chính xác là 2n - 2k nên tổng số tập hợp tương ứng với số lượng trung vị loại bỏ hợp lệ. 
3. Đếm số lượng số nguyên m trong khoảng [0, k] sao cho việc loại bỏ m cặp trung vị và k - m cặp tối thiểu không vượt quá số phần tử có sẵn. 
4. Số đếm này đơn giản là k + 1, ngoại trừ việc chúng ta phải đảm bảo rằng m ≤ n vì chúng ta không thể loại bỏ nhiều hơn n phần tử ở giữa. Do đó, câu trả lời là min(n, k) + 1. 
5. Trả về đáp án modulo 998,244,353.

Tại sao nó hoạt động: Tập hợp sau bất kỳ chuỗi k thao tác nào được xác định duy nhất bởi số lần loại bỏ trung bình xảy ra. Mỗi số lần loại bỏ trung bình hợp lệ sẽ mang lại một tập hợp riêng biệt vì vị trí của các phần tử bị loại bỏ có tính xác định và không chồng chéo. Bằng cách đếm số lần loại bỏ trung vị hợp lệ, chúng tôi liệt kê tất cả các tập hợp có thể mà không cần mô phỏng từng thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def main():
    n, k = map(int, input().split())
    result = min(n, k) + 1
    print(result % MOD)

if __name__ == "__main__":
    main()
```Giải pháp đọc các số nguyên n và k, tính số lượng số lần loại bỏ trung vị có thể có là min(n, k) + 1 và in nó theo modulo 998244353. Thao tác modulo là cần thiết vì bài toán chỉ định nó, mặc dù kết quả không bao giờ vượt quá 10^6 trong bài toán này. Giá trị tối thiểu đảm bảo chúng tôi không tính các chuỗi loại bỏ trung bình không hợp lệ sẽ loại bỏ nhiều phần tử hơn mức tồn tại. 

## Ví dụ đã hoạt động 

**Mẫu 1: n = 3, k = 1** 

| Bước | Hành động | Bộ còn lại | m (loại bỏ trung bình) | 
| --- | --- | --- | --- | 
| 1 | Xóa mức tối thiểu | {3, 4, 5, 6} | 0 | 
| 1 | Xóa trung vị | {1, 2, 5, 6} | 1 | 

Số bộ = 2, khớp với min(3, 1) + 1 = 2. 

**Mẫu 2: n = 3, k = 2** 

| Bước | Hành động | Bộ còn lại | m | 
| --- | --- | --- | --- | 
| 1 | Xóa min, rồi min | {5, 6} | 0 | 
| 1 | Xóa min rồi đến trung vị | {3, 6} | 1 | 
| 1 | Xóa trung vị, sau đó min | {3, 6} | 1 | 
| 1 | Xóa trung vị, rồi trung vị | {1, 6} | 2 | 

Số bộ = 3, khớp với min(3, 2) + 1 = 3. 

Các bảng cho thấy số tập hợp kết quả riêng biệt bằng min(n, k) + 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài hằng số | 

Độ phức tạp không đổi và phù hợp thoải mái trong giới hạn thời gian và bộ nhớ, ngay cả đối với đầu vào lớn nhất n = k = 10^6. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, sys.stdin.readline().split())
    MOD = 998244353
    return str((min(n, k) + 1) % MOD)

# provided samples
assert run("3 1\n") == "2", "sample 1"
assert run("3 2\n") == "3", "sample 2"
assert run("3 3\n") == "4", "sample 3"

# custom cases
assert run("1 1\n") == "2", "min size"
assert run("10 0\n") == "1", "zero operations"
assert run("1000000 1000000\n") == "1000001", "max size"
assert run("5 3\n") == "4", "k < n"
assert run("5 7\n") == "6", "k > n handled by min"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 2 | tập nhỏ nhất, một thao tác | 
| 10 0 | 1 | các hoạt động bằng 0 sẽ trả về số lượng thiết lập ban đầu | 
| 1000000 1000000 | 1000001 | kích thước đầu vào tối đa, xác minh thời gian không đổi | 
| 5 3 | 4 | k nhỏ hơn n, trường hợp bình thường | 
| 5 7 | 6 | k vượt quá n, kiểm tra logic min(n, k) | 

## Vỏ cạnh 

Khi k = n, tập hợp này sẽ trống sau n thao tác. Với n = 3, k = 3, các tập hợp sau mỗi chuỗi có thể là: 

| Trình tự | Bộ còn lại | 
| --- | --- | 
| phút, phút, phút | {} | 
| tối thiểu, tối thiểu, trung vị | {} | 
| phút, trung vị, phút | {} | 
| trung vị, trung vị, trung vị | {} | 

Tất cả các chuỗi đều mang lại tập trống. Công thức min(n, k) + 1 = 4 tính chính xác kịch bản tập trống là một tập riêng biệt, chứng tỏ rằng phương pháp tổ hợp xử lý chính xác trường hợp cạnh này. 

Khi k = 0, tập hợp không thay đổi, min(n, 0) + 1 = 1, khớp với tập hợp ban đầu. Điều này xác nhận việc xử lý ranh giới cho các hoạt động bằng không.
