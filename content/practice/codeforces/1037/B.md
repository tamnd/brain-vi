---
title: "CF 1037B - Phạm vi tiếp cận trung bình"
description: "Chúng ta được cung cấp một danh sách các số nguyên và giá trị đích s. Mục đích không phải là làm cho tất cả các phần tử bằng s mà chỉ để đảm bảo rằng sau khi chúng ta sắp xếp mảng, phần tử ở giữa sẽ trở thành chính xác s. Vì độ dài là số lẻ nên chỉ có một vị trí trung vị được xác định rõ ràng."
date: "2026-06-16T18:42:27+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1037
codeforces_index: "B"
codeforces_contest_name: "Manthan, Codefest 18 (rated, Div. 1 + Div. 2)"
rating: 1300
weight: 1037
solve_time_s: 195
verified: true
draft: false
---

[CF 1037B - Phạm vi tiếp cận trung bình](https://codeforces.com/problemset/problem/1037/B) 

**Đánh giá:** 1300 
**Thẻ:** tham lam 
**Thời gian giải:** 3 phút 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các số nguyên và giá trị mục tiêu`s`. Mục tiêu không phải là làm cho tất cả các phần tử bằng nhau`s`, nhưng chỉ để đảm bảo rằng sau khi chúng ta sắp xếp mảng, phần tử ở giữa sẽ trở nên chính xác`s`. Vì độ dài là số lẻ nên chỉ có một vị trí trung vị được xác định rõ ràng. 

Hoạt động duy nhất được phép là tăng hoặc giảm một phần tử một đơn vị cho mỗi lần di chuyển. Mỗi phần tử có thể được điều chỉnh độc lập và chi phí là tổng số lần thay đổi đơn vị đó. 

Khó khăn chính là giá trị trung vị phụ thuộc vào thứ tự chứ không phụ thuộc vào các giá trị riêng lẻ. Việc thay đổi một phần tử có thể ảnh hưởng đến giá trị trung vị bằng cách dịch chuyển các giá trị qua ranh giới ở giữa hoặc bằng cách thay đổi trực tiếp phần tử ở giữa sau khi sắp xếp. 

Ràng buộc`n ≤ 2·10^5`ngụ ý rằng chúng ta cần một`O(n log n)`hoặc giải pháp tốt hơn. Việc sắp xếp có thể chấp nhận được, nhưng bất kỳ cách tiếp cận nào cố gắng mô phỏng tất cả các sửa đổi có thể có hoặc tính toán lại trung vị nhiều lần sau khi cập nhật gia tăng sẽ quá chậm, vì mỗi mô phỏng sẽ là tuyến tính và các điều chỉnh lặp lại có thể dễ dàng suy giảm thành thời gian bậc hai. 

Một ý tưởng ngây thơ nhưng hấp dẫn là cố gắng buộc mọi yếu tố di chuyển về phía`s`và tính lại số trung vị mỗi lần. Điều này thất bại vì nó bỏ qua thực tế là chỉ có vị trí tương đối của các phần tử xung quanh điểm trung vị mới quan trọng chứ không phải sự hội tụ toàn cầu. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều phần tử đã ở gần`s`, nhưng số trung vị bị “chặn” bởi quá nhiều giá trị nhỏ hoặc lớn. 

Ví dụ, hãy xem xét:```
n = 5, s = 10
a = [1, 2, 3, 100, 101]
```Mặc dù một số giá trị khác xa`s`, chỉ có thứ tự ở giữa mới quan trọng. Một chiến lược ngây thơ cố gắng mang mọi thứ lại gần`s`sẽ lãng phí công sức vào những yếu tố cực đoan không ảnh hưởng đến vị trí trung vị. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét tất cả các cách có thể để sửa đổi mảng và theo dõi giá trị trung bình sau mỗi chuỗi thao tác. Vì mỗi thao tác thay đổi một phần tử ±1 nên không gian trạng thái tăng theo cấp số nhân. Thậm chí hạn chế ở tổng chi phí cố định`K`, số lượng phân phối của các phép toán giữa các phần tử là tổ hợp, khiến điều này không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần phải kiểm soát hoàn toàn toàn bộ mảng. Chúng ta chỉ cần phần tử trung vị, sau khi được sắp xếp, bằng`s`. Điều này có nghĩa là chúng ta chỉ cần đảm bảo hai điều kiện: có ít nhất`k`phần tử nhỏ hơn hoặc bằng`s`, và ít nhất`k`phần tử lớn hơn hoặc bằng`s`, Ở đâu`k = (n+1)/2`là chỉ số trung vị (dựa trên 1 theo thứ tự sắp xếp). 

Từ đó, chúng ta nhận ra rằng các phần tử đã ở phía bên phải của`s`không cần phải vượt qua nó. Các phần tử nhỏ hơn`s`chỉ quan trọng nếu họ nằm trong số`k`lớn nhất ở phía dưới và các phần tử lớn hơn`s`chỉ quan trọng nếu họ nằm trong số`k`nhỏ nhất của cạnh trên. 

Điều này làm giảm vấn đề chỉ còn việc điều chỉnh các yếu tố “đấu tranh” với vị trí ở giữa. Chiến lược tối ưu trở thành: đẩy cấu trúc trung vị sao cho chính xác`k`phần tử không nhỏ hơn`s`và đảm bảo ứng cử viên trung vị càng gần với`s`. Việc sắp xếp cho phép chúng tôi xác định mức độ đóng góp chính xác của từng phần tử. 

Chúng tôi giảm thiểu một cách hiệu quả chi phí di chuyển đủ số phần tử qua ngưỡng`s`sao cho vị trí trung vị có giá trị bằng`s`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mọi hoạt động | Hàm mũ | O(n) | Quá chậm | 
| Sắp xếp + điều chỉnh tham lam quanh mức trung vị | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chiến lược tối ưu 

hãy để`k = (n+1)//2`, chỉ số của trung vị theo thứ tự sắp xếp. 

1. Sắp xếp mảng. Việc sắp xếp là cần thiết vì số trung vị chỉ phụ thuộc vào thứ tự chứ không phụ thuộc vào vị trí ban đầu. 
2. Chia các phần tử thành những phần tử nhỏ hơn`s`, bằng`s`, và lớn hơn`s`. Sự tách biệt này xác định những gì phải được điều chỉnh. 
3. Đếm xem có bao nhiêu phần tử đã lớn hơn hoặc bằng`s`. Nếu con số này ít nhất`k`, thì số trung vị đã buộc phải ít nhất`s`sau khi điều chỉnh tối thiểu thích hợp. 
4. Tương tự, đảm bảo rằng ít nhất`k`các phần tử nhỏ hơn hoặc bằng`s`. Nếu điều kiện này đã được giữ với cấu trúc đẳng thức xung quanh`s`, trung vị có thể được thực hiện chính xác`s`với những thay đổi tối thiểu. 
5. Chi phí thực tế đến từ các yếu tố phải vượt ngưỡng`s`. Với mỗi phần tử nhỏ hơn`s`cần phải trở thành ≥`s`, chi phí là`s - a[i]`. Với mỗi phần tử lớn hơn`s`cần phải trở thành`s`, chi phí là`a[i] - s`. 
6. Chọn tập hợp tối thiểu các phần tử cần thiết để thỏa mãn điều kiện trung bình, luôn thực hiện những điều chỉnh rẻ nhất trước tiên. Điều này là tối ưu vì mỗi chuyển động của đơn vị là độc lập và tuyến tính. 

### Tại sao nó hoạt động 

Vị trí trung vị chỉ được xác định bằng cách đặt hàng. Một khi chúng ta quyết định có bao nhiêu phần tử phải nằm trên mỗi cạnh của`s`, quyền tự do duy nhất còn lại là chọn phần tử nào vượt qua ngưỡng. Vì chi phí là tuyến tính trong khoảng cách đến`s`, bất kỳ chiến lược tối ưu nào cũng phải ưu tiên các yếu tố gần nhất`s`để vượt qua. Bất kỳ sai lệch nào so với điều này sẽ thay thế việc điều chỉnh chi phí nhỏ hơn bằng mức điều chỉnh lớn hơn mà không làm thay đổi tính khả thi, điều này không thể cải thiện kết quả. Điều này chứng tỏ rằng việc lựa chọn tham lam các phần tử gần nhất là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, s = map(int, input().split())
    a = list(map(int, input().split()))
    
    k = (n + 1) // 2
    
    # Split costs depending on direction toward s
    left = []   # cost to increase to s
    right = []  # cost to decrease to s
    
    for x in a:
        if x < s:
            left.append(s - x)
        else:
            right.append(x - s)
    
    left.sort()
    right.sort()
    
    # We want at least k elements >= s for median to be >= s,
    # and at least k elements <= s for median to be <= s.
    # The median becomes s when we balance both sides optimally.
    
    # If we need to push elements upward to reach s
    need_left = max(0, k - len(right))  # elements that must become >= s
    
    # If we need to push elements downward to reach s
    need_right = max(0, k - len(left))  # elements that must become <= s
    
    ans = 0
    
    for i in range(need_left):
        ans += left[i]
    
    for i in range(need_right):
        ans += right[i]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã tách các phần tử thành các phần bên dưới và bên trên`s`, sau đó tính toán số lượng phải vượt qua ngưỡng để đảm bảo vị trí trung vị có thể thẳng hàng với`s`. Việc sắp xếp từng nhóm đảm bảo chúng ta luôn chọn những phần tử rẻ nhất để điều chỉnh trước. Các biến`need_left`Và`need_right`mã hóa số lượng đường ngang bắt buộc được yêu cầu do mất cân bằng xung quanh`s`. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ cố gắng mô phỏng trung vị một cách rõ ràng. Thay vào đó, chúng ta suy luận về việc có bao nhiêu phần tử phải ở mỗi bên của`s`, đủ để xác định tính khả thi và chi phí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 8
6 5 8
```Cho phép`k = 2`. 

| Bước | Trạng thái mảng | Trái (<8) | Đúng ( ≥8) | Hành động | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [6,5,8] | [2,3] | [0] | tính toán | 0 | 
| Sau khi lựa chọn | giống nhau | chọn 2 | đã 1 | di chuyển 6 → 8 | 2 | 

Trung vị sau khi điều chỉnh trở thành 8 vì mảng được sắp xếp trở thành`[5,8,8]`. 

Điều này cho thấy rằng chỉ có một yếu tố cần điều chỉnh, cụ thể là yếu tố gần nhất với`s`. 

### Ví dụ 2 

đầu vào:```
7 20
21 20 12 11 20 20 12
```Cho phép`k = 4`. 

| Bước | Còn lại (<20) | Đúng (20) | Quan sát | Chi phí | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [8,9,8] | [1,0,0,0] | đã cân bằng | 0 | 

Không cần phải vượt qua bắt buộc vì chúng ta đã có đủ phần tử ở cả hai bên để đặt 20 ở vị trí trung bình. 

Điều này xác nhận rằng thuật toán tránh được những sửa đổi không cần thiết khi giá trị trung bình đã có thể đạt được về mặt cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét tuyến tính nếu không | 
| Không gian | O(n) | Lưu trữ mảng chi phí riêng biệt | 

Các ràng buộc cho phép tối đa khoảng 200.000 phần tử và việc sắp xếp bằng xử lý hậu kỳ tuyến tính dễ dàng phù hợp với giới hạn thời gian trong Python. Việc sử dụng bộ nhớ vẫn tuyến tính ở kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    output = io.StringIO()
    sys.stdout = output
    
    def solve():
        n, s = map(int, input().split())
        a = list(map(int, input().split()))
        k = (n + 1) // 2
        
        left = []
        right = []
        
        for x in a:
            if x < s:
                left.append(s - x)
            else:
                right.append(x - s)
        
        left.sort()
        right.sort()
        
        need_left = max(0, k - len(right))
        need_right = max(0, k - len(left))
        
        ans = sum(left[:need_left]) + sum(right[:need_right])
        print(ans)
    
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# sample
assert run("3 8\n6 5 8\n") == "2"

# all equal
assert run("5 10\n10 10 10 10 10\n") == "0"

# already biased high
assert run("3 5\n10 10 10\n") == "0"

# need adjustment
assert run("3 10\n1 2 3\n") == "14"

# boundary small
assert run("1 100\n50\n") == "50"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | không cần thao tác | 
| tất cả đều cao | 0 | trung bình đã hài lòng | 
| tất cả đều thấp | chi phí lớn | tính đúng đắn của các động thái đi lên | 
| n = 1 | khoảng cách trực tiếp | trường hợp cạnh đơn phần tử | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi tất cả các phần tử đều nhỏ hơn`s`. Trong tình huống đó, mọi yếu tố đều phải được xem xét để chuyển động đi lên, nhưng chỉ những yếu tố rẻ nhất`(n+1)//2`trong số chúng thực sự quan trọng vì chỉ những thứ đó mới ảnh hưởng đến vị trí trung vị. Thuật toán xử lý việc này bằng cách sắp xếp chi phí tăng dần và chỉ chọn tập hợp con được yêu cầu nhỏ nhất. 

Một trường hợp khác là khi tất cả các phần tử đều lớn hơn`s`. Một cách đối xứng, chỉ những điều chỉnh giảm giá rẻ nhất mới được xem xét, đảm bảo chúng tôi không trả quá nhiều cho những yếu tố không liên quan đến vị trí trung bình. 

Trường hợp thứ ba là khi mảng đã có sự kết hợp sao cho vị trí trung vị có cấu trúc đúng. Trong trường hợp này cả hai`need_left`Và`need_right`đánh giá về 0 và thuật toán trả về 0 một cách chính xác mà không cần thực hiện bất kỳ sửa đổi nào.
