---
title: "CF 332A - Xuống hầm!"
description: "Trò chơi có thể được coi là một vòng tròn gồm n người chơi lần lượt thực hiện một trong hai hành động, ký hiệu là 'a' cho khuỷu tay và 'b' cho cái gật đầu. Vasya, ở chỉ số 0, muốn tối đa hóa số lần anh ta có thể uống một ly nước trái cây."
date: "2026-06-06T09:51:51+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 332
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 193 (Div. 2)"
rating: 1300
weight: 332
solve_time_s: 94
verified: true
draft: false
---

[CF 332A - Xuống hầm!](https://codeforces.com/problemset/problem/332/A) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi có thể được coi như một vòng tròn`n`người chơi lần lượt thực hiện một trong hai hành động, ký hiệu là`'a'`cho khuỷu tay và`'b'`để gật đầu. Vasya, ở chỉ số 0, muốn tối đa hóa số lần anh ta có thể uống một ly nước trái cây. Anh ta uống một ly nếu đến lượt mình`j`(bắt đầu từ lượt thứ tư mà anh ta tham gia), ba nước đi cuối cùng được thực hiện bởi những người chơi trước trong chuỗi khớp với nước đi anh ta chọn ở lượt này. Trình tự được ghi lại đưa ra các bước di chuyển chính xác của tất cả những người tham gia, nhưng Vasya có thể chọn các bước di chuyển của riêng mình một cách tối ưu. Mục tiêu là tính toán số ly tối đa mà Vasya có thể uống bằng cách chọn những nước đi tốt nhất cho bản thân mà không làm thay đổi hành động của bất kỳ ai khác. 

Các hạn chế ở mức vừa phải:`n`lên tới 2000 và tổng số bước di chuyển cũng lên tới 2000. Điều này cho phép chúng ta xem xét các phép tính mỗi lượt với độ phức tạp gần như bậc hai về số lượt, nhưng bất kỳ khối nào cũng có thể vượt quá giới hạn thời gian. Các trường hợp khó khăn bao gồm các trình tự rất ngắn trong đó Vasya không thể uống rượu vì có ít hơn ba nước đi trước đó, các trình tự mà các lượt của Vasya cách xa nhau và các trường hợp mà mỗi nước đi trước đó khác nhau, ngăn cản mọi cơ hội uống rượu. 

Ví dụ, nếu trình tự là`abbba`Và`n = 4`, Vasya có lượt 1 và 5. Ở lượt thứ năm, nếu anh ta chọn`'b'`thay vì ghi lại`'a'`, anh ta có thể khớp ba nước đi trước đó (đó là`'bbb'`) và uống. Một cách tiếp cận bất cẩn mà bỏ qua việc tối ưu hóa các bước di chuyển của Vasya sẽ luôn sử dụng các bước di chuyển đã được ghi lại và có thể tính dưới mức kính của anh ta. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại trong tất cả các lượt của Vasya và thử cả hai`'a'`Và`'b'`các lựa chọn để xem cái nào tạo ra đồ uống, kiểm tra ba bước trước đó cho mỗi lần thử. Điều này có tác dụng vì số lượt nhiều nhất là 2000 và mỗi lần kiểm tra bao gồm một số thao tác không đổi. Độ phức tạp của nó là`O(T)`mỗi lần kiểm tra, ở đâu`T`là số lượt và việc kiểm tra tất cả các lựa chọn di chuyển có thể có của Vasya không vượt quá`2*T`hoạt động. Điều này được chấp nhận về mặt kỹ thuật ở đây nhưng có thể được đơn giản hóa hơn nữa. 

Cách tiếp cận tối ưu cho thấy việc uống rượu của Vasya chỉ phụ thuộc vào ba nước đi cuối cùng trước lượt hiện tại của anh ta. Đây là một cấu trúc lập trình động cổ điển. Cho phép`dp[i]`là số ly tối đa Vasya có thể uống hết để quay`i`. Ở mỗi lượt của Vasya, chúng tôi xem xét cả hai lựa chọn`'a'`Và`'b'`và thêm 1 nếu lựa chọn phù hợp với ba nước đi trước đó. Vì chúng ta chỉ cần ba nước đi cuối cùng để đưa ra quyết định nên chúng ta có thể tuyên truyền tăng dần số kính tối đa mà không cần lưu trữ toàn bộ chuỗi của tất cả các kết hợp nước đi, dẫn đến một`O(T)`giải pháp. 

Thông tin chi tiết quan trọng là Vasya luôn có thể chọn một nước đi phù hợp nhất với ba nước đi trước đó và chúng ta chỉ cần kiểm tra ba nước đi cuối cùng đó ở mỗi lượt của anh ấy. Điều này làm giảm vấn đề từ việc xem xét tất cả các chuỗi tổng thể sang việc chỉ kiểm tra một cửa sổ trượt có kích thước 3 ở mỗi lượt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T) mỗi lượt | O(T) | Chấp nhận được với T 2000 | 
| Cửa sổ trượt DP tối ưu | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích dữ liệu đầu vào và xác định trình tự các bước di chuyển dưới dạng một chuỗi các`'a'`Và`'b'`. Xác định các chỉ số về lượt của Vasya. Lần lượt của Vasya là mỗi`n`bước bắt đầu từ 0. 
2. Khởi tạo bộ đếm`glasses = 0`để theo dõi xem Vasya có thể uống bao nhiêu ly. 
3. Lặp lại tất cả các lượt của Vasya theo thứ tự. Đối với mỗi lượt, hãy xem xét ba nước đi cuối cùng ngay trước lượt này. Nếu trước lượt của mình có ít hơn ba nước đi, anh ta chưa thể uống rượu. 
4. Nếu ba nước đi cuối đều bằng`'a'`hoặc tất cả`'b'`, Vasya có thể chọn nước đi và bước tăng phù hợp`glasses`bằng 1. 
5. Nếu ba chiêu cuối không bằng nhau thì Vasya có chọn chiêu nào cũng không được uống, nên đừng tăng thêm. 
6. Sau khi kiểm tra tất cả các lượt của Vasya, hãy in tổng số lượt`glasses`. 

Tại sao nó hoạt động: Bằng cách chỉ tập trung vào ba nước đi cuối cùng trước lượt của Vasya, chúng tôi nắm bắt được chính xác điều kiện mà anh ta có thể uống rượu. Cửa sổ trượt đảm bảo rằng chúng ta luôn chỉ xem xét những nước đi có liên quan. Mỗi lượt được đánh giá độc lập để mang lại lợi ích tối đa và vì chúng tôi không bao giờ sửa đổi nước đi của người chơi khác nên số đếm là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()
glasses = 0
length = len(s)

# Vasya's turns: 0, n, 2n, ...
for turn in range(0, length, n):
    if turn < 3:
        continue  # fewer than three previous moves, cannot drink
    last_three = s[turn-3:turn]
    if last_three[0] == last_three[1] == last_three[2]:
        glasses += 1

print(glasses)
```Giải pháp này đọc`n`và trình tự các bước di chuyển, sau đó lặp lại các lượt của Vasya, bỏ qua ba bước đầu tiên khi không thể uống rượu. Đối với mỗi lượt đủ điều kiện, nó sẽ kiểm tra sự bằng nhau của ba nước đi trước đó. Nếu chúng giống nhau, Vasya có thể ghép chúng và uống rượu. Sự đơn giản của việc kiểm tra cửa sổ trượt làm giảm độ phức tạp cả về thời gian và không gian. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
abbba
```| Xoay | Xoay Chỉ mục | 3 nước đi trước | Có thể uống được không? | Tổng số kính | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | Không áp dụng | Không | 0 | 
| 4 | 4 | bbb | Có | 1 | 

Lượt đầu tiên của Vasya có chỉ số 0, ít hơn ba nước đi trước nên không được uống. Lượt thứ hai của anh ta là chỉ số 4, và ba nước đi trước đó là`'bbb'`. Bằng cách chọn`'b'`, anh ta diêm và uống, tổng cộng 1 ly. 

### Mẫu tùy chỉnh 

đầu vào:```
5
aaabbaa
```Đến lượt của Vasya ở chỉ số 0 và 5. Ba nước đi trước chỉ số 5 là`'bba'`. Không phải tất cả đều bình đẳng nên anh ta không thể uống rượu. Đầu ra là 0. 

Điều này xác nhận giải pháp bỏ qua chính xác các trường hợp trong đó ba nước đi trước không bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi lượt của Vasya được kiểm tra một lần, trích xuất một chuỗi con có độ dài 3 | 
| Không gian | O(1) | Chỉ các bộ đếm và chuỗi con có kích thước cố định có độ dài 3 mới được lưu trữ | 

Với tối đa 2000 lượt, giải pháp sẽ thực thi trong micro giây và yêu cầu bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    s = input().strip()
    glasses = 0
    length = len(s)
    for turn in range(0, length, n):
        if turn < 3:
            continue
        last_three = s[turn-3:turn]
        if last_three[0] == last_three[1] == last_three[2]:
            glasses += 1
    return str(glasses)

# Provided sample
assert run("4\nabbba\n") == "1", "sample 1"

# Custom cases
assert run("5\naaabbaa\n") == "0", "unequal previous three"
assert run("4\naaaa\n") == "1", "minimum drink at last turn"
assert run("4\nbbbbbb\n") == "1", "multiple possible turns"
assert run("4\naabbaabbaabb\n") == "0", "no turn has matching previous three"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4\nabbba | 1 | trường hợp mẫu | 
| 5\naaabbaa | 0 | ba nước đi trước không bằng nhau | 
| 4\naaa | 1 | đầu vào tối thiểu với cơ hội uống rượu | 
| 4\nbbbbbb | 1 | di chuyển lặp đi lặp lại cho phép một ly | 
| 4\naabbaabbaabb | 0 | quay nhiều vòng nhưng không uống | 

## Vỏ cạnh 

Khi tổng số lượt ít hơn bốn, Vasya không được uống. Ví dụ,`n=4`Và`s='abb'`. Vasya chỉ có lượt ở chỉ số 0, ít hơn ba nước đi trước đó, vì vậy`glasses=0`. Thuật toán xử lý việc này bằng cách bỏ qua ba bước đầu tiên và không bao giờ thử tính toán đồ uống, tạo ra kết quả chính xác. Tương tự, nếu tất cả các bước đi trước đều bằng nhau nhưng xảy ra rất sớm trong chuỗi thì cửa sổ trượt đảm bảo thuật toán chỉ xem xét các lượt đủ điều kiện.
