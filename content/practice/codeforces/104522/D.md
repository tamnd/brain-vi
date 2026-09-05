---
title: "CF 104522D - Vật liệu không khớp"
description: "Chúng ta được cho một mảng a có độ dài n. Chúng tôi được phép thay đổi một số yếu tố của nó. Sau những thay đổi này, chúng tôi muốn mảng tương thích với sự tồn tại của một mảng b khác có độ dài n+1, trong đó mọi giá trị trong a được xác định là giá trị lớn nhất của hai giá trị liền kề trong b."
date: "2026-06-30T10:12:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104522
codeforces_index: "D"
codeforces_contest_name: "CerealCodes II Intermediate"
rating: 0
weight: 104522
solve_time_s: 114
verified: false
draft: false
---

[CF 104522D - Tài liệu không khớp](https://codeforces.com/problemset/problem/104522/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`a`chiều dài`n`. Chúng tôi được phép thay đổi một số yếu tố của nó. Sau những thay đổi này, chúng tôi muốn mảng tương thích với sự tồn tại của mảng khác`b`chiều dài`n+1`, trong đó mọi giá trị trong`a`được định nghĩa là giá trị lớn nhất của hai giá trị liền kề trong`b`. 

Nói cách khác, mỗi`a_i`đại diện cho đỉnh của đoạn có độ dài-2 trong`b`: nó phải bằng`max(b_i, b_{i+1})`. Vì vậy mỗi`a_i`thực thi một ràng buộc cục bộ trên hai vị trí lân cận trong`b`, đồng thời yêu cầu ít nhất một trong hai vị trí đó trong`b`thực sự đạt được mức tối đa này. 

Nhiệm vụ là sửa đổi tối thiểu`a`sao cho tồn tại ít nhất một giá trị`b`satisfying all these constraints.

 Điểm quan trọng là chúng tôi không xây dựng`b`một cách rõ ràng. Chúng tôi chỉ quan tâm liệu một điều như vậy có`b`tồn tại sau khi điều chỉnh một số phần tử của`a`càng tốt. 

Các ràng buộc lớn, với tổng chiều dài của các trường hợp thử nghiệm lên tới`2⋅10^5`. Điều này loại trừ bất kỳ giải pháp nào cố gắng mô phỏng hoặc tìm kiếm các cấu trúc có thể có của`b`một cách rõ ràng, vì ngay cả suy luận bậc hai cho mỗi bài kiểm tra cũng sẽ quá chậm. Giải pháp phải tuyến tính cho mỗi trường hợp thử nghiệm. 

Chế độ lỗi ngây thơ xuất hiện khi một phần tử của`a`là “quá lớn” so với các nước láng giềng. Ví dụ: nếu một phần tử hoàn toàn lớn hơn cả hai phần tử lân cận của nó, thì nó có xu hướng buộc một yêu cầu không thể thực hiện được đối với cấu trúc ở giữa của`b`, bởi vì nó phải thống trị đồng thời cả hai ràng buộc liền kề trong khi vẫn có thể thực hiện được ở mức tối đa của một cạnh chung. Những mâu thuẫn cục bộ này chính xác là những gì chúng ta cần phát hiện và khắc phục. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ là cố gắng xây dựng`b`cho một cố định`a`. Chúng tôi sẽ xử lý từng vị trí`i`như thực thi một hạn chế trên`(b_i, b_{i+1})`, sau đó cố gắng gán giá trị cho`b`thoả mãn đồng thời mọi ràng buộc. Điều này trở thành một vấn đề thỏa mãn ràng buộc toàn cầu trên`n+1`biến với`n`hạn chế chồng chéo. 

Điều này có thể được giải quyết bằng cách quay lui hoặc truyền bá trạng thái, nhưng sự tương tác giữa các ràng buộc sẽ lan truyền dọc theo toàn bộ mảng. Trong trường hợp xấu nhất, mọi quyết định đối với một cặp đều ảnh hưởng đến tất cả các cặp trong tương lai, dẫn đến sự phân nhánh theo cấp số nhân hoặc ít nhất là lan truyền bậc hai cho mỗi trường hợp thử nghiệm. 

Quan sát quan trọng là cấu trúc này hoàn toàn mang tính cục bộ: mỗi`a_i`chỉ kết nối hai vị trí liền kề trong`b`. Vì vậy, thay vì suy nghĩ toàn cầu về`b`, chúng ta có thể suy luận cục bộ về việc liệu mỗi`a_i`có thể được làm cho tương thích với các nước láng giềng của nó. 

Sự đơn giản hóa quan trọng là loại bỏ`b`hoàn toàn và diễn giải lại tính khả thi về tính nhất quán cục bộ giữa các yếu tố lân cận của`a`. Một khi được viết lại theo cách này, vấn đề sẽ giảm xuống còn việc phát hiện và khắc phục các vi phạm cục bộ nhằm ngăn cản bất kỳ sự phân bổ nhất quán nào của`b`. 

Sau quá trình chuyển đổi này, mỗi vị trí có thể được kiểm tra liên tục bằng cách sử dụng các vị trí lân cận của nó và chúng tôi chỉ đếm số lượng vị trí phải được sửa đổi để loại bỏ tất cả các vi phạm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Xây dựng lực lượng vũ phu của`b`| Hàm mũ | O(n) | Quá chậm | 
| Kiểm tra tính nhất quán cục bộ`a`| O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta diễn giải lại điều kiện theo cách tránh xây dựng một cách rõ ràng`b`. Mỗi`a_i`tương ứng với một “ràng buộc đỉnh cao” đối với hai vị trí liền kề trong`b`. Để một cấu trúc như vậy có tính nhất quán, không có ràng buộc đơn lẻ nào có thể buộc một giá trị biệt lập không thể được hỗ trợ bởi các ràng buộc lân cận của nó. 

Điểm mấu chốt là sự không nhất quán chỉ phát sinh cục bộ: tại các vị trí mà`a_i`quá lớn so với cả hai giá trị liền kề trong`a`. Phần tử như vậy không thể được hỗ trợ bởi bất kỳ bên nào trong bất kỳ cách xây dựng hợp lệ nào của`b`, vì vậy nó phải được sửa đổi. 

Chúng tôi tiến hành như sau. 

1. Với mỗi test case, hãy đọc mảng`a`. 
2. Đối với từng vị trí`i`, hãy tính “độ hỗ trợ” mạnh nhất mà nó có thể nhận được từ các hàng xóm của nó, giá trị nhỏ hơn trong các giá trị liền kề của nó. Đối với các vị trí bên trong, đây là`min(a_{i-1}, a_{i+1})`. Đối với các điểm cuối, chúng tôi coi những người hàng xóm bị thiếu là vô cùng dễ dãi theo hướng thích hợp. 
3. Kiểm tra xem`a_i`vượt quá sự hỗ trợ có sẵn từ cả hai phía. Nếu như`a_i`lớn hơn cả hai hàng xóm thì không có cấu hình hợp lệ của`b`có thể nhận ra giá trị này mà không cần sửa đổi`a_i`. 
4. Đếm tất cả các vị trí như vậy. Mỗi cái tương ứng với một sửa đổi bắt buộc. 
5. Xuất ra tổng số lượng cho mỗi ca kiểm thử. 

### Tại sao nó hoạt động 

Một cấu hình hợp lệ yêu cầu mọi giá trị trong`a`có thể được nhận ra là mức tối đa của một cạnh chung trong`b`. Nếu một phần tử`a_i`hoàn toàn lớn hơn cả hai ràng buộc lân cận thì không có đoạn liền kề nào trong`b`có thể “lưu trữ” nó mà không vi phạm điều kiện tối đa trên cấu trúc cạnh chồng lên nhau. Vì không có vị trí thay thế nào cho đỉnh đó nên cách duy nhất để khôi phục tính khả thi là sửa đổi phần tử đó. 

Ngược lại, nếu`a_i`không lớn hơn cả hai lân cận thì tồn tại ít nhất một hướng mà nó có thể được hỗ trợ, nghĩa là chúng ta luôn có thể định hướng ràng buộc tương ứng trong`b`một cách nhất quán. Điều này đảm bảo rằng việc sửa chính xác các vị trí không thể thực hiện cục bộ là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    INF = 10**18
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        if n == 1:
            print(0)
            continue
        
        ans = 0
        
        for i in range(n):
            left = a[i - 1] if i - 1 >= 0 else INF
            right = a[i + 1] if i + 1 < n else INF
            
            if a[i] > left and a[i] > right:
                ans += 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập và kiểm tra mọi vị trí trong thời gian không đổi. Đối với các phần tử ranh giới, chúng tôi coi các phần tử lân cận bị thiếu là không hạn chế nên chỉ tính các xung đột nội bộ thực sự. 

Việc triển khai dựa trên một lần quét tuyến tính duy nhất và không cần cấu trúc phụ trợ. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó`a = [2, 4, 6, 3]`. 

| tôi | một [tôi] | trái | đúng | vi phạm điều kiện? | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | INF | 4 | không | giữ | 
| 1 | 4 | 2 | 6 | không | giữ | 
| 2 | 6 | 4 | 3 | vâng | sửa đổi | 
| 3 | 3 | 6 | INF | không | giữ | 

Chỉ có một vị trí vi phạm điều kiện khả thi cục bộ nên câu trả lời là`1`. 

Bây giờ hãy xem xét`a = [5, 1, 4]`. 

| tôi | một [tôi] | trái | đúng | vi phạm điều kiện? | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | INF | 1 | không | giữ | 
| 1 | 1 | 5 | 4 | không | giữ | 
| 2 | 4 | 1 | INF | không | giữ | 

Không có phần tử nào lớn hơn cả hai phần tử lân cận, do đó không cần sửa đổi. 

Trường hợp thứ hai xác nhận rằng các giá trị thấp bị cô lập không gây ra vấn đề và chỉ có các đỉnh cục bộ nghiêm ngặt mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được kiểm tra một lần với công việc O(1) | 
| Không gian | O(1) thêm | Chỉ một số biến được sử dụng ngoài bộ nhớ đầu vào | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng của`n`, thỏa mãn ràng buộc của`2⋅10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if n == 1:
            out.append("0")
            continue
        ans = 0
        INF = 10**18
        for i in range(n):
            left = a[i-1] if i-1 >= 0 else INF
            right = a[i+1] if i+1 < n else INF
            if a[i] > left and a[i] > right:
                ans += 1
        out.append(str(ans))
    return "\n".join(out) + "\n"

# provided sample
assert run("1\n4\n2 4 6 3\n") == "1\n"

# custom cases
assert run("1\n1\n100\n") == "0\n", "single element"
assert run("1\n3\n1 2 3\n") == "0\n", "monotone increasing"
assert run("1\n3\n3 2 1\n") == "0\n", "monotone decreasing"
assert run("1\n5\n1 5 1 5 1\n") == "2\n", "alternating peaks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 100`|`0`| mảng kích thước tối thiểu | 
|`1 3 1 2 3`|`0`| không có đỉnh cục bộ | 
|`1 3 3 2 1`|`0`| giảm hành vi ranh giới | 
|`1 5 1 5 1 5 1`|`2`| luân phiên vi phạm | 

## Vỏ cạnh 

cho`n = 1`, không có ràng buộc lân cận, do đó, bất kỳ giá trị đơn lẻ nào cũng luôn có thể được nhận ra bằng cách chọn`b = [x, x]`. Thuật toán trả về 0 chính xác vì không có vị trí nào có hai lân cận vi phạm điều kiện. 

Đối với các mảng đơn điệu nghiêm ngặt, mọi phần tử đều được hỗ trợ bởi ít nhất một bên, do đó không có phần tử nào lớn hơn cả hai phần tử lân cận. Quá trình quét không tạo ra bất kỳ sửa đổi nào, phù hợp với thực tế là một bản nhất quán`b`luôn có thể được xây dựng bằng cách truyền bá các giá trị dọc theo chuỗi. 

Đối với các mô hình cao-thấp xen kẽ, các đỉnh cục bộ xuất hiện chính xác ở các vị trí cao vượt quá cả hai đỉnh lân cận và mỗi đỉnh như vậy phải được sửa đổi. Thuật toán tính chính xác các vị trí đó, phù hợp với mức sửa chữa tối thiểu cần thiết để loại bỏ tính không khả thi.
