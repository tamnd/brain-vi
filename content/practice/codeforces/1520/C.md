---
title: "CF 1520C - Ma trận không liền kề"
description: "Chúng ta được yêu cầu điền vào một lưới vuông $n lần n$ với các số từ $1$ đến $n^2$ sao cho không có hai ô lân cận theo chiều ngang hoặc chiều dọc nào chứa các số nguyên liên tiếp. Nói cách khác, sự khác biệt giữa các số trong các ô liền kề không thể chính xác bằng một."
date: "2026-06-10T18:04:40+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1520
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 719 (Div. 3)"
rating: 1000
weight: 1520
solve_time_s: 138
verified: true
draft: false
---

[CF 1520C - Ma trận không liền kề](https://codeforces.com/problemset/problem/1520/C) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu điền vào một$n \times n$lưới vuông với các số từ$1$ĐẾN$n^2$sao cho không có hai ô lân cận theo chiều ngang hoặc chiều dọc chứa các số nguyên liên tiếp. Nói cách khác, sự khác biệt giữa các số trong các ô liền kề không thể chính xác bằng một. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp một số nguyên duy nhất$n$và đầu ra là ma trận hợp lệ cho kích thước đó hoặc$-1$nếu không có giải pháp tồn tại. 

Các ràng buộc khá nhỏ:$n$nằm trong khoảng từ 1 đến 100 và có thể có tới 100 trường hợp thử nghiệm. Điều này có nghĩa là chúng ta có thể cung cấp các giải pháp có độ phức tạp gần như$O(n^2)$cho mỗi trường hợp thử nghiệm, vì$100 \cdot 100^2 = 10^6$hoạt động có thể chấp nhận được trong giới hạn thời gian 4 giây. 

Một số trường hợp khó nhận thấy ngay lập tức. Vì$n = 1$, giải pháp rất đơn giản: một ô duy nhất có số 1. Đối với$n = 2$Tuy nhiên, không có sự sắp xếp nào tồn tại. Bất kỳ vị trí nào từ 1 đến 4 sẽ buộc ít nhất một cặp số liên tiếp có chung một cạnh. Đối với lớn hơn$n$, thách thức là sắp xếp các số để tránh sự kề nhau của các số nguyên liên tiếp và việc điền từng hàng một cách ngây thơ sẽ thất bại vì các số liên tiếp chắc chắn sẽ là hàng xóm. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các hoán vị của$1$ĐẾN$n^2$và kiểm tra từng vi phạm lân cận. Mặc dù điều này đúng về nguyên tắc nhưng nó hoàn toàn không khả thi: vì$n = 5$, có$25!$hoán vị, có giá trị lớn về mặt thiên văn. 

Quan sát quan trọng là chúng ta có thể tránh các số liên tiếp liền kề nhau bằng cách tách chúng thành hai bộ: số lẻ và số chẵn. Nếu chúng ta điền vào lưới trước tiên tất cả các số lẻ và sau đó là tất cả các số chẵn, thì sự khác biệt giữa hai số liên tiếp bất kỳ (luôn cách nhau một số) không bao giờ liền kề theo chiều ngang hoặc chiều dọc. Điều này có tác dụng với tất cả$n \ge 3$. Sự sắp xếp trong các tập hợp con này có thể theo mô hình đơn giản từ trên xuống dưới, từ trái sang phải. 

Ý tưởng này làm giảm đáng kể độ phức tạp: thay vì kiểm tra hoán vị, chúng tôi sắp xếp các số theo thứ tự chẵn lẻ một cách có hệ thống. Ngoại lệ duy nhất là$n = 2$, không thể giải quyết được và$n = 1$, điều đó thật tầm thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n^2)!) | O(n^2) | Quá chậm | 
| Vị trí Chẵn Lẻ | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$. Nếu như$n = 2$, đầu ra$-1$ngay lập tức vì không có giải pháp nào tồn tại. Nếu như$n = 1$, xuất ra một ô chứa 1. 
2. Khởi tạo một khoảng trống$n \times n$lưới. 
3. Tạo hai danh sách: một danh sách chứa tất cả các số lẻ từ 1 đến$n^2$và một có tất cả các số chẵn từ 1 đến$n^2$. 
4. Ghép danh sách lẻ với danh sách chẵn để tạo thành chuỗi cuối cùng. Điều này đảm bảo các số liên tiếp không bao giờ xuất hiện liên tiếp trong lưới. 
5. Điền vào từng hàng lưới, từ trái sang phải, sử dụng trình tự đã nối. Duy trì một con trỏ để theo dõi số tiếp theo để đặt. 
6. Xuất lưới đã hoàn thành. 

Tại sao nó hoạt động: tính bất biến chính là mỗi cặp số nguyên liên tiếp được tách thành các tập hợp khác nhau và chúng ta điền vào lưới một cách tuần tự. Bằng cách đặt tất cả các số lẻ lên đầu tiên, không có hai số có chênh lệch 1 có thể nằm ở các ô liền kề theo chiều ngang hoặc chiều dọc. Thứ tự đơn giản này giải quyết ràng buộc kề cận cho tất cả$n \ge 3$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def not_adjacent_matrix(n):
    if n == 2:
        return -1
    if n == 1:
        return [[1]]
    
    grid = [[0] * n for _ in range(n)]
    odds = [i for i in range(1, n*n + 1) if i % 2 == 1]
    evens = [i for i in range(1, n*n + 1) if i % 2 == 0]
    sequence = odds + evens
    
    idx = 0
    for r in range(n):
        for c in range(n):
            grid[r][c] = sequence[idx]
            idx += 1
    return grid

t = int(input())
for _ in range(t):
    n = int(input())
    result = not_adjacent_matrix(n)
    if result == -1:
        print(-1)
    else:
        for row in result:
            print(' '.join(map(str, row)))
```Mã xử lý đầu vào nhanh bằng cách sử dụng`sys.stdin.readline`. các`not_adjacent_matrix`chức năng đầu tiên kiểm tra những gì không thể giải quyết được$n = 2$và tầm thường$n = 1$trường hợp. Nó xây dựng các danh sách chẵn và lẻ và nối chúng lại, sau đó điền vào từng hàng trong lưới. Việc sử dụng một chỉ số đơn giản`idx`đảm bảo chúng ta sắp xếp các số một cách tuần tự mà không mắc lỗi. Việc sử dụng tính năng hiểu danh sách đảm bảo tính chính xác và tránh các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

Đối với đầu vào`n = 3`, các số lẻ là`[1, 3, 5, 7, 9]`và các số chẵn`[2, 4, 6, 8]`. Nối:`[1, 3, 5, 7, 9, 2, 4, 6, 8]`. Việc điền từng hàng sẽ tạo ra: 

| r/c | 0 | 1 | 2 | 
| --- | --- | --- | --- | 
| 0 | 1 | 3 | 5 | 
| 1 | 7 | 9 | 2 | 
| 2 | 4 | 6 | 8 | 

Không có hàng xóm ngang hoặc dọc nào khác nhau 1. 

Đối với đầu vào`n = 2`, chúng tôi lập tức quay lại`-1`bởi vì bất kỳ vị trí nào từ 1-4 sẽ có cặp lân cận có chênh lệch 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Điền vào lưới và tạo danh sách lẻ/chẵn là O(n^2) | 
| Không gian | O(n^2) | Lưu trữ lưới và chuỗi chẵn/lẻ | 

Với$n \le 100$, thuật toán thực hiện tối đa 10.000 thao tác cho mỗi trường hợp thử nghiệm, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
```
