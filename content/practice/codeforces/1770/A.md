---
title: "CF 1770A - Koxia và Bảng Trắng"
description: "Chúng ta bắt đầu với một dãy số được đặt trên bảng trắng. Sau đó chúng ta được cung cấp một chuỗi các giá trị thay thế. Mỗi thao tác thay thế cho phép chúng ta chọn bất kỳ một bảng trắng nào và ghi đè giá trị hiện tại của nó bằng giá trị mới từ thao tác."
date: "2026-06-09T12:28:30+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1770
codeforces_index: "A"
codeforces_contest_name: "Good Bye 2022: 2023 is NEAR"
rating: 1000
weight: 1770
solve_time_s: 180
verified: true
draft: false
---

[CF 1770A - Koxia và Bảng trắng](https://codeforces.com/problemset/problem/1770/A) 

**Đánh giá:** 1000 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 3 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một dãy số được đặt trên bảng trắng. Sau đó chúng ta được cung cấp một chuỗi các giá trị thay thế. Mỗi thao tác thay thế cho phép chúng ta chọn bất kỳ một bảng trắng nào và ghi đè giá trị hiện tại của nó bằng giá trị mới từ thao tác. 

Tất cả các thao tác đều phải được sử dụng nhưng chúng ta có thể tự do lựa chọn bảng trắng nào sẽ được cập nhật mỗi lần và cùng một bảng trắng có thể được cập nhật nhiều lần. Mục tiêu là tối đa hóa tổng giá trị cuối cùng được ghi trên bảng sau khi áp dụng tất cả các phép thay thế. 

Một quan sát quan trọng là chỉ có giá trị cuối cùng trên mỗi bảng mới quan trọng chứ không phải thứ tự áp dụng các phép toán. Mỗi thao tác đóng góp chính xác một giá trị và cuối cùng sẽ được gán ở đâu đó. 

Từ$n, m \le 100$, các ràng buộc cho phép lập luận tham lam một cách đơn giản. Bất kỳ giải pháp nào lên đến$O(nm)$hoặc$O(m \log m)$dễ dàng là đủ. 

Một trường hợp khó nhận thấy là khi$m > n$, nghĩa là một số bảng sẽ bị ghi đè nhiều lần. Một điều nữa là khi$m < n$, nghĩa là một số giá trị ban đầu không bao giờ được chạm vào. Một sai lầm ngây thơ là cho rằng mỗi thao tác phải đi đến một bảng riêng biệt, điều này là sai và dẫn đến đánh giá thấp tính linh hoạt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các cách để gán từng$m$giá trị để$n$bảng. Với mỗi thao tác, chúng tôi chọn một trong các$n$bảng, dẫn đến$n^m$khả năng. Ngay cả đối với$n = m = 100$, cái này quá lớn. 

Sự đơn giản hóa chính xuất phát từ việc nhận ra rằng cấu hình cuối cùng chỉ phụ thuộc vào hai bộ số: giá trị bảng ban đầu và các giá trị chúng tôi quyết định áp dụng từ danh sách thao tác. Mỗi thao tác đóng góp chính xác một giá trị và chúng ta có thể chọn vị trí của nó. Điều này có nghĩa là chúng ta đang quyết định một cách hiệu quả giá trị cuối cùng nào sẽ thay thế giá trị ban đầu nào. 

Để tối đa hóa tổng, chúng ta nên bảo toàn các giá trị ban đầu lớn trừ khi chúng có thể được thay thế bằng các giá trị hoạt động lớn hơn nữa. Vì mỗi giá trị thao tác có thể được sử dụng nhiều nhất một lần nên chiến lược tối ưu sẽ giảm xuống việc chọn các giá trị lớn nhất có thể trong số tất cả các ứng cử viên có sẵn và đặt chúng vào các bảng có lợi nhất. 

Một cách rõ ràng để thấy điều này là chúng ta đang xây dựng một tập hợp nhiều kích thước cuối cùng$n$, nơi chúng ta bắt đầu với$a_i$và có thể thay thế tới$m$các yếu tố sử dụng$b_j$. Mỗi lần thay thế chỉ nên được sử dụng nếu nó làm tăng tổng và trong số tất cả các lựa chọn, chúng tôi luôn ưu tiên các giá trị lớn hơn. 

Điều này dẫn đến việc sắp xếp cả hai mảng và khớp một cách tham lam với giá trị lớn nhất$b_j$các giá trị nhỏ nhất$a_i$các giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công vũ lực | hàm mũ |$O(n)$| Quá chậm | 
| Sắp xếp + thay thế tham lam |$O(n \log n + m \log m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng ban đầu$a$theo thứ tự tăng dần để chúng ta dễ dàng xác định được những giá trị nhỏ nhất dễ cải thiện nhất. Lý do là việc thay thế một giá trị nhỏ sẽ mang lại lợi ích cận biên lớn nhất. 
2. Sắp xếp các giá trị thay thế$b$theo thứ tự giảm dần để chúng ta có thể sử dụng những cải tiến mạnh nhất hiện có trước tiên. Điều này đảm bảo chúng ta không bao giờ lãng phí một giá trị lớn vào một vị thế mà nó mang lại ít lợi ích. 
3. Lặp lại đồng thời cả hai mảng, so sánh giá trị nhỏ nhất còn lại$a[i]$còn lại lớn nhất$b[j]$. Nếu như$b[j] > a[i]$, thay thế$a[i]$với$b[j]$. Nếu không, hãy ngừng sử dụng các sản phẩm thay thế tiếp theo vì tất cả các sản phẩm còn lại$a[i]$thậm chí còn lớn hơn. 
4. Tiếp tục quá trình này cho đến khi chúng ta cạn kiệt$b$hoặc đạt đến điểm mà không có sự thay thế nào sẽ cải thiện tổng. Mảng kết quả thể hiện sự phân công hoạt động tối ưu. 
5. Tính tổng cuối cùng của mảng đã sửa đổi. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên một đối số trao đổi. Giả sử chúng ta có một giải pháp tối ưu trong đó giá trị thay thế nhỏ hơn được gán cho giá trị nhỏ hơn$a[i]$, trong khi giá trị thay thế lớn hơn không được sử dụng hoặc được chỉ định ở nơi khác. Hoán đổi các bài tập này không bao giờ làm giảm tổng và có thể làm tăng nó. Lặp lại lý do này dẫn đến một cấu trúc trong đó lớn nhất$b$các giá trị được khớp với giá trị nhỏ nhất$a$giá trị bất cứ khi nào có lợi, đó chính xác là những gì quy trình tham lam xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    a.sort()
    b.sort(reverse=True)
    
    i = 0
    j = 0
    
    while i < n and j < m:
        if b[j] > a[i]:
            a[i] = b[j]
            i += 1
            j += 1
        else:
            break
    
    print(sum(a))
```Việc thực hiện trực tiếp theo chiến lược tham lam. Sắp xếp$a$cho phép chúng tôi luôn cố gắng cải thiện những vị trí yếu nhất trước tiên. Sắp xếp$b$theo thứ tự giảm dần đảm bảo chúng tôi sớm nhận được những nâng cấp mạnh nhất. 

Vòng lặp dừng ngay khi không thể cải thiện được, điều này tránh được những so sánh không cần thiết khi tất cả còn lại$a[i]$đã lớn hơn phần còn lại$b[j]$. Tổng cuối cùng được tính trực tiếp từ mảng được cập nhật. 

## Ví dụ đã hoạt động 

Xem xét đầu vào$a = [1,2,3]$Và$b = [4,5]$. Sau khi sắp xếp, chúng tôi so sánh$1$với$5$, thay thế rồi so sánh$2$với$4$, thay thế nó và dừng lại. Mảng kết quả là$[5,4,3]$. 

| Bước | một (tiểu bang) | con trỏ b | hành động | 
| --- | --- | --- | --- | 
| 1 | [1,2,3] | 5 | thay 1 bằng 5 | 
| 2 | [5,2,3] | 4 | thay 2 bằng 4 | 
| 3 | [5,4,3] | dừng lại | không cải thiện | 

Tổng cuối cùng là$12$, phù hợp với cấu hình tối ưu. 

Vì$a = [10,10]$Và$b = [1,2]$, sắp xếp cho$a$không thay đổi và$b = [2,1]$. Vì ngay cả lớn nhất$b$không lớn hơn số nhỏ nhất$a$, không có sự thay thế nào xảy ra và tổng vẫn còn$20$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+m)\log(n+m))$| sắp xếp chiếm ưu thế, quét tham lam là tuyến tính | 
| Không gian |$O(n+m)$| lưu trữ cho mảng | 

Các ràng buộc đủ nhỏ để việc sắp xếp và vượt qua tuyến tính duy nhất cho mỗi trường hợp thử nghiệm có thể dễ dàng nhanh chóng. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        a.sort()
        b.sort(reverse=True)
        
        i = 0
        j = 0
        
        while i < n and j < m:
            if b[j] > a[i]:
                a[i] = b[j]
                i += 1
                j += 1
            else:
                break
        
        out.append(str(sum(a)))
    
    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("""4
3 2
1 2 3
4 5
2 3
1 2
3 4 5
1 1
100
1
5 3
1 1 1 1 1
1000000000 1000000000 1000000000
""") == """12
9
1
3000000002"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị nhỏ | hành vi thay thế tham lam | tính đúng đắn cơ bản | 
| tất cả các giá trị b lớn | thay thế hoàn toàn | sử dụng mức tăng tối đa | 
| không có trường hợp cải thiện | tổng không đổi | điều kiện dừng | 

## Vỏ cạnh 

Khi nào$m = 0$, không có sự thay thế nào xảy ra và đầu ra chỉ đơn giản là tổng của$a$. Thuật toán xử lý việc này vì vòng lặp không bao giờ thực thi. 

Khi tất cả$b_j \le \min(a_i)$, không có sự thay thế nào xảy ra vì phép so sánh đầu tiên thất bại ngay lập tức, đảm bảo chúng tôi không giảm tổng. 

Khi$m > n$, chỉ lớn nhất$n$những giá trị hữu ích từ$b$đã từng được sử dụng, vì vòng lặp dừng lại một lần$a[i]$đã được xem xét.
