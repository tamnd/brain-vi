---
title: "CF 106508J - Chuỗi GCD và LCM"
description: "Chúng ta được cho một dãy số nguyên và chúng ta được yêu cầu xem xét tất cả các dãy con khác trống của nó. Với mỗi dãy con, chúng ta tính hai giá trị: ước chung lớn nhất của tất cả các phần tử trong đó và bội số chung nhỏ nhất của tất cả các phần tử trong đó."
date: "2026-06-18T19:12:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106508
codeforces_index: "J"
codeforces_contest_name: "2026 SCUT Programming Contest\uff082026 \u534e\u5357\u7406\u5de5\u5927\u5b66\u7a0b\u5e8f\u8bbe\u8ba1\u6821\u8d5b\uff09"
rating: 0
weight: 106508
solve_time_s: 45
verified: true
draft: false
---

[CF 106508J - Chuỗi con GCD và LCM](https://codeforces.com/problemset/problem/106508/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và chúng ta được yêu cầu xem xét tất cả các dãy con khác trống của nó. Với mỗi dãy con, chúng ta tính hai giá trị: ước chung lớn nhất của tất cả các phần tử trong đó và bội số chung nhỏ nhất của tất cả các phần tử trong đó. Nhiệm vụ là đếm xem có bao nhiêu dãy con có tính chất là hai giá trị này bằng nhau. 

Một cách hữu ích để giải thích điều kiện này là suy nghĩ xem ý nghĩa của việc một tập hợp số có gcd và lcm giống hệt nhau. gcd của một tập hợp luôn nhỏ hơn hoặc bằng mọi phần tử trong tập hợp đó, trong khi lcm luôn lớn hơn hoặc bằng mọi phần tử. Để cả hai trùng khớp, mọi phần tử phải có cùng giá trị, vì nếu không thì lcm sẽ vượt quá phần tử nhỏ nhất hoặc gcd sẽ giảm xuống dưới phần tử lớn nhất. Quan sát này biến vấn đề từ một vấn đề có vẻ nặng nề về mặt lý thuyết số thành một vấn đề đếm cấu trúc trên các giá trị bằng nhau. 

Đầu vào chỉ đơn giản là một mảng số nguyên và đầu ra là số dãy con hợp lệ trong điều kiện này. 

Từ góc độ ràng buộc, hàm ý chính là liệu chúng ta có đủ khả năng chi trả cho bất kỳ thứ gì lặp lại trên tất cả các chuỗi con hay không. Một chuỗi có kích thước lên tới khoảng 10^5 ngay lập tức loại trừ mọi hoạt động khám phá theo cấp số nhân. Ngay cả việc đếm bậc hai theo cặp cũng trở nên quá chậm trong trường hợp xấu nhất nếu lặp lại qua nhiều trường hợp thử nghiệm. Điều này thúc đẩy chúng tôi hướng tới một giải pháp nén mảng thành số lượng tần số và hoạt động theo thời gian tuyến tính hoặc gần tuyến tính. 

Một trường hợp thất bại tinh tế xuất hiện khi cố gắng suy luận cục bộ về gcd hoặc lcm thay vì toàn cục. Ví dụ: trong một mảng như`[2, 3, 4]`, một nỗ lực ngây thơ có thể nghĩ rằng một số chuỗi con hỗn hợp có thể hoạt động nếu gcd và lcm “tình cờ gặp nhau”, nhưng việc kiểm tra sẽ tiết lộ: 

tiếp theo`[2, 4]`có gcd 2 và lcm 4, không bằng nhau và`[2, 3, 4]`có gcd 1 và lcm 12. Chỉ các chuỗi phần tử đơn mới hoạt động. Điều này củng cố rằng bất kỳ sự kết hợp nào của các giá trị riêng biệt sẽ phá vỡ điều kiện đẳng thức ngay lập tức. 

Một trường hợp cạnh khác là khi tất cả các phần tử giống hệt nhau, ví dụ`[5, 5, 5]`. Mọi chuỗi con không trống đều hoạt động, vì vậy câu trả lời phải là$2^3 - 1 = 7$. Bất kỳ giải pháp nào quên đếm bội số sẽ trả về sai 1. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất dễ mô tả. Chúng tôi lặp lại mọi dãy con không trống, tính gcd và lcm của nó bằng cách gấp các phần tử của nó và kiểm tra xem chúng có khớp hay không. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Vấn đề là số lượng các chuỗi con: một mảng có kích thước$n$có$2^n - 1$các dãy tiếp theo, và thậm chí đối với$n = 40$điều này đã trở nên không khả thi, trong khi$n = 10^5$làm cho nó hoàn toàn không thể. 

Cái nhìn sâu sắc về cấu trúc quan trọng là sự bằng nhau của gcd và lcm buộc tất cả các phần tử trong dãy con phải giống hệt nhau. Khi điều này được nhận ra, vấn đề sẽ dừng lại tùy thuộc vào tính toán gcd hoặc lcm và giảm xuống việc đếm xem có bao nhiêu cách chúng ta có thể chọn một tập hợp con không trống từ mỗi nhóm có giá trị bằng nhau. Nếu một giá trị xuất hiện$f$lần, bất kỳ tập hợp con nào của những lần xuất hiện đó đều tạo thành một chuỗi con hợp lệ, đưa ra$2^f - 1$sự lựa chọn. 

Chúng ta giải quyết vấn đề từ việc liệt kê các dãy con đến việc đếm tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Đếm tần số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và đếm số lần mỗi số riêng biệt xuất hiện. Bước này nén chuỗi thành các lớp tương đương có giá trị giống hệt nhau, là các ứng cử viên duy nhất có thể hình thành các chuỗi con hợp lệ. 
2. Với mỗi giá trị riêng biệt có tần số$f$, hãy tính xem có thể tạo được bao nhiêu dãy con không trống chỉ bằng giá trị đó. Đây là$2^f - 1$, vì mỗi lần xuất hiện có thể được bao gồm hoặc loại trừ, ngoại trừ lựa chọn trống. 
3. Tính tổng những đóng góp này trên tất cả các giá trị riêng biệt. 
4. Trả về số tiền cuối cùng làm đáp án. 

Lý do đằng sau việc tính tổng độc lập theo giá trị là bất kỳ dãy con nào chứa hai giá trị khác nhau sẽ ngay lập tức vi phạm điều kiện, do đó các dãy con hợp lệ sẽ được phân chia rõ ràng theo giá trị. 

### Tại sao nó hoạt động 

Một dãy con thỏa mãn gcd bằng lcm chỉ khi tất cả các phần tử của nó giống hệt nhau. Nếu hai yếu tố khác nhau, hãy nói$a < b$, thì lcm ít nhất là$b$trong khi gcd nhiều nhất là$a$, vì vậy sự bình đẳng là không thể trừ khi$a = b$. Điều này buộc mọi dãy con hợp lệ phải được chứa hoàn toàn trong một lớp giá trị duy nhất. Thuật toán liệt kê chính xác các lớp này và đếm tất cả các tập con không trống bên trong mỗi lớp, đảm bảo tính đầy đủ và không tính quá mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

def solve():
    n = int(input().strip())
    arr = list(map(int, input().split()))
    
    freq = Counter(arr)
    
    ans = 0
    for v, f in freq.items():
        ans += (1 << f) - 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh lý do trực tiếp. Bảng tần số nén mảng và dịch chuyển bit`(1 << f)`tính toán hiệu quả$2^f$. Trừ một sẽ loại bỏ dãy con trống. Không cần tính toán gcd hoặc lcm vì đối số cấu trúc loại bỏ hoàn toàn các chuỗi con có giá trị hỗn hợp. 

Một chi tiết triển khai tinh tế là việc sử dụng dịch chuyển bit thay vì`pow(2, f)`, giúp tránh được chi phí về dấu phẩy động và là tiêu chuẩn trong lập trình cạnh tranh để tính lũy thừa số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 2 2 3
```Chúng tôi tính toán tần số: 1 xuất hiện một lần, 2 xuất hiện ba lần, 3 xuất hiện một lần. 

| Giá trị | Tần số | Sự đóng góp$2^f - 1$| 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | 7 | 
| 3 | 1 | 1 | 

Tổng số câu trả lời là 9. 

Dấu vết này cho thấy rằng chỉ các chuỗi con được hình thành trong các giá trị giống hệt nhau mới đóng góp và các giá trị trộn không bao giờ xuất hiện trong quá trình tính toán. 

### Ví dụ 2 

đầu vào:```
4
7 7 7 7
```| Giá trị | Tần số | Đóng góp | 
| --- | --- | --- | 
| 7 | 4 | 15 | 

Mọi tập hợp con không trống đều hợp lệ vì tất cả các phần tử đều giống hệt nhau, xác nhận rằng công thức sẽ giảm chính xác xuống việc đếm tất cả các tập hợp con trừ đi phần tử trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt đếm tần số và một lượt đếm các giá trị riêng biệt | 
| Không gian | O(n) | Lưu trữ bản đồ tần số | 

Giải pháp này chia tỷ lệ tuyến tính với kích thước đầu vào, đủ cho các ràng buộc thông thường lên tới 10^5 trở lên, luôn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    from collections import Counter

    n = int(sys.stdin.readline().strip())
    arr = list(map(int, sys.stdin.readline().split()))
    freq = Counter(arr)

    ans = 0
    for v, f in freq.items():
        ans += (1 << f) - 1
    return str(ans)

# provided sample-like checks
assert run("5\n1 2 2 2 3\n") == "9"

# all equal
assert run("4\n7 7 7 7\n") == str((1 << 4) - 1)

# all distinct
assert run("3\n1 2 3\n") == "3"

# single element
assert run("1\n42\n") == "1"

# two groups
assert run("6\n5 5 1 1 1 9\n") == str((1<<2)-1 + (1<<3)-1 + (1<<1)-1)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều khác biệt | n | chỉ những người độc thân mới đóng góp | 
| tất cả đều bình đẳng | 2^n - 1 | vụ nổ tập hợp con đầy đủ | 
| tần số hỗn hợp | tổng mỗi nhóm | sự độc lập của các giá trị | 

## Vỏ cạnh 

Khi tất cả các phần tử đều giống hệt nhau, chẳng hạn như`[10, 10, 10]`, thuật toán tính tất cả$2^3 - 1$các chuỗi tiếp theo. Bản đồ tần số tạo ra một mục nhập duy nhất có tần số 3 và tính toán tổng hợp chính xác tất cả các tập hợp con không trống. 

Khi tất cả các phần tử đều khác biệt, chẳng hạn như`[1, 2, 3, 4]`, mỗi tần số là 1, do đó mỗi tần số đóng góp$2^1 - 1 = 1$. Kết quả là 4, khớp với thực tế là chỉ các chuỗi con có một phần tử là hợp lệ. 

Khi có một giá trị vượt trội và nhiều giá trị hiếm, chẳng hạn như`[5, 5, 5, 1, 2]`, thuật toán phân tách rõ ràng các đóng góp theo giá trị, đảm bảo không có chuỗi con hỗn hợp không hợp lệ nào được xem xét.
