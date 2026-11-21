---
title: "CF 2209B - Mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên và đối với mỗi phần tử, chúng ta cần xác định xem có bao nhiêu phần tử ở bên phải nó có thể bị nó “chi phối” theo một nghĩa cụ thể."
date: "2026-06-07T19:21:48+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2209
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1087 (Div. 2)"
rating: 900
weight: 2209
solve_time_s: 87
verified: true
draft: false
---

[CF 2209B - Mảng](https://codeforces.com/problemset/problem/2209/B) 

**Xếp hạng:** 900 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và đối với mỗi phần tử, chúng ta cần xác định xem có bao nhiêu phần tử ở bên phải nó có thể bị nó “chi phối” theo một nghĩa cụ thể. Đối với một phần tử`a[i]`, chúng tôi muốn đếm số lượng chỉ số tối đa`j > i`sao cho tồn tại một số nguyên`k`trong đó sự khác biệt tuyệt đối`|a[i] - k|`thực sự lớn hơn`|a[j] - k|`. Mấu chốt là ở chỗ đó`k`có thể được chọn độc lập cho mỗi`i`, và chúng tôi muốn số lượng là tối đa. 

Điều này tương đương với việc hỏi: cho mỗi phần tử`a[i]`, có bao nhiêu phần tử sau`a[j]`nghiêm ngặt giữa`a[i]`và một số được chọn`k`? Bằng trực giác, việc lựa chọn`k`rất lớn hoặc rất nhỏ đều cho phép`a[i]`thống trị tất cả các phần tử nhỏ hơn hoặc lớn hơn chính nó. Tối ưu`k`luôn nằm ngoài phạm vi của hai phần tử để tối đa hóa số lượng. 

Các ràng buộc chỉ ra rằng mảng có thể có tổng cộng tối đa 5000 phần tử trong tất cả các trường hợp thử nghiệm. Một cách tiếp cận đơn giản để kiểm tra từng cặp`(i, j)`cho mọi tiềm năng`k`sẽ thực hiện các phép toán O(n²), điều này có thể chấp nhận được vì n ≤ 5000, nhưng có thể có một phương pháp đơn giản hơn bằng cách sử dụng các thuộc tính của các số thay vì lặp lại một cách thô bạo trên`k`. 

Các trường hợp cạnh bao gồm mảng có kích thước 1, mảng có phần tử lặp lại và mảng trong đó phần tử lớn nhất hoặc nhỏ nhất xuất hiện ở đầu hoặc cuối. Ví dụ, một mảng`[5, 5, 5]`phải sản xuất`[0, 0, 0]`, bởi vì dù có lựa chọn`k`, không có phần tử nào thực sự “gần” hơn với`k`hơn cái khác khi các giá trị bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên từng phần tử`a[i]`, thì với mỗi`i`, kiểm tra mọi phần tử`a[j]`vì`j > i`. Đối với mỗi cặp`(i, j)`, chúng ta sẽ cố gắng tìm một số nguyên`k`như vậy`|a[i] - k| > |a[j] - k|`. Sự bất bình đẳng này rút gọn thành một phép kiểm tra đơn giản:`k`nên nằm ngoài khoảng cách giữa`a[i]`Và`a[j]`. Cụ thể thì`k < min(a[i], a[j])`hoặc`k > max(a[i], a[j])`. Vì như vậy`k`luôn tồn tại với mọi`i ≠ j`, điều kiện rút gọn về việc kiểm tra`a[i] ≠ a[j]`. Độ phức tạp ban đầu là O(n²) cho mỗi trường hợp thử nghiệm, có thể chấp nhận được với các ràng buộc. 

Cách tiếp cận tối ưu khai thác quan sát rằng đối với bất kỳ`a[i]`, số lượng phần tử chiếm ưu thế tối đa đạt được bằng cách chọn`k`xa về bên trái hoặc xa về bên phải, do đó`a[i]`xa hơn từ`k`hơn`a[j]`nếu như`a[j]`nằm giữa`a[i]`Và`k`. Điều này làm giảm vấn đề đếm số phần tử ở bên phải của`i`nhỏ hơn hoặc lớn hơn rất nhiều so với`a[i]`, và lấy giá trị lớn nhất của hai số này. Chúng ta không còn cần phải tính toán hoặc lặp lại`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Chấp nhận được với n ≤ 5000 | 
| Tối ưu hóa tham lam | O(n²) | O(1) | Được chấp nhận, logic đơn giản hơn | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng`a`chiều dài`n`. 
2. Khởi tạo mảng kết quả`res`chiều dài`n`với số không. 
3. Lặp lại từng chỉ mục`i`từ`0`ĐẾN`n-1`. 
4. Đối với`i`, khởi tạo hai bộ đếm`count_less`Và`count_greater`về không. 
5. Lặp lại từng chỉ mục`j`từ`i+1`ĐẾN`n-1`. Nếu như`a[j] < a[i]`, tăng`count_less`. Nếu như`a[j] > a[i]`, tăng`count_greater`. 
6. Đặt`res[i]`đến mức tối đa`count_less`Và`count_greater`. 
7. Sau khi xử lý tất cả các chỉ số, xuất mảng kết quả cho ca kiểm thử. 

Tại sao nó hoạt động: Đối với mỗi`i`, số phần tử có thể bị chi phối chỉ phụ thuộc vào giá trị tương đối của`a[i]`Và`a[j]`. Lựa chọn`k`ở xa bên trái sẽ chiếm ưu thế tất cả các phần tử nhỏ hơn và việc chọn`k`ở xa bên phải sẽ chiếm ưu thế tất cả các yếu tố lớn hơn. Vì chúng ta muốn đạt mức tối đa nên việc lấy số lớn hơn trong hai số này sẽ cho câu trả lời đúng. Không có phần tử nào được tính hai lần vì sự thống trị là độc lập đối với mỗi phần tử`i`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        res = [0] * n
        for i in range(n):
            count_less = 0
            count_greater = 0
            for j in range(i + 1, n):
                if a[j] < a[i]:
                    count_less += 1
                elif a[j] > a[i]:
                    count_greater += 1
            res[i] = max(count_less, count_greater)
        print(" ".join(map(str, res)))
```Mã trực tiếp thực hiện các bước thuật toán. Mỗi`i`chỉ được so sánh với các chỉ số sau nó. sử dụng`max(count_less, count_greater)`đảm bảo chúng tôi chọn tối ưu`k`một cách ngầm định. Cách tiếp cận này tránh được việc tính toán không cần thiết về giá trị thực tế`k`giá trị, chỉ dựa vào thứ tự tương đối. 

## Ví dụ đã hoạt động 

Đối với mảng`[1, 2, 93, 84, 2]`: 

| tôi | một [tôi] | Các phần tử sau i | vô số | đếm_Greater | độ phân giải[i] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | [2, 93, 84, 2] | 0 | 4 | 4 | 
| 1 | 2 | [93, 84, 2] | 1 | 2 | 2 | 
| 2 | 93 | [84, 2] | 2 | 0 | 2 | 
| 3 | 84 | [2] | 1 | 0 | 1 | 
| 4 | 2 | [] | 0 | 0 | 0 | 

Điều này xác nhận rằng việc lấy số lượng tối đa của các phần tử lớn hơn và nhỏ hơn sau`i`tính toán đúng lời giải. 

Đối với mảng`[105, -105]`: 

| tôi | một [tôi] | Các phần tử sau i | vô số | đếm_Greater | độ phân giải[i] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 105 | [-105] | 1 | 0 | 1 | 
| 1 | -105 | [] | 0 | 0 | 0 | 

Điều này chứng tỏ thuật toán xử lý chính xác các số âm và mảng nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi phần tử được so sánh với tất cả các phần tử ở bên phải của nó | 
| Không gian | O(n) | Mảng kết quả có độ dài n; không có cấu trúc bổ sung | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm là 5000, số lượng so sánh trong trường hợp xấu nhất là khoảng 25 triệu, có thể chấp nhận được trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("6\n1\n1092\n2\n105 -105\n5\n1 2 93 84 2\n7\n2 9 38 4 7 1 6\n10\n1 9 20 9 829 3 87 1 283 7\n11\n9 18 29817 283 3 3928 5726 1942 1000000000 -1000000000 19\n") == \
"0\n1 0\n4 2 2 1 0\n5 4 4 2 2 1 0\n8 4 4 3 5 3 2 2 1 0\n8 7 7 4 5 3 3 2 2 1 0"

# Custom cases
assert run("1\n1\n42\n") == "0", "single element"
assert run("1\n3\n5 5 5\n") == "0 0 0", "all equal"
assert run("1\n4\n1 2 3 4\n") == "3 2 1 0", "strictly increasing"
assert run("1\n4\n4 3 2 1\n") == "3 2 1 0", "strictly decreasing"
assert run("1\n5\n1 -1 2 -2 0\n") == "3 2 2 1 0", "mixed signs"
```| Kiểm tra đầu vào | Sản lượng dự kiến
