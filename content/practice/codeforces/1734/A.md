---
title: "CF 1734A - Chọn Ba Que"
description: "Chúng ta được cho một tập hợp các que, mỗi que có chiều dài nguyên dương. Mục tiêu của chúng tôi là điều chỉnh độ dài bằng cách sử dụng số phép toán tối thiểu để ba trong số chúng có thể tạo thành một tam giác đều."
date: "2026-06-09T18:18:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1734
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 822 (Div. 2)"
rating: 800
weight: 1734
solve_time_s: 145
verified: true
draft: false
---

[CF 1734A - Chọn ba gậy](https://codeforces.com/problemset/problem/1734/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, phân loại 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các que, mỗi que có chiều dài nguyên dương. Mục tiêu của chúng tôi là điều chỉnh độ dài bằng cách sử dụng số phép toán tối thiểu để ba trong số chúng có thể tạo thành một tam giác đều. Một thao tác là tăng hoặc giảm độ dài của thanh chính xác một đơn vị và không có thanh nào có thể có độ dài bằng 0 hoặc nhỏ hơn sau một thao tác. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp số lượng que và độ dài của chúng. Kết quả đầu ra là số thao tác tối thiểu cần thiết để đảm bảo rằng một số bộ ba que có thể được tạo thành bằng nhau, tương đương với việc tạo thành một tam giác đều. 

Xem xét các ràng buộc, chúng tôi có tổng cộng tối đa 300 que trong tất cả các trường hợp thử nghiệm. Giá trị này đủ nhỏ để xem xét các giải pháp bậc hai về số que trong mỗi trường hợp thử nghiệm. Bản thân chiều dài của thanh có thể rất lớn (lên đến$10^9$), do đó, bất kỳ giải pháp nào cố gắng lặp lại một cách rõ ràng trên tất cả các độ dài có thể sẽ không khả thi. Thách thức không nằm ở việc lặp lại các que mà ở việc tính toán sự khác biệt một cách hiệu quả. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó các que đã bằng nhau, trong đó số lượng thao tác tối thiểu yêu cầu đưa que ở giữa khớp với các cực trị hoặc khi ba độ dài gần nhất là các số liên tiếp. Ví dụ, cho gậy`[1, 2, 3]`, tam giác đều tối ưu có độ dài cạnh`2`với hai thao tác: tăng`1`ĐẾN`2`và giảm`3`ĐẾN`2`. Một giải pháp bất cẩn có thể luôn cố gắng chọn cây gậy nhỏ nhất hoặc lớn nhất làm mục tiêu, điều này sẽ tạo ra những câu trả lời dưới mức tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ là xem xét mọi bộ ba que có thể có, tính toán số lượng thao tác tối thiểu cần thiết để cân bằng chúng và lấy số nhỏ nhất trong số tất cả các bộ ba. Với tối đa 300 que cho mỗi trường hợp thử nghiệm, có$\binom{300}{3} \approx 4.5 \times 10^6$sinh ba. Đối với mỗi bộ ba, chúng ta sẽ cần một vài phép tính số học, khả thi về mặt kỹ thuật với các ràng buộc, nhưng chậm và cồng kềnh không cần thiết. 

Cái nhìn sâu sắc quan trọng là sắp xếp các que. Sau khi được sắp xếp, ba que gần nhất sẽ luôn cho giải pháp tối ưu vì việc cân bằng các que cách xa nhau sẽ tốn nhiều thao tác hơn so với việc cân bằng các độ dài gần nhau. Sau khi sắp xếp, chúng ta chỉ cần xét từng nhóm ba que liên tiếp. Đối với bất kỳ bộ ba nào`(x, y, z)`với`x ≤ y ≤ z`, các phép toán tối thiểu để làm cho cả ba bằng nhau là`abs(y-x) + abs(z-y)`. Công thức này phát sinh vì di chuyển`y`để phù hợp`x`hoặc`z`không bao giờ tốt hơn việc di chuyển từng cây gậy về phía dải phân cách. Việc sắp xếp giúp giảm bớt vấn đề từ quét khối sang quét tuyến tính sau khi sắp xếp, nhanh hơn nhiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm với n=300 | 
| Sắp xếp + Trượt bộ ba | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`n`và mảng độ dài thanh. 
3. Sắp xếp mảng theo thứ tự không giảm. Việc sắp xếp đảm bảo rằng những điều chỉnh tối thiểu luôn được tìm thấy giữa các que liên tiếp. 
4. Khởi tạo một biến`min_ops`đến vô cùng. Điều này sẽ theo dõi số lượng hoạt động tối thiểu cho trường hợp thử nghiệm này. 
5. Lặp lại mảng từ chỉ mục`0`ĐẾN`n-3`. Đối với mỗi chỉ số`i`, xét bộ ba`(a[i], a[i+1], a[i+2])`. 
6. Tính số phép toán cần thiết để làm cho bộ ba bằng nhau:`a[i+2] - a[i]`. Điều này là do các bước di chuyển tối thiểu để cân bằng ba số đạt được bằng cách làm cho tất cả chúng bằng trung vị, mà trong một bộ ba được sắp xếp luôn là`a[i+1]`. Di chuyển`a[i]`Và`a[i+2]`ĐẾN`a[i+1]`chi phí`(a[i+1]-a[i]) + (a[i+2]-a[i+1]) = a[i+2]-a[i]`. 
7. Cập nhật`min_ops`nếu giá trị này nhỏ hơn. 
8. Sau khi quét tất cả các bộ ba, xuất ra`min_ops`. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng chúng tôi xem xét mức chênh lệch nhỏ nhất giữa ba cột. Việc sử dụng số trung vị sẽ giảm thiểu tổng số chênh lệch tuyệt đối. Bằng cách lặp qua các bộ ba liên tiếp, chúng tôi đảm bảo rằng chúng tôi không bỏ lỡ số lượng phép toán nhỏ hơn, bởi vì bất kỳ bộ ba nào có mức chênh lệch lớn hơn sẽ yêu cầu nhiều phép toán hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    min_ops = float('inf')
    for i in range(n - 2):
        ops = a[i+2] - a[i]
        if ops < min_ops:
            min_ops = ops
    print(min_ops)
```Đầu tiên, giải pháp sẽ đọc số lượng trường hợp thử nghiệm và lặp lại chúng. Việc sắp xếp được thực hiện cho từng mảng để sắp xếp các que để điều chỉnh tối thiểu. Sự tinh tế quan trọng là nhận ra rằng`a[i+2] - a[i]`là đủ; cố gắng di chuyển tất cả các cây gậy đến một trong hai cực sẽ là không tối ưu. Xử lý ranh giới (`n-2`) đảm bảo không có lỗi ngoài phạm vi. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`[1, 2, 3]`(đã sắp xếp rồi) 

| tôi | Bộ ba | ops = a[i+2]-a[i] | phút_ops | 
| --- | --- | --- | --- | 
| 0 | (1,2,3) | 2 | 2 | 

Đầu ra:`2`. Điều này phù hợp với các hoạt động tối thiểu bằng cách di chuyển`1 → 2`Và`3 → 2`. 

**Ví dụ 2:** 

đầu vào:`[7, 3, 7, 3]`→ sắp xếp`[3, 3, 7, 7]`| tôi | Bộ ba | hoạt động | phút_ops | 
| --- | --- | --- | --- | 
| 0 | (3,3,7) | 4 | 4 | 
| 1 | (3,7,7) | 4 | 4 | 

Đầu ra:`4`. Sự điều chỉnh tối thiểu đến từ việc cân bằng`(3,3,7)`bằng cách di chuyển`3 → 7`Và`7 → 7`. 

Những ví dụ này cho thấy thuật toán chọn chính xác mức chênh lệch tối thiểu giữa các gậy liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp chiếm ưu thế; quét bộ ba là tuyến tính | 
| Không gian | O(n) | Lưu trữ mảng cho từng trường hợp thử nghiệm | 

Với`n`lên tới 300 cho mỗi trường hợp thử nghiệm và tổng số`n`300, giá trị này rất phù hợp trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("4\n3\n1 2 3\n4\n7 3 7 3\n5\n3 4 2 1 1\n8\n3 1 4 1 5 9 2 6\n") == "2\n4\n1\n1", "sample 1"

# Custom cases
assert run("1\n3\n5 5 5\n") == "0", "all equal sticks"
assert run("1\n3\n1 1 10\n") == "9", "one long stick"
assert run("1\n4\n1 2 2 3\n") == "1", "multiple minimal triplets"
assert run("1\n5\n1 2 3 4 5\n") == "2", "consecutive numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 5 5 5`|`0`| Đã bình đẳng, không có hoạt động | 
|`3 1 1 10`|`9`| Một cây gậy cực đoan, cần điều chỉnh lớn | 
|`4 1 2 2 3`|`1`| Nhiều bộ ba ứng cử viên | 
|`5 1 2 3 4 5`|`2`| Bộ ba gần nhất giảm thiểu hoạt động | 

## Vỏ cạnh 

cho`[1, 2, 3]`, thuật toán chọn`(1,2,3)`và tính toán`3-1=2`. Di chuyển`1 → 2`Và`3 → 2`đạt được tam giác đều, cho thấy phương pháp này nhắm mục tiêu chính xác vào trung tuyến chứ không phải cực trị. Vì`[1, 1, 10]`, bộ ba`(1,1,10)`sản lượng`10-1=9`, chỉ đúng số lần di chuyển để đưa tất cả gậy về`1`hoặc`10`. Quá trình quét được sắp xếp đảm bảo không bỏ sót sự kết hợp nào tốt hơn.
