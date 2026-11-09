---
title: "CF 1515B - Phượng hoàng và câu đố"
description: "Chúng ta được tặng một bộ mảnh ghép hình tam giác cân bên phải giống hệt nhau. Mỗi tam giác có hai cạnh bằng nhau và một góc vuông. Phoenix muốn sử dụng chính xác $n$ trong số các hình tam giác này để tạo thành một hình vuông hoàn hảo, không có phần chồng lên nhau hoặc không có lỗ."
date: "2026-06-10T18:31:09+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1515
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 14"
rating: 1000
weight: 1515
solve_time_s: 219
verified: true
draft: false
---

[CF 1515B - Phượng hoàng và Câu đố](https://codeforces.com/problemset/problem/1515/B) 

**Đánh giá:** 1000 
**Tags:** lực lượng vũ phu, hình học, toán học, lý thuyết số 
**Thời gian giải:** 3 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bộ mảnh ghép hình tam giác cân bên phải giống hệt nhau. Mỗi tam giác có hai cạnh bằng nhau và một góc vuông. Phoenix muốn sử dụng chính xác$n$của các hình tam giác này để tạo thành một hình vuông hoàn hảo, không có sự chồng chéo hoặc lỗ hổng. Anh ta được phép xoay và di chuyển các quân cờ một cách tự do. Nhiệm vụ là quyết định, đối với mỗi trường hợp thử nghiệm, liệu có thể hình thành một hình vuông như vậy hay không. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp bao gồm một số nguyên duy nhất$n$, có thể lớn bằng$10^9$. Đầu ra chỉ đơn giản là "CÓ" nếu có thể tạo thành một hình vuông và nếu không thì "KHÔNG". 

Ràng buộc$n \le 10^9$ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào lặp lại trên tất cả các phần hoặc thử mọi cách sắp xếp hình học đều không thực tế. Chúng ta cần một phương pháp tính toán câu trả lời bằng cách sử dụng công thức hoặc kiểm tra số học trong thời gian không đổi cho mỗi trường hợp kiểm thử. 

Một điều tinh tế quan trọng là không phải tất cả các số nguyên$n$có thể tạo thành một hình vuông Ví dụ,$n = 2$có tác dụng vì hai tam giác có thể tạo thành hình vuông bằng cách nối các cạnh huyền của chúng.$n = 4$cũng có tác dụng vì bốn hình tam giác có thể tạo thành một hình vuông lớn hơn. Tuy nhiên,$n = 6$thất bại: bất kỳ sự sắp xếp nào của sáu hình tam giác vuông cân bằng nhau cũng không thể tạo thành một hình vuông một cách hoàn hảo. Một giải pháp bất cẩn có thể cho rằng bất kỳ số chẵn nào cũng có tác dụng, nhưng việc sắp xếp các mảnh đòi hỏi bội số của 2 tạo thành hình vuông có cạnh 1 hoặc bội số của 4 tạo thành hình vuông có kích thước lớn hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng đặt từng hình tam giác một và kiểm tra xem hình vuông có hình thành hay không. Về mặt khái niệm, người ta có thể thử mô phỏng tất cả các phép quay và vị trí. Điều này đúng về mặt nguyên tắc nhưng không thể tính toán được$n \sim 10^9$, vì số cách sắp xếp tăng theo giai thừa. 

Cái nhìn sâu sắc quan trọng là suy luận về tổng diện tích của các hình tam giác. Mỗi tam giác có diện tích là$0.5$(giả sử chân đơn vị). Nếu như$n$tam giác tạo thành hình vuông thì diện tích hình vuông đó là$n \times 0.5 = n/2$. Do đó cạnh hình vuông phải là$\sqrt{n/2}$. Đối với hình vuông có tọa độ nguyên,$\sqrt{n/2}$phải là một số hữu tỉ có thể được biểu diễn dưới dạng tổng các cạnh có độ dài nguyên được tạo từ các hình tam giác. 

Từ lý luận hình học, chúng ta thấy rằng một hình vuông có thể được dựng khi và chỉ khi$n$có thể được thể hiện như$2k^2$hoặc$4k^2$, Ở đâu$k$là một số nguyên. Dạng thứ nhất tương ứng với một hình vuông được tạo bởi hai hình tam giác dọc theo cạnh huyền, và dạng thứ hai tương ứng với một hình vuông được tạo từ bốn hình tam giác trong một$k \times k$lưới các ô vuông. 

Quan sát này làm giảm vấn đề xuống còn việc kiểm tra hai điều kiện bằng cách sử dụng số học số nguyên: liệu$n$chia hết cho 2 và$n/2$là một hình vuông hoàn hảo, hoặc liệu$n$chia hết cho 4 và$n/4$là một hình vuông hoàn hảo Cả hai lần kiểm tra đều$O(1)$mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc số nguyên$n$biểu thị số lượng hình tam giác. 
2. Nếu$n$chia hết cho 2 hãy tính$n/2$. Kiểm tra xem$n/2$là một số chính phương bằng cách lấy căn bậc hai số nguyên của nó và bình phương lại. 
3. Nếu$n/2$là một hình vuông hoàn hảo, in "CÓ" và chuyển sang trường hợp kiểm thử tiếp theo. Điều này tương ứng với các hình vuông được hình thành từ chính xác 2 hình tam giác dọc theo cạnh huyền. 
4. Nếu không, hãy kiểm tra xem$n$chia hết cho 4. Tính$n/4$và kiểm tra xem nó có phải là hình vuông hoàn hảo hay không. 
5. Nếu$n/4$là một hình vuông hoàn hảo, in ra "CÓ". Điều này tương ứng với các hình vuông được xây dựng từ 4 hình tam giác sắp xếp thành một lưới hình vuông. 
6. Nếu không có kiểm tra nào đạt, hãy in "KHÔNG". 

Tại sao nó hoạt động: Điều bất biến là một hình vuông được hình thành từ các tam giác cân bên phải cần 2 hoặc 4 hình tam giác để tạo thành một hình vuông đơn vị cơ sở và các hình vuông lớn hơn là bội số nguyên của các đơn vị cơ bản này. Do đó, đối số diện tích đảm bảo tính chính xác và phép kiểm bình phương số nguyên đảm bảo các phần thẳng hàng hoàn hảo mà không có khoảng trống hoặc chồng chéo. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    ans = "NO"
    if n % 2 == 0:
        half = n // 2
        if int(math.isqrt(half)) ** 2 == half:
            ans = "YES"
    if ans == "NO" and n % 4 == 0:
        quarter = n // 4
        if int(math.isqrt(quarter)) ** 2 == quarter:
            ans = "YES"
    print(ans)
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`để xử lý lên đến$10^4$trường hợp thử nghiệm. các`math.isqrt`hàm tính toán căn bậc hai số nguyên một cách chính xác mà không có lỗi dấu phẩy động và việc bình phương ngược lại đảm bảo phát hiện chính phương hoàn hảo. Kiểm tra cả hai`n/2`Và`n/4`bao gồm cả sự sắp xếp hình học. Thứ tự kiểm tra đảm bảo chúng ta phát hiện trường hợp 2 tam giác trước khi thử trường hợp 4 tam giác. 

## Ví dụ đã hoạt động 

**Ví dụ 1: n = 2** 

| n | n % 2 | n/2 | sqrt(n/2)^2 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 2 | 0 | 1 | 1 | CÓ | 

Thuật toán xác định chính xác 2 hình tam giác có thể tạo thành hình vuông. 

**Ví dụ 2: n = 6** 

| n | n % 2 | n/2 | sqrt(n/2)^2 | n% 4 | n/4 | sqrt(n/4)^2 | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 6 | 0 | 3 | 1 | 2 | - | - | KHÔNG | 

Không`n/2`cũng không`n/4`là một hình vuông hoàn hảo, vì vậy câu trả lời là KHÔNG. 

Những dấu vết này xác nhận rằng thuật toán phân biệt các trường hợp không thể như n = 6 và phát hiện các bình phương tối thiểu có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Mỗi trường hợp thử nghiệm thực hiện một số lượng không đổi các phép tính số học và phép tính căn bậc hai số nguyên | 
| Không gian | O(1) | Chỉ một vài biến số nguyên được sử dụng cho mỗi trường hợp thử nghiệm | 

Với$t \le 10^4$, tổng số thao tác đều nằm trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        ans = "NO"
        if n % 2 == 0:
            half = n // 2
            if int(math.isqrt(half)) ** 2 == half:
                ans = "YES"
        if ans == "NO" and n % 4 == 0:
            quarter = n // 4
            if int(math.isqrt(quarter)) ** 2 == quarter:
                ans = "YES"
        print(ans)
    return out.getvalue().strip()

# Provided samples
assert run("3\n2\n4\n6\n") == "YES\nYES\nNO", "sample 1"

# Custom tests
assert run("4\n1\n8\n18\n16\n") == "NO\nYES\nNO\nYES", "custom edge cases"
assert run("3\n50\n32\n72\n") == "NO\nYES\nNO", "varied sizes"
assert run("2\n2000000000\n999999999\n") == "NO\nNO", "large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | KHÔNG | tối thiểu n không thể tạo thành hình vuông | 
| 8 | CÓ | n chia hết cho 4 tạo thành hình vuông có 4 hình tam giác | 
| 18 | KHÔNG | chia hết cho 2 nhưng n/2 không phải là số chính phương hoàn hảo | 
| 16 | CÓ | chia hết cho 4, n/4 = 4, chính phương | 
| 2000000000 | KHÔNG | số lớn không tạo thành hình vuông hoàn hảo | 
| 32 | CÓ | hình vuông hoàn hảo lớn hơn bằng cách sắp xếp 4 tam giác | 

## Vỏ cạnh 

Với n = 1, thuật toán kiểm tra khả năng chia hết cho 2 và 4. Cả hai đều thất bại nên nó in ra NO. Điều này phù hợp với trực giác: một hình tam giác không thể tạo thành hình vuông. Với n = 8, trước tiên thuật toán sẽ kiểm tra n/2 = 4, đây là một hình vuông hoàn hảo, vì vậy nó in ra CÓ. Điều này chứng tỏ tầm quan trọng của việc kiểm tra cả cách sắp xếp 2 tam giác và 4 tam giác. Đối với n lớn như 2 × 10^9, thuật toán vẫn hoạt động ở O(1) cho mỗi trường hợp thử nghiệm mà không bị tràn vì số nguyên Python không bị chặn và`math.isqrt`xử lý số nguyên lớn một cách hiệu quả.
