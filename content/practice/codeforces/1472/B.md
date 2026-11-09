---
title: "CF 1472B - Phân chia công bằng"
description: "Chúng tôi có một bộ sưu tập kẹo trong đó mỗi viên kẹo nặng 1 hoặc 2 gam. Alice và Bob muốn chia số kẹo sao cho tổng trọng lượng mỗi người nhận được bằng nhau. Đầu vào cho mỗi trường hợp thử nghiệm là số lượng kẹo và trọng lượng riêng của chúng."
date: "2026-06-11T00:33:46+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1472
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 693 (Div. 3)"
rating: 800
weight: 1472
solve_time_s: 325
verified: true
draft: false
---

[CF 1472B - Phân chia công bằng](https://codeforces.com/problemset/problem/1472/B) 

**Đánh giá:** 800 
**Tags:** dp, tham lam, toán học 
**Thời gian giải:** 5 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập kẹo trong đó mỗi viên kẹo nặng 1 hoặc 2 gam. Alice và Bob muốn chia số kẹo sao cho tổng trọng lượng mỗi người nhận được bằng nhau. Đầu vào cho mỗi trường hợp thử nghiệm là số lượng kẹo và trọng lượng riêng của chúng. Đầu ra chỉ đơn giản là "CÓ" nếu có thể phân chia công bằng và "KHÔNG" nếu ngược lại. 

Các ràng buộc của bài toán cho chúng ta biết rằng số lượng kẹo trong mỗi trường hợp thử nghiệm,\(n\), nhiều nhất là 100, nhưng có thể lên tới 10.000 trường hợp thử nghiệm. Tổng số kẹo trong tất cả các trường hợp thử nghiệm không vượt quá 100.000. Điều này có nghĩa là chúng tôi không thể chấp nhận bất cứ điều gì tệ hơn O(n) cho mỗi trường hợp thử nghiệm nếu chúng tôi muốn phù hợp với giới hạn thời gian 2 giây, vì các hoạt động trong phạm vi\(10^8\)thường thì ổn nhưng\(10^9\)có thể chặt chẽ trong Python. 

Một điểm tinh tế nảy sinh vì kẹo không thể chia được. Ngay cả khi tổng trọng số là chẵn thì không phải lúc nào cũng đủ để đảm bảo sự phân chia công bằng. Ví dụ: với ba viên kẹo có trọng lượng 2, 1 và 2 gam, tổng số là 5, là số lẻ và do đó ngay lập tức không thể thực hiện được. Một giải pháp bất cẩn có thể chỉ kiểm tra xem tổng số tiền có chẵn không, điều này sẽ thất bại trong một số trường hợp đặc biệt như chỉ có một viên kẹo có trọng lượng 2 hoặc các kết hợp trong đó tổng số chẵn nhưng việc phân phối không thể cân bằng khi sử dụng kẹo 1 gam và 2 gam có sẵn. 

Các trường hợp đặc biệt bao gồm chỉ có kẹo 1 gam, chỉ có kẹo 2 gam, tổng trọng lượng lẻ và các kết hợp trong đó tổng trọng lượng chẵn nhưng có số lẻ kẹo 1 gam. Những điều này ảnh hưởng đến khả năng hình thành hai tập hợp con bằng nhau, vì vậy chúng ta phải suy luận cẩn thận về cả tính chẵn lẻ của tổng số và số lượng của từng loại kẹo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi tập hợp con kẹo có thể có và kiểm tra xem có tập hợp con nào có tổng trọng lượng bằng một nửa tổng trọng lượng hay không. Điều này tương đương với bài toán tổng con. Đối với một trường hợp thử nghiệm có n=100, số lượng tập hợp con là\(2^{100}\), lớn về mặt thiên văn và do đó hoàn toàn không thực tế. Ngay cả khi sử dụng phương pháp lập trình động để kiểm tra tổng bằng tổng trọng số chia cho 2 cũng sẽ mất O(n * sum(trọng số)), có thể lên tới 200 cho mỗi trường hợp thử nghiệm. Điều đó có thể chấp nhận được, nhưng chúng ta còn có thể làm tốt hơn nữa bằng cách suy luận số học đơn giản. 

Quan sát quan trọng là chỉ có hai loại kẹo. Cho phép\(c_1\)là số viên kẹo 1 gam và\(c_2\)là số kẹo 2 gram. Tổng trọng lượng là\(c_1 + 2 c_2\). Để có sự phân chia công bằng, tổng số này phải bằng nhau. Hơn nữa, nếu tổng trọng số là chẵn thì thử thách sẽ giảm xuống còn việc hình thành một tập hợp con có tổng bằng một nửa tổng. Bởi vì chúng ta chỉ có thể chọn số nguyên của kẹo 1 gam và 2 gam, bằng một nửa tổng trọng lượng, hãy gọi nó là\(S\), phải có thể đạt được bằng cách sử dụng sự kết hợp của\(c_1\)những cái và\(c_2\)hai. Điều này chỉ không thể xảy ra nếu S là số lẻ và không có viên kẹo 1 gam nào để điều chỉnh tính chẵn lẻ. Nói cách khác, nếu tổng trọng lượng là số lẻ thì câu trả lời là "KHÔNG". Nếu tổng trọng lượng là số chẵn thì câu trả lời là "CÓ" trừ khi S là số lẻ và không có viên kẹo nào nặng 1 gram làm cho nó là số lẻ, trong trường hợp đó câu trả lời là "KHÔNG". 

Thông tin chi tiết này cho phép kiểm tra O(1) rất đơn giản cho mỗi trường hợp thử nghiệm bằng cách sử dụng số lượng 1 giây và 2 giây. Chúng ta không cần phải lặp qua các tập hợp con hoặc sử dụng lập trình động nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc n và danh sách trọng lượng kẹo. 
2. Đếm số kẹo 1 gram,\(c_1\), và số kẹo 2 gam,\(c_2\). Bước này cung cấp cho chúng ta dữ liệu quan trọng để suy luận về phân phối tổng. 
3. Tính tổng trọng lượng\(T = c_1 + 2 c_2\). Nếu như\(T\)là số lẻ, ghi "KHÔNG" vì không thể phân chia công bằng. 
4. Nếu\(T\)chẵn, tính một nửa tổng số,\(S = T / 2\). Đây là tổng mục tiêu cho một trong hai tập hợp con. 
5. Kiểm tra xem có thể đạt được S bằng cách sử dụng kẹo 2 gram có sẵn hay không. Gọi k là tối thiểu của S // 2 và c_2, số lượng kẹo 2 gram tối đa chúng ta có thể sử dụng mà không vượt quá S. 
6. Trừ đi trọng lượng của số kẹo 2 gram,\(2 k\), từ S. Nếu trọng lượng còn lại có thể được che phủ chính xác bởi số kẹo 1 gram, nghĩa là nếu S - 2 k ≤ c_1, xuất ra "CÓ". Nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: thuật toán đảm bảo tính chính xác vì chúng tôi luôn chọn càng nhiều kẹo 2 gam càng tốt mà không vượt quá một nửa tổng số, điều này làm giảm số tiền còn lại được bao phủ bởi những viên kẹo 1 gam. Vì trọng số chỉ là 1 và 2 nên bất kỳ số tiền nào có thể đạt được đều là sự kết hợp của cả hai, vì vậy lựa chọn tham lam này là đủ. Việc kiểm tra tính chẵn lẻ đảm bảo rằng chúng ta không bao giờ thử chia tách không thể thực hiện được khi tổng số tiền là số lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_divide(weights):
    c1 = weights.count(1)
    c2 = weights.count(2)
    total = c1 + 2 * c2
    if total % 2 != 0:
        return "NO"
    half = total // 2
    # use as many 2s as possible without exceeding half
    max_twos = min(c2, half // 2)
    remaining = half - 2 * max_twos
    if remaining <= c1:
        return "YES"
    return "NO"

t = int(input())
for _ in range(t):
    n = int(input())
    weights = list(map(int, input().split()))
    print(can_divide(weights))
```Mã đếm số 1 và 2 trong mỗi trường hợp thử nghiệm. Nó ngay lập tức bác bỏ tổng số tiền lẻ. Đối với tổng số chẵn, nó tham lam lấp đầy một nửa số tiền bằng càng nhiều kẹo 2 gram càng tốt và kiểm tra xem số tiền còn lại có thể được tạo ra bằng kẹo 1 gram hay không. Sử dụng min(c2, Half // 2) sẽ tránh vượt quá tổng mục tiêu. Giải pháp xử lý nhiều trường hợp thử nghiệm một cách hiệu quả và tránh việc lặp lại không cần thiết. 

## Ví dụ đã hoạt động 

Trace Sample 1, trường hợp thử nghiệm thứ nhất và thứ tư. 

| n | tạ | c1 | c2 | tổng cộng | một nửa | max_twos | còn lại | đầu ra | 
|---|---|---|---|---|---|---|---|---| 
| 2 | [1,1] | 2 | 0 | 2 | 1 | 0 | 1 | CÓ | 
| 3 | [2,2,2] | 0 | 3 | 6 | 3 | 1 | 1 | KHÔNG | 

Dấu vết đầu tiên cho thấy với hai viên kẹo 1 gram, tổng số là 2, một nửa là 1 và chúng ta cần 1 viên kẹo có trọng lượng 1 để đạt được mục tiêu tồn tại, cho kết quả "CÓ". Dấu vết thứ tư có tổng cộng 6, nửa 3, chúng ta có thể lấy một viên kẹo 2 gam đóng góp 2, nhưng sau đó cần thêm 1 viên nữa để đạt 3, và vì không có viên kẹo 1 gam nên chúng ta không thể, cho "KHÔNG". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(n) | Đếm 1 và 2 yêu cầu phải duyệt qua các trọng số một lần cho mỗi trường hợp thử nghiệm | 
| Không gian | O(n) | Lưu trữ mảng trọng số cho mỗi trường hợp thử nghiệm | 

Điều này phù hợp một cách thoải mái trong giới hạn vì tổng n trên tất cả các trường hợp thử nghiệm là ≤ 10^5 và chúng tôi chỉ thực hiện một lần vượt qua cho mỗi trường hợp thử nghiệm với bộ nhớ bổ sung O(1). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        weights = list(map(int, input().split()))
        out.append(can_divide(weights))
    return "\n".join(out)

# provided samples
assert run("5\n2\n1 1\n2\n1 2\n4\n1 2 1 2\n3\n2 2 2\n3\n2 1 2\n") == "YES\nNO\nYES\nNO\nNO", "sample 1"

# custom cases
assert run("2\n1\n2\n2\n2 2\n") == "NO\nYES", "single candy and two equal twos"
assert run("2\n3\n1 1 1\n4\n2 2 2 2\n") == "NO\nYES", "odd number of 1s and all twos"
assert run("2\n4\n1 1 2 2\n3\n1 2 2\n") == "YES\nNO", "mixed combinations"
assert run("1\n5\n1 1 1 1 2\n") == "YES", "uneven but fair split possible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|
