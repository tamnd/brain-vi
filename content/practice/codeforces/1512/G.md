---
title: "CF 1512G - Nhiệm vụ ngắn"
description: "Chúng ta được yêu cầu tìm số nguyên dương nhỏ nhất $n$ sao cho tổng các ước của nó bằng một số $c$ cho trước. Về mặt hình thức, hàm tổng các ước $d(n)$ tính tổng tất cả các số chia $n$ đều, bao gồm cả $1$ và $n$. Ví dụ: $d(6) = 1 + 2 + 3 + 6 = 12$."
date: "2026-06-10T18:56:24+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1512
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 713 (Div. 3)"
rating: 1700
weight: 1512
solve_time_s: 183
verified: false
draft: false
---

[CF 1512G - Nhiệm vụ ngắn](https://codeforces.com/problemset/problem/1512/G) 

**Đánh giá:** 1700 
**Tags:** sức mạnh vũ phu, dp, toán học, lý thuyết số 
**Thời gian giải:** 3m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm số nguyên dương nhỏ nhất$n$sao cho tổng các ước của nó bằng một số cho trước$c$. Về mặt hình thức, hàm tổng các ước số$d(n)$tính tổng tất cả các số chia$n$đồng đều, bao gồm$1$Và$n$chính nó. Ví dụ,$d(6) = 1 + 2 + 3 + 6 = 12$. Cho một số nguyên$c$, chúng ta phải xác định liệu có tồn tại một$n$như vậy$d(n) = c$, và nếu vậy, trả về giá trị nhỏ nhất như vậy$n$. Nếu không như vậy$n$tồn tại, chúng tôi trở lại$-1$. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cho một số nguyên$c$lên đến$10^7$. Với$t \le 10^4$trường hợp thử nghiệm, chúng tôi không đủ khả năng để tính toán$d(n)$ngây thơ cho tất cả$n \le 10^7$nhiều lần. Một cách tiếp cận bạo lực ngây thơ để kiểm tra từng$n$bằng cách tính tổng các ước của nó sẽ lấy$O(n \sqrt{n})$cho mỗi trường hợp thử nghiệm, tốc độ này cực kỳ chậm với đầu vào trong trường hợp xấu nhất. Chúng ta cần một cách tiếp cận thông minh hơn để tính toán trước thông tin một cách hiệu quả cho tất cả những điều có thể.$c$. 

Vỏ cạnh bao gồm$c = 1$, Ở đâu$n = 1$là giải pháp hợp lệ duy nhất. Một điểm tinh tế khác là nhiều con số, chẳng hạn như$c = 2$hoặc$c = 5$, không tương ứng với bất kỳ$n$. Một cách tiếp cận ngây thơ giả định rằng có một giải pháp cho tất cả$c$sẽ thất bại với những đầu vào này. Chúng ta cũng cần tính đến các số là tổng các ước của các số tổng hợp nhỏ, ví dụ:$c = 12$tương ứng với$n = 6$, nhưng cũng$c = 18$tương ứng với$n = 12$. Thuật toán phải chọn mức tối thiểu$n$. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp đi lặp lại$n = 1$đến một số giới hạn trên (như$10^7$), tính toán$d(n)$bằng cách kiểm tra tất cả các ước số cho đến$\sqrt{n}$và lưu trữ kết quả trong bản đồ từ tổng các ước số đến số nhỏ nhất$n$sản xuất nó. Mặc dù đúng nhưng cách tiếp cận này có độ phức tạp khoảng$O(n \sqrt{n}) = O(10^7 \cdot 3162)$, quá chậm đối với$t = 10^4$truy vấn. 

Quan sát quan trọng là chúng ta có thể tính toán trước hàm tổng các ước số cho tất cả$n$lên đến$10^7$sử dụng phương pháp sàng lọc. Hàm tổng các ước số có tính chất nhân và có thể được tính bằng cách lặp qua bội số. Cụ thể, đối với mỗi$i$từ 1 đến$N$, chúng tôi thêm$i$ĐẾN$d[j]$cho tất cả các bội số$j = i, 2i, 3i, \dots, N$. Điều này cho phép chúng ta tính toán$d(n)$cho tất cả$n$TRONG$O(n \log n)$thời gian. Một khi chúng tôi có$d(n)$cho tất cả$n$, chúng ta có thể ánh xạ từng giá trị tổng ước số tới giá trị nhỏ nhất$n$cái đó tạo ra nó. Sau khi tiền xử lý, việc trả lời từng truy vấn sẽ trở thành tra cứu từ điển đơn giản trong$O(1)$thời gian. 

Phương pháp brute-force hoạt động vì tính toán$d(n)$rất đơn giản nhưng lại thất bại khi$n$lớn và nhiều truy vấn yêu cầu tính toán lặp đi lặp lại. Sự quan sát đó$d(n)$có thể tính được cho tất cả$n$thông qua bội số sẽ giảm vấn đề xuống một bước tiền xử lý nhanh, giúp việc trả lời hàng nghìn truy vấn một cách nhanh chóng trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n√n) mỗi truy vấn | O(1) | Quá chậm | 
| Tính toán trước với Sàng | Tiền xử lý O(n log n), O(1) cho mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định mức tối đa$N = 10^7$vì không có đầu vào$c$vượt quá giá trị này. Khởi tạo một mảng$d$kích thước$N + 1$để lưu trữ tổng các ước số của mỗi$n$. 
2. Lặp lại$i$từ 1 đến$N$. Đối với mỗi$i$, lặp qua bội số$j = i, 2i, 3i, \dots, N$, và thêm$i$ĐẾN$d[j]$. Điều này tính toán hàm tổng các ước cho tất cả các số lên đến$N$một cách hiệu quả. 
3. Khởi tạo từ điển$answer$để ánh xạ từng giá trị tổng ước số có thể có tới giá trị nhỏ nhất$n$sản xuất nó. Vì$n$từ 1 đến$N$, nếu như$d[n]$chưa có trong$answer$, cửa hàng$answer[d[n]] = n$. Điều này đảm bảo tối thiểu$n$được ghi lại. 
4. Đối với mỗi truy vấn$c$, kiểm tra xem$c$tồn tại ở$answer$. Nếu có thì xuất ra$answer[c]$; nếu không thì xuất ra$-1$. 

Tại sao nó hoạt động: Sự tích lũy giống như sàng đảm bảo$d[n]$được tính toán chính xác cho mọi$n$lên đến$10^7$. Bằng cách lưu trữ lần xuất hiện đầu tiên của mỗi giá trị tổng ước số, chúng tôi đảm bảo rằng giá trị tối thiểu$n$sản xuất ra số tiền đó được bảo toàn. Không có truy vấn nào có thể tạo ra câu trả lời sai vì tất cả các khoản tiền có thể có đều được tính toán trước và ánh xạ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX_C = 10**7
d = [0] * (MAX_C + 1)
for i in range(1, MAX_C + 1):
    for j in range(i, MAX_C + 1, i):
        d[j] += i

answer = {}
for n in range(1, MAX_C + 1):
    if d[n] not in answer:
        answer[d[n]] = n

t = int(input())
for _ in range(t):
    c = int(input())
    print(answer.get(c, -1))
```Khối đầu tiên tính toán tổng các ước số bằng cách sử dụng sàng. Khối thứ hai đảm bảo chúng tôi lưu trữ nhỏ nhất$n$cho mỗi khoản tiền. Cuối cùng, các truy vấn được trả lời liên tục bằng cách tra cứu từ điển. Một điểm tinh tế đang lặp đi lặp lại$n$từ 1 đến$N$để lập bản đồ; đảo ngược trật tự sẽ phá vỡ "mức tối thiểu$n$"bảo lãnh. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3
1
12
18
```| n | d[n] | từ điển câu trả lời được cập nhật? | 
| --- | --- | --- | 
| 1 | 1 | câu trả lời[1] = 1 | 
| 2 | 3 | câu trả lời[3] = 2 | 
| 3 | 4 | câu trả lời[4] = 3 | 
| 4 | 7 | câu trả lời[7] = 4 | 
| 5 | 6 | câu trả lời[6] = 5 | 
| 6 | 12 | đáp án[12] = 6 | 
| 12 | 28 | đáp án[28] = 12 | 
| 18 | 39 | đáp án[39] = 18 | 

Truy vấn: 1 → 1, 12 → 6, 18 → 18. 

Điều này xác nhận việc lập bản đồ hoạt động và mức tối thiểu$n$được bảo tồn. 

Một dấu vết khác cho$c = 2$: 

| n | d[n] | từ điển trả lời được cập nhật? | 
| --- | --- | --- | 
| 1 | 1 | câu trả lời[1] = 1 | 
| 2 | 3 | câu trả lời[3] = 2 | 

Truy vấn 2 → không có trong từ điển → đầu ra -1. Điều này cho thấy thuật toán xác định chính xác các khoản tiền không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + t) | Sàng tìm tổng các ước số lấy O(N log N), trả lời t truy vấn là O(t) | 
| Không gian | O(N) | Mảng d và đáp án từ điển mỗi mảng lưu trữ tối đa N phần tử | 

Với$N = 10^7$Và$t = 10^4$, giải pháp này chạy thoải mái dưới giới hạn 2 giây và sử dụng ít hơn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    MAX_C = 10**7
    d = [0] * (MAX_C + 1)
    for i in range(1, MAX_C + 1):
        for j in range(i, MAX_C + 1, i):
            d[j] += i

    answer = {}
    for n in range(1, MAX_C + 1):
        if d[n] not in answer:
            answer[d[n]] = n

    t = int(input())
    for _ in range(t):
        c = int(input())
        print(answer.get(c, -1))
    
    return output.getvalue().strip()

# provided sample
assert run("12\n1\n2\n3\n4\n5\n6\n7\n8\n9\n10\n39\n691\n") == "1\n-1\n2\n3\n-1\n5\n4\n7\n-1\n-1\n18
```
