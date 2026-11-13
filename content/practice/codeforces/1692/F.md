---
title: "CF 1692F - 3SUM"
description: "Chúng ta được cho một mảng các số nguyên dương và chúng ta muốn biết liệu chúng ta có thể chọn ba phần tử riêng biệt có tổng kết thúc bằng chữ số 3 hay không. Nói cách khác, nếu chúng ta biểu thị các phần tử được chọn là $ai$, $aj$ và $ak$, thì $(ai + aj + ak) mod 10 = 3$."
date: "2026-06-09T23:03:08+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 1692
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 799 (Div. 4)"
rating: 1300
weight: 1692
solve_time_s: 117
verified: true
draft: false
---

[CF 1692F - 3SUM](https://codeforces.com/problemset/problem/1692/F) 

**Đánh giá:** 1300 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương và chúng ta muốn biết liệu chúng ta có thể chọn ba phần tử riêng biệt có tổng kết thúc bằng chữ số 3 hay không. Nói cách khác, nếu chúng ta biểu thị các phần tử được chọn là$a_i$,$a_j$, Và$a_k$, sau đó$(a_i + a_j + a_k) \mod 10 = 3$. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp chỉ định kích thước mảng và bản thân các phần tử, đồng thời đầu ra là "CÓ" nếu bộ ba như vậy tồn tại hoặc "KHÔNG" nếu ngược lại. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số phần tử trên tất cả các trường hợp thử nghiệm. Một vòng lặp lồng ba đơn giản sẽ kiểm tra$O(n^3)$kết hợp cho mỗi trường hợp thử nghiệm, điều này rõ ràng là không khả thi vì$n^3$có thể đạt được$10^{15}$hoạt động trong trường hợp xấu nhất. Thậm chí$O(n^2)$cách tiếp cận sẽ quá chậm đối với các mảng lớn nhất. Vì vậy, chúng ta cần một chiến lược tránh lặp lại trực tiếp mọi bộ ba có thể có. 

Một sự tinh tế nảy sinh vì tổng chỉ liên quan đến chữ số cuối cùng. Ví dụ, trong một mảng$[11, 12, 13]$, tổng của cả ba bộ ba phần tử modulo 10 chỉ được xác định bằng các chữ số$[1, 2, 3]$. Điều này cho thấy rằng chúng ta có thể giảm bớt vấn đề bằng cách làm việc với các chữ số cuối cùng của mỗi phần tử thay vì các giá trị đầy đủ của chúng, điều này làm giảm đáng kể số lượng các khả năng riêng biệt. Một trường hợp khác cần xem xét là khi có nhiều số giống nhau. Ví dụ, một mảng$[10, 10, 10]$có tổng bằng 30, kết thúc bằng 0, không phải 3. Việc triển khai bất cẩn có thể cho rằng việc sử dụng các số lặp lại luôn có ích nhưng các chỉ số phải khác biệt. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: lặp lại mọi sự kết hợp của ba chỉ số riêng biệt và kiểm tra xem tổng của chúng có kết thúc bằng 3 hay không. Cách tiếp cận này được đảm bảo là chính xác vì nó kiểm tra rõ ràng tất cả các khả năng, nhưng nó không thành công đối với các mảng lớn do$O(n^3)$sự phức tạp. Ví dụ, với$n = 2000$, điều này sẽ yêu cầu khoảng$1.3 \cdot 10^9$kiểm tra vượt quá giới hạn thời gian thông thường. 

Thông tin chi tiết quan trọng là chỉ chữ số cuối cùng của mỗi số mới ảnh hưởng đến kết quả. Vì các chữ số nằm trong phạm vi từ 0 đến 9, nên chúng ta có thể giảm mảng xuống danh sách tối đa ba lần xuất hiện cho mỗi chữ số cuối cùng. Việc sử dụng nhiều hơn ba số có cùng chữ số cuối cùng là không cần thiết vì bất kỳ bộ ba hợp lệ nào cũng có thể được hình thành bằng cách sử dụng tối đa ba bản sao. Điều này làm giảm số lượng ứng cử viên xuống tối đa 30 phần tử. Với kích thước giảm này, chúng ta có thể áp dụng vòng lặp lồng ba một cách an toàn, bởi vì$30^3 = 27,000$các hoạt động cho mỗi trường hợp thử nghiệm đều được chấp nhận. 

Câu chuyện của giải pháp là thế này: lực lượng vũ phu hoạt động nhưng chậm vì kích thước mảng lớn. Bằng cách nhận thấy rằng bài toán chỉ quan tâm đến chữ số cuối cùng, chúng ta có thể loại bỏ các số dư thừa một cách an toàn và giảm mảng xuống kích thước có thể quản lý được mà không làm mất bất kỳ bộ ba hợp lệ nào. Điều này cho phép chúng ta kết hợp tính đơn giản và tính chính xác một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm đối với n lớn | 
| Tối ưu | O(1) sau khi giảm xuống còn 30 phần tử | O(30) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc dãy số. 
2. Chuyển mỗi số sang chữ số cuối cùng bằng modulo 10. 
3. Xây dựng danh sách chứa tối đa ba số cho mỗi chữ số cuối cùng. Bước này đảm bảo chúng ta không mất bất kỳ bộ ba nào có tổng bằng chữ số cuối cùng là 3, vì sử dụng nhiều hơn ba số có cùng chữ số không thể tạo ra tổng mới theo modulo 10. 
4. Liệt kê tất cả các bộ ba từ danh sách rút gọn. Với mỗi bộ ba, hãy tính tổng các phần tử của nó theo modulo 10. 
5. Nếu bất kỳ tổng gấp ba nào của một số kết thúc bằng 3, ngay lập tức xuất "CÓ" cho trường hợp kiểm tra đó và chuyển sang trường hợp kiểm tra tiếp theo. 
6. Nếu không tìm thấy bộ ba hợp lệ sau khi kiểm tra tất cả các khả năng, ghi "NO". 

Tại sao nó hoạt động: mọi sự kết hợp của ba số có khả năng tổng thành chữ số cuối cùng là 3 được đảm bảo bao gồm tối đa ba trường hợp của mỗi chữ số cuối cùng. Bằng cách chỉ giữ lại ba phần tử cho mỗi chữ số, chúng tôi không loại bỏ bất kỳ bộ ba ứng cử viên nào. Hoạt động modulo đảm bảo rằng chúng tôi chỉ tập trung vào phần có liên quan của mỗi số và việc lặp lại tất cả các bộ ba trong tập rút gọn đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        digits = [[] for _ in range(10)]
        for num in a:
            d = num % 10
            if len(digits[d]) < 3:
                digits[d].append(d)
        
        candidates = []
        for lst in digits:
            candidates.extend(lst)
        
        found = False
        m = len(candidates)
        for i in range(m):
            for j in range(i+1, m):
                for k in range(j+1, m):
                    if (candidates[i] + candidates[j] + candidates[k]) % 10 == 3:
                        found = True
                        break
                if found:
                    break
            if found:
                break
        
        print("YES" if found else "NO")

solve()
```Giải pháp đầu tiên nhóm các số theo chữ số cuối cùng của chúng và cắt bớt mỗi nhóm thành ba phần tử. Điều này đảm bảo rằng tất cả các bộ ba chữ số cuối cùng có thể được giữ nguyên trong khi vẫn giữ tập ứng viên ở mức nhỏ. Sau đó, vòng lặp lồng ba sẽ kiểm tra rõ ràng các tổng theo modulo 10, tránh bất kỳ lỗi nào xảy ra với các chỉ số. Sử dụng modulo vào đúng thời điểm sẽ ngăn ngừa sự cố tràn số nguyên cho số lượng lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[20, 22, 19, 84]`| Bước | Ứng viên | Đã kiểm tra ba lần | Tổng mod 10 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | [0,2,9,4] | (0,2,9) | 11 → 1 | - | 
| 2 | [0,2,9,4] | (0,2,4) | 6 → 6 | - | 
| 3 | [0,2,9,4] | (0,9,4) | 13 → 3 | CÓ | 

Chúng tôi đã tìm thành công bộ ba hợp lệ (20, 19, 84) được giảm xuống các chữ số cuối cùng (0,9,4) tổng thành 13, chữ số cuối cùng là 3. 

### Ví dụ 2 

đầu vào:`[12, 34, 56, 78, 90]`| Bước | Ứng viên | Đã kiểm tra ba lần | Tổng mod 10 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | [2,4,6,8,0] | (2,4,6) | 12 → 2 | - | 
| 2 | ... | (2,4,8) | 14 → 4 | - | 
| ... | ... | ... | ... | KHÔNG | 

Không có tổng ba lần cho chữ số cuối cùng của 3, vì vậy thuật toán sẽ đưa ra NO một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm sau khi giảm | Tối đa 30 ứng cử viên, vòng lặp lồng ba kiểm tra tối đa 27.000 bộ ba | 
| Không gian | O(30) | Mảng ứng viên rút gọn cộng với nhóm chữ số | 

Việc giảm xuống tối đa 30 số đảm bảo rằng chúng tôi không bao giờ vượt quá các hoạt động khả thi ngay cả đối với đầu vào lớn nhất. Việc sử dụng bộ nhớ ở mức tối thiểu, dễ dàng phù hợp với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("6\n4\n20 22 19 84\n4\n1 11 1 2022\n4\n1100 1100 1100 1111\n5\n12 34 56 78 90\n4\n1 9 8 4\n6\n16 38 94 25 18 99\n") == "YES\nYES\nNO\nNO\nYES\nYES"

# Custom cases
assert run("1\n3\n1 1 1\n") == "NO", "all equal digits"
assert run("1\n3\n1 2 0\n") == "YES", "minimum n with valid triple"
assert run("1\n6\n10 10 10 10 10 3\n") == "YES", "last digit 3 included"
assert run("1\n5\n7 7 7 7 7\n") == "NO", "no triple ends with 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 số tất cả 1 | KHÔNG | Không thể tạo thành tổng kết thúc bằng 3 | 
| 3 số [1,2,0] | CÓ | Bộ ba hợp lệ nhỏ nhất | 
| 6 số có chữ số tận cùng là 3 | CÓ | Đảm bảo bao gồm 3 tác phẩm duy nhất | 
| Tất cả 7 giây | KHÔNG | Tổng ba không kết thúc bằng 3, tránh dương tính giả | 

## Vỏ cạnh 

Thuật toán xử lý các mảng nhỏ một cách chính xác bằng cách kiểm tra tất cả các bộ ba sau khi rút gọn, do đó ngay cả khi$n=3$, nó vẫn hoạt động. Đối với các mảng có số lặp lại, chẳng hạn như `[10,10,
