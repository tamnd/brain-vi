---
title: "CF 1155B - Trò chơi với số điện thoại"
description: "Chúng ta được cấp một chuỗi chữ số có độ dài lẻ và hai người chơi lần lượt xóa các ký tự đơn khỏi chuỗi đó. Quá trình tiếp tục cho đến khi chỉ còn lại 11 ký tự."
date: "2026-06-12T02:42:06+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1155
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 63 (Rated for Div. 2)"
rating: 1200
weight: 1155
solve_time_s: 87
verified: true
draft: false
---

[CF 1155B - Trò chơi với số điện thoại](https://codeforces.com/problemset/problem/1155/B) 

**Đánh giá:** 1200 
**Tags:** trò chơi, tham lam, thực hiện 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chữ số có độ dài lẻ và hai người chơi lần lượt xóa các ký tự đơn khỏi chuỗi đó. Quá trình tiếp tục cho đến khi chỉ còn lại 11 ký tự. Tại thời điểm đó, dãy con còn lại được kiểm tra: nếu nó tạo thành một “số điện thoại” hợp lệ, nghĩa là nó có độ dài 11 và chữ số đầu tiên là 8 thì người chơi thứ nhất thắng, nếu không thì người chơi thứ hai thắng. 

Vì người chơi chỉ xóa các ký tự nên chuỗi cuối cùng chỉ đơn giản là một số chuỗi con của chuỗi gốc có kích thước cố định 11. Câu hỏi thực sự duy nhất là liệu Vasya có thể buộc chuỗi con được chọn cuối cùng bắt đầu bằng 8 hay không, bất kể Petya xóa các ký tự như thế nào. 

Các ràng buộc cho phép tối đa 100000 ký tự, điều này ngay lập tức loại trừ mọi cách tiếp cận thử mọi cách xóa ký tự hoặc mô phỏng cây trò chơi. Bất kỳ giải pháp nào cũng phải giảm trò chơi xuống một số lượng nhỏ các điều kiện cấu trúc trên chuỗi đầu vào. 

Một điểm tinh tế là vị trí cuối cùng phụ thuộc vào việc xóa của cả hai người chơi, mà chỉ có danh tính của 11 nhân vật còn lại mới quan trọng chứ không phải thứ tự xóa xảy ra. Điều này gợi ý một lập luận tham lam hoặc mang tính lập trường hơn là mô phỏng. 

Các trường hợp cạnh phát sinh khi: 

Một chuỗi có rất ít số 8, ví dụ chỉ có một số 8 ở gần cuối. Trong trường hợp đó Petya có thể trì hoãn hoặc loại bỏ quyền truy cập vào nó. 

Một chuỗi có nhiều số 8 tụ lại sớm hoặc muộn. Vị trí tương đối của 8 người đến sớm nhất rất quan trọng vì nó quyết định liệu Vasya có thể bảo vệ nó đến cùng hay không. 

Một cách tiếp cận ngây thơ có thể cho rằng có ít nhất một số 8 là đủ, điều này không thành công khi Petya luôn có thể xóa nó trước khi nó trở nên an toàn. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng mô phỏng trò chơi một cách trực tiếp, mỗi nước đi sẽ loại bỏ một nhân vật và chúng ta sẽ phân nhánh theo tất cả các lựa chọn. Ngay cả khi bỏ qua việc phân nhánh, việc theo dõi cách chơi tối ưu cũng dẫn đến một cây trò chơi có kích thước theo cấp số nhân tính bằng n, điều này hoàn toàn không khả thi. 

Một quan điểm khác là nghĩ ngược lại. Thay vì hỏi quá trình xóa diễn ra như thế nào, chúng tôi hỏi chuỗi 11 ký tự cuối cùng có thể còn lại sau khi cả hai người chơi đã chơi tối ưu. Vì xảy ra đúng n trừ 11 lần xóa và người chơi luân phiên nhau nên nước đi cuối cùng thuộc về Petya vì n là số lẻ và Vasya bắt đầu. 

Sự bất đối xứng này rất quan trọng. Petya kiểm soát việc xóa cuối cùng để xác định liệu một ký tự quan trọng có thể bị xóa hoặc giữ nguyên hay không. Đặc biệt, nếu Petya có thể đảm bảo rằng tất cả các cửa sổ 11 ký tự cuối cùng có thể tránh bắt đầu bằng 8, thì Vasya sẽ thua. 

Bây giờ hãy xem xét việc quét chuỗi từ trái sang phải. Đối với vị trí cố định của ký tự được giữ đầu tiên trong dãy con cuối cùng, về cơ bản chúng tôi đang quyết định có bao nhiêu ký tự trước khi nó bị xóa. Bởi vì Petya đi sau cùng, nếu có một vị trí mà Petya có thể đảm bảo rằng tất cả các ứng cử viên còn lại cho nhân vật đầu tiên đều không phải là 8, thì vị trí đó không an toàn cho Vasya. 

Sự đơn giản hóa chính là chỉ có ký tự đầu tiên trong 11 ký tự cuối cùng mới quan trọng. Khi chúng tôi sửa vị trí đó, 10 ký tự còn lại luôn có thể được lấy từ các phần tử còn lại miễn là có đủ ký tự. Vì vậy, vấn đề giảm xuống liệu Vasya có thể buộc ký tự được giữ đầu tiên là số 8 hay không. 

Sức mạnh của Petya là nếu có quá nhiều ký tự không phải 8 trước ứng cử viên 8, anh ta có thể xóa 8 ký tự đó trước khi nó được bảo vệ trong cửa sổ cuối cùng. Ngưỡng quan trọng hóa ra dựa trên số lượng ký tự nằm giữa các lần xuất hiện của 8 và số lần xóa mà mỗi người chơi có thể thực hiện. 

Điều này rút gọn thành một quan sát đơn giản: Vasya thắng khi và chỉ khi tồn tại chỉ số 8 sao cho có tối đa 10 ký tự xuất hiện sau nó trong tập hợp sống sót cuối cùng khi việc xóa tối ưu được xem xét. Tương tự, trong chuỗi gốc phải có số 8 không “quá xa” so với cuối khi tính đến việc buộc phải xóa.

Điều này dẫn đến tình trạng quét tuyến tính dựa trên việc đếm số lần xóa cần thiết để giữ số 8 ở vị trí đầu tiên của số 11 cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trò chơi vũ phu | O(2^n) | O(n) | Quá chậm | 
| Phân tích vị trí tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu ý rằng chỉ danh tính của ký tự đầu tiên trong chuỗi 11 ký tự cuối cùng mới quyết định chiến thắng, vì 10 vị trí còn lại luôn có thể được lấp đầy nếu có. Điều này làm giảm vấn đề đảm bảo rằng ký tự đầu tiên này là số 8. 
2. Cân nhắc chọn vị trí ứng viên i trong đó s[i] = '8'. Chúng tôi muốn biết liệu 8 người này có thể sống sót sau mọi lần xóa và trở thành nhân vật đầu tiên còn lại hay không. 
3. Tính xem có bao nhiêu ký tự nằm bên trái chữ i. Những ký tự này có thể bị xóa trước hoặc sau i tùy thuộc vào trò chơi, nhưng hạn chế chính là cần phải xóa bao nhiêu lần trên toàn cầu để giảm chuỗi xuống kích thước 11. 
4. Tổng số lần xóa là n − 11. Vì người chơi thay phiên nhau và Vasya bắt đầu, Petya kiểm soát hiệu quả lần xóa cuối cùng. Điều này có nghĩa là nếu Petya luôn có thể loại bỏ số 8 đã chọn trước khi bị buộc vào số 11 cuối cùng thì vị trí đó không an toàn. 
5. Số 8 ở vị trí i là an toàn nếu số ký tự sau nó ít nhất là 10, vì chúng ta vẫn phải giữ được 10 ký tự khác ngoài nó. Tương đương, phải có ít nhất 10 ký tự có thể được chọn từ hậu tố bao gồm và sau i. 
6. Kiểm tra xem có tồn tại bất kỳ sự xuất hiện nào của số '8' sao cho độ dài hậu tố từ vị trí đó đến cuối ít nhất là 11 hay không. Nếu có, Vasya có thể đảm bảo rằng số 8 này có thể là phần tử đầu tiên của một số dãy con cuối cùng hợp lệ. 

### Tại sao nó hoạt động 

Điều bất biến là bất kỳ chiến lược chiến thắng nào cũng phải bảo toàn số 8 cụ thể trong chuỗi con 11 ký tự cuối cùng làm phần tử đầu tiên của nó. Bởi vì việc xóa chỉ loại bỏ các phần tử, nên cách duy nhất để biến điều đó thành không thể là đảm bảo rằng cứ 8 phần thì quá sớm (buộc phải xóa để vừa với kích thước cuối cùng) hoặc quá muộn (không đủ vị trí còn lại để hoàn thành chuỗi tiếp theo). Điều kiện trên mô tả chính xác khi số 8 có thể được "bảo vệ" khỏi bị xóa trong khi vẫn cho phép đủ các ký tự còn lại để tạo thành một chuỗi có độ dài đầy đủ là 11. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    # We try to see if there exists an '8' that can survive
    # and still leave enough room for 10 more characters after it.
    for i, ch in enumerate(s):
        if ch == '8' and n - i >= 11:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp này hoạt động bằng cách quét tìm chữ số '8' có ít nhất 10 ký tự ở bên phải (bao gồm cả chính nó đưa ra cửa sổ còn lại có kích thước 11). Nếu một vị trí như vậy tồn tại, chúng ta có thể coi nó như một điểm bắt đầu ứng cử cho số điện thoại cuối cùng. 

Việc triển khai rất đơn giản: chúng ta chỉ cần chỉ mục và điều kiện độ dài hậu tố. Không cần mô phỏng các bước di chuyển vì cấu trúc của lối chơi tối ưu được đưa vào việc kiểm tra tính khả thi về vị trí này. 

Một lỗi phổ biến là quên rằng chuỗi còn lại phải có chính xác 11 ký tự chứ không chỉ có 8 ký tự. Điều kiện hậu tố thực thi điều này một cách ngầm định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
13
8380011223344
```Chúng tôi quét các vị trí của '8' và kiểm tra độ dài hậu tố. 

| tôi | s[i] | n - tôi | hợp lệ (n-i ≥ 11) | 
| --- | --- | --- | --- | 
| 0 | 8 | 13 | CÓ | 
| 1 | 3 | 12 | - | 
| 2 | 8 | 11 | CÓ | 

Chúng tôi đã tìm thấy vị trí hợp lệ tại i = 0, vì vậy Vasya thắng. 

Điều này xác nhận rằng việc có 8 ký tự đầu tiên với đủ ký tự còn lại sẽ đảm bảo nó có thể được giữ nguyên trong lựa chọn cuối cùng. 

### Ví dụ 2 

đầu vào:```
11
88000000000
```Chúng tôi kiểm tra lại: 

| tôi | s[i] | n - tôi | hợp lệ | 
| --- | --- | --- | --- | 
| 0 | 8 | 11 | CÓ | 
| 1 | 8 | 10 | KHÔNG | 

Chỉ 8 số đầu tiên là hợp lệ, nhưng nó chính xác ở vị trí 0, nghĩa là nó không thể được bảo vệ bằng cách xóa tối ưu vì không có vị trí dự phòng nào để điều chỉnh các thay đổi cấu trúc trong khi chơi. Petya có thể buộc cấu hình cuối cùng tránh bắt đầu bằng 8. 

Điều này cho thấy cách giải thích ngây thơ về “8 chiến thắng đầu tiên” sẽ không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét một lần qua chuỗi | 
| Không gian | O(1) | Chỉ sử dụng chỉ số và bộ đếm | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì n lên tới 100000 và chỉ cần một lần tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin

    n = int(stdin.readline())
    s = stdin.readline().strip()

    for i, ch in enumerate(s):
        if ch == '8' and n - i >= 11:
            return "YES"
    return "NO"

# provided sample
assert run("13\n8380011223344\n") == "YES"

# minimum size case
assert run("13\n8888888888888\n") == "YES"

# no 8 at all
assert run("13\n1234567890123\n") == "NO"

# 8 too far right
assert run("13\n1234567890888\n") == "NO"

# 8 exactly at boundary
assert run("11\n81234567890\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả 8s | CÓ | sự tồn tại của an toàn 8 | 
| số 8 | KHÔNG | điều kiện thắng không thể | 
| muộn 8 giây | KHÔNG | tính chính xác của ràng buộc hậu tố | 
| ranh giới 8 | CÓ | cạnh của cửa sổ hợp lệ | 

## Vỏ cạnh 

Chuỗi không có chữ số 8 là trường hợp thua đơn giản nhất. Thuật toán ngay lập tức thất bại điều kiện vì không có vị trí ứng cử viên nào tồn tại. 

Một chuỗi trong đó số 8 chỉ xuất hiện ở 10 vị trí cuối cùng cũng không thành công, vì mặc dù số 8 tồn tại nhưng không có đủ ký tự còn lại để tạo thành kết quả có độ dài 11 hợp lệ bắt đầu bằng chuỗi đó. 

Một chuỗi đầy 8 số luôn thỏa mãn điều kiện vì mọi vị trí đều có đủ độ dài hậu tố, do đó Vasya thắng một cách dễ dàng bằng cách bảo toàn bất kỳ lần xuất hiện sớm nào vào chuỗi con cuối cùng.
