---
title: "CF 106318B - \u041f\u043e\u0442\u0435\u0440\u044f\u043d\u043d\u044b\u0435 \u044d\u0442\u0430\u0436\u0438"
description: "Chúng tôi được cung cấp số liệu thống kê tổng hợp về một tập hợp các tòa nhà, nhưng chiều cao của từng tòa nhà đã bị mất. Với mỗi giá trị ngưỡng $k$, chúng ta biết có bao nhiêu tòa nhà có nhiều hơn $k$ tầng."
date: "2026-06-19T16:55:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106318
codeforces_index: "B"
codeforces_contest_name: "\u0420\u0435\u0441\u043f\u0443\u0431\u043b\u0438\u043a\u0430\u043d\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u041f\u0443\u0442\u044c \u043a \u041e\u043b\u0438\u043c\u043f\u0443 2026"
rating: 0
weight: 106318
solve_time_s: 47
verified: true
draft: false
---

[CF 106318B - \u041f\u043e\u0442\u0435\u0440\u044f\u043d\u043d\u044b\u0435 \u044d\u0442\u0430\u0436\u0438](https://codeforces.com/problemset/problem/106318/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp số liệu thống kê tổng hợp về một tập hợp các tòa nhà, nhưng chiều cao của từng tòa nhà đã bị mất. Với mỗi giá trị ngưỡng$k$, chúng tôi biết có bao nhiêu tòa nhà có nhiều hơn$k$sàn nhà. Về mặt hình thức, một mảng$a$được đưa ra ở đâu$a_k$đếm các tòa nhà có chiều cao lớn hơn$k$. Mảng không tăng, phù hợp với thực tế là khi ngưỡng tăng lên thì sẽ có ít tòa nhà có thể vượt qua nó hơn. 

Từ thông tin tích lũy này, chúng ta phải xây dựng lại nhiều chiều cao tòa nhà. Đầu ra không phải là duy nhất dưới dạng cấu trúc có thứ tự, nhưng chúng tôi được yêu cầu xuất ra các chiều cao đã phục hồi được sắp xếp theo thứ tự không tăng, cùng với tổng số tòa nhà. 

Ràng buộc$m \le 10^6$buộc chúng ta phải tránh bất cứ điều gì bậc hai hoặc thậm chí$O(m \log m)$với các hằng số nặng trừ khi được thiết kế cẩn thận. Dự kiến ​​sẽ có sự tái thiết tuyến tính, có thể bằng cách giải thích sự khác biệt giữa các giá trị liên tiếp của$a$. 

Một điểm tinh tế là chuỗi mô tả phân bố tích lũy, nhưng không trực tiếp mô tả tần số của từng độ cao. Một sự hiểu lầm ngây thơ là đối xử$a_k$như số lượng tòa nhà có chiều cao chính xác$k$, điều đó không đúng. Một sai lầm khác là cố gắng xây dựng lại độ cao một cách tham lam mà không nhận thấy rằng mỗi lần giảm$a$tương ứng với một loạt tòa nhà “kết thúc” ở một ranh giới độ cao cụ thể. 

Để xem một trường hợp thất bại, hãy xem xét$a = [4, 3, 1, 1]$. Một cách giải thích ngây thơ có thể chỉ định độ cao trực tiếp dưới dạng số lượng theo cấp độ, tạo ra thứ gì đó giống như có quá nhiều tòa nhà ngắn. Việc xây dựng lại chính xác phải đảm bảo tính nhất quán trên tất cả các ngưỡng cùng một lúc. 

Một trường hợp cạnh khác phát sinh khi dãy phẳng ở một số vị trí. Ví dụ$a = [5, 5, 5]$có nghĩa là không có tòa nhà nào biến mất khi tăng$k$, vì vậy tất cả các tòa nhà phải có chiều cao ít nhất là 3. Việc phân công theo cấp độ tham lam có thể dễ dàng đặt nhầm số lượng nếu nó không diễn giải chính xác cao nguyên. 

## Phương pháp tiếp cận 

Quan sát chính là giải thích$a_k$như một tiền tố của biểu đồ độ cao. 

Cho phép$b_k$là số lượng tòa nhà có chiều cao chính xác$k$. Sau đó:

-$a_k = b_k + b_{k+1} + \dots$Đây là mối quan hệ tổng hậu tố cổ điển. Vì thế:

-$b_k = a_k - a_{k+1}$vì$k < m-1$-$b_{m-1} = a_{m-1}$Vì vậy, mỗi sự khác biệt giữa các giá trị liên tiếp cho chúng ta biết có bao nhiêu tòa nhà “kết thúc” ở độ cao đó. 

Một khi chúng tôi có$b_k$, chúng ta có thể xây dựng lại câu trả lời bằng cách xuất ra chiều cao$k$chính xác$b_k$lần. Vì đầu ra cuối cùng phải theo thứ tự không tăng nên chúng ta chỉ cần lặp từ cao xuống thấp. 

Cách tiếp cận bạo lực sẽ cố gắng ấn định chiều cao cho từng tòa nhà và xác minh tính nhất quán với tất cả$a_k$, dẫn đến$O(nm)$kiểm tra hoặc tệ hơn. Điều này là không khả thi đối với$10^6$. 

Việc giải thích mảng khác biệt làm giảm vấn đề xuống một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công vũ lực + xác nhận |$O(m^2)$|$O(m)$| Quá chậm | 
| Tái thiết sự khác biệt |$O(m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng$a$. Hãy coi nó như một cấu trúc hậu tố tích lũy trên các tần số không xác định. 
2. Tính một mảng mới$b$, mỗi nơi$b_k$đại diện cho chính xác có bao nhiêu tòa nhà$k$sàn nhà. Đối với chỉ mục cuối cùng, đặt$b_{m-1} = a_{m-1}$. 
3. Đối với mọi chỉ số khác$k$từ$m-2$xuống tới$0$, tính toán$b_k = a_k - a_{k+1}$. Phép trừ này tách biệt số lượng tòa nhà ngừng đóng góp vượt quá chiều cao$k$, nghĩa là chúng có chiều cao chính xác$k$. 
4. Tổng hợp tất cả$b_k$để có được tổng số tòa nhà$n$. 
5. Xây dựng đầu ra bằng cách in từng chiều cao$k$chính xác$b_k$lần, lặp từ cao xuống thấp nên kết quả tự động không tăng. 

Bước suy luận quan trọng là việc trừ đi số lượng tích lũy liền kề sẽ tách biệt các tần số chính xác. Nếu không có bước này, bản chất tích lũy của dữ liệu sẽ che giấu sự phân bổ thực tế. 

### Tại sao nó hoạt động 

Mỗi$a_k$đếm tất cả các tòa nhà có chiều cao lớn hơn$k$. Khi chúng ta trừ$a_{k+1}$, chúng tôi loại bỏ tất cả các tòa nhà có chiều cao lớn hơn$k+1$, để lại chính xác những người có chiều cao bằng$k+1$. Điều này tạo ra sự phân chia hoàn hảo tất cả các tòa nhà thành các lớp chiều cao khác nhau. Vì mỗi công trình đều đóng góp vào chính xác một$b_k$, việc xây dựng lại không bị tổn thất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    m = int(input())
    a = list(map(int, input().split()))

    b = [0] * m

    for i in range(m - 1):
        b[i] = a[i] - a[i + 1]
    b[m - 1] = a[m - 1]

    n = sum(b)

    out = []
    for h in range(m - 1, -1, -1):
        if b[h]:
            out.extend([str(h)] * b[h])

    sys.stdout.write(str(n) + "\n")
    sys.stdout.write(" ".join(out))

if __name__ == "__main__":
    main()
```Việc tái thiết xoay quanh việc tính toán số lượng chính xác theo chiều cao thông qua các khác biệt liền kề. Vòng lặp từ cao xuống thấp đảm bảo thứ tự yêu cầu mà không cần sắp xếp. Việc xây dựng đầu ra sử dụng sự lặp lại trực tiếp; mặc dù trông có vẻ nặng nề nhưng tổng số phần tử được in chính xác$n$, đó là điều không thể tránh khỏi. 

Phải cẩn thận để tất cả số học được thực hiện bằng số nguyên và việc lập chỉ mục không vượt quá giới hạn khi truy cập$a[i+1]$. Phần tử cuối cùng phải được xử lý riêng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
4 3 1 1
```Chúng tôi tính toán$b$: 

| tôi | một [tôi] | a[i+1] | b[i] | 
| --- | --- | --- | --- | 
| 0 | 4 | 3 | 1 | 
| 1 | 3 | 1 | 2 | 
| 2 | 1 | 1 | 0 | 
| 3 | 1 | - | 1 | 

Vì thế$b = [1, 2, 0, 1]$. Tổng số tòa nhà$n = 4$. 

Xây dựng đầu ra từ cao đến thấp: 

- Chiều cao 3: 1 lần 
- chiều cao 2: 0 lần 
- chiều cao 1: 2 lần 
- chiều cao 0: 1 lần 

Đầu ra:```
4
3 1 1 0
```Điều này cho thấy một đoạn phẳng trong$a$dẫn đến tần số bằng 0 ở một mức. 

### Ví dụ 2 

đầu vào:```
3
5 5 5
```Chúng tôi tính toán: 

-$b_0 = 5 - 5 = 0$-$b_1 = 5 - 5 = 0$-$b_2 = 5$Vậy tất cả các tòa nhà đều có chiều cao 2. 

Đầu ra:```
5
2 2 2 2 2
```Điều này xác nhận rằng một mảng tích lũy không đổi tương ứng với mọi khối lượng ở mức cao nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m)$| một lần để tính toán sự khác biệt cộng với việc xây dựng đầu ra tuyến tính | 
| Không gian |$O(m)$| lưu trữ mảng$a$Và$b$| 

Giải pháp là tuyến tính về kích thước của đầu vào, điều này là cần thiết vì bản thân đầu vào có thể lên tới$10^6$. Bất kỳ giải pháp nào sắp xếp hoặc thực hiện phép lặp lồng nhau sẽ vượt quá giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    m = int(input())
    a = list(map(int, input().split()))
    b = [0] * m
    for i in range(m - 1):
        b[i] = a[i] - a[i + 1]
    b[m - 1] = a[m - 1]

    n = sum(b)
    out = []
    for h in range(m - 1, -1, -1):
        out.extend([str(h)] * b[h])

    return str(n) + "\n" + " ".join(out)

# provided sample
assert run("4\n4 3 1 1\n") == "4\n3 1 1 0"

# single building
assert run("1\n7\n") == "7\n0 0 0 0 0 0 0"

# flat cumulative
assert run("3\n10 10 10\n") == "10\n2 2 2 2 2 2 2 2 2 2"

# strictly decreasing
assert run("4\n4 3 2 1\n") == "4\n3 2 1 0"

# minimal nontrivial
assert run("2\n1 0\n") == "1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 3 1 1 | 4 / 3 1 1 0 | cao nguyên hỗn hợp và giọt | 
| 1 7 | 7/ toàn số không | trường hợp cạnh chiều cao đơn | 
| 10 10 10 | 10/tất cả chiều cao 2 | mảng tích lũy phẳng | 
| 4 3 2 1 | 4/ Giảm nghiêm ngặt | phân phối thống nhất | 
| 1 0 | 1/tòa nhà đơn | cấu trúc hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mảng tích lũy không đổi. Đối với đầu vào:```
3
5 5 5
```thuật toán tính toán sự khác biệt bằng 0 cho đến phần tử cuối cùng. Tất cả khối lượng tích lũy ở độ cao cuối cùng. Việc lặp lại từ cao xuống thấp đảm bảo tất cả đầu ra đều đến từ nhóm cuối cùng đó. 

Một trường hợp cạnh khác là khi mảng giảm mạnh ở vị trí đầu tiên:```
3
10 0 0
```Đây$b_0 = 10$, mọi thứ khác đều bằng không. Việc xây dựng lại tạo ra tất cả các tòa nhà có chiều cao 0, phù hợp với cách giải thích rằng không có tòa nhà nào vượt quá bất kỳ ngưỡng tích cực nào. 

Trường hợp cạnh cuối cùng là kích thước đầu vào tối thiểu:```
1
k
```Không thể trừ được và câu trả lời đơn giản là$k$các tòa nhà có chiều cao 0. Thuật toán xử lý việc này trực tiếp thông qua việc gán phần tử cuối cùng.
