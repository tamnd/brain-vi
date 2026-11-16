---
title: "CF 1914A - Nhật ký giải quyết vấn đề"
description: "Nhật ký cuộc thi của Monocarp là một chuỗi có độ dài $n$ trong đó ký tự $i$-th thể hiện vấn đề mà anh ấy đang giải quyết trong phút $i$. Mỗi bài toán từ 'A' đến 'Z' đều có thời gian cần thiết để giải: 'A' mất 1 phút, 'B' 2 phút, đến 'Z' mất 26 phút."
date: "2026-06-08T20:01:26+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1914
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 916 (Div. 3)"
rating: 800
weight: 1914
solve_time_s: 123
verified: true
draft: false
---

[CF 1914A - Nhật ký giải quyết vấn đề](https://codeforces.com/problemset/problem/1914/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhật ký cuộc thi của Monocarp là một chuỗi có độ dài$n$ở đâu$i$-ký tự thứ đại diện cho vấn đề anh ấy đang giải quyết trong phút$i$. Mỗi bài toán từ 'A' đến 'Z' đều có thời gian cần thiết để giải: 'A' mất 1 phút, 'B' 2 phút, đến 'Z' mất 26 phút. Một vấn đề được coi là đã giải quyết được nếu Monocarp đã dành ít nhất thời gian cần thiết cho nó. Mục tiêu là đếm xem anh ấy đã giải quyết được bao nhiêu vấn đề riêng biệt vào cuối cuộc thi. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có thời lượng cuộc thi và chuỗi nhật ký.$n$có thể lên tới 500 và có tới 100 trường hợp kiểm thử, vì vậy tổng công việc trên tất cả các trường hợp kiểm thử là nhiều nhất$5 \cdot 10^4$hoạt động. Điều này cho phép các giải pháp xử lý toàn bộ chuỗi trên mỗi trường hợp thử nghiệm theo thời gian tuyến tính. Một cách tiếp cận đơn giản kiểm tra mọi vấn đề trên toàn bộ chuỗi nhiều lần sẽ phức tạp không cần thiết và có thể gây ra lỗi. Các trường hợp đặc biệt bao gồm nhật ký trong đó Monocarp liên tục làm việc trên một vấn đề mà không hoàn thành nó hoặc nhật ký có nhiều cách giải quyết cùng một vấn đề; thuật toán phải đếm từng vấn đề nhiều nhất một lần sau khi đáp ứng đủ thời gian yêu cầu. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ lặp lại tất cả 26 vấn đề cho mỗi trường hợp thử nghiệm và đối với mỗi vấn đề, hãy đếm xem có bao nhiêu phút trong nhật ký tương ứng với vấn đề đó. Nếu số lượng đáp ứng hoặc vượt quá thời gian yêu cầu, hãy tăng số lượng đã giải quyết. Phương pháp này đúng nhưng đòi hỏi tối đa$26 \cdot n$hoạt động cho mỗi trường hợp thử nghiệm. Với những ràng buộc đã cho, điều này có thể chấp nhận được nhưng hơi kém hiệu quả vì nó lặp lại cùng một lần quét nhiều lần. 

Phương pháp tối ưu tận dụng sự tích lũy trực tiếp thời gian cho mỗi vấn đề khi chúng ta duyệt qua nhật ký một lần. Chúng tôi duy trì một mảng gồm 26 bộ đếm tương ứng với từng bài toán. Cứ mỗi phút, chúng tôi tăng bộ đếm cho sự cố được chỉ ra bởi nhật ký. Sau khi duyệt, chúng tôi kiểm tra xem bộ đếm nào đáp ứng hoặc vượt quá thời gian cần thiết cho các vấn đề tương ứng của chúng. Phương pháp này xử lý mỗi lần đăng nhập$O(n)$thời gian và$O(1)$thêm không gian và tránh được việc quét lặp lại. Điểm mấu chốt là thời gian cần thiết cho một vấn đề được xác định chặt chẽ theo chỉ mục theo thứ tự bảng chữ cái của nó và chúng ta có thể tích lũy thời gian trong một lần mà không cần xem xét đến thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26 * n) | O(26) | Đã chấp nhận | 
| Tối ưu | O(n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`times[26]`về không. Mỗi phần tử tương ứng với một bài toán từ “A” đến “Z” và sẽ theo dõi số phút tích lũy mà Monocarp đã dành cho bài toán đó. 
2. Đối với mỗi nhân vật`c`trong nhật ký cuộc thi, tính chỉ số của nó như`idx = ord(c) - ord('A')`và tăng dần`times[idx]`bởi một. Bước này tích lũy thời gian dành cho từng vấn đề trong suốt cuộc thi. 
3. Khởi tạo bộ đếm`solved = 0`. 
4. Lặp lại`i`từ 0 đến 25, biểu thị các vấn đề từ 'A' đến 'Z'. Đối với mỗi vấn đề, hãy kiểm tra xem`times[i] >= i + 1`vì vấn đề 'A' cần 1 phút, 'B' 2 phút, ..., 'Z' 26 phút. Nếu điều kiện được giữ thì tăng`solved`. 
5. Đầu ra`solved`bằng số lượng vấn đề mà Monocarp đã giải quyết trong trường hợp thử nghiệm này. 

Thuật toán này hoạt động vì thời gian yêu cầu của mỗi vấn đề là độc lập và thời gian tích lũy trên nhật ký sẽ tính chính xác cho tất cả số phút đã sử dụng. Điều bất biến là sau khi xử lý nhật ký,`times[i]`thể hiện chính xác tổng số phút Monocarp dành cho vấn đề`chr(ord('A') + i)`. Kiểm tra đối chiếu`i + 1`xác định chính xác liệu vấn đề có được giải quyết hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    log = input().strip()
    times = [0] * 26
    for c in log:
        times[ord(c) - ord('A')] += 1
    solved = sum(1 for i in range(26) if times[i] >= i + 1)
    print(solved)
```Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. Mảng`times`theo dõi thời gian cho mỗi vấn đề và chúng tôi chuyển đổi các chữ cái thành chỉ số bằng số học ASCII. Tổng hợp mảng ở cuối sẽ kiểm tra xem vấn đề nào có đủ thời gian tích lũy. Điều này tránh việc quét lặp lại và đảm bảo tính chính xác bằng cách đếm chính xác từng phút một. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
6
ACBCBC
```| Phút | Nhật ký | lần['A'..'C'] | Giải thích | 
| --- | --- | --- | --- | 
| 1 | A | [1,0,0] | 'A' được 1 phút | 
| 2 | C | [1,0,1] | 'C' được 1 phút | 
| 3 | B | [1,1,1] | 'B' được 1 phút | 
| 4 | C | [1,1,2] | 'C' tích lũy 2 | 
| 5 | B | [1,2,2] | 'B' tích lũy 2 | 
| 6 | C | [1,2,3] | 'C' tích lũy 3 | 

Kiểm tra thời gian cần thiết: 'A' 1 → đã giải quyết, 'B' 2 → đã giải quyết, 'C' 3 → đã giải quyết. Tổng số giải quyết được = 3. 

Đầu vào mẫu:```
7
AAAAFPC
```| Phút | Nhật ký | lần['A'..'F','P','C'] | Giải thích | 
| --- | --- | --- | --- | 
| 1-4 | A | 4 | 'A' tích lũy 4 | 
| 5 | F | 1 | 'F' tích lũy 1 | 
| 6 | P | 1 | 'P' tích lũy 1 | 
| 7 | C | 1 | 'C' tích lũy 1 | 

Số lần yêu cầu: 'A' 1 → giải quyết, 'C' 3 → không giải quyết, 'F' 6 → không giải quyết, 'P' 16 → không giải quyết. Tổng số giải quyết được = 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ký tự nhật ký được xử lý một lần, tổng mảng là O(26) | 
| Không gian | O(26) | Mảng`times`giữ số phút tích lũy cho mỗi bài toán | 

Tổng số thao tác trên tất cả các trường hợp thử nghiệm tối đa là 500 * 100 = 50.000, trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ là tối thiểu do mảng có kích thước cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        log = input().strip()
        times = [0]*26
        for c in log:
            times[ord(c)-ord('A')] += 1
        solved = sum(1 for i in range(26) if times[i] >= i+1)
        print(solved)
    return output.getvalue().strip()

# Provided samples
assert run("3\n6\nACBCBC\n7\nAAAAFPC\n22\nFEADBBDFFEDFFFDHHHADCC\n") == "3\n1\n4"

# Custom cases
assert run("1\n1\nA\n") == "1", "single minute, single problem solved"
assert run("1\n26\nABCDEFGHIJKLMNOPQRSTUVWXYZ\n") == "1", "all problems touched once, only A solved"
assert run("1\n52\nABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ\n") == "2", "all problems twice, only A and B solved"
assert run("1\n500\n" + "A"*500 + "\n") == "1", "large input, all time on A"
assert run("1\n26\nZ"*26 + "\n") == "0", "all time on hardest problem, not solved"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | nhật ký một phút, trường hợp đơn giản nhất | 
| 26 chữ cái một lần | 1 | chỉ 'A' đáp ứng yêu cầu | 
| 2×26 chữ cái | 2 | tích lũy qua nhiều lần xuất hiện | 
| 500×A | 1 | đầu vào lớn, kiểm tra hiệu suất | 
| 26×Z | 0 | bài toán khó chưa giải quyết được do không đủ thời gian | 

## Vỏ cạnh 

Nếu Monocarp dành nhiều phút cho một vấn đề mà không đạt đến ngưỡng yêu cầu thì thuật toán sẽ báo cáo chính xác là 0. Ví dụ: nhật ký 5 phút cho bài toán 'F' (yêu cầu 6) sẽ dẫn đến`times[5] = 5 < 6`, nên không được tính. Nếu xảy ra nhiều lần giải quyết cùng một vấn đề, thì quá trình tích lũy sẽ tính chính xác tổng thời gian mà không cần tính hai lần, đảm bảo tính chính xác ngay cả đối với các chuỗi lặp lại dài.
