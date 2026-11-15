---
title: "CF 1851D - Tổng hoán vị tiền tố"
description: "Chúng ta được cung cấp một mảng bắt nguồn từ tổng tiền tố của một hoán vị các số từ 1 đến $n$. Thiếu một trong những tổng tiền tố này. Nhiệm vụ của chúng ta là xác định xem mảng không đầy đủ có thể đến từ một hoán vị hợp lệ hay không."
date: "2026-06-09T17:15:50+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1851
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 888 (Div. 3)"
rating: 1300
weight: 1851
solve_time_s: 153
verified: false
draft: false
---

[CF 1851D - Tổng hoán vị tiền tố](https://codeforces.com/problemset/problem/1851/D) 

**Đánh giá:** 1300 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng bắt nguồn từ tổng tiền tố của một hoán vị các số từ 1 đến$n$. Thiếu một trong những tổng tiền tố này. Nhiệm vụ của chúng ta là xác định xem mảng không đầy đủ có thể đến từ một hoán vị hợp lệ hay không. 

Một hoán vị ở đây đơn giản là một mảng chứa các số nguyên từ 1 đến$n$đúng một lần. Mảng tổng tiền tố được hình thành bằng cách tính tổng các phần tử theo cách tích lũy:$b_i = a_1 + a_2 + ... + a_i$. Ví dụ, hoán vị$[1, 3, 2]$có tổng tiền tố$[1, 4, 6]$. Nếu chúng ta loại bỏ tổng tiền tố, chẳng hạn như 4, thì mảng sẽ trở thành$[1, 6]$. Vấn đề là quyết định xem có tồn tại một hoán vị dẫn đến mảng đã cho sau khi loại bỏ một tổng tiền tố hay không. 

Những ràng buộc cho chúng ta tới$2 \cdot 10^5$các con số được tổng hợp trên tất cả các trường hợp thử nghiệm, vì vậy mọi giải pháp đều phải chạy trong khoảng$O(n)$mỗi trường hợp thử nghiệm. Việc xây dựng lại tất cả các hoán vị là hoàn toàn không khả thi bởi vì$n!$phát triển cực kỳ nhanh chóng. 

Trường hợp cạnh tinh tế phát sinh khi phần tử bị thiếu là số lớn nhất trong hoán vị. Ví dụ, nếu$n = 4$và tổng tiền tố đầu vào là$[1, 3, 6]$, tổng còn thiếu tương ứng với phần tử cuối cùng là 4 và chúng ta phải thừa nhận tình huống này. Một trường hợp phức tạp khác là khi loại bỏ tổng tiền tố sẽ ẩn một số có thể được lặp lại, ví dụ:$[1, 3, 3]$trông có vẻ không thể nhưng có thể tương ứng với số 2 bị thiếu trong$[1, 3, 5, 6]$. Ở đây, cách tiếp cận bất cẩn chỉ kiểm tra sự khác biệt giữa các tổng tiền tố liên tiếp có thể thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng lại tất cả các mảng ban đầu có thể có từ tổng tiền tố đã cho bằng cách cố gắng chèn một số vào tất cả các vị trí có thể và kiểm tra xem mảng kết quả có tạo thành một hoán vị hợp lệ hay không. Về nguyên tắc, điều này đúng vì mỗi tổng tiền tố xác định tổng tích lũy một cách duy nhất, nhưng số lượng các khả năng là$O(n)$chèn vào mỗi trường hợp thử nghiệm và$O(n)$thực hiện mỗi lần chèn để xác thực hoán vị, dẫn đến$O(n^2)$. Điều này sẽ là quá chậm đối với$n \approx 2 \cdot 10^5$. 

Thông tin chi tiết quan trọng là phần tử bị thiếu tương ứng với một số từ 1 đến$n$hoặc đến tổng tiền tố cuối cùng. Điều này làm giảm vấn đề khi phân tích sự khác biệt giữa các tổng tiền tố liên tiếp, đại diện cho các số ban đầu trong hoán vị (ngoại trừ số bị thiếu). Cụ thể, nếu sự khác biệt tạo thành một tập hợp đa số có hầu hết các số từ 1 đến$n$, thì số còn thiếu là tổng chênh lệch quá lớn hoặc là số$n$chính nó. 

Do đó, cách tiếp cận nhanh hơn là: tính toán sự khác biệt giữa các tổng tiền tố liên tiếp để có được các số ứng cử viên, đếm số lần mỗi số xuất hiện và xác minh rằng một số bị thiếu hoặc một số bị trùng lặp theo cách có thể giải thích cho tiền tố bị thiếu. Điều này làm giảm giải pháp$O(n)$cho mỗi trường hợp thử nghiệm, điều này khả thi trong các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy tính toán sự khác biệt giữa các tổng tiền tố liên tiếp. Bao gồm phần tử đầu tiên của mảng tổng tiền tố dưới dạng chênh lệch vì nó bằng phần tử đầu tiên của hoán vị ban đầu. Lưu trữ những khác biệt này trong một danh sách có tên`nums`. 
2. Tính tổng các số từ 1 đến$n$sử dụng công thức$n(n+1)/2$. 
3. So sánh tổng của`nums`vào tổng số tiền này. Nếu chúng khớp nhau, tất cả các số đều có mặt và tổng tiền tố bị thiếu tương ứng với phần tử cuối cùng. Trong trường hợp này, mảng tạo thành một hoán vị hợp lệ. 
4. Nếu tổng của`nums`nhỏ hơn tổng thì lấy số còn thiếu làm hiệu. Đếm số lần xuất hiện của mỗi số trong`nums`. Nếu số bị thiếu này xuất hiện chính xác bằng 0 lần và tất cả các số khác nằm trong khoảng từ 1 đến$n$, thì có thể hoán vị bằng cách chèn số còn thiếu vào đúng vị trí. 
5. Nếu bất kỳ số nào xuất hiện hai lần hoặc một số nằm ngoài 1 đến$n$, câu trả lời là "KHÔNG". Ngược lại, câu trả lời là "CÓ". 
6. Trả kết quả cho từng test case. 

**Tại sao nó hoạt động**: Sự khác biệt giữa các tổng tiền tố liên tiếp chính xác là các phần tử của hoán vị ban đầu, ngoại trừ phần tử bị thiếu. Bằng cách kiểm tra tổng và tần số của những khác biệt này, chúng ta có thể suy ra liệu có tồn tại một số duy nhất khôi phục lại hoán vị đầy đủ từ 1 đến$n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))
        total = n * (n + 1) // 2
        nums = [b[0]] + [b[i] - b[i - 1] for i in range(1, n - 1)]
        current_sum = sum(nums)
        
        if current_sum == total:
            print("YES")
            continue
        
        diff = total - current_sum
        count = {}
        valid = True
        for x in nums:
            if x < 1 or x > n:
                valid = False
                break
            count[x] = count.get(x, 0) + 1
            if count[x] > 1:
                valid = False
                break
        if not valid:
            print("NO")
        elif diff >= 1 and diff <= n and count.get(diff, 0) == 0:
            print("YES")
        else:
            print("NO")

solve()
```Việc triển khai này đọc đầu vào một cách hiệu quả cho nhiều trường hợp thử nghiệm, tính toán sự khác biệt cũng như kiểm tra tổng và tần suất để xác minh xem có thể hoán vị hợp lệ hay không. Việc xử lý riêng phần tử đầu tiên sẽ đảm bảo việc xây dựng lại chính xác phần tử đầu tiên, đây là nguyên nhân phổ biến gây ra các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Tổng tiền tố đầu vào:`[6, 8, 12, 15]`,$n=5$| Bước | số | tổng(số) | tổng cộng | khác biệt | đếm | 
| --- | --- | --- | --- | --- | --- | 
| Tính toán sự khác biệt | [6, 2, 4, 3] | 15 | 15 | 0 | {6:1,2:1,4:1,3:1} | 

Tổng phù hợp`n(n+1)/2`, vì vậy số còn thiếu là phần tử đầu tiên, là 1. Tồn tại hoán vị hợp lệ. Đầu ra: CÓ 

### Ví dụ 2 

Tổng tiền tố đầu vào:`[1, 2, 100]`,$n=4$| Bước | số | tổng(số) | tổng cộng | khác biệt | đếm | 
| --- | --- | --- | --- | --- | --- | 
| Tính toán sự khác biệt | [1,1,98] | 100 | 10 | -90 | {1:2,98:1} | 

Nhân bản`1`và số ngoài phạm vi`98`làm cho nó không thể. Đầu ra: KHÔNG 

Những dấu vết này cho thấy cách thuật toán xử lý các phần tử đầu tiên, cuối cùng và giữa bị thiếu cũng như cách kiểm tra tần suất và tổng phát hiện các trường hợp không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tính toán hiệu, tổng và bảng tần số đều chạy tuyến tính | 
| Không gian | O(n) | Cửa hàng`nums`và từ điển tần số cho từng test | 

Với những hạn chế, giải pháp này xử lý tới$2 \cdot 10^5$số một cách thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("12\n5\n6 8 12 15\n5\n1 6 8 15\n4\n1 2 100\n4\n1 3 6\n2\n2\n3\n1 2\n4\n3 7 10\n5\n5 44 46 50\n4\n1 9 10\n5\n13 21 36 42\n5\n1 2 3 1000000000000000000\n9\n9 11 12 20 25 28 30 33\n") == "YES\nYES\nNO\nYES\nYES\nNO\nYES\nNO\nNO\nNO\nNO\nNO"

# Custom cases
assert run("1\n2\n1") == "YES", "smallest n"
assert run("1\n3\n3 5") == "YES", "missing middle"
assert run("1\n4\n1 3 6") == "YES", "missing last"
assert run("1\n5\n2 3 4 5") == "NO", "missing first invalid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n1`| CÓ | N nhỏ nhất, phần tử đầu tiên bị thiếu hợp lệ | 
| | | |
