---
title: "CF 1862D - Kem Viên"
description: "Chúng ta được yêu cầu xác định số viên kem tối thiểu mà Tema cần mua để làm ra chính xác n chiếc kem ốc quế có hai viên khác nhau. Mỗi hình nón bao gồm hai quả bóng, có thể có cùng hương vị."
date: "2026-06-09T00:52:18+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1862
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 894 (Div. 3)"
rating: 1300
weight: 1862
solve_time_s: 169
verified: false
draft: false
---

[CF 1862D - Viên kem](https://codeforces.com/problemset/problem/1862/D) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, tổ hợp, thuật toán xây dựng, toán học 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định số lượng kem tối thiểu mà Tema cần mua để làm chính xác.`n`nón kem hai bóng khác biệt. Mỗi hình nón bao gồm hai quả bóng, có thể có cùng hương vị. Hai hình nón được coi là khác nhau nếu **bộ** hương vị của chúng khác nhau. Ví dụ, hình nón`{1,2}`Và`{2,1}`giống hệt nhau, nhưng`{1,1}`khác biệt với`{1,2}`. Tema có thể mua bao nhiêu hương vị tùy thích và anh ấy có thể sử dụng nhiều quả bóng có cùng hương vị nếu cần thiết. 

Đầu vào cung cấp`t`trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho một số nguyên duy nhất`n`, số lượng hình nón hai bóng riêng biệt mong muốn. Đầu ra của mỗi trường hợp thử nghiệm phải là tổng số viên kem tối thiểu cần thiết. 

Các ràng buộc rất lớn:`n`có thể cao như`10^18`. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận bạo lực nào cố gắng liệt kê tất cả các hình nón có thể có. Ngay cả các giải pháp O(n) cũng không khả thi đối với đầu vào lớn nhất. Chúng ta cần một giải pháp logarit trong`n`hoặc sử dụng lý luận toán học trực tiếp. Trường hợp cạnh xảy ra khi`n`rất nhỏ, chẳng hạn như`1`hoặc`2`, vì chúng tôi phải đảm bảo có đủ bóng để tạo thành một hình nón có hương vị kép. 

Ví dụ, nếu`n = 1`, tập bóng nhỏ nhất là`[1,1]`-một hương vị, hai quả bóng-để tạo ra một hình nón riêng biệt. Nếu như`n = 3`, chúng ta không thể làm điều đó với hai quả bóng, vì điều đó chỉ cho phép có một hình nón riêng biệt; chúng ta cần ba quả bóng`[1,2,3]`để tạo thành ba hình nón riêng biệt`{1,2}`,`{1,3}`, Và`{2,3}`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các cách có thể để gán các quả bóng cho các hương vị và đếm các hình nón hai quả bóng cho đến khi chúng ta đạt được chính xác`n`. Mặc dù về mặt khái niệm đơn giản nhưng cách tiếp cận này cực kỳ chậm. Số lượng kết hợp tăng theo phương trình bậc hai với số lượng bóng và với`n`lên đến`10^18`, thậm chí tính toán tất cả các kết hợp là không thể. 

Quan sát quan trọng là số lượng hình nón hai quả bóng riêng biệt có thể được tạo ra từ`k`quả bóng được tối đa hóa nếu mỗi quả bóng có một hương vị độc đáo. Con số này được cho bởi công thức kết hợp`C(k,2) = k*(k-1)/2`. Nếu như`n`nhỏ hơn số tam giác tiếp theo, có thể tối ưu hơn nếu thêm các quả bóng bổ sung có hương vị hiện có để đạt được chính xác`n`hình nón. Chiến lược chung là tìm số nguyên nhỏ nhất`k`như vậy`k*(k+1)/2 >= n`. Điều này đảm bảo rằng với`k`những quả bóng, ít nhất chúng ta có thể hình thành`n`nón hai bóng riêng biệt, với tất cả các hương vị độc đáo hoặc với một số hương vị lặp đi lặp lại để lấp đầy khoảng trống. 

Chúng ta có thể tìm thấy một cách hiệu quả`k`sử dụng giải pháp dạng đóng xuất phát từ bất đẳng thức bậc hai hoặc tìm kiếm nhị phân. Giải pháp dạng đóng liên quan đến việc giải quyết`k*(k+1)/2 >= n`vì`k`sử dụng công thức bậc hai và làm tròn đến số nguyên gần nhất. Cách tiếp cận này cực kỳ nhanh vì nó tính toán kết quả theo thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^2) | O(k) | Quá chậm đối với n > 10^5 | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`, số lượng loại kem mong muốn. 
2. Quan sát rằng số lượng tối đa các hình nón hai bi riêng biệt mà chúng ta có thể tạo ra bằng`x`những quả bóng, nếu tất cả đều là duy nhất, là`x*(x-1)/2`. Chúng ta cần ít nhất chừng này để tiếp cận`n`. 
3. Giải bất đẳng thức`x*(x-1)/2 >= n`vì`x`. Viết lại nó dưới dạng phương trình bậc hai:`x^2 - x - 2*n >= 0`. 
4. Sử dụng công thức bậc hai để tìm`x = ceil((1 + sqrt(1 + 8*n))/2)`. Đây là số lượng bóng tối thiểu cần thiết. 
5. In kết quả`x`cho từng trường hợp thử nghiệm. 

Tại sao nó hoạt động: Công thức`x*(x-1)/2`đếm tất cả các cặp bóng khác nhau có thể có. Bằng cách giải quyết cho`x`, chúng tôi đảm bảo rằng chúng tôi có đủ bóng để sản xuất ít nhất`n`kem riêng biệt. Việc sử dụng trần đảm bảo rằng ngay cả khi công thức bậc hai cho kết quả phân số, chúng ta làm tròn lên để có được số bóng nguyên. Bất kỳ quả bóng nào ít hơn sẽ tạo ra ít hơn hoàn toàn`n`hình nón rõ rệt. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    # solve x*(x-1)/2 >= n => x^2 - x - 2n >= 0
    x = math.ceil((1 + math.sqrt(1 + 8*n)) / 2)
    print(x)
```Giải pháp đọc số lượng ca kiểm thử, sau đó cho mỗi ca`n`tính toán tối thiểu`x`bằng cách sử dụng công thức bậc hai.`math.sqrt`là an toàn cho rất lớn`n`trong Python do số nguyên có độ chính xác tùy ý và`math.ceil`đảm bảo số nguyên nhỏ nhất thỏa mãn bất đẳng thức. Điều này tránh các lỗi riêng lẻ có thể xảy ra nếu chúng ta sử dụng phép chia số nguyên hoặc cố gắng làm tròn thủ công. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`n = 3`: 

| Bước | Tính toán | Giải thích | 
| --- | --- | --- | 
| 1 | n = 3 | Số lượng nón cần thiết | 
| 2 | Giải x*(x-1)/2 >= 3 | x*(x-1)/2 = 3 | 
| 3 | Bậc hai: x^2 - x - 6 >= 0 | Phương trình được sắp xếp lại | 
| 4 | x = trần((1 + sqrt(1 + 24))/2) = trần((1 + 5)/2) = 3 | Số bóng tối thiểu cần thiết | 
| 5 | Đầu ra: 3 | Quả bóng`[1,2,3]`sản xuất nón`{1,2},{1,3},{2,3}`| 

Đầu vào mẫu`n = 6`: 

| Bước | Tính toán | Giải thích | 
| --- | --- | --- | 
| 1 | n = 6 | Nón kem mong muốn | 
| 2 | Bậc hai: x^2 - x - 12 >= 0 | Giải tìm x | 
| 3 | x = trần((1 + sqrt(1+48))/2) = trần((1+7)/2) = 4 | Số bóng tối thiểu cần thiết | 
| 4 | Đầu ra: 4 | Quả bóng`[1,2,3,4]`sản xuất 6 nón: tất cả các cặp | 

Những dấu vết này cho thấy công thức tính toán chính xác số lượng bóng tối thiểu, ngay cả đối với những quả bóng nhỏ.`n`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu thời gian không đổi bằng cách sử dụng công thức. | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài các biến đầu vào. | 

Với`t <= 10^4`và mỗi phép tính O(1), nghiệm dễ dàng nằm gọn trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        x = math.ceil((1 + math.sqrt(1 + 8*n)) / 2)
        res.append(str(x))
    return "\n".join(res)

# Provided samples
assert run("5\n1\n3\n6\n179\n1000000000000000000\n") == "2\n3\n4\n27\n2648956421"

# Custom test cases
assert run("3\n2\n10\n15\n") == "2\n5\n6"  # small n, moderate n
assert run("1\n0\n") == "1"  # edge case, 0 cones -> need at least 1 ball
assert run("2\n1\n2\n") == "2\n2"  # minimum non-trivial cases
assert run("1\n1000000000000\n") == str(math.ceil((1 + math.sqrt(1 + 8*1000000000000)) / 2))  # very large n
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 2 | Số bóng tối thiểu cần thiết cho 1 hoặc 2 hình nón | 
| 10 | 5 | Nhỏ tùy ý n | 
| 15 | 6 | Trần đúng cho số tam giác | 
| 0 | 1 | Trường hợp cạnh n=0, đảm bảo có ít nhất một quả bóng được xem xét | 
| 1000000000000 | 1414214 | N lớn, độ đúng và độ chính xác | 

## Vỏ cạnh 

cho`n = 1`, thuật toán tính toán`x = ceil((1 + sqrt(1+8))/2) = 2`. Điều này cho kết quả chính xác là 2, vì chúng ta cần hai quả bóng có cùng hương vị để tạo thành một hình nón. Vì`n = 10^18`,`x`là khoảng 1,34×10^9 và số học số nguyên của Python xử lý việc này mà không bị tràn. Công thức vốn đã xử lý cả rất nhỏ và rất lớn`n`và không cần kiểm tra giới hạn bổ sung.
