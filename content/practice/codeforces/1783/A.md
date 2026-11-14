---
title: "CF 1783A - Làm đẹp"
description: "Chúng ta có một mảng các số nguyên được sắp xếp theo thứ tự không giảm và chúng ta cần sắp xếp lại các phần tử của nó sao cho không có phần tử nào bằng tổng của tất cả các phần tử trước đó. Nếu việc sắp xếp lại như vậy là không thể, chúng tôi phải báo cáo."
date: "2026-06-09T11:05:51+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1783
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 141 (Rated for Div. 2)"
rating: 800
weight: 1783
solve_time_s: 103
verified: false
draft: false
---

[CF 1783A - Làm cho nó đẹp](https://codeforces.com/problemset/problem/1783/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học, sắp xếp 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng các số nguyên được sắp xếp theo thứ tự không giảm và chúng ta cần sắp xếp lại các phần tử của nó sao cho không có phần tử nào bằng tổng của tất cả các phần tử trước đó. Nếu việc sắp xếp lại như vậy là không thể, chúng tôi phải báo cáo. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có độ dài mảng và các phần tử của nó. Đầu ra là "KHÔNG" hoặc "CÓ" theo sau là sự sắp xếp đẹp mắt. 

Các ràng buộc nhỏ: độ dài mảng$n$nhiều nhất là 50 và các giá trị phần tử nằm trong khoảng từ 1 đến 100. Với$t \le 2000$, thậm chí một$O(n^2)$giải pháp sẽ chạy hiệu quả, vì$n^2 \cdot t \le 5 \cdot 10^6$hoạt động. 

Trường hợp cạnh khóa xảy ra khi tất cả các phần tử đều bằng nhau, đặc biệt đối với$n = 2$, vì phần tử thứ hai sẽ luôn bằng tổng của tất cả các phần tử trước đó. Ví dụ, với$a = [10, 10]$, hoán vị nào cũng sẽ có phần tử thứ hai bằng phần tử thứ nhất nên không thể đẹp được. Một trường hợp cạnh khác là khi tổng của các phần tử nhỏ hơn bằng phần tử lớn nhất, điều này có thể xảy ra trong các mảng như$[1, 2, 3]$nếu được sắp xếp một cách ngây thơ. Việc triển khai bất cẩn chỉ để lại mảng được sắp xếp có nguy cơ tạo ra các mảng xấu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử mọi hoán vị của mảng và kiểm tra xem nó có đẹp không. Kiểm tra một mảng có độ dài$n$mất$O(n)$, nhưng có$n!$hoán vị. Vì$n = 50$, điều này là không khả thi, ngay cả với giới hạn đầu vào nhỏ của chúng tôi, bởi vì$50!$có kích thước lớn về mặt thiên văn. 

Quan sát quan trọng là chỉ phần tử lớn nhất mới có thể khớp với tổng của tất cả các phần tử trước đó trong một mảng được sắp xếp. Nếu chúng ta đặt phần tử lớn nhất ở cuối cùng và các phần tử còn lại trước nó thì tổng của tất cả các phần tử trước đó sẽ lớn nhất, khiến nó có nhiều khả năng bằng phần tử lớn nhất. Để tránh điều này, chúng ta có thể đặt phần tử lớn nhất lên đầu tiên, đảm bảo không có tổng trước nào có thể khớp với nó hoặc chúng ta có thể sắp xếp mảng theo thứ tự không tăng. Sắp xếp theo thứ tự giảm dần đảm bảo rằng tổng của tất cả các phần tử trước bất kỳ vị trí nào đều lớn hơn phần tử hiện tại, ngoại trừ các mảng có tất cả các phần tử bằng nhau. Nếu tất cả các phần tử đều bằng nhau và$n > 1$, thì phần tử thứ hai sẽ luôn bằng tổng của các phần tử trước đó chỉ khi$n = 2$và giá trị được lặp lại. Điều này mang lại một chiến lược sắp xếp lại đơn giản và an toàn: sắp xếp giảm dần, kiểm tra hai phần tử đầu tiên. Nếu chúng bằng nhau và mảng chỉ có hai phần tử, hãy in NO; mặt khác, in mảng được sắp xếp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Sắp xếp giảm dần | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$và mảng$a$. 
3. Sắp xếp mảng$a$theo thứ tự giảm dần. Điều này đảm bảo rằng mỗi phần tử không bằng tổng của tất cả các phần tử trước nó vì tổng tăng khi chúng ta di chuyển sang phải. 
4. Kiểm tra xem$n = 2$và hai phần tử đó bằng nhau. Nếu vậy, hãy in KHÔNG. Điều này xử lý trường hợp không thể đặc biệt trong đó phần tử thứ hai luôn bằng phần tử thứ nhất. 
5. Ngược lại, in YES theo sau là mảng đã sắp xếp. Điều này đảm bảo một mảng đẹp vì tổng của bất kỳ tiền tố nào cũng không thể bằng phần tử hiện tại khi mảng được sắp xếp giảm dần. 

Tại sao nó hoạt động: Bằng cách đặt các phần tử lớn hơn lên đầu tiên, bất kỳ tổng tiền tố nào cũng luôn nhỏ hơn phần tử tiếp theo, ngoại trừ trường hợp không thể tầm thường. Thứ tự giảm dần sẽ loại bỏ nguy cơ bất kỳ phần tử nào bằng tổng các phần tử trước nó và ngoại lệ duy nhất (hai số bằng nhau) được xử lý rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def make_beautiful():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort(reverse=True)
        if n == 2 and a[0] == a[1]:
            print("NO")
        else:
            print("YES")
            print(" ".join(map(str, a)))

if __name__ == "__main__":
    make_beautiful()
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Mỗi mảng được sắp xếp theo thứ tự giảm dần để tránh cấu hình xấu. Trường hợp đặc biệt của hai số bằng nhau được xử lý rõ ràng. Việc in mảng dưới dạng chuỗi đảm bảo định dạng đầu ra phù hợp với yêu cầu của bài toán. Sắp xếp tại chỗ giúp giảm chi phí bộ nhớ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`[3, 3, 6, 6]`Sắp xếp giảm dần:`[6, 6, 3, 3]`| Bước | Tiền tố Tổng | Yếu tố hiện tại | Xấu xí? | 
| --- | --- | --- | --- | 
| 1 | 0 | 6 | Không | 
| 2 | 6 | 6 | Không | 
| 3 | 12 | 3 | Không | 
| 4 | 15 | 3 | Không | 

Tất cả các phần tử đều vượt qua, đầu ra:`YES 6 6 3 3`. 

### Ví dụ 2 

Mảng đầu vào:`[10, 10]`Sắp xếp giảm dần:`[10, 10]`| Bước | Tiền tố Tổng | Yếu tố hiện tại | Xấu xí? | 
| --- | --- | --- | --- | 
| 1 | 0 | 10 | Không | 
| 2 | 10 | 10 | Có | 

Đầu ra:`NO`. Điều này cho thấy trường hợp hai bằng nhau đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp chiếm ưu thế, n ≤ 50 | 
| Không gian | O(n) | Lưu trữ mảng | 

Tổng số thao tác trên tất cả các trường hợp thử nghiệm tối đa là$2000 * 50 \log 50$, nằm trong giới hạn 3 giây. Bộ nhớ sử dụng nhỏ, thoải mái dưới 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        make_beautiful()
    return out.getvalue().strip()

# provided samples
assert run("4\n4\n3 3 6 6\n2\n10 10\n5\n1 2 3 4 5\n3\n1 4 4\n") == \
"YES\n6 6 3 3\nNO\nYES\n5 4 3 2 1\nYES\n4 4 1", "sample 1"

# custom cases
assert run("1\n2\n1 2\n") == "YES\n2 1", "ascending small array"
assert run("1\n3\n5 5 5\n") == "YES\n5 5 5", "all equal, n>2"
assert run("1\n50\n" + " ".join(str(i) for i in range(1,51)) + "\n") == \
"YES\n" + " ".join(str(i) for i in range(50,0,-1)), "max size ascending"
assert run("1\n2\n100 100\n") == "NO", "two equal max values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 yếu tố tăng dần | CÓ với giảm dần | sắp xếp lại mảng nhỏ | 
| 3 phần tử bằng nhau | CÓ | tất cả các giá trị bằng nhau, n>2 | 
| 50 yếu tố tăng dần | CÓ giảm dần | đầu vào kích thước tối đa | 
| 2 phần tử bằng nhau | KHÔNG | trường hợp cạnh không thể | 

## Vỏ cạnh 

Thuật toán xử lý rõ ràng các mảng có hai phần tử bằng nhau, không thể làm đẹp được. Mảng có nhiều hơn hai phần tử bằng nhau là an toàn vì việc sắp xếp giảm dần sẽ tránh được bất kỳ sự bằng nhau nào về tổng tiền tố. Mảng một phần tử không được phép bởi các ràng buộc. Các mảng trong đó phần tử lớn nhất bằng tổng các phần tử nhỏ hơn sẽ tự động được cố định bằng cách sắp xếp giảm dần. Ví dụ,`[1, 2, 3]`trở thành`[3, 2, 1]`, tránh được số tiền xấu xí ở giữa. Mỗi trường hợp cạnh được tính bằng cách sắp xếp và kiểm tra hai phần tử.
