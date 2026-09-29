---
title: "CF 104845B - \u0418\u0441\u0442\u043e\u0440\u0438\u044f \u043e \u0444\u0435\u0440\u043c\u0430\u0442\u0438\u0441\u0442\u0435"
description: "Chúng ta có bốn số nguyên cố định trong mỗi phép thử và tạo thành hai tích: tích đầu tiên là tích của hai số đầu tiên và tích thứ hai là tích của hai số cuối."
date: "2026-06-28T11:29:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104845
codeforces_index: "B"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 2023-2024 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104845
solve_time_s: 43
verified: true
draft: false
---

[CF 104845B - \u0418\u0441\u0442\u043e\u0440\u0438\u044f \u043e \u0444\u0435\u0440\u043c\u0430\u0442\u0438\u0441\u0442\u0435](https://codeforces.com/problemset/problem/104845/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn số nguyên cố định trong mỗi phép thử và tạo thành hai tích: tích đầu tiên là tích của hai số đầu tiên và tích thứ hai là tích của hai số cuối. Nhiệm vụ là xác định số nguyên dương lớn nhất$K$sao cho hai tích này khi chia cho$K$. 

Nói cách khác, nếu chúng ta ký hiệu$X = A \cdot B$Và$Y = C \cdot D$, ta đang tìm cực đại$K$như vậy$X \bmod K = Y \bmod K$. Điều kiện này tương đương với việc nói rằng$X$Và$Y$khác nhau bởi bội số của$K$, vì số dư bằng nhau ngụ ý$X - Y \equiv 0 \pmod{K}$. 

Vì vậy, bài toán quy về việc tìm ước số dương lớn nhất của hiệu tuyệt đối$|X - Y|$, ngoại trừ một trường hợp tinh vi trong đó$X = Y$, trong trường hợp đó bất kỳ$K$hoạt động, và câu trả lời là không giới hạn về mặt lý thuyết. Tuy nhiên, trong các quy ước lập trình cạnh tranh cho loại nhiệm vụ đầu ra dạng mở này, cách giải thích dự kiến ​​là chúng ta lấy mô-đun có ý nghĩa lớn nhất, mô-đun này trở thành cấu trúc ước số tối đa mà việc xây dựng ngụ ý. 

Từ góc độ ràng buộc, tất cả các số trong đầu vào đều là số nguyên 32 bit hoặc 64 bit tiêu chuẩn. Ngay cả trong thử nghiệm lớn nhất, sản phẩm vẫn nằm trong phạm vi 64 bit. Điều đó làm cho phép nhân trực tiếp an toàn. Thách thức tính toán thực sự không phải là hiệu năng mà là việc nhận biết cấu trúc lý thuyết số. 

Một cách giải thích ngây thơ có thể thử kiểm tra các giá trị của$K$đi xuống từ$\max(X, Y)$, kiểm tra điều kiện chia hết. Điều đó ngay lập tức trở nên không khả thi khi giá trị đạt tới$10^{18}$, vì việc lặp qua tất cả các ứng cử viên sẽ có độ lớn tuyến tính. 

Sai lầm phổ biến thứ hai là diễn giải điều kiện theo hướng yêu cầu$K \mid X$Và$K \mid Y$, điều đó không đúng. Sự bằng nhau của số dư không có nghĩa là cả hai số đều chia hết cho$K$; nó chỉ ngụ ý sự khác biệt của họ được chia cho$K$. 

Trường hợp cạnh chính là khi$X = Y$. Trong trường hợp đó mọi$K \ge 1$thỏa mãn điều kiện và cách tiếp cận ngây thơ “lấy gcd sai phân” sẽ tạo ra số 0, không tương ứng với một mô đun hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại tất cả các giá trị có thể có của$K$từ 1 đến$\max(X, Y)$, kiểm tra xem$X \bmod K = Y \bmod K$. Mỗi lần kiểm tra là thời gian không đổi, nhưng bản thân vòng lặp là tuyến tính theo kích thước của các số. Với giá trị tiềm năng lên đến khoảng$10^{18}$, điều này dẫn đến một điều không thể$O(10^{18})$trường hợp xấu nhất. 

Việc đơn giản hóa cấu trúc xuất phát từ việc viết lại điều kiện. Nếu như$X \bmod K = Y \bmod K$, sau đó$X - Y$chia hết cho$K$. Điều đó có nghĩa là mọi giá trị hợp lệ$K$là ước số của$D = |X - Y|$. Lớn nhất như vậy$K$do đó là$D$chính nó, vì một số luôn tự chia cho chính nó. 

Điều này thu gọn toàn bộ không gian tìm kiếm thành một phép tính số học duy nhất: tính hai tích, lấy hiệu tuyệt đối của chúng và xuất ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\max(X,Y))$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Tính hai tích$X = A \cdot B$Và$Y = C \cdot D$. Điều này trực tiếp xây dựng hai đại lượng có mối quan hệ mô-đun xác định vấn đề. 
2. Tính chênh lệch tuyệt đối$D = |X - Y|$. Bước này chuyển đổi điều kiện đẳng thức mô đun thành điều kiện chia hết. Lý do điều này có tác dụng là vì số dư bằng nhau có nghĩa là modulo hủy chính xác$K$. 
3. Đầu ra$D$như câu trả lời. Vì mỗi ước của$D$là một mô đun hợp lệ và$D$tự phân chia thì đó là sự lựa chọn tối đa có thể. 

### Tại sao nó hoạt động 

hãy để$X \equiv Y \pmod{K}$. Điều này tương đương với việc nói$K \mid (X - Y)$. Do đó tập hợp tất cả hợp lệ$K$chính xác là tập hợp các ước số dương của$D = |X - Y|$. Phần tử lớn nhất trong tập hợp này là$D$chính nó, điều này luôn đúng vì mọi số nguyên đều chia hết cho chính nó. Không lớn hơn$K$có thể làm việc vì bất kỳ$K > D$sẽ làm$X \bmod K = X$Và$Y \bmod K = Y$, sẽ không bảo toàn sự bình đẳng trừ khi$X = Y$, và trong trường hợp đó$D = 0$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B, C, D = map(int, input().split())
    X = A * B
    Y = C * D
    print(abs(X - Y))

if __name__ == "__main__":
    solve()
```Việc thực hiện theo thuật toán trực tiếp. Điều tinh tế duy nhất là tính toán các sản phẩm bằng Python một cách an toàn; Số nguyên Python xử lý độ chính xác tùy ý, do đó, tràn không phải là vấn đề đáng lo ngại ngay cả đối với đầu vào lớn. 

Sự khác biệt tuyệt đối được tính toán ở cuối chứ không phải trước khi trừ, đảm bảo tính chính xác bất kể thứ tự. Không cần phân nhánh trong trường hợp đặc biệt, kể cả khi kết quả bằng 0. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp minh họa nhỏ trong đó$A = 3, B = 5, C = 11, D = 2$. Sau đó$X = 15$Và$Y = 22$. 

| Bước | X | Y | |X - Y| | 

|---|---|---|---| 

| Sản phẩm ban đầu | 15 | 22 | - | 

| Sự khác biệt | - | - | 7 | 

Đầu ra là 7. Điều này cho thấy rằng tất cả các mô đun hợp lệ đều là ước số của 7 và bản thân giá trị lớn nhất là 7. 

Bây giờ hãy xem xét$A = 2, B = 3, C = 5, D = 7$. Sau đó$X = 6$Và$Y = 35$. 

| Bước | X | Y | |X - Y| | 

|---|---|---|---| 

| Sản phẩm ban đầu | 6 | 35 | - | 

| Sự khác biệt | - | - | 29 | 

Đầu ra là 29, một lần nữa phù hợp với cách giải thích ước số lớn nhất. 

Những dấu vết này xác nhận rằng toàn bộ quá trình tính toán rút gọn thành một phép biến đổi số học duy nhất từ ​​tích sang sai phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Số phép nhân không đổi và một sai số tuyệt đối | 
| Không gian |$O(1)$| Chỉ sử dụng một số biến số nguyên cố định | 

Giải pháp này nằm trong giới hạn thoải mái vì nó không thực hiện lặp lại trong phạm vi phụ thuộc vào cường độ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    A, B, C, D = map(int, sys.stdin.readline().split())
    X = A * B
    Y = C * D
    return str(abs(X - Y))

# provided samples
assert run("2 3 5 7\n") == "29"

# custom cases
assert run("2 2 2 2\n") == "0", "all equal products"
assert run("3 1 4 1\n") == "1", "minimal difference case"
assert run("10 10 1 1\n") == "99", "large skewed products"
assert run("5 7 5 7\n") == "0", "identical products again"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 2 2 | 0 | sản phẩm giống nhau, không khác biệt | 
| 3 1 4 1 | 1 | khác biệt tối thiểu khác 0 | 
| 10 10 1 1 | 99 | sản phẩm lớn bất đối xứng | 
| 5 7 5 7 | 0 | cấu trúc đối xứng lặp đi lặp lại | 

## Vỏ cạnh 

Khi nào$X = Y$, sự khác biệt trở thành số không. Trong trường hợp này thuật toán trả về 0, phù hợp với việc tính toán$|X - Y|$. điều kiện$X \bmod K = Y \bmod K$giữ cho tất cả$K$, do đó không có mô đun tối đa hữu hạn có ý nghĩa. Việc triển khai tự nhiên tạo ra 0 mà không cần xử lý đặc biệt. 

Ví dụ, nếu$A = 2, B = 3, C = 1, D = 6$, sau đó$X = 6$Và$Y = 6$. Sự khác biệt là bằng 0 và đầu ra là 0. 

Quá trình tính toán tiến hành giống hệt với tất cả các trường hợp khác, xác nhận rằng không cần phân nhánh và công thức số học nắm bắt đầy đủ hành vi.
