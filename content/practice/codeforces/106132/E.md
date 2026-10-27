---
title: "CF 106132E - Đánh giá biểu hiện"
description: "Chúng ta có một biểu thức số học duy nhất được viết dưới dạng một chuỗi nhỏ gọn. Biểu thức chỉ chứa các chữ số thập phân, dấu cộng và dấu nhân. Mỗi chữ số là một số độc lập nên không có số nguyên có nhiều chữ số và không có dấu ngoặc đơn."
date: "2026-06-19T19:46:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106132
codeforces_index: "E"
codeforces_contest_name: "National Yang Ming Chiao Tung University 2025 Individual Programming Contest"
rating: 0
weight: 106132
solve_time_s: 46
verified: true
draft: false
---

[CF 106132E - Đánh giá biểu thức](https://codeforces.com/problemset/problem/106132/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biểu thức số học duy nhất được viết dưới dạng một chuỗi nhỏ gọn. Biểu thức chỉ chứa các chữ số thập phân, dấu cộng và dấu nhân. Mỗi chữ số là một số độc lập nên không có số nguyên có nhiều chữ số và không có dấu ngoặc đơn. 

Nhiệm vụ là đánh giá biểu thức này theo các quy tắc toán tử tiêu chuẩn trong đó phép nhân mạnh hơn phép cộng và trong cùng một loại toán tử, việc đánh giá sẽ tiến hành từ trái sang phải. Đầu ra cuối cùng là kết quả số nguyên của biểu thức được đánh giá đầy đủ, có thể trở nên rất lớn, do đó cần có số học chính xác tùy ý. 

Các ràng buộc có độ dài nhỏ, tối đa 1000 ký tự. Điều này ngay lập tức loại trừ mọi lo ngại về việc kỹ thuật phân tích cú pháp bậc hai hoặc bậc ba quá chậm. Ngay cả cách tiếp cận O(n²) cũng có thể tồn tại nhưng không cần thiết. Quét tuyến tính rõ ràng là đủ nếu chúng ta xử lý quyền ưu tiên một cách chính xác. 

Một cạm bẫy tinh vi xuất phát từ việc giả định đánh giá từ trái sang phải mà không tôn trọng quyền ưu tiên. Ví dụ, giải thích`2+3*4`hoàn toàn từ trái sang phải sẽ cho`(2+3)*4 = 20`, điều này không đúng theo quy định. Một sai lầm khác là cố gắng đánh giá các chữ số riêng lẻ mà không nhóm các chuỗi nhân đúng cách. 

Các trường hợp đặc biệt xuất hiện khi các số 0 tham gia vào chuỗi nhân hoặc khi biểu thức bắt đầu hoặc kết thúc bằng chuỗi nhân dài. Ví dụ,`0*1*2+3`phải thu gọn toàn bộ tích về 0 một cách chính xác trước khi thêm số hạng cuối cùng. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ liên tục quét chuỗi và đánh giá hoạt động có mức ưu tiên cao nhất hoặc mô phỏng dấu ngoặc đơn đầy đủ. Một cách là chuyển đổi biểu thức thành tất cả các dấu ngoặc đơn có thể có hoặc rút gọn biểu thức nhiều lần bằng cách tìm các phân đoạn nhân và thay thế chúng bằng các giá trị được tính toán của chúng. Điều này nhanh chóng trở nên không hiệu quả vì mỗi lần giảm có thể dịch chuyển chuỗi và yêu cầu quét lại, dẫn đến hành vi O(n²) hoặc tệ hơn. 

Một cách tiếp cận mạnh mẽ có cấu trúc hơn là chuyển đổi rõ ràng biểu thức thành mã thông báo và mô phỏng việc đánh giá bằng cách giải quyết nhiều lần phép nhân trước, sau đó là phép cộng. Thậm chí điều này thường yêu cầu nhiều lần chuyển qua danh sách mã thông báo. Trong trường hợp xấu nhất, mỗi lượt chỉ loại bỏ một toán tử, dẫn đến hành vi O(n²). 

Quan sát quan trọng là phép nhân chỉ tương tác cục bộ: nó tạo thành các phân đoạn liền kề được phân tách bằng dấu cộng. Vì phép nhân có độ ưu tiên cao hơn và được để ở dạng kết hợp nên mọi phân đoạn cực đại của dạng`a * b * c * ...`có thể được thu gọn hoàn toàn một cách độc lập. Khi mỗi phân đoạn như vậy được giảm xuống còn một số duy nhất, chỉ còn lại phần bổ sung, có thể được tích lũy trong một lần vượt qua. 

Điều này cho phép quét tuyến tính một lần trong đó chúng tôi duy trì một sản phẩm đang chạy cho chuỗi nhân hiện tại và thêm nó vào tổng số đang chạy bất cứ khi nào gặp dấu cộng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý biểu thức từ trái sang phải trong khi vẫn duy trì hai phần trạng thái: bộ tích lũy nhân hiện tại và tổng cuối cùng. 

1. Bắt đầu với tổng hiện có bằng 0 và tích hiện tại bằng 1. Chúng đại diện cho việc đánh giá mọi thứ được xử lý cho đến nay và chuỗi nhân hoạt động tương ứng. 
2. Đọc từng ký tự biểu thức. Khi gặp một chữ số, nó sẽ được chuyển đổi thành số nguyên và được nhân thành tích hiện tại. Điều này có tác dụng vì các chữ số thuộc về chuỗi nhân cho đến khi toán tử dấu cộng phá vỡ chuỗi đó. 
3. Khi gặp dấu cộng, chúng ta hoàn tất chuỗi nhân hiện tại bằng cách cộng tích hiện tại vào tổng hiện có, sau đó đặt lại tích thành một. Điều này phản ánh rằng phép cộng tách các khối nhân độc lập. 
4. Khi gặp dấu nhân, chúng ta không làm gì ngay lập tức vì phép nhân chỉ ảnh hưởng đến cách nhóm các chữ số tiếp theo vào tích hiện tại. 
5. Sau khi xử lý toàn bộ chuỗi, chúng ta cộng sản phẩm cuối cùng vào tổng vì chuỗi nhân cuối cùng không kết thúc bằng dấu cộng. 

Ý tưởng quan trọng là phép nhân được xử lý một cách lười biếng bằng cách liên tục gấp các chữ số thành một tích, trong khi phép cộng sẽ đưa các sản phẩm đã hoàn thành vào kết quả cuối cùng. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình quét, kết quả hiện tại thể hiện chính xác giá trị của đoạn liền kề của biểu thức kể từ dấu cộng cuối cùng. Điều này đúng vì phép nhân có tính chất kết hợp và đánh giá kết hợp bên trái của một chuỗi tương đương với phép nhân tăng dần. Mỗi khi chúng tôi thấy một dấu cộng, chúng tôi sẽ cắt biểu thức thành các khối độc lập có giá trị không tương tác thêm. Do đó, việc tính tổng các tích khối này sẽ tái tạo lại đánh giá chính xác về độ ưu tiên của biểu thức ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

total = 0
cur = 1

for ch in s:
    if ch.isdigit():
        cur *= int(ch)
    elif ch == '*':
        continue
    else:  # '+'
        total += cur
        cur = 1

total += cur
print(total)
```Việc triển khai phản ánh trực tiếp cấu trúc bất biến được mô tả trước đó. Biến`cur`tích lũy một phân đoạn nhân đầy đủ, trong khi`total`lưu trữ tổng của tất cả các phân đoạn đã hoàn thành. 

Một chi tiết tinh tế nhưng quan trọng là sự bổ sung cuối cùng của`cur`sau khi vòng lặp kết thúc. Nếu không có bước này, chuỗi nhân cuối cùng sẽ không bao giờ được thực hiện nếu biểu thức không kết thúc bằng dấu cộng, điều này luôn đúng theo định nghĩa. 

Một chi tiết khác là dấu nhân bị bỏ qua một cách rõ ràng. Điều này an toàn vì phép nhân không yêu cầu thực hiện hành động tại thời điểm đọc toán tử, chỉ ở các chữ số mà nó kết nối. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`2*3+4`| Bước | Char | cur | tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 0 | 
| 2 | * | 2 | 0 | 
| 3 | 3 | 6 | 0 | 
| 4 | + | 6 | 6 | 
| 5 | 4 | 4 | 6 | 
| kết thúc | | | 10 | 

Điều này cho thấy phép nhân được giải quyết hoàn toàn như thế nào trước khi phép cộng được áp dụng. Sản phẩm`2*3`được hoàn thành trước khi được cộng vào tổng. 

### Ví dụ 2:`3*4+5*6*7`| Bước | Char | cur | tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 0 | 
| 2 | * | 3 | 0 | 
| 3 | 4 | 12 | 0 | 
| 4 | + | 12 | 12 | 
| 5 | 5 | 5 | 12 | 
| 6 | * | 5 | 12 | 
| 7 | 6 | 30 | 12 | 
| 8 | * | 30 | 12 | 
| 9 | 7 | 210 | 12 | 
| kết thúc | | | 222 | 

Dấu vết này xác nhận rằng chuỗi nhân được thu gọn độc lập và sau đó được tính tổng, khớp với các quy tắc ưu tiên của toán tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Kích thước đầu vào tối đa là 1000 ký tự, do đó, một đường truyền tuyến tính duy nhất là đủ dễ dàng. Mặc dù các kết quả trung gian có thể trở nên lớn, các số nguyên chính xác tùy ý của Python xử lý chúng mà không lo tràn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import prod
    s = _sys.stdin.readline().strip()

    total = 0
    cur = 1
    for ch in s:
        if ch.isdigit():
            cur *= int(ch)
        elif ch == '*':
            continue
        else:
            total += cur
            cur = 1
    total += cur
    return str(total)

# basic samples
assert run("2*3+4") == "10"
assert run("3*4+5*6*7") == "222"

# single digit
assert run("7") == "7"

# all multiplication
assert run("2*3*4") == "24"

# all addition
assert run("1+2+3+4") == "10"

# zero propagation
assert run("0*1*2+3") == "3"

# mixed chains
assert run("9+1*2+3*4*5+6") == "9+2+60+6"  # conceptual validation ignored below
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2*3+4 | 10 | ưu tiên cơ bản | 
| 3_4+5_6*7 | 222 | nhiều chuỗi nhân | 
| 7 | 7 | trường hợp cạnh mã thông báo đơn | 
| 2_3_4 | 24 | phép nhân thuần túy | 
| 1+2+3+4 | 10 | phép cộng thuần túy | 
| 0_1_2+3 | 3 | sự hủy diệt bằng không | 

## Vỏ cạnh 

Trường hợp một cạnh là khi biểu thức chứa chuỗi nhân dài kết thúc ở ranh giới chuỗi, chẳng hạn như`2*3*4`. Thuật toán xử lý các chữ số thành`cur`cho đến hết và vì không có dấu cộng nào xuất hiện nên bước cộng cuối cùng sẽ xác nhận chính xác sản phẩm`24`. 

Một trường hợp cạnh khác là phép nhân với số 0 ở sâu bên trong một chuỗi, chẳng hạn`9*0*5+7`. Tích lũy nhân ngay lập tức giảm xuống 0 ở chữ số 0 đầu tiên và số 0 này tồn tại trong suốt phần còn lại của chuỗi cho đến khi nó được cộng vào tổng, tạo ra`0 + 7 = 7`. 

Trường hợp tinh vi cuối cùng là các phép toán liên tiếp trộn lẫn cả hai toán tử, chẳng hạn như`8+2*3*4+1`. Thuật toán đảm bảo rằng mỗi khi gặp một dấu cộng, phân đoạn nhân hiện tại sẽ được cố định và thêm vào, ngăn chặn bất kỳ sự rò rỉ nào của phép nhân qua các ranh giới phép cộng.
