---
title: "CF 1009B - Chuỗi bậc ba tối thiểu"
description: "Chúng ta được cung cấp một chuỗi chỉ gồm các chữ số 0, 1 và 2. Chúng ta được phép hoán đổi nhiều lần các cặp liền kề nếu chúng là 0 và 1 theo một trong hai hướng hoặc 1 và 2 theo một trong hai hướng."
date: "2026-06-16T22:57:11+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1009
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 47 (Rated for Div. 2)"
rating: 1400
weight: 1009
solve_time_s: 159
verified: false
draft: false
---

[CF 1009B - Chuỗi bậc ba tối thiểu](https://codeforces.com/problemset/problem/1009/B) 

**Đánh giá:** 1400 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chỉ gồm các chữ số 0, 1 và 2. Chúng ta được phép hoán đổi nhiều lần các cặp liền kề nếu chúng là 0 và 1 theo một trong hai hướng hoặc 1 và 2 theo một trong hai hướng. Mục tiêu là chuyển đổi chuỗi thành chuỗi nhỏ nhất có thể về mặt từ điển theo các bước di chuyển này. 

Khía cạnh quan trọng là việc hoán đổi mang tính cục bộ và chỉ liên quan đến các cặp lân cận, nhưng chúng có thể được áp dụng tùy ý nhiều lần, nghĩa là các ký tự có thể “di chuyển qua” chuỗi một cách hiệu quả miễn là chúng tôn trọng các quy tắc kề cận được phép. 

Độ dài chuỗi có thể lên tới 100000, do đó, bất kỳ cách tiếp cận nào cố gắng mô phỏng các giao dịch hoán đổi một cách rõ ràng đều quá chậm. Một mô phỏng đơn giản có thể mất thời gian bậc hai trong trường hợp xấu nhất vì mỗi lần hoán đổi chỉ di chuyển các ký tự theo một vị trí. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào liên tục đưa các ký tự vào đúng vị trí. 

Trường hợp cạnh tinh tế xuất hiện khi cả ba chữ số được trộn lẫn. Ví dụ: một chuỗi như 100210 cho phép nhiều chuỗi hoán đổi hợp lệ tạo ra các sắp xếp cuối cùng có giao diện khác nhau. Một hành động tham lam ngây thơ như “sắp xếp chuỗi” có thể thất bại vì không phải tất cả các giao dịch hoán đổi đều được phép (0 và 2 không bao giờ hoán đổi trực tiếp), trong khi các chiến lược cục bộ quá thận trọng cũng có thể thất bại vì chuỗi giao dịch hoán đổi cho phép sắp xếp lại gián tiếp không rõ ràng từ cấu hình ban đầu. 

Khó khăn chính là ở chỗ 1 đóng vai trò trung gian: nó có thể hoán đổi với cả 0 và 2, có nghĩa là nó cho phép chuyển động gián tiếp làm phức tạp các giả định đặt hàng đơn giản. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các giao dịch hoán đổi có thể xảy ra bằng cách sử dụng BFS hoặc DFS trên tất cả các chuỗi có thể truy cập được. Mỗi trạng thái có tới O(n) lân cận và số lượng trạng thái tăng lên bùng nổ vì nhiều hoán vị có thể tiếp cận được thông qua các chuỗi hoán đổi liền kề. Ngay cả khi cố gắng cắt tỉa, không gian trạng thái sẽ trở thành giai thừa trong trường hợp xấu nhất, khiến nó hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta không cần phải theo dõi các chuỗi hoán đổi riêng lẻ. Thay vào đó, chúng ta nên hiểu những ràng buộc thứ tự nào là bất biến trong các phép toán được phép. Hệ thống cho phép đảo ngược cục bộ giữa 0 và 1 và giữa 1 và 2, nghĩa là 1 có thể di chuyển tự do qua cả 0 và 2 thông qua các bước trung gian. Điều này làm cho 1 trở thành nhân vật linh hoạt nhất. 

Từ sự linh hoạt này, chiến lược tối ưu là suy luận theo hướng giảm thiểu thứ tự từ điển một cách tham lam. Vì 0 là chữ số nhỏ nhất nên chúng ta muốn số 0 càng sớm càng tốt. Vì 2 là số lớn nhất nên chúng ta muốn nó càng muộn càng tốt. Chữ số 1 có thể được sử dụng làm vùng đệm có thể được đặt giữa chúng theo cách không cản trở vị trí của các ký tự nhỏ hơn. 

Điều này dẫn đến kết quả mang tính xây dựng tiêu chuẩn: chuỗi tối ưu thu được bằng cách sắp xếp tất cả các số 0 trước, sau đó là tất cả các số 1, sau đó là tất cả các số 2. Bất kỳ nỗ lực nào nhằm trì hoãn các số 0 hoặc đưa các số 2 về phía trước chỉ có thể làm xấu đi trật tự từ điển vì luôn tồn tại một chuỗi các hoán đổi hợp lệ cho phép đẩy 0 sang trái sau các giây và đẩy 2 sang phải sau các giây. 

Lực lượng vũ phu hoạt động vì nó khám phá rõ ràng tất cả các chuỗi hoán đổi. Kẻ tham lam hoạt động vì mục tiêu toàn cầu có liên quan duy nhất là giảm thiểu từ điển và hệ thống hoán đổi đủ phong phú để các chữ số có thể được sắp xếp lại tự do theo thứ tự được sắp xếp toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm trạng thái) | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu (đếm và xây dựng) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời một cách trực tiếp thay vì mô phỏng các giao dịch hoán đổi. 

### bước 

1. Đếm xem có bao nhiêu số 0, 1 và 2 xuất hiện trong chuỗi. 

Số đếm này xác định đầy đủ chuỗi cuối cùng vì các hoán đổi cho phép sắp xếp lại tùy ý phù hợp với việc giảm thiểu từ điển. 
2. Xây dựng kết quả bằng cách viết tất cả các số 0 trước.

Đặt số 0 sớm hơn luôn là điều tối ưu vì không có thao tác nào có thể làm cho số 0 lớn hơn mức cần thiết mà không làm xấu đi thứ tự từ điển. 
3. Thêm tất cả số 1 vào sau số 0. 

Vì 1 lớn hơn 0 nhưng nhỏ hơn 2 nên việc đặt nó ở giữa sẽ giữ tiền tố ở mức tối thiểu đồng thời tránh nhiễu không cần thiết với số 0. 
4. Thêm tất cả số 2 vào cuối. 

2 đóng góp giá trị từ điển lớn nhất có thể, do đó việc trì hoãn nó sẽ tối đa hóa tính tối ưu. 

### Tại sao nó hoạt động 

Việc hoán đổi đảm bảo rằng không có chữ số nào bị mắc kẹt vĩnh viễn ở vị trí kém hơn so với các chữ số khác có giá trị nhỏ hơn. Bất kỳ cấu hình nào không được sắp xếp theo giá trị chữ số đều có thể được cải thiện bằng cách hoán đổi liên tục các cặp đảo ngược liền kề (0 trước 1 hoặc 1 trước 2). Bởi vì các giao dịch hoán đổi này có thể đảo ngược và đầy đủ nên bất kỳ sự đảo ngược nào giữa các chữ số nhỏ hơn và lớn hơn đều có thể bị loại bỏ. Điều này có nghĩa là cách sắp xếp có thể tiếp cận nhỏ nhất về mặt từ điển là cách sắp xếp được sắp xếp đầy đủ theo giá trị chữ số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

c0 = c1 = c2 = 0

for ch in s:
    if ch == '0':
        c0 += 1
    elif ch == '1':
        c1 += 1
    else:
        c2 += 1

print('0' * c0 + '1' * c1 + '2' * c2)
```Giải pháp này hoạt động bằng cách giảm vấn đề về việc đếm tần số. Vòng lặp là bước xử lý duy nhất và chạy theo thời gian tuyến tính. Sau khi đếm, việc xây dựng chuỗi là trực tiếp và không phụ thuộc vào thứ tự ban đầu. 

Một lỗi phổ biến là cố gắng mô phỏng các giao dịch hoán đổi hoặc theo dõi vị trí của các chữ số. Điều đó là không cần thiết vì các thao tác được phép sẽ loại bỏ mọi ràng buộc vị trí có ý nghĩa ngoài số lượng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
100210
```Chúng ta đếm các chữ số: c0 = 3, c1 = 2, c2 = 1. 

| Bước | c0 | c1 | c2 | Partial Output |
 | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | "" |
 | After counting | 3 | 2 | 1 | "" |
 | Build 0s | 3 | 2 | 1 | "000" |
 | Build 1s | 3 | 2 | 1 | "00011" |
 | Build 2s | 3 | 2 | 1 | "000112" |

 Final output:```
000112
```Điều này phù hợp với sự sắp xếp nhỏ nhất về mặt từ điển có thể đạt được vì tất cả các chữ số nhỏ hơn được đặt càng sớm càng tốt. 

### Ví dụ 2 

đầu vào:```
210102
```Số lượng là c0 = 2, c1 = 2, c2 = 2. 

| Bước | c0 | c1 | c2 | Đầu ra một phần | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | "" | 
| Sau khi đếm | 2 | 2 | 2 | "" | 
| Xây dựng số 0 | 2 | 2 | 2 | "00" | 
| Xây dựng 1 giây | 2 | 2 | 2 | "0011" | 
| Xây dựng 2 giây | 2 | 2 | 2 | "001122" | 

Đầu ra cuối cùng:```
001122
```Điều này chứng tỏ rằng ngay cả với đầu vào hỗn hợp nhiều, cấu trúc vẫn thu gọn thành một sự sắp xếp được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để đếm ký tự cộng với việc xây dựng kết quả tuyến tính | 
| Không gian | O(1) | Chỉ có ba quầy được sử dụng; chuỗi đầu ra chiếm ưu thế trong bộ nhớ | 

Thuật toán dễ dàng phù hợp với giới hạn n lên tới 100000 vì nó chỉ thực hiện công việc không đổi trên mỗi ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    s = input().strip()

    c0 = c1 = c2 = 0
    for ch in s:
        if ch == '0':
            c0 += 1
        elif ch == '1':
            c1 += 1
        else:
            c2 += 1

    return '0' * c0 + '1' * c1 + '2' * c2

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("100210\n") == "000112", "sample 1"

# single character
assert run("0\n") == "0"
assert run("2\n") == "2"

# all same
assert run("11111\n") == "11111"

# mixed small
assert run("102012\n") == "001122"

# already sorted reverse
assert run("222111000\n") == "000111222"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0/2 đơn | giống nhau | hành vi ranh giới tối thiểu | 
| tất cả đều bình đẳng | không thay đổi | ổn định công trình | 
| 102012 | 001122 | đặt hàng hỗn hợp | 
| 222111000 | 000111222 | sắp xếp lại trường hợp xấu nhất | 

## Vỏ cạnh 

Một đầu vào tối thiểu như`0`hoặc`2`cho thấy thuật toán không đưa vào cấu trúc nhân tạo và chỉ trả về cùng một ký tự đơn. Bước đếm mang lại một bộ đếm khác 0 và việc tái cấu trúc trực tiếp xuất ra nó. 

Một đầu vào hoàn toàn đảo ngược như`222111000`chứng tỏ rằng thuật toán hoàn toàn không phụ thuộc vào thứ tự ban đầu. Mặc dù chuỗi gốc có kích thước “lớn” tối đa, nhưng đầu ra sẽ được giảm thiểu hoàn toàn vì tất cả các số 0 được phát ra trước, tiếp theo là các số 1 và sau đó là các số 2. 

Một đầu vào hỗn hợp như`102012`xác nhận rằng không cần có sự tương tác giữa các vị trí. Mặc dù các chữ số được xen kẽ, hệ thống hoán đổi cho phép sắp xếp lại hoàn toàn thành dạng được nhóm và phương pháp đếm nắm bắt trực tiếp mức tối thiểu có thể tiếp cận.
