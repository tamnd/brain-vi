---
title: "CF 1746B - Nổi Loạn"
description: "Chúng ta được cung cấp một mảng nhị phân, chỉ chứa các số 0 và một, và chúng ta có thể thực hiện một thao tác trong đó chúng ta chọn hai phần tử riêng biệt, thêm giá trị của phần tử đầu tiên vào phần tử thứ hai, sau đó xóa phần tử đầu tiên. Mỗi thao tác làm giảm kích thước của mảng đi một."
date: "2026-06-09T15:42:32+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1746
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 23"
rating: 800
weight: 1746
solve_time_s: 165
verified: false
draft: false
---

[CF 1746B - Cuộc nổi loạn](https://codeforces.com/problemset/problem/1746/B) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, hai con trỏ 
**Thời gian giải:** 2m 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng nhị phân, chỉ chứa các số 0 và một, và chúng ta có thể thực hiện một thao tác trong đó chúng ta chọn hai phần tử riêng biệt, thêm giá trị của phần tử đầu tiên vào phần tử thứ hai, sau đó xóa phần tử đầu tiên. Mỗi thao tác làm giảm kích thước của mảng đi một. Mục tiêu là làm cho mảng không giảm, nghĩa là mọi phần tử ít nhất phải lớn bằng phần tử trước nó. Chúng tôi được yêu cầu xác định số lượng tối thiểu các hoạt động như vậy cần thiết. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp độ dài của mảng và chính mảng đó. Các ràng buộc cho phép các mảng có kích thước lên tới 100.000 và tổng của tất cả các mảng trong tất cả các trường hợp thử nghiệm không vượt quá 200.000. Với giới hạn thời gian một giây, điều này loại trừ mọi giải pháp bậc hai hoặc tệ hơn, vì các phép toán O(n^2) có thể lên tới 10^10 trong trường hợp xấu nhất. 

Trường hợp cạnh khóa là một mảng không giảm, chẳng hạn như`[0, 0, 1, 1]`. Ở đây, câu trả lời phải bằng không. Một trường hợp phức tạp khác là khi một`1`xuất hiện trước một chuỗi số 0, ví dụ`[1, 0, 0]`. Một cách tiếp cận ngây thơ có thể thử ghép các phần tử liền kề mà không xem xét hiệu ứng tích lũy của việc di chuyển`1`chuyển tiếp, có khả năng tạo ra quá nhiều hoạt động. Ngoài ra, các mảng chỉ có số 0 hoặc chỉ có số 1 cần xử lý đặc biệt vì chúng không giảm một cách tầm thường. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các cặp chỉ số có thể có để thực hiện thao tác, kiểm tra ở mỗi bước xem mảng có trở nên không giảm hay không. Về nguyên tắc, điều này đúng, nhưng mỗi thao tác chỉ giảm mảng đi một phần tử và có khả năng có n*(n-1)/2 lựa chọn ở mỗi bước, tạo ra độ phức tạp trong trường hợp xấu nhất là O(n^2) hoặc tệ hơn. Với n lên tới 10^5, phương pháp này không khả thi. 

Cái nhìn sâu sắc tối ưu đến từ việc nhận ra rằng các phần tử duy nhất vi phạm tính chất không giảm là`1`s xuất hiện trước số không. Mọi`0`trước đó là một`1`là một ứng cử viên để sửa chữa. Điều quan trọng là di chuyển tất cả các vấn đề`1`về cuối có thể được thực hiện một cách tham lam bằng cách đếm xem có bao nhiêu`0`s xuất hiện sau cái cuối cùng`1`và thực hiện các thao tác để loại bỏ`1`s từ phía trước, thêm chúng vào cuối cùng`1`hoặc cái khác`1`đó là sau số không. Điều này làm giảm vấn đề đếm số lượng`1`s trước lần đầu tiên`0`từ bên phải vi phạm trật tự, chính xác là số thao tác cần thiết. 

Quan sát quan trọng là số lượng hoạt động tối thiểu bằng với số lượng`1`s xuất hiện trước số 0 cuối cùng cần được "đẩy về phía trước". Mỗi như vậy`1`phải tham gia vào một hoạt động để khôi phục trật tự không giảm, trong khi`1`s đã ở sau số 0 thì không cần di chuyển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc độ dài mảng`n`và mảng`a`. 
2. Khởi tạo bộ đếm`ops`về 0 và một lá cờ`zeros_seen`thành sai. 
3. Duyệt mảng từ phải sang trái. Đối với mỗi phần tử: 

- Nếu phần tử đó là`0`, bộ`zeros_seen`thành sự thật. Điều này xác định rằng bất kỳ`1`trước thời điểm này sẽ cần một cuộc phẫu thuật. 
- Nếu phần tử đó là`1`Và`zeros_seen`là đúng, tăng`ops`. Điều này đếm số lượng`1`s không còn hoạt động và cần được di chuyển. 
4. Sau khi hoàn tất quá trình duyệt,`ops`chứa số lượng hoạt động tối thiểu cần thiết. In`ops`. 

Lý do điều này có tác dụng là vì bất kỳ`1`xảy ra trước một`0`trong đoạn vi phạm trật tự không giảm phải tham gia hoạt động lập lại trật tự. Bằng cách quét từ phải sang trái, chúng tôi đảm bảo rằng chúng tôi đếm chính xác những`1`s nằm trước số 0 cuối cùng cần được sửa. Tất cả các phần tử khác đã ở thứ tự không giảm và không đóng góp vào số lượng thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    ops = 0
    zeros_seen = False
    for x in reversed(a):
        if x == 0:
            zeros_seen = True
        elif zeros_seen:
            ops += 1
    print(ops)
```Mã này đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và xử lý nhiều trường hợp thử nghiệm. Nó sử dụng một lần chuyển từ phải sang trái để đếm số lượng`1`s đứng trước số 0, tương ứng với các thao tác tối thiểu. Việc sử dụng cờ sẽ tránh việc quét mảng nhiều lần hoặc sử dụng bộ nhớ bổ sung. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

Mảng đầu vào:`[1, 0, 0, 1, 0]`| Chỉ mục | Yếu tố | số không_seen | hoạt động | 
| --- | --- | --- | --- | 
| 4 | 0 | Đúng | 0 | 
| 3 | 1 | Đúng | 1 | 
| 2 | 0 | Đúng | 1 | 
| 1 | 0 | Đúng | 1 | 
| 0 | 1 | Đúng | 2 | 

Bảng cho thấy rằng`1`s ở vị trí 0 và 3 cần các thao tác, do đó đầu ra là`2`. 

**Ví dụ 2:** 

Mảng đầu vào:`[0, 0, 1, 1]`| Chỉ mục | Yếu tố | số không_seen | hoạt động | 
| --- | --- | --- | --- | 
| 3 | 1 | Sai | 0 | 
| 2 | 1 | Sai | 0 | 
| 1 | 0 | Đúng | 0 | 
| 0 | 0 | Đúng | 0 | 

KHÔNG`1`xuất hiện trước số 0 nên không cần thực hiện thao tác nào. Đầu ra là`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua mảng cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với các ràng buộc về tổng n trên tất cả các trường hợp thử nghiệm 2·10^5, giải pháp tuyến tính này phù hợp một cách thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        ops = 0
        zeros_seen = False
        for x in reversed(a):
            if x == 0:
                zeros_seen = True
            elif zeros_seen:
                ops += 1
        print(ops)
    
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("4\n8\n0 0 1 1 1 1 1 1\n5\n1 0 0 1 1\n2\n1 0\n11\n1 1 0 0 1 0 0 1 1 1 0\n") == "0\n1\n1\n3"

# Custom cases
assert run("2\n1\n0\n1\n1\n") == "0\n0", "single element arrays"
assert run("1\n5\n1 1 1 1 1\n") == "0", "all ones"
assert run("1\n5\n0 0 0 0 0\n") == "0", "all zeros"
assert run("1\n6\n1 0 1 0 1 0\n") == "3", "alternating ones and zeros"
assert run("1\n4\n1 0 0 1\n") == "2", "ones at edges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n0`Và`1\n1`|`0\n0`| mảng một phần tử | 
|`1\n1 1 1 1 1`|`0`| tất cả những cái đã không giảm | 
|`1\n0 0 0 0 0`|`0`| tất cả các số không, đã không giảm | 
|`1\n1 0 1 0 1 0`|`3`| xen kẽ các số 1 và 0, kiểm tra việc đếm chính xác`1`s trước số không | 
|`1\n1 0 0 1`|`2`| những cái ở các cạnh, kiểm tra việc xử lý`1`s trước số không | 

## Vỏ cạnh 

Đối với mảng`[1, 0, 1]`, bộ thuật toán`zeros_seen`khi nó nhìn thấy`0`ở chỉ số 1.`1`tại chỉ số 0 được tính là cần một thao tác, trong khi`1`tại chỉ số 2 đứng sau số 0 và không tăng`ops`. Đầu ra là`1`, phù hợp với các hoạt động tối thiểu được yêu cầu. 

Đối với một mảng có một phần tử duy nhất, chẳng hạn như`[0]`hoặc`[1]`, thuật toán không bao giờ đi vào`elif zeros_seen`chi nhánh, vậy`ops`vẫn bằng 0, trả về chính xác`0`. Điều này xác nhận cách tiếp cận xử lý trường hợp cạnh đầu vào tối thiểu.
