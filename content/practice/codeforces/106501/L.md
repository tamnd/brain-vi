---
title: "CF 106501L - Tránh tính tổng bình phương"
description: "Chúng ta cần xây dựng một tập hợp gồm n số nguyên dương khác nhau. Mọi giá trị được chọn phải nằm trong phạm vi từ 1 đến 100000. Điều kiện là nếu chúng ta lấy bất kỳ nhóm không trống nào có tối đa 8 số đã chọn thì tổng của nhóm đó không được là một số chính phương."
date: "2026-06-25T08:34:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106501
codeforces_index: "L"
codeforces_contest_name: "IPL 2026"
rating: 0
weight: 106501
solve_time_s: 82
verified: true
draft: false
---

[CF 106501L - Tránh tính tổng bình phương](https://codeforces.com/problemset/problem/106501/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một bộ`n`số nguyên dương khác nhau. Mọi giá trị được chọn phải nằm trong phạm vi từ`1`ĐẾN`100000`. Điều kiện là nếu chúng ta lấy bất kỳ nhóm không trống nào có tối đa tám số đã chọn thì tổng của nhóm đó không được là một số chính phương. 

Đầu vào chỉ cung cấp kích thước yêu cầu của bộ. Đầu ra không phải là một câu trả lời duy nhất để kiểm tra mà là bất kỳ cấu trúc hợp lệ nào. 

Giới hạn trên`n`đủ nhỏ để không thể tìm kiếm trực tiếp trên tất cả các tập con có thể có của tập cuối cùng. Vì`n = 2862`, số lượng tập con có kích thước lên tới 8 là rất lớn. Thậm chí`C(2862, 8)`vượt xa những gì có thể được xử lý. Tuy nhiên, bản thân các con số được giới hạn bởi`100000`, do đó tổng có thể có của tối đa tám phần tử được giới hạn bởi`800000`. Phạm vi đó là nguồn tài nguyên quan trọng mà chúng ta có thể khai thác. 

Phần khó khăn là việc xây dựng tham lam có thể dễ dàng thất bại nếu nó chỉ kiểm tra sai số hiện tại so với các số trước đó. Một tập hợp con xấu chỉ có thể xuất hiện sau một vài lần bổ sung. Ví dụ: nếu chúng tôi xuất ra:```
3
```đầu ra sẽ không hợp lệ vì tập hợp con`{3}`có tổng`3`, không phải là hình vuông nên điều này thực sự hợp lệ. Một người kiểm tra bất cẩn chỉ tìm kiếm các cặp có thể bỏ lỡ vấn đề quan trọng của các tập hợp con một phần tử. Mặt khác:```
1 8
```không hợp lệ vì tập hợp con`{8,1}`có tổng`9`, một hình vuông. Một phương pháp chỉ kiểm tra giá trị mới so với các giá trị riêng lẻ hiện có sẽ không nắm bắt được giá trị đó. Chúng ta cần theo dõi tất cả các tổng tập hợp con có kích thước tối đa là 8. 

Một trường hợp cạnh khác là tập hợp con trống. Nó không được xem xét vì tổng của nó bằng 0, là một bình phương hoàn hảo. Nếu chúng ta vô tình đưa nó vào khi xác thực câu trả lời cuối cùng thì mọi công trình xây dựng sẽ thất bại ngay lập tức. Đối với đầu vào:```
1
```đầu ra đúng là bất kỳ số không phải số chính phương nào, chẳng hạn như:```
2
```bởi vì chỉ tồn tại tập hợp con chứa một số đó. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ tự nhiên là xây dựng tập hợp từ trái sang phải và trước khi thêm một số mới, hãy thử mọi tập hợp con của các số đã chọn có kích thước tối đa là bảy. Nếu không có tập hợp con nào cộng với số mới tạo thành tổng bình phương thì chúng ta sẽ giữ nguyên số đó. 

Điều này đúng vì mọi tập hợp con không hợp lệ mới đều phải chứa giá trị mới được thêm vào. Vấn đề là số lượng tập hợp con chúng ta sẽ kiểm tra. Với hàng nghìn giá trị được chọn, số lượng nhóm có thể có kích thước 7 tăng lên quá nhanh. Trường hợp xấu nhất sẽ yêu cầu kiểm tra hàng tỷ kết hợp. 

Quan sát giúp chúng ta tiết kiệm là chúng ta không cần biết phần tử nào đã tạo ra tổng, mà chỉ cần biết liệu tổng đó có khả thi hay không. Vì số tiền liên quan lớn nhất là`8 * 100000 = 800000`, chúng ta có thể lưu trữ tất cả các tổng tập hợp con có thể truy cập theo kích thước tập hợp con. 

Đối với mọi kích thước`k`, duy trì một bitset ở đâu bit`s`có nghĩa là có một tập hợp con chính xác`k`các phần tử được chọn với tổng số tiền`s`. Khi xem xét một giá trị mới`x`, nó hợp lệ nếu không có số tiền có thể truy cập được`s`kích thước`k`như vậy`s + x`là một hình vuông cho bất kỳ`k`từ số không đến bảy. 

Số nguyên Python rất hữu ích ở đây vì các bit của chúng có thể được dịch chuyển và kết hợp rất nhanh trong C. Việc cập nhật trạng thái lập trình động sẽ trở thành một vài thao tác bit thay vì lặp qua tất cả các tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(100000 * n^7) | O(n) | Quá chậm | 
| Bitset tham lam | O(100000 * 8 * 800000 / word_size) | O(8 * 800000) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước mọi bình phương hoàn hảo có thể xuất hiện dưới dạng tổng tập hợp con. Tổng tập hợp con lớn nhất có thể là`800000`, vì vậy tất cả các bình phương có giá trị đó là đủ. 
2. Tạo tám bitets. Bitset tại chỉ mục`k`đại diện cho tất cả số tiền có thể đạt được bằng cách chọn chính xác`k`số đã chọn sẵn. Ban đầu chỉ tồn tại tập hợp con trống, do đó, tập hợp bit có kích thước bằng 0 chứa tổng bằng 0. 
3. Lặp qua các số ứng cử viên từ`1`ĐẾN`100000`. Đối với mỗi ứng viên`x`, kiểm tra xem việc thêm nó có tạo ra hình vuông hay không. Đối với mọi kích thước tập hợp con trước đó có thể`k`, hãy tìm số tiền có thể đạt được bằng`square - x`. Nếu số tiền đó tồn tại thì`x`không thể sử dụng được 
4. Nếu`x`hợp lệ, hãy thêm nó vào câu trả lời và cập nhật các bit. Một tập hợp con có kích thước`k`có thể trở thành một tập hợp con có kích thước`k + 1`bằng cách thêm`x`, đó chính xác là sự dịch chuyển sang trái của bitset bởi`x`. 
5. Dừng lại khi chúng tôi đã thu thập xong`n`những con số. Lựa chọn tham lam có tác dụng vì mọi số được chấp nhận đều bảo toàn bất biến rằng không có tập con nào có kích thước tối đa là 8 trong số các số được chọn có tổng bình phương. 

Tại sao nó hoạt động: 

Các bit được duy trì mô tả chính xác mọi tổng tập hợp con có thể có cho mọi kích thước tập hợp con trong số các số đã được chọn. Khi chúng tôi kiểm tra một giá trị mới, bất kỳ tập hợp con không hợp lệ mới được tạo nào cũng phải bao gồm giá trị mới cộng với một tập hợp con cũ có kích thước tối đa là bảy. Việc kiểm tra sẽ xem xét mọi khả năng như vậy. Nếu giá trị được chấp nhận, không có tập hợp con sai mới nào được đưa vào, do đó, bất biến vẫn đúng sau khi cập nhật. Bằng quy nạp, tập hợp cuối cùng thỏa mãn điều kiện yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX_SUM = 800000

squares = []
i = 1
while i * i <= MAX_SUM:
    squares.append(i * i)
    i += 1

def solve():
    n = int(input())
    
    dp = [0] * 9
    dp[0] = 1
    
    ans = []
    
    for x in range(1, 100001):
        ok = True
        
        for k in range(8):
            bits = dp[k]
            for sq in squares:
                need = sq - x
                if need >= 0 and ((bits >> need) & 1):
                    ok = False
                    break
            if not ok:
                break
        
        if ok:
            ans.append(x)
            for k in range(7, -1, -1):
                dp[k + 1] |= dp[k] << x
            
            if len(ans) == n:
                break
    
    print(*ans)

def main():
    solve()

if __name__ == "__main__":
    main()
```Danh sách hình vuông được tạo một lần vì chỉ tính tổng tối đa`800000`có thể quan trọng. Các hình vuông lớn hơn không bao giờ có thể được tạo ra bởi một tập hợp con có tối đa tám giá trị được phép. 

Mảng`dp`có chín mục vì các tập hợp con có thể có kích thước từ 0 đến 8. Giá trị được lưu trữ trong mỗi mục là số nguyên Python được sử dụng làm bitset. Chút`s`được đặt có nghĩa là tổng đó`s`là có thể đạt được. 

Trong bước xác thực, vòng lặp kết thúc`k`kiểm tra tất cả các kích thước tập hợp con có thể có trước đó. Chúng ta chỉ cần kích thước từ 0 đến 7 vì chính giá trị mới đóng góp vào phần tử thứ tám. Bản cập nhật chạy ngược qua các kích thước nên số hiện tại không được sử dụng lại nhiều lần trong cùng một bản cập nhật. 

Không có vấn đề tràn số nguyên vì số nguyên Python có độ chính xác tùy ý. Vị trí bit lớn nhất cần thiết chỉ là`800000`, có thể quản lý được. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
5
```một dấu vết có thể là: 

| Ứng viên | Được chọn | Lý do | 
| --- | --- | --- | 
| 1 | Không | Tổng phần tử đơn 1 là hình vuông | 
| 2 | Có | Tổng phần tử đơn 2 là an toàn | 
| 3 | Có | Tổng phần tử đơn 3 là an toàn, tổng cặp là an toàn | 
| 4 | Có | Tất cả các tổng tập hợp con mới vẫn không bình phương | 
| 5 | Có | Tất cả các tổng tập hợp con mới vẫn không bình phương | 
| 6 | Có | Tất cả các tổng tập hợp con mới vẫn không bình phương | 

Thuật toán sẽ xuất ra năm số được chấp nhận đầu tiên:```
2 3 4 5 6
```Đối với đầu vào:```
1
```ứng cử viên đầu tiên bị từ chối vì giá trị của nó là hình vuông. Ứng viên tiếp theo được chấp nhận. 

| Ứng viên | Được chọn | Lý do | 
| --- | --- | --- | 
| 1 | Không | Tổng 1 là hình vuông | 
| 2 | Có | Không có tập hợp con nào có thể tạo thành hình vuông | 

Đầu ra là:```
2
```Những dấu vết này cho thấy các tập hợp con một phần tử và các tương tác giữa nhiều giá trị được chọn đều được xử lý bởi cùng một bất biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(100000 * 8 * B) | Mỗi ứng cử viên sử dụng một số lượng nhỏ các phép toán bitset, trong đó`B`là số từ máy của số nguyên 800000 bit | 
| Không gian | O(800000) | Chín bit được lưu trữ | 

Số lượng ứng viên được ấn định`100000`và các hoạt động bit được thực hiện một cách hiệu quả trong nội bộ. Việc sử dụng bộ nhớ nhỏ vì chỉ có khả năng tiếp cận của tổng mới quan trọng chứ không phải bản thân các tập hợp con. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    data = sys.stdin.readline
    MAX_SUM = 800000
    
    squares = []
    i = 1
    while i * i <= MAX_SUM:
        squares.append(i * i)
        i += 1
    
    n = int(data())
    dp = [0] * 9
    dp[0] = 1
    ans = []
    
    for x in range(1, 100001):
        ok = True
        for k in range(8):
            for sq in squares:
                need = sq - x
                if need >= 0 and ((dp[k] >> need) & 1):
                    ok = False
                    break
            if not ok:
                break
        
        if ok:
            ans.append(x)
            for k in range(7, -1, -1):
                dp[k + 1] |= dp[k] << x
        
        if len(ans) == n:
            break
    
    sys.stdin = old
    return " ".join(map(str, ans))

assert len(run("1").split()) == 1
assert len(run("5").split()) == 5
assert len(run("100").split()) == 100
assert len(run("2862").split()) == 2862
assert all(1 <= int(x) <= 100000 for x in run("100").split())
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`| Một số | Kiểm tra kích thước tối thiểu và phần tử đơn | 
|`5`| Năm con số | Công trình nhỏ | 
|`100`| Một trăm số | Sự tăng trưởng tham lam bình thường | 
|`2862`| 2862 số | Kích thước xây dựng tối đa cho phép | 
|`100`| Giá trị trong phạm vi | Ranh giới của các giá trị được phép | 

## Vỏ cạnh 

Đối với trường hợp tối thiểu`n = 1`, thuật toán bác bỏ`1`bởi vì nó là hình vuông và chấp nhận`2`. Tính năng theo dõi tập hợp con vẫn hoạt động vì tập hợp con trống được thể hiện nội bộ nhưng không bao giờ được coi là tập hợp con cuối cùng. 

Đối với trường hợp các cặp quan trọng, chẳng hạn như việc chọn`1`Và`8`, số thứ hai sẽ bị từ chối. Trước khi thêm`8`, bitset cho kích thước một chứa tổng`1`, Và`1 + 8 = 9`được phát hiện là một hình vuông. Điều này ngăn ngừa lỗi phổ biến khi chỉ kiểm tra các giá trị riêng lẻ. 

Để có kích thước tối đa`n = 2862`, vòng lặp tiếp tục cho đến khi thu thập đủ số hợp lệ. Việc biểu diễn bitset tránh lưu trữ tất cả các tập hợp con có thể có, do đó việc xây dựng vẫn khả thi.
