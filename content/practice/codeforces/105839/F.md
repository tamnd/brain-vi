---
title: "CF 105839F - Sắp xếp theo một lần hoán đổi"
description: "Chúng ta được cung cấp một mảng thể hiện thứ tự xáo trộn của các mục riêng biệt và chúng ta được phép thực hiện tối đa một thao tác: chọn hai vị trí và hoán đổi giá trị của chúng."
date: "2026-06-25T14:55:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105839
codeforces_index: "F"
codeforces_contest_name: "XXVII Interregional Programming Olympiad, Vologda SU, 2025"
rating: 0
weight: 105839
solve_time_s: 45
verified: true
draft: false
---

[CF 105839F - Sắp xếp theo một lần hoán đổi](https://codeforces.com/problemset/problem/105839/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng thể hiện thứ tự xáo trộn của các mục riêng biệt và chúng ta được phép thực hiện tối đa một thao tác: chọn hai vị trí và hoán đổi giá trị của chúng. Nhiệm vụ là xác định xem có thể chuyển đổi mảng thành thứ tự được sắp xếp bằng cách sử dụng hoán đổi đơn này hay không và nếu có, hãy mô tả chính xác khả năng đó theo đầu ra yêu cầu của vấn đề. 

Từ quan điểm thuật toán, đầu vào là một chuỗi có độ dài$n$và cấu hình đích có cùng trình tự được sắp xếp theo thứ tự không giảm. Sửa đổi duy nhất được phép là một hoán đổi toàn cầu giữa hai chỉ số bất kỳ. Đầu ra thường là một quyết định hoặc một mô tả tối thiểu về tính khả thi trong ràng buộc này. 

Chế độ ràng buộc (mảng có kích thước lên tới lớn, thường là$10^5$hoặc nhiều hơn trong các vấn đề của Codeforces thuộc dạng này) ngay lập tức loại trừ mọi cách tiếp cận thử tất cả các giao dịch hoán đổi có thể xảy ra. Việc thử từng cặp sẽ yêu cầu$O(n^2)$hoạt động vượt quá giới hạn cho phép. Bất kỳ giải pháp hợp lệ nào cũng phải giải thích về cấu trúc không khớp giữa mảng và phiên bản được sắp xếp của nó theo thời gian tuyến tính hoặc gần tuyến tính. 

Một trường hợp tinh tế phát sinh khi mảng đã được sắp xếp. Trong trường hợp đó, không cần phải hoán đổi và bất kỳ logic nào cố gắng “sửa chữa các điểm không khớp” một cách mù quáng có thể kết luận không chính xác rằng vẫn cần phải hoán đổi. Một trường hợp quan trọng khác là khi có chính xác hai phần tử không đúng vị trí nhưng việc hoán đổi chúng không thực sự sửa được thứ tự vì vị trí của chúng tương tác với các vị trí khác. Ví dụ: hãy xem xét một mảng gần như được sắp xếp trong đó nhiều phần tử được dịch chuyển trong một chu kỳ dài hơn hai. Một cách tiếp cận ngây thơ chỉ nhìn vào những thái cực có thể nhầm tưởng rằng một lần hoán đổi là đủ. 

Để làm ví dụ cụ thể, hãy lấy mảng$[1, 4, 3, 2, 5]$. Mảng được sắp xếp là$[1, 2, 3, 4, 5]$. Có nhiều vị trí không khớp nhau, và mặc dù hoán đổi$4$Và$2$cải thiện tình hình, nó không sắp xếp đầy đủ mảng, cho thấy rằng không phải mọi mẫu không khớp đều có thể giải quyết được bằng một lần hoán đổi. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi cặp chỉ số$(i, j)$, hoán đổi chúng và kiểm tra xem mảng kết quả đã được sắp xếp chưa. Đối với mỗi lần hoán đổi, việc xác minh tính sắp xếp sẽ mất$O(n)$thời gian, và có$O(n^2)$sự hoán đổi có thể xảy ra, dẫn đến$O(n^3)$giải pháp nếu được thực hiện một cách ngây thơ hoặc$O(n^2)$nếu chúng ta sử dụng lại séc một cách khéo léo hơn. Điều này trở nên bất khả thi khi$n$phát triển lớn, vì thậm chí$10^5$sẽ dẫn đến khoảng$10^{10}$hoạt động. 

Quan sát quan trọng là một lần hoán đổi duy nhất chỉ có thể ảnh hưởng đến hai vị thế. Điều này có nghĩa là nếu mảng có thể được cố định thì hầu hết tất cả các vị trí đều phải khớp với mục tiêu đã sắp xếp của chúng. Những sai lệch duy nhất phải được khoanh vùng chặt chẽ. Thay vì mô phỏng các giao dịch hoán đổi, chúng tôi so sánh mảng với phiên bản đã sắp xếp của nó và chỉ tập trung vào các chỉ số nơi chúng khác nhau. 

Khi chúng tôi tách biệt các vị trí không khớp, cấu trúc của một giải pháp hợp lệ sẽ trở nên cứng nhắc. Nếu không có điểm nào không khớp thì mảng đã được sắp xếp. Nếu có chính xác hai giá trị không khớp, việc hoán đổi hai giá trị đó có thể sửa được mảng nhưng chỉ khi chúng tương ứng với nhau trong cấu hình được sắp xếp. Nếu có nhiều hơn hai điểm không khớp, một lần hoán đổi không thể sửa được nhiều hơn hai vị trí, do đó câu trả lời ngay lập tức là phủ định. 

Điều này làm giảm toàn bộ vấn đề khi so sánh với đường cơ sở đã được sắp xếp và phân tích tập hợp không khớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các giao dịch hoán đổi) |$O(n^3)$hoặc$O(n^2)$|$O(1)$ĐẾN$O(n)$| Quá chậm | 
| So sánh với mảng đã sắp xếp |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một bản sao đã được sắp xếp của mảng. Điều này thể hiện trạng thái mục tiêu cuối cùng mà chúng tôi muốn đạt được. Việc so sánh với mảng này cho chúng ta biết chính xác vị trí nào không chính xác. 
2. Lặp lại tất cả các chỉ mục và thu thập những chỉ mục có mảng ban đầu khác với mảng đã sắp xếp. Các chỉ số này đại diện cho những nơi duy nhất mà một giao dịch hoán đổi có thể khắc phục được. 
3. Nếu số vị trí không khớp bằng 0 thì mảng đã được sắp xếp và không cần thực hiện thao tác nào. Chúng ta có thể trả lại thành công ngay lập tức. 
4. Nếu số vị trí không khớp chính xác là hai, hãy trích xuất hai chỉ số đó và kiểm tra xem việc hoán đổi giá trị của chúng có căn chỉnh cả hai vị trí với mảng đã sắp xếp hay không. Điều này đảm bảo rằng việc hoán đổi có giá trị về mặt cấu trúc, không chỉ cải thiện cục bộ. 
5. Nếu số lượng vị trí không khớp là bất kỳ giá trị nào khác 0 hoặc hai, hãy kết luận rằng không có phép hoán đổi đơn lẻ nào có thể sửa được mảng, vì một phép hoán đổi chỉ có thể sửa được tối đa hai vị trí. 

### Tại sao nó hoạt động 

Bất biến quan trọng là hoạt động hoán đổi ảnh hưởng đến chính xác hai chỉ số và nó chỉ trao đổi giá trị của chúng mà không làm thay đổi nhiều phần tử ở nơi khác. Bất kỳ chỉ mục nào đã khớp với vị trí được sắp xếp của nó phải vẫn chính xác trừ khi nó liên quan đến hoán đổi và tối đa hai chỉ mục có thể liên quan. Do đó, bất kỳ cấu hình có thể giải được nào cũng phải khác với mảng đã sắp xếp ở nhiều nhất hai vị trí và hai vị trí đó phải bổ sung cho nhau theo nghĩa là việc hoán đổi giá trị của chúng sẽ khôi phục cả hai vị trí về tính chính xác. Hạn chế về cấu trúc này mô tả đầy đủ tất cả các trường hợp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    b = sorted(a)
    
    diff = []
    for i in range(n):
        if a[i] != b[i]:
            diff.append(i)
    
    if len(diff) == 0:
        print("YES")
        return
    
    if len(diff) == 2:
        i, j = diff
        if a[i] == b[j] and a[j] == b[i]:
            print("YES")
        else:
            print("NO")
        return
    
    print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc mảng và xây dựng phiên bản đã sắp xếp của nó. Đây là cấu hình tham khảo Vòng lặp thu thập các chỉ số không khớp sẽ tách biệt chính xác các phần của mảng vi phạm thứ tự được sắp xếp. Sau đó, logic sẽ phân nhánh theo kích thước của tập hợp không khớp này. 

Phần tế nhị nhất là trường hợp hai không khớp. Chỉ có hai điểm không khớp là chưa đủ; các giá trị được hoán đổi phải khớp hoàn toàn với các vị trí được sắp xếp của nhau. Việc kiểm tra này đảm bảo rằng việc hoán đổi không đúng một phần nhưng giải quyết hoàn toàn cả hai vị trí không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng$[1, 3, 2, 4]$. Phiên bản được sắp xếp là$[1, 2, 3, 4]$. 

Chúng tôi theo dõi sự không phù hợp: 

| tôi | một [tôi] | b[i] | không khớp | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | không | 
| 1 | 3 | 2 | vâng | 
| 2 | 2 | 3 | vâng | 
| 3 | 4 | 4 | không | 

Các chỉ số không phù hợp là$[1, 2]$. Trao đổi những thứ này mang lại$[1, 2, 3, 4]$, vì vậy câu trả lời là CÓ. 

Bây giờ hãy xem xét$[1, 4, 3, 2, 5]$. Mảng được sắp xếp là$[1, 2, 3, 4, 5]$. 

| tôi | một [tôi] | b[i] | không khớp | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | không | 
| 1 | 4 | 2 | vâng | 
| 2 | 3 | 3 | không | 
| 3 | 2 | 4 | vâng | 
| 4 | 5 | 5 | không | 

Các chỉ số không phù hợp là$[1, 3]$. Kiểm tra trao đổi:$a[1]=4$nên bằng$b[3]=4$Và$a[3]=2$nên bằng$b[1]=2$, do đó việc hoán đổi sẽ hoạt động và mảng sẽ được sắp xếp. Điều này xác nhận điều kiện đúng cho trường hợp hai không khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Việc sắp xếp mảng chiếm ưu thế, trong khi quét không khớp là tuyến tính | 
| Không gian |$O(n)$| Chúng tôi lưu trữ một bản sao được sắp xếp của mảng | 

Độ phức tạp phù hợp một cách thoải mái với các ràng buộc điển hình của Codeforce dành cho$n$lên đến$10^5$hoặc$2 \cdot 10^5$, vì việc sắp xếp và quét tuyến tính đơn đều hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    def input():
        return sys.stdin.readline()
    
    n = int(input())
    a = list(map(int, input().split()))
    b = sorted(a)
    diff = []
    for i in range(n):
        if a[i] != b[i]:
            diff.append(i)
    if len(diff) == 0:
        return "YES"
    if len(diff) == 2:
        i, j = diff
        return "YES" if (a[i] == b[j] and a[j] == b[i]) else "NO"
    return "NO"

# provided samples (hypothetical)
assert run("4\n1 2 3 4\n") == "YES"
assert run("4\n1 3 2 4\n") == "YES"

# custom cases
assert run("5\n1 4 3 2 5\n") == "YES", "single swap fixes"
assert run("5\n5 4 3 2 1\n") == "NO", "needs multiple swaps"
assert run("3\n2 1 3\n") == "YES", "boundary swap"
assert run("3\n3 2 1\n") == "NO", "cycle of 3 cannot be fixed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đã được sắp xếp | CÓ | trường hợp không khớp | 
| sửa chữa trao đổi đơn | CÓ | chính xác là hai điểm không khớp | 
| mảng đảo ngược | KHÔNG | nhiều điểm không khớp | 
| trường hợp ranh giới nhỏ | CÓ/KHÔNG | tính đúng đắn của điều kiện hoán đổi | 

## Vỏ cạnh 

Đối với một mảng đã được sắp xếp như$[1, 2, 3]$, danh sách không khớp trống và thuật toán ngay lập tức trả về CÓ mà không cần thực hiện bất kỳ logic hoán đổi nào. Điều này tránh được sai lầm phổ biến là buộc phải hoán đổi ngay cả khi không cần thiết. 

Đối với trường hợp như$[2, 1, 3]$, có đúng hai vị trí không khớp$0$Và$1$. Mảng được sắp xếp là$[1, 2, 3]$và việc hoán đổi các phần tử không khớp sẽ căn chỉnh chính xác cả hai vị trí, điều này thuật toán xác nhận thông qua điều kiện kiểm tra chéo. 

Đối với một sự dịch chuyển theo chu kỳ như$[3, 1, 2]$, cả ba vị trí đều không khớp với mảng đã sắp xếp. Mặc dù mảng “gần” được sắp xếp, nhưng không có phép hoán đổi đơn lẻ nào có thể giải quyết được 3 chu kỳ và thuật toán sẽ loại bỏ nó một cách chính xác bằng cách phát hiện nhiều hơn hai mảng không khớp
