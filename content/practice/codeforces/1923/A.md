---
title: "CF 1923A - Chip di chuyển"
description: "Chúng ta được cấp một hàng ô, mỗi ô chứa một con chip hoặc trống. Mục tiêu là di chuyển các con chip sang trái thành các nhóm liền kề bằng cách liên tục chọn một con chip và di chuyển nó vào ô trống gần nhất ở bên trái."
date: "2026-06-08T19:12:27+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1923
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 162 (Rated for Div. 2)"
rating: 800
weight: 1923
solve_time_s: 112
verified: false
draft: false
---

[CF 1923A - Chip chuyển động](https://codeforces.com/problemset/problem/1923/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng ô, mỗi ô chứa một con chip hoặc trống. Mục tiêu là di chuyển các con chip sang trái thành các nhóm liền kề bằng cách liên tục chọn một con chip và di chuyển nó vào ô trống gần nhất ở bên trái. Nhiệm vụ là xác định số lần di chuyển tối thiểu cần thiết để làm cho tất cả các chip tạo thành một khối duy nhất không bị gián đoạn. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm có một số ô`n`(từ 2 đến 50) và một mảng mô tả ô nào chứa chip. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất biểu thị số bước di chuyển ít nhất cần thiết. 

Bởi vì`n`tối đa là 50, bất kỳ thuật toán nào thực hiện quét đơn giản trên mảng một vài lần sẽ chạy hiệu quả. Với`t`lên tới 1000 trường hợp thử nghiệm, một giải pháp xử lý từng mảng theo thời gian tuyến tính là có thể chấp nhận được. 

Vấn đề có những trường hợp phức tạp. Ví dụ: nếu tất cả các chip đã liền kề nhau thì không cần di chuyển. Nếu có một con chip có ô trống ở bên trái thì không cần di chuyển. Một cách triển khai ngây thơ đếm từng ô trống hoặc cố gắng mô phỏng các bước di chuyển một cách mù quáng có thể bị tính quá mức, bởi vì chỉ các ô trống **giữa chip ngoài cùng bên trái và ngoài cùng bên phải** mới quan trọng. Ví dụ,`[0, 1, 0, 1, 0, 1, 0]`có 3 chip và 3 khoảng trống nhưng chỉ cần đóng 2 khoảng trống giữa chip đầu tiên và chip cuối cùng nên chỉ cần 2 nước đi. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là mô phỏng quá trình một cách rõ ràng: liên tục chọn các con chip và di chuyển chúng sang trái cho đến khi hình thành một khối liền kề. Mặc dù đúng nhưng điều này là không cần thiết vì chúng ta chỉ quan tâm đến số lần di chuyển tối thiểu chứ không quan tâm đến trình tự chính xác. Việc mô phỏng từng bước di chuyển không hiệu quả ngay cả đối với các mảng nhỏ và có nguy cơ xảy ra lỗi riêng lẻ. 

Quan sát quan trọng là chỉ các ô trống **giữa quân đầu tiên và quân cuối cùng** mới góp phần tạo nên nước đi. Bất kỳ ô trống nào ở bên trái của chip ngoài cùng bên trái hoặc ở bên phải của chip ngoài cùng bên phải không bao giờ cần phải được lấp đầy vì sẽ không có chip nào di chuyển vào chúng. Do đó, số lần di chuyển tối thiểu chỉ đơn giản là số số 0 giữa con chip đầu tiên và con chip cuối cùng. 

Điều này làm giảm vấn đề phải quét mảng một lần để tìm chỉ số của chip đầu tiên và chip cuối cùng, sau đó đếm các số 0 trong mảng con đó. Giải pháp này là tuyến tính, cực kỳ đơn giản để thực hiện và tránh được những cạm bẫy của mô phỏng đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) cho mỗi trường hợp thử nghiệm | O(n) | Quá mức cần thiết / không cần thiết | 
| Đếm số không giữa chip đầu tiên và chip cuối cùng | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Xác định chip ngoài cùng bên trái bằng cách quét từ trái sang phải. Lưu trữ chỉ mục của nó dưới dạng`first`. 
4. Xác định chip ngoài cùng bên phải bằng cách quét từ phải sang trái. Lưu trữ chỉ mục của nó dưới dạng`last`. 
5. Khởi tạo bộ đếm`moves`về không. 
6. Lặp lại từ`first`ĐẾN`last`. Đối với mỗi ô, nếu nó trống (`0`), tăng`moves`. 
7. Sau khi quét,`moves`hiện giữ số lượng hoạt động tối thiểu cần thiết. In nó. 

Tại sao nó hoạt động: di chuyển bất kỳ con chip nào sang trái có thể lấp đầy chính xác một ô trống. Vì tất cả các nước đi đều độc lập và chỉ có số 0 giữa vật liệu chip đầu tiên và cuối cùng, nên việc đếm chúng sẽ cho ra số nước đi tối thiểu. Không có động thái nào có thể làm giảm số lượng này hơn nữa và không cần di chuyển ra ngoài phạm vi này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    first = a.index(1)  # leftmost chip
    last = len(a) - 1 - a[::-1].index(1)  # rightmost chip
    
    moves = 0
    for i in range(first, last + 1):
        if a[i] == 0:
            moves += 1
    print(moves)
```Giải pháp sử dụng`list.index`để xác định vị trí con chip đầu tiên và quét ngược lại để xác định vị trí con chip cuối cùng. Việc lặp lại giữa các chỉ số này sẽ đếm tất cả các ô trống, tương ứng trực tiếp với số lần di chuyển cần thiết. Không có mảng bổ sung nào được tạo và tất cả các thao tác là O(n) cho mỗi trường hợp thử nghiệm. Đảo ngược mảng để tìm chỉ mục cuối cùng là an toàn vì`n <= 50`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`[0, 1, 1, 1, 0, 1, 1, 0]`| tôi | một [tôi] | di chuyển | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 1 | 0 | 
| 3 | 1 | 0 | 
| 4 | 0 | 1 | 
| 5 | 1 | 1 | 
| 6 | 1 | 1 | 

Giải thích: Chip đầu tiên ở chỉ số 1, chip cuối cùng ở chỉ số 6. Giữa chúng có chính xác một số 0 (chỉ số 4), vì vậy`moves = 1`. 

Một ví dụ khác:`[1, 0, 1, 0, 1]`| tôi | một [tôi] | di chuyển | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 0 | 1 | 
| 2 | 1 | 1 | 
| 3 | 0 | 2 | 
| 4 | 1 | 2 | 

Giải thích: Chip đầu tiên là 0, cuối cùng là 4. Có các số 0 ở vị trí 1 và 3. Cả hai đều cần được điền đầy đủ nên nước đi tối thiểu là 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp kiểm thử sẽ quét mảng hai lần: một lần cho chip đầu tiên, một lần cho chip cuối cùng, sau đó một lần để đếm các số 0 ở giữa. Mỗi lần quét là O(n). | 
| Không gian | O(n) | Chỉ có mảng đầu vào được lưu trữ. Không có cấu trúc dữ liệu bổ sung ngoài các biến vòng lặp. | 

Với`t <= 1000`Và`n <= 50`, số lượng thao tác trong trường hợp xấu nhất là 50.000, nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        first = a.index(1)
        last = len(a) - 1 - a[::-1].index(1)
        moves = sum(1 for i in range(first, last + 1) if a[i] == 0)
        output.append(str(moves))
    return "\n".join(output)

# provided samples
assert run("5\n8\n0 1 1 1 0 1 1 0\n6\n0 1 0 0 0 0\n6\n1 1 1 1 1 1\n5\n1 0 1 0 1\n9\n0 1 1 0 0 0 1 1 0\n") == "1\n0\n0\n2\n3"

# custom tests
assert run("2\n2\n1 0\n3\n0 1 0\n") == "0\n0"  # minimal n, single chip
assert run("1\n6\n0 0 1 0 1 0\n") == "2"  # zeros outside first/last ignored
assert run("1\n7\n1 0 0 0 0 0 1\n") == "5"  # all
```
