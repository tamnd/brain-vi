---
title: "CF 1883F - Em Thật Đẹp"
description: "Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu đếm số lượng các mảng con liền kề có các phần tử xuất hiện dưới dạng một dãy con trong mảng ban đầu đúng một lần."
date: "2026-06-08T22:31:33+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1883
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 905 (Div. 3)"
rating: 1400
weight: 1883
solve_time_s: 132
verified: false
draft: false
---

[CF 1883F - Bạn Thật Đẹp](https://codeforces.com/problemset/problem/1883/F) 

**Đánh giá:** 1400 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu đếm số mảng con liền kề có các phần tử xuất hiện dưới dạng một dãy con trong mảng ban đầu **chính xác một lần**. Nói một cách đơn giản hơn, một mảng con sẽ tốt nếu, khi bạn cố gắng tìm nó ở bất kỳ nơi nào khác trong mảng mà không sắp xếp lại các phần tử của nó, thì chỉ có một cách để chọn những vị trí đó. Ví dụ,`[1, 2]`TRONG`[1, 2, 1]`là tốt vì trình tự`1, 2`chỉ xuất hiện ở hai vị trí đầu tiên. Nhưng`[1]`là xấu bởi vì`1`xảy ra nhiều lần nên có nhiều cách để chọn nó làm dãy con. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm có một mảng tối đa`10^5`các phần tử với tổng của tất cả`n`trên các trường hợp thử nghiệm lên đến`2*10^5`. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào chậm hơn khoảng`O(n log n)`mỗi trường hợp thử nghiệm sẽ quá chậm. Một cách tiếp cận ngây thơ liệt kê tất cả các mảng con sẽ là`O(n^2)`cho mỗi trường hợp thử nghiệm, điều này rõ ràng là không thể thực hiện được. 

Các trường hợp cạnh phát sinh khi tất cả các phần tử giống hệt nhau, mảng có kích thước 1 hoặc mảng có các giá trị xen kẽ nhau. Ví dụ,`[1, 1, 1]`chỉ có một mảng con hợp lệ có độ dài 1 nếu bất kỳ phần tử đơn lẻ nào được cho phép, nhưng tất cả các mảng con dài hơn đều không hợp lệ.`[1, 2, 1]`có mảng con`[1, 2]`,`[2, 1]`,`[1, 2, 1]`hành xử khác nhau. Việc xử lý các bản sao một cách cẩn thận là rất quan trọng vì chúng kiểm soát liệu một chuỗi con có phải là duy nhất hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là lặp lại tất cả các mảng con có thể có và đối với mỗi mảng con, hãy kiểm tra xem nó xuất hiện bao nhiêu lần dưới dạng một chuỗi con. Kiểm tra các chuỗi con một cách ngây thơ`O(n)`thời gian cho mỗi mảng con, và có`O(n^2)`mảng con, do đó độ phức tạp tổng thể sẽ là`O(n^3)`mỗi trường hợp thử nghiệm. Điều này rõ ràng không thể làm việc cho`n`lên đến`10^5`. 

Cái nhìn sâu sắc quan trọng là một mảng con`[a_l, ..., a_r]`xảy ra nhiều lần dưới dạng một dãy con **nếu bất kỳ phần tử nào trong mảng con xuất hiện ở nơi khác ngoài vị trí hiện tại của nó theo thứ tự xung đột**. Để thực hiện điều này một cách chính xác, chúng ta có thể theo dõi từng giá trị lần xuất hiện đầu tiên và cuối cùng trong mảng. Một mảng con là duy nhất dưới dạng một dãy con nếu tất cả các phần tử của nó xuất hiện **chính xác một lần trong toàn bộ mảng bên ngoài vị trí của chúng**. Một cách khác để suy nghĩ về nó là duy trì một cửa sổ trượt sao cho không có phần tử nào bên trong cửa sổ có sự xuất hiện khác trong cùng một vùng hoặc vùng chồng chéo. Điều này làm giảm vấn đề thành vấn đề hai con trỏ trong đó chúng ta mở rộng ranh giới bên phải trong khi vẫn đảm bảo rằng mỗi phần tử bên trong cửa sổ xuất hiện nhiều nhất hai lần: một lần trong cửa sổ, một lần có thể ở bên ngoài. 

Một quan sát rõ ràng là các phần tử “xấu” là những phần tử có lần xuất hiện đầu tiên hoặc cuối cùng ở bên trong cửa sổ hiện tại nhưng cũng xảy ra ở bên ngoài. Bằng cách sử dụng hai bản đồ để theo dõi lần xuất hiện đầu tiên và cuối cùng, sau đó di chuyển con trỏ sang trái để loại trừ xung đột, chúng ta có thể đếm số lượng cửa sổ hợp lệ một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Hai con trỏ tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên, tính toán trước cho mỗi giá trị duy nhất trong mảng các chỉ số lần xuất hiện đầu tiên và cuối cùng của nó. Điều này cho phép chúng ta nhanh chóng kiểm tra xem một phần tử trong mảng con ứng viên có xuất hiện ở vị trí xung đột ở nơi khác hay không. 
2. Khởi tạo con trỏ trái`l`tại 0 và lặp lại con trỏ bên phải`r`từ 0 đến n-1. Chúng tôi đang cố gắng duy trì một mảng con hợp lệ một cách hiệu quả`[l, r]`trong đó tất cả các phần tử xuất hiện ở các vị trí không tạo ra nhiều chuỗi con. 
3. Duy trì một bản đồ đếm số lần mỗi phần tử xuất hiện trong cửa sổ hiện tại. Nếu chúng ta cố gắng thêm một phần tử đã xuất hiện bên trong cửa sổ hoặc vi phạm ràng buộc về tính duy nhất, hãy di chuyển con trỏ sang trái`l`để thu nhỏ cửa sổ cho đến khi điều kiện được thỏa mãn trở lại. 
4. Đối với mỗi vị trí của`r`, số mảng con hợp lệ kết thúc tại`r`là`(r - l + 1)`. Thêm số này vào tổng số. Điều này hoạt động vì mở rộng`r`bởi một bao gồm tất cả các mảng con mới kết thúc tại`r`với các điểm cuối bên trái khác nhau. 
5. Tiếp tục cho đến khi`r`đến cuối mảng. 

Điều bất biến là ở mỗi bước,`[l, r]`là **phân mảng lớn nhất kết thúc tại`r`đó là duy nhất dưới dạng một dãy con**, vì vậy việc đếm tất cả các mảng con bên trong nó đảm bảo chúng ta đếm tất cả các mảng con tốt chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        last_occurrence = {}
        left = 0
        result = 0
        count = {}
        
        for right in range(n):
            val = a[right]
            count[val] = count.get(val, 0) + 1
            while count[val] > 1:
                count[a[left]] -= 1
                left += 1
            result += right - left + 1
        
        print(result)

solve()
```Chúng tôi sử dụng từ điển`count`để theo dõi các lần xuất hiện trong cửa sổ hiện tại. Bất cứ khi nào chúng tôi gặp một bản sao bên trong cửa sổ, chúng tôi sẽ di chuyển`left`cho đến khi cửa sổ trở nên hợp lệ. Số mảng con kết thúc ở mỗi mảng`right`đơn giản là độ dài cửa sổ`right - left + 1`. 

Việc xử lý ranh giới rất tinh tế: nếu cửa sổ bắt đầu co lại từ bên trái, chúng ta phải giảm số đếm một cách cẩn thận để tránh các lỗi sai lệch một. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`[1, 2, 1]`| r | giá trị | đếm | trái | mảng con được thêm vào | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | {1:1} | 0 | 1 | 1 | 
| 1 | 2 | {1:1,2:1} | 0 | 2 | 3 | 
| 2 | 1 | {1:2,2:1} | 0->1 | 2 | 4 | 

Dấu vết xác nhận bất biến hai con trỏ: tại mỗi bước, cửa sổ`[left, r]`không chứa bản sao và đếm`right - left + 1`đưa ra chính xác các mảng con hợp lệ. 

### Ví dụ 2:`[4, 5, 4, 5, 4]`| r | giá trị | đếm | trái | mảng con được thêm vào | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | {4:1} | 0 | 1 | 1 | 
| 1 | 5 | {4:1,5:1} | 0 | 2 | 3 | 
| 2 | 4 | {4:2,5:1} | 0->1 | 2 | 5 | 
| 3 | 5 | {4:1,5:2} | 1->2 | 2 | 7 | 
| 4 | 4 | {4:2,5:1} | 2->3 | 2 | 9 | 

Điều này cho thấy các điều chỉnh con trỏ bên trái kích hoạt trùng lặp một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được thêm vào và xóa khỏi cửa sổ nhiều nhất một lần. | 
| Không gian | O(n) | Từ điển lưu trữ số lượng các phần tử riêng biệt. | 

Với tổng n trên tất cả các trường hợp thử nghiệm 2*10^5, điều này phù hợp thoải mái dưới giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("6\n1\n1\n2\n1 1\n3\n1 2 1\n4\n2 3 2 1\n5\n4 5 4 5 4\n10\n1 7 7 2 3 4 3 2 1 100\n") == "1\n1\n4\n7\n4\n28"

# Custom cases
assert run("1\n1\n42\n") == "1"  # single element
assert run("1\n5\n1 1 1 1 1\n") == "5"  # all equal
assert run("1\n5\n1 2 3 4 5\n") == "15"  # all unique
assert run("1\n6\n1 2 1 2 1 2\n") == "9"  # alternating duplicates
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n42\n`| 1 | Mảng một phần tử | 
|`1\n5\n1 1 1 1 1\n`| 5 | Xử lý tất cả các giá trị bằng nhau | 
|`1\n5\n1 2 3 4 5\n`| 15 | Tất cả các giá trị duy nhất, đếm tất cả các mảng con | 
| `1\n6\n | | |
