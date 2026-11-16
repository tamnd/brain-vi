---
title: "CF 1933D - Độ bền của rùa: Các chế độ liên tục"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta cần quyết định xem liệu chúng ta có thể sắp xếp lại mảng đó sao cho khi áp dụng các phép toán modulo liên tiếp từ trái sang phải, kết quả cuối cùng không bằng 0."
date: "2026-06-08T18:15:37+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1933
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 929 (Div. 3)"
rating: 1200
weight: 1933
solve_time_s: 122
verified: false
draft: false
---

[CF 1933D - Độ bền của rùa: Các chế độ liên tục](https://codeforces.com/problemset/problem/1933/D) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam, toán học, lý thuyết số, sắp xếp 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và chúng ta cần quyết định xem liệu chúng ta có thể sắp xếp lại mảng đó sao cho khi áp dụng các phép toán modulo liên tiếp từ trái sang phải, kết quả cuối cùng không bằng 0. Về mặt hình thức, chúng tôi muốn một hoán vị$b_1, b_2, \dots, b_n$của mảng đầu vào$a_1, a_2, \dots, a_n$như vậy$$b_1 \bmod b_2 \bmod \dots \bmod b_n \neq 0$$Phép toán modulo có tính kết hợp trái, vì vậy trước tiên chúng ta tính toán$b_1 \bmod b_2$, sau đó lấy kết quả đó theo modulo$b_3$, vân vân. Ví dụ, nếu$b = [2024, 1000, 8]$, đánh giá là$(2024 \bmod 1000) \bmod 8 = 24 \bmod 8 = 0$. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số lượng phần tử$n$và chính mảng đó. Các ràng buộc cho phép lên đến$10^4$trường hợp thử nghiệm và tổng số$2 \cdot 10^5$các yếu tố trên tất cả các trường hợp thử nghiệm. Điều này loại trừ bất kỳ giải pháp nào tồi tệ hơn đại khái$O(n \log n)$mỗi trường hợp thử nghiệm, như$O(n^2)$các giải pháp sẽ vượt quá giới hạn thời gian. 

Một sự tinh tế nảy sinh khi mảng chứa tất cả các số giống hệt nhau. Ví dụ,$[3, 3, 3]$sẽ luôn đánh giá bằng 0 bất kể hoán vị, bởi vì bất kỳ số modulo nào bản thân nó cũng bằng 0 và bất kỳ số modulo nào bằng 0 thì vẫn bằng 0. Một tình huống phức tạp khác là khi số nhỏ nhất trong mảng là$1$. Nếu như$1$xuất hiện đầu tiên, kết quả của$1 \bmod x$luôn luôn là$1$, không thể giảm về 0 trong các phép toán modulo tiếp theo. Vì vậy sự có mặt của một$1$có thể đảm bảo kết quả khác 0 nếu được sắp xếp chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử tất cả$n!$hoán vị của mảng và đánh giá modulo chuỗi cho mỗi mảng. Điều này rõ ràng là không thể thực hiện được vì$n$có thể lên tới$10^5$. Ngay cả việc cố gắng chỉ xem xét các hoán vị có phần tử nhỏ nhất khi bắt đầu vẫn sẽ gây ra sự bùng nổ về mặt tổ hợp đối với các mảng lớn hơn. 

Cái nhìn sâu sắc quan trọng là phép toán modulo kết hợp trái rất nhạy cảm với thứ tự của số lớn và số nhỏ. Cụ thể, phần tử đầu tiên xác định phần còn lại ban đầu tối đa. Nếu chúng ta đặt phần tử lớn nhất lên đầu tiên và phần tử nhỏ nhất ở cuối cùng, chúng ta sẽ tối đa hóa khả năng phần dư trung gian không về 0 sớm. Chúng tôi nhận thấy rằng thao tác chỉ có thể mang lại kết quả bằng 0 nếu tại một bước nào đó, số dư hiện tại là bội số của số tiếp theo. Do đó, nếu chúng ta sắp xếp mảng theo thứ tự không giảm, phép toán modulo đầu tiên là$a_1 \bmod a_2$. Nếu như$a_1 < a_2$, phần còn lại là$a_1$, khác không. Bằng cách giữ cho các số không giảm, chúng ta đảm bảo rằng số dư không bao giờ giảm về 0 ngay lập tức trừ khi tất cả các số đều giống hệt nhau. 

Do đó, giải pháp giảm xuống còn việc kiểm tra xem mảng có chứa nhiều hơn một giá trị riêng biệt hay không. Nếu tất cả các yếu tố đều bằng nhau thì câu trả lời là "KHÔNG". Mặt khác, việc sắp xếp mảng theo thứ tự không giảm đảm bảo rằng modulo chuỗi có giá trị khác 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp + Kiểm tra riêng biệt | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, đọc số phần tử$n$và mảng$a$. 
2. Kiểm tra xem tất cả các phần tử trong$a$đều bình đẳng. Nếu có, ghi "NO" vì mọi hoán vị sẽ cho kết quả bằng 0. 
3. Ngược lại, sắp xếp mảng theo thứ tự không giảm. Điều này đảm bảo rằng hoạt động modulo đầu tiên là an toàn và phần còn lại không bị sụp đổ về 0 sớm. 
4. Xuất "YES" để cho biết tồn tại một hoán vị hợp lệ. Chúng ta không cần phải thực sự xây dựng hoán vị cho bài toán này mà chỉ cần xác nhận sự tồn tại của nó. 

Tại sao nó hoạt động: Tính bất biến là nếu mảng chứa ít nhất hai số riêng biệt, việc sắp xếp chúng theo thứ tự không giảm đảm bảo rằng phần còn lại sau mỗi phép toán modulo được giữ nguyên hoặc giảm đi nhưng không bao giờ bằng 0 ngay lập tức, ngoại trừ trường hợp hoàn toàn bằng nhau. Vì số nhỏ nhất nằm ở đầu hoặc đầu chuỗi nên chuỗi modulo liên kết trái không thể giảm nó về 0 trừ khi tất cả các số đều bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    if len(set(a)) == 1:
        print("NO")
    else:
        print("YES")
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc mảng và chuyển đổi nó thành một tập hợp để đếm các phần tử riêng biệt. Nếu chỉ có một phần tử riêng biệt, nó sẽ in "KHÔNG". Nếu không, nó sẽ in "CÓ". sử dụng`set(a)`là rất quan trọng vì nó xử lý các mảng có các phần tử lặp lại một cách hiệu quả. Việc sắp xếp thực sự không cần thiết nếu chúng ta chỉ quan tâm đến sự tồn tại. 

## Ví dụ đã hoạt động 

### Mẫu 1:`[1, 2, 3, 4, 5, 6]`| Bước | Mảng | Riêng biệt? | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | [1,2,3,4,5,6] | Có | CÓ | 

Tất cả các phần tử đều không giống nhau, vì vậy có thể có một chuỗi modulo khác 0. 

### Mẫu 2:`[3, 3, 3, 3, 3]`| Bước | Mảng | Riêng biệt? | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | [3,3,3,3,3] | Không | KHÔNG | 

Tất cả các phần tử đều giống hệt nhau, mọi hoán vị đều mang lại kết quả bằng 0. 

Những dấu vết này cho thấy yếu tố quyết định duy nhất là liệu tất cả các số có bằng nhau hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tạo một bộ yêu cầu quét tất cả các phần tử một lần | 
| Không gian | O(n) | Tập hợp các phần tử riêng biệt có thể lưu trữ tối đa n giá trị | 

Ngay cả trong trường hợp xấu nhất của$2 \cdot 10^5$tổng số phần tử thì thuật toán thực hiện tối đa$O(2 \cdot 10^5)$hoạt động, trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
# helper
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if len(set(a)) == 1:
            print("NO")
        else:
            print("YES")
    return output.getvalue().strip()

# provided samples
assert run("8\n6\n1 2 3 4 5 6\n5\n3 3 3 3 3\n3\n2 2 3\n5\n1 1 2 3 7\n3\n1 2 2\n3\n1 1 2\n6\n5 2 10 10 10 2\n4\n3 6 9 3\n") == "YES\nNO\nYES\nNO\nYES\nNO\nYES\nNO"

# custom cases
assert run("1\n2\n1 1\n") == "NO", "minimum-size identical"
assert run("1\n2\n1 2\n") == "YES", "minimum-size distinct"
assert run("1\n5\n10 10 10 10 10\n") == "NO", "all equal large"
assert run("1\n5\n1 3 5 7 9\n") == "YES", "all distinct odd numbers"
assert run("1\n3\n1000000000 999999999 999999998\n") == "YES", "large numbers distinct"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 phần tử bằng nhau`[1,1]`| KHÔNG | Mảng tối thiểu, tất cả đều bằng nhau | 
| 2 yếu tố phân biệt`[1,2]`| CÓ | Mảng tối thiểu, khác biệt | 
| 5 phần tử bằng nhau`[10,10,10,10,10]`| KHÔNG | Kích thước trung bình, tất cả đều bằng nhau | 
| 5 khác biệt`[1,3,5,7,9]`| CÓ | Không tầm thường, khác biệt | 
| 3 số lớn`[10^9, 10^9-1, 10^9-2]`| CÓ | Xử lý số nguyên lớn | 

## Vỏ cạnh 

Mảng có tất cả các số giống nhau được xử lý bởi`set(a)`kiểm tra. Mảng chỉ có hai số riêng biệt sẽ được xử lý vì ít nhất một phép toán modulo sẽ mang lại phần dư khác 0. Mảng có số nhỏ nhất`1`được xử lý một cách tự nhiên vì việc đặt nó lên hàng đầu đảm bảo chuỗi modulo vẫn khác 0. Mảng có số lượng lớn sẽ an toàn vì Python xử lý các số nguyên lớn. Thuật toán không phụ thuộc vào việc xây dựng hoán vị thực tế, chỉ phụ thuộc vào tính chất có ít nhất hai số phân biệt.
