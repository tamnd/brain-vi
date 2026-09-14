---
title: "CF 104678D – Khám cơ bản"
description: "Chúng ta được cung cấp một chuỗi chỉ bao gồm dấu ngoặc đơn mở và đóng. Nhiệm vụ là quyết định xem chuỗi này có thể phát sinh từ một số biểu thức số học hợp lệ hay không sau khi loại bỏ mọi thứ trừ dấu ngoặc đơn."
date: "2026-06-29T09:06:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104678
codeforces_index: "D"
codeforces_contest_name: "October come back. Together training"
rating: 0
weight: 104678
solve_time_s: 52
verified: true
draft: false
---

[CF 104678D - Kiểm tra cơ bản](https://codeforces.com/problemset/problem/104678/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chỉ bao gồm dấu ngoặc đơn mở và đóng. Nhiệm vụ là quyết định xem chuỗi này có thể phát sinh từ một số biểu thức số học hợp lệ hay không sau khi loại bỏ mọi thứ trừ dấu ngoặc đơn. 

Một quan sát quan trọng là bất kỳ biểu thức số học nào, sau khi loại bỏ các số và toán tử, sẽ chỉ còn lại cấu trúc được áp đặt bởi dấu ngoặc đơn. Cấu trúc đó hợp lệ chính xác khi mọi dấu ngoặc đóng khớp với cấu trúc đã mở trước đó và không có tiền tố nào đóng nhiều dấu ngoặc hơn số đã được mở. 

Vì vậy, đầu vào chỉ đơn giản là một cấu trúc khung ứng cử viên và đầu ra là một quyết định nhị phân: liệu cấu trúc này có thể tương ứng với một biểu thức được định dạng chính xác hay không. 

Ràng buộc n lên tới 200000 ngay lập tức loại trừ mọi mô phỏng bậc hai hoặc quét lại lặp đi lặp lại. Bất kỳ giải pháp nào cố gắng khớp các cặp bằng cách tìm kiếm hoặc sửa đổi chuỗi liên tục sẽ quá chậm. Quét tuyến tính là lựa chọn thực tế duy nhất. 

Một số tình huống nguy hiểm bộc lộ những lỗi phổ biến. 

Nếu chuỗi bắt đầu bằng dấu ngoặc đóng, chẳng hạn như đầu vào`")("`, không thể diễn giải nó đến từ bất kỳ biểu thức hợp lệ nào, bởi vì không có ngữ cảnh nào trước đó có thể cung cấp dấu ngoặc mở phù hợp. Đầu ra đúng là`NO`. 

Nếu chuỗi có tổng số đếm chính xác nhưng thứ tự không hợp lệ, chẳng hạn như`"())("`, tổng số lần mở bằng số lần đóng, tuy nhiên tại một số tiền tố, chuỗi trở nên không hợp lệ. Đầu ra đúng vẫn là`NO`, điều này cho thấy chỉ cân bằng thôi thì chưa đủ. 

Nếu chuỗi được cân bằng hoàn hảo như`"(())()"`, cả thứ tự và số đếm đều thẳng hàng, và câu trả lời là`YES`. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để xác thực cấu trúc là liên tục tìm kiếm các cặp khớp liền kề`"()"`và loại bỏ chúng cho đến khi không thể loại bỏ được nữa. Nếu chuỗi trở nên trống thì nó hợp lệ. 

Điều này hoạt động vì mọi cặp ngoặc hợp lệ cuối cùng phải được khớp và loại bỏ. Tuy nhiên, mỗi lần loại bỏ đều có khả năng làm dịch chuyển chuỗi và buộc phải quét lại. Trong trường hợp xấu nhất, chẳng hạn như`"((((....))))"`, mỗi lần xóa sẽ quét lại gần như toàn bộ chuỗi. Với n lên tới 200000, điều này dẫn đến các hoạt động khoảng O(n^2), quá chậm. 

Cái nhìn sâu sắc về cấu trúc quan trọng là tính hợp lệ chỉ phụ thuộc vào việc liệu chúng ta có gặp nhiều dấu ngoặc đóng hơn số dấu mở có sẵn khi đọc từ trái sang phải hay không. Thay vì mô phỏng việc loại bỏ, chúng tôi duy trì một bộ đếm biểu thị số lượng dấu ngoặc mở không khớp tồn tại ở mỗi bước. 

Bất cứ khi nào chúng ta nhìn thấy`'('`, chúng tôi tăng bộ đếm. Bất cứ khi nào chúng ta nhìn thấy`')'`, chúng ta phải có ít nhất một cái chưa từng có`'('`có sẵn; nếu không thì trình tự sẽ không hợp lệ ngay lập tức. Cuối cùng, trình tự chỉ có hiệu lực nếu không còn phần mở nào chưa khớp. 

Điều này biến vấn đề thành một lượt với công việc không đổi cho mỗi ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng loại bỏ Brute Force | O(n²) | O(n) | Quá chậm | 
| Bộ đếm một lần | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét chuỗi từ trái sang phải trong khi theo dõi một số nguyên biểu thị số dấu ngoặc đơn hiện đang mở nhưng chưa khớp. 

1. Khởi tạo bộ đếm`balance = 0`. Điều này tượng trưng cho bao nhiêu`'('`đã được nhìn thấy mà chưa phù hợp bởi một`')'`. 
2. Lặp lại từng ký tự trong chuỗi. 
3. Nếu nhân vật là`'('`, tăng`balance`bởi một. Điều này phản ánh việc giới thiệu một khung mở đầu mới chưa từng có. 
4. Nếu nhân vật là`')'`, giảm`balance`bởi một. Trước hoặc sau khi giảm, chúng ta phải đảm bảo rằng`balance`chưa bằng 0, vì điều đó có nghĩa là chúng ta đang cố gắng đóng một dấu ngoặc không tồn tại. Nếu điều này xảy ra, chuỗi không thể tương ứng với bất kỳ biểu thức hợp lệ nào. 
5. Nếu tại bất kỳ thời điểm nào`balance`trở nên âm, ngay lập tức kết luận dãy không hợp lệ. 
6. Sau khi xử lý toàn bộ chuỗi, kiểm tra xem`balance`bằng không. Nếu đúng như vậy thì mọi dấu ngoặc mở đã được khớp; nếu không thì một số vẫn chưa khớp và trình tự không hợp lệ. 

Tính chính xác phụ thuộc vào thực tế là ở bất kỳ tiền tố nào, số lượng dấu ngoặc đóng không thể vượt quá số lượng dấu ngoặc mở trong bất kỳ biểu thức hợp lệ nào. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến: sau khi xử lý từng tiền tố,`balance`bằng số dấu ngoặc mở không khớp trong tiền tố đó, giả sử tiền tố đó hợp lệ cho đến nay. Mỗi`'('`đưa ra một yêu cầu mới chưa từng có, và mỗi`')'`loại bỏ chính xác một yêu cầu như vậy. Nếu dấu ngoặc đóng xuất hiện khi không có yêu cầu nào tồn tại thì nó sẽ vi phạm ràng buộc ghép nối cơ bản của dấu ngoặc đơn được định dạng đúng, khiến toàn bộ chuỗi không thể xuất phát từ bất kỳ biểu thức hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()

    balance = 0

    for ch in s:
        if ch == '(':
            balance += 1
        else:
            balance -= 1
            if balance < 0:
                print("NO")
                return

    print("YES" if balance == 0 else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh quá trình quét tuyến tính được mô tả trước đó. Việc thoát sớm khi`balance`trở nên phủ định là rất quan trọng, bởi vì việc tiếp tục tiếp tục không thể sửa chữa được tiền tố vốn đã không hợp lệ. Kiểm tra đẳng thức cuối cùng đảm bảo rằng không còn dấu ngoặc mở nào chưa khớp. 

Một điểm tinh tế là chúng ta không bao giờ cần lưu trữ rõ ràng vị trí của dấu ngoặc hoặc thử ghép nối. Bộ đếm mã hóa đầy đủ cấu trúc cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`(())()`| Bước | Nhân vật | Số dư | Tiền tố hợp lệ | 
| --- | --- | --- | --- | 
| 1 | ( | 1 | có | 
| 2 | ( | 2 | có | 
| 3 | ) | 1 | vâng | 
| 4 | ) | 0 | vâng | 
| 5 | ( | 1 | có | 
| 6 | ) | 0 | vâng | 

Số dư không bao giờ âm và kết thúc bằng 0, vì vậy dãy số là hợp lệ. 

Điều này xác nhận tính bất biến rằng mọi tiền tố đều duy trì ít nhất số lần mở bằng số lần đóng. 

### Ví dụ 2 

đầu vào:`())`| Bước | Nhân vật | Số dư | Tiền tố hợp lệ | 
| --- | --- | --- | --- | 
| 1 | ( | 1 | có | 
| 2 | ) | 0 | vâng | 
| 3 | ) | -1 | không | 

Ở bước 3, thuật toán phát hiện tiền tố không hợp lệ vì dấu ngoặc đóng xuất hiện mà không có dấu mở phù hợp. Quá trình dừng lại ngay lập tức. 

Điều này chứng tỏ việc chấm dứt sớm sẽ nắm bắt được các cấu trúc không hợp lệ một cách hiệu quả như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý chính xác một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có một bộ đếm duy nhất được duy trì | 

Với n lên tới 200000, quét tuyến tính vừa vặn thoải mái trong giới hạn thời gian và bộ nhớ không đổi đảm bảo không có chi phí hoạt động từ các cấu trúc phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("6\n(())()\n") == "YES"
assert run("3\n())\n") == "NO"
assert run("4\n)(()\n") == "NO"

# minimum size valid
assert run("2\n()\n") == "YES"

# minimum invalid
assert run("2\n)\n(") == "NO"

# all opens
assert run("5\n(((((") == "NO"

# balanced but wrong order
assert run("4\n())(") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`()`| CÓ | dãy hợp lệ nhỏ nhất | 
|`)(`| KHÔNG | tiền tố không hợp lệ sớm | 
|`(((((`| KHÔNG | vẫn còn mở chưa từng có | 
|`())(`| KHÔNG | cân đúng nhưng vi phạm tiền tố | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi chuỗi bắt đầu bằng dấu ngoặc đóng. Đối với đầu vào`")("`, thuật toán ngay lập tức giảm số dư xuống dưới 0 ở ký tự đầu tiên. Vì không thấy dấu ngoặc mở nào nên điều này phản ánh một cấu trúc không thể thực hiện được và kết quả đầu ra là chính xác.`NO`. 

Một trường hợp khác là khi tổng số dấu ngoặc mở và đóng bằng nhau nhưng thứ tự không hợp lệ, chẳng hạn như`"())("`. Số dư trở về 0 ở giữa nhưng sau đó giảm xuống dưới 0. Thuật toán phát hiện hành vi vi phạm tại thời điểm chính xác mà nó xảy ra thay vì dựa vào số liệu cuối cùng. 

Trường hợp thứ ba là một chuỗi dài chỉ có dấu ngoặc mở như`"((((("`. Số dư không bao giờ trở nên âm, nhưng cuối cùng nó vẫn dương. Điều này cho thấy rằng tính hợp lệ đòi hỏi cả tính chính xác của tiền tố và sự so khớp hoàn toàn, và việc kiểm tra 0 cuối cùng sẽ nắm bắt được yêu cầu này.
