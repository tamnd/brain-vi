---
title: "CF 1610B - Mảng Kalindrom"
description: "Chúng ta được cung cấp một mảng và được phép thực hiện một kiểu chuyển đổi rất cụ thể: chúng ta có thể chọn một giá trị x và loại bỏ bất kỳ lần xuất hiện nào của giá trị đó khỏi mảng, không nhất thiết phải tất cả chúng."
date: "2026-06-10T07:09:18+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1610
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 17"
rating: 1100
weight: 1610
solve_time_s: 111
verified: true
draft: false
---

[CF 1610B - Mảng Kalindrome](https://codeforces.com/problemset/problem/1610/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và được phép thực hiện một kiểu chuyển đổi rất cụ thể: chúng ta có thể chọn một giá trị`x`và xóa bất kỳ lần xuất hiện nào của giá trị đó khỏi mảng, không nhất thiết phải tất cả chúng. Sau khi xóa, các phần tử còn lại sẽ thu gọn lại với nhau và chúng tôi kiểm tra xem chuỗi kết quả có phải là một bảng màu hay không. 

Nhiệm vụ là quyết định xem có tồn tại ít nhất một lựa chọn về`x`sao cho thao tác này có thể tạo ra mảng palindromic. Chúng ta không cần xây dựng việc xóa một cách rõ ràng ở đầu ra cuối cùng, chỉ trả lời xem có thể thực hiện được hay không. 

Các ràng buộc cho phép tối đa 10^4 trường hợp thử nghiệm và tổng độ dài mảng trên tất cả các thử nghiệm lên tới 2 × 10^5. Điều này ngay lập tức ngụ ý rằng O(n^2) trên mỗi bài kiểm tra hoặc bất cứ điều gì tệ hơn tuyến tính trên mỗi bài kiểm tra đều không an toàn. Bất kỳ giải pháp nào cũng phải xử lý hiệu quả từng thử nghiệm theo thời gian tuyến tính, có thể với một số lần quét tuyến tính nhỏ. 

Một cách tiếp cận ngây thơ sẽ thử mọi giá trị có thể có của`x`, xóa nó và kiểm tra xem mảng kết quả có phải là một bảng màu hay không. Đó sẽ là O(n^2) cho mỗi lần kiểm tra trong trường hợp xấu nhất, quá chậm khi các giá trị lặp lại thường xuyên. 

Một trường hợp lỗi tinh vi hơn xuất hiện khi các mảng gần như là các bảng màu nhưng yêu cầu loại bỏ một giá trị xuất hiện ở nhiều vị trí. Một nỗ lực xóa tham lam nhằm loại bỏ giá trị không khớp đầu tiên gặp phải sẽ thất bại vì việc lựa chọn đúng`x`là toàn cầu, không phải địa phương. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi giá trị riêng biệt trong mảng, hãy mô phỏng loại bỏ tất cả các lần xuất hiện của giá trị đó, sau đó kiểm tra xem mảng kết quả có phải là một bảng màu hay không. Mỗi lần kiểm tra có chi phí O(n) và có thể có các giá trị riêng biệt O(n), do đó tổng chi phí sẽ trở thành O(n^2). Điều này ngay lập tức phá vỡ các ràng buộc. 

Quan sát quan trọng là cấu trúc của một giải pháp hợp lệ bị hạn chế rất nhiều bởi sự không khớp trong mảng ban đầu. Chúng tôi bắt đầu từ việc kiểm tra bảng màu tiêu chuẩn bằng cách sử dụng hai con trỏ. Nếu mảng đã là một palindrome thì chúng ta đã hoàn thành. Ngược lại, chúng ta tìm vị trí đầu tiên từ bên trái và bên phải nơi có các giá trị khác nhau. Tại thời điểm này, bất kỳ giải pháp hợp lệ nào cũng phải loại bỏ tất cả các lần xuất hiện của giá trị bên trái hoặc giá trị bên phải liên quan đến giá trị không khớp, bởi vì đây là những ứng cử viên duy nhất có khả năng loại bỏ xung đột ở vị trí đó. 

Điều này làm giảm vấn đề chỉ còn việc kiểm tra tối đa hai ứng viên. Đối với mỗi giá trị ứng cử viên, chúng tôi mô phỏng việc kiểm tra bảng màu hai con trỏ trong khi bỏ qua giá trị đó bất cứ khi nào nó xuất hiện. Nếu một trong hai mô phỏng thành công thì mảng đó là kalindrome. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Brute Force trên tất cả x | O(n^2) | O(n) | Quá chậm | 
| Kiểm tra ứng viên bằng hai con trỏ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta bắt đầu với hai con trỏ ở cuối mảng. Chúng tôi di chuyển chúng vào trong trong khi các giá trị khớp nhau. Nếu chúng ta đến giữa, mảng đã là một bảng màu. 

Nếu chúng ta gặp phải sự không phù hợp ở vị trí`l`Và`r`, cách khắc phục duy nhất có thể là loại bỏ tất cả các lần xuất hiện của`a[l]`hoặc tất cả các lần xuất hiện của`a[r]`. Chúng tôi kiểm tra cả hai tùy chọn một cách độc lập. 

Để kiểm tra giá trị ứng cử viên`x`, chúng ta lại sử dụng hai con trỏ. Bất cứ khi nào chúng ta nhìn thấy`x`, chúng ta bỏ qua nó bằng cách di chuyển con trỏ vào trong. Còn không thì chúng ta so sánh bình thường. Nếu tại bất kỳ thời điểm nào các giá trị không bị bỏ qua không khớp, ứng cử viên này sẽ thất bại. 

Chúng tôi trả lại CÓ nếu một trong hai ứng viên thành công. 

Lý do điều này có tác dụng là vì sự không khớp đầu tiên trong quá trình quét hai con trỏ xác định chính xác vùng nơi đối xứng bị phá vỡ. Mọi giải pháp hợp lệ đều phải loại bỏ một trong hai giá trị xung đột, nếu không thì sự không khớp vẫn tồn tại trong mọi phiên bản rút gọn của mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_pal_after_removal(a, x):
    l, r = 0, len(a) - 1
    while l < r:
        while l < r and a[l] == x:
            l += 1
        while l < r and a[r] == x:
            r -= 1
        if l >= r:
            break
        if a[l] != a[r]:
            return False
        l += 1
        r -= 1
    return True

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        l, r = 0, n - 1
        
        while l < r and a[l] == a[r]:
            l += 1
            r -= 1
        
        if l >= r:
            print("YES")
            continue
        
        x, y = a[l], a[r]
        
        if is_pal_after_removal(a, x) or is_pal_after_removal(a, y):
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Sau khi xác định điểm không khớp đầu tiên, mã sẽ tách biệt chính xác hai giá trị có thể ảnh hưởng đến cấu trúc của bất kỳ giải pháp hợp lệ nào. Hàm trợ giúp thực hiện kiểm tra bảng màu được lọc theo thời gian tuyến tính, bỏ qua hoàn toàn một giá trị ứng cử viên. Điều này tránh việc xây dựng lại mảng và duy trì mức sử dụng bộ nhớ không đổi. 

Một điểm tinh tế là chúng tôi không cố gắng loại bỏ các lần xuất hiện một phần một cách tùy tiện. Logic bỏ qua ngầm cho phép xóa có chọn lọc trong khi vẫn giữ nguyên thứ tự, phù hợp với định nghĩa vấn đề. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
5
1 4 4 1 4
```Quá trình quét hai con trỏ ban đầu so sánh 1 và 4, ngay lập tức không khớp. Vậy ứng viên là 1 và 4. 

Kiểm tra việc loại bỏ 1 mang lại [4, 4, 4], là một bảng màu. 

Thử nghiệm loại bỏ 4 sản lượng [1, 1], cũng là một palindrome. 

Thuật toán trả về đúng CÓ. 

Bây giờ hãy xem xét:```
3
1 2 3
```Sự không khớp đầu tiên xảy ra ngay lập tức ở cả hai đầu (1 và 3). Chỉ loại bỏ 1 mang lại [2, 3], không phải là bảng màu. Chỉ loại bỏ 3 mang lại [1, 2], cũng không phải là một bảng màu. Thuật toán trả về đúng NO. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi bài kiểm tra thực hiện tối đa hai lần quét tuyến tính | 
| Không gian | O(1) | Chỉ sử dụng con trỏ và biến | 

Tổng kích thước đầu vào trong các thử nghiệm được giới hạn bởi 2 × 10^5, do đó, chiến lược xử lý từng phần tử tuyến tính dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    def is_pal_after_removal(a, x):
        l, r = 0, len(a) - 1
        while l < r:
            while l < r and a[l] == x:
                l += 1
            while l < r and a[r] == x:
                r -= 1
            if l >= r:
                break
            if a[l] != a[r]:
                return False
            l += 1
            r -= 1
        return True

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        l, r = 0, n - 1
        while l < r and a[l] == a[r]:
            l += 1
            r -= 1
        if l >= r:
            out.append("YES")
        else:
            x, y = a[l], a[r]
            if is_pal_after_removal(a, x) or is_pal_after_removal(a, y):
                out.append("YES")
            else:
                out.append("NO")
    return "\n".join(out)

assert run("""4
1
1
2
1 2
3
1 2 3
5
1 4 4 1 4
""") == """YES
YES
NO
YES"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| đã nhạt màu | CÓ | sự chấp nhận tầm thường | 
| đơn không khớp có thể sửa được | CÓ | công việc loại bỏ ứng viên | 
| trường hợp bất khả thi | KHÔNG | từ chối tính đúng đắn | 
| trường hợp đa giá trị đối xứng | CÓ | cả hai đường dẫn ứng viên đều hợp lệ | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi mảng đã là một bảng màu. Trong trường hợp đó, các con trỏ không khớp không bao giờ phân kỳ và thuật toán kết thúc sớm một cách chính xác mà không cần kiểm tra bất kỳ ứng cử viên xóa nào. Điều này ngăn cản việc tính toán không cần thiết và tránh những kết quả âm tính giả. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều giống hệt nhau. Vòng lặp không khớp không bao giờ được kích hoạt và câu trả lời ngay lập tức là CÓ, vì bất kỳ thao tác xóa nào vẫn để lại một bảng màu hoặc mảng trống.
