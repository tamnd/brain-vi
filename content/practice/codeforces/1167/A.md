---
title: "CF 1167A - Số điện thoại"
description: "Chúng ta được cấp một chuỗi các chữ số và được phép xóa các ký tự một cách tự do mà không cần thay đổi thứ tự tương đối của các chữ số còn lại. Nhiệm vụ là quyết định xem liệu chúng ta có thể trích xuất một dãy con có độ dài chính xác bằng 11 để tạo thành một số điện thoại hợp lệ hay không."
date: "2026-06-13T08:57:47+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1167
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 65 (Rated for Div. 2)"
rating: 800
weight: 1167
solve_time_s: 123
verified: true
draft: false
---

[CF 1167A - Số điện thoại](https://codeforces.com/problemset/problem/1167/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, dây 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ số và được phép xóa các ký tự một cách tự do mà không cần thay đổi thứ tự tương đối của các chữ số còn lại. Nhiệm vụ là quyết định xem liệu chúng ta có thể trích xuất một dãy con có độ dài chính xác bằng 11 để tạo thành một số điện thoại hợp lệ hay không. Một số điện thoại hợp lệ có cấu trúc rất cụ thể: nó phải có độ dài 11 và chữ số đầu tiên phải là 8. 10 chữ số còn lại có thể là bất kỳ số nào. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem chuỗi có chứa chuỗi con có độ dài 11 trong đó ký tự được chọn đầu tiên là`8`. 

Ràng buộc đối với mỗi trường hợp kiểm thử là nhỏ, với độ dài chuỗi lên tới 100 và tối đa 100 trường hợp kiểm thử. Điều này ngay lập tức loại trừ mọi nhu cầu về tiền xử lý nặng hoặc cấu trúc dữ liệu nâng cao. Quét tuyến tính cho mỗi trường hợp thử nghiệm là đủ dễ dàng. 

Một sai lầm ngây thơ nhưng phổ biến là cho rằng chúng ta chỉ cần kiểm tra xem chuỗi có chứa ít nhất một`8`và ít nhất 10 chữ số nữa ở đâu đó sau nó. Điều này gần đúng nhưng không chính xác nếu không được xử lý cẩn thận theo thứ tự, vì việc lựa chọn chuỗi con phụ thuộc vào vị trí. Một sai lầm khác là cố gắng xây dựng tất cả các dãy con có độ dài 11, làm bùng nổ tổ hợp ngay cả ở độ dài 100, vì có$\binom{100}{11}$khả năng. 

Trường hợp cạnh tinh tế phát sinh khi có nhiều`8`S. Ví dụ, trong`88888888888`, bất kỳ vị trí nào cũng có tác dụng như một điểm bắt đầu, nhưng trong một chuỗi có vị trí đầu tiên`8`đã quá muộn, chúng ta có thể không có đủ ký tự sau đó. Quét tham lam phải tính toán chính xác độ dài có sẵn còn lại. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra mọi chuỗi con có độ dài 11 và kiểm tra xem có chuỗi nào bắt đầu bằng`8`. Điều này đúng vì nó khám phá tất cả các thao tác xóa có thể xảy ra. Tuy nhiên, số lượng các chuỗi tiếp theo tăng lên nhanh chóng. Ngay cả đối với một trường hợp thử nghiệm duy nhất với$n = 100$, số lượng các chuỗi con có độ dài 11 lớn về mặt thiên văn, khiến phương pháp này không khả thi. 

Quan sát quan trọng là chúng ta không cần xây dựng các chuỗi con một cách rõ ràng. Chúng ta chỉ cần biết liệu có tồn tại vị trí của một`8`sao cho có ít nhất 10 ký tự theo sau nó trong chuỗi. Vì việc xóa giữ nguyên trật tự nên khi chúng tôi chọn một`8`tại chỉ mục`i`, chúng ta chỉ cần xác minh rằng có ít nhất 10 ký tự nữa sau chỉ mục`i`. Những nhân vật đó có thể là bất cứ thứ gì; chúng tôi chỉ đơn giản coi chúng như chúng xuất hiện. 

Điều này làm giảm vấn đề quét tìm vị trí bắt đầu hợp lệ và đếm độ dài còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các phần tiếp theo) | O(2^n) | O(11) | Quá chậm | 
| Tham lam quét từ mỗi 8 | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc chuỗi và quét qua tất cả các vị trí có chữ số`8`. Mỗi vị trí như vậy là một ứng viên bắt đầu bằng một số điện thoại. 
2. Đối với mỗi chỉ số`i`Ở đâu`s[i] == '8'`, kiểm tra xem độ dài hậu tố`n - i`ít nhất là 11. Điều này đảm bảo chúng ta có thể chọn 11 ký tự bắt đầu từ đây`8`đồng thời giữ gìn trật tự. 
3. Nếu bất kỳ vị trí nào như vậy thỏa mãn điều kiện, hãy kết luận ngay rằng tồn tại một dãy con hợp lệ. 
4. Nếu không tìm thấy vị trí bắt đầu hợp lệ, xuất ra`NO`. 

Lý do đằng sau bước 2 là khi chúng ta sửa chữ số đầu tiên thành`8`, chúng ta chỉ cần thêm 10 ký tự sau nó. Vì chúng ta không bị ràng buộc bởi các giá trị của chúng nên chỉ tính sẵn có của chúng mới quan trọng. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thuộc tính sẵn có đơn điệu: nếu một`8`xuất hiện tại chỉ mục`i`, thì mọi ký tự sau nó đều đủ điều kiện để trở thành một phần của dãy con và chúng ta chỉ cần 10 ký tự trong số đó. Không có lợi ích gì khi bỏ qua hợp lệ trước đó`8`s nếu cái sau hoạt động, bởi vì các vị trí trước đó luôn cung cấp ít nhất nhiều ký tự còn lại. Điều này đảm bảo rằng việc kiểm tra các vị trí là đủ mà không cần xây dựng các chuỗi con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()

    ok = False
    for i in range(n):
        if s[i] == '8' and n - i >= 11:
            ok = True
            break

    print("YES" if ok else "NO")
```Giải pháp lặp lại từng trường hợp thử nghiệm và kiểm tra tất cả các vị trí có chữ số`8`. Chi tiết triển khai chính là điều kiện`n - i >= 11`, điều này đảm bảo chúng ta có thể chọn dòng điện`8`cộng thêm ít nhất 10 ký tự tiếp theo. Việc thoát sớm ngăn chặn việc quét không cần thiết khi tìm thấy vị trí hợp lệ. 

Một sai lầm phổ biến là quên rằng dãy con được chọn phải đảm bảo trật tự, vì vậy chỉ tính sẵn có của hậu tố mới quan trọng chứ không phải số lượng toàn cục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
13
7818005553535
```Chúng tôi quét các vị trí: 

| tôi | s[i] | là '8'? | n - tôi | bắt đầu hợp lệ? | 
| --- | --- | --- | --- | --- | 
| 0 | 7 | không | 13 | không | 
| 1 | 8 | vâng | 12 | vâng | 

Tại chỉ số 1, chúng ta có đủ ký tự còn lại để tạo thành độ dài 11. Chúng ta có thể chọn`8`tại chỉ số 1 và sau đó là 10 chữ số tiếp theo. Vậy câu trả lời là`YES`. 

Điều này cho thấy rằng chúng ta không cần phải xây dựng dãy con một cách rõ ràng; sự sẵn có một mình xác định tính khả thi. 

### Ví dụ 2 

đầu vào:```
11
31415926535
```| tôi | s[i] | là '8'? | n - tôi | bắt đầu hợp lệ? | 
| --- | --- | --- | --- | --- | 
| tất cả | - | không | - | không | 

Không có chữ số`8`trong chuỗi nên không tồn tại điểm bắt đầu hợp lệ. Kết quả là`NO`. 

Điều này xác nhận rằng việc thiếu chữ số hàng đầu được yêu cầu sẽ ngay lập tức làm mất hiệu lực của cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét tuyến tính đơn trên chuỗi | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Cho rằng$n \le 100$Và$t \le 100$, giải pháp thực hiện tối đa$10^4$hoạt động nằm trong giới hạn cho phép. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()

        ok = False
        for i in range(n):
            if s[i] == '8' and n - i >= 11:
                ok = True
                break
        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided samples
assert run("2\n13\n7818005553535\n11\n31415926535\n") == "YES\nNO"

# custom cases
assert run("1\n11\n80000000000\n") == "YES"
assert run("1\n11\n70000000000\n") == "NO"
assert run("1\n12\n888888888888\n") == "YES"
assert run("1\n20\n12345678901234567890\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xây dựng chính xác hợp lệ duy nhất | CÓ | trường hợp hợp lệ tối thiểu | 
| số 8 có mặt | KHÔNG | chữ số đầu bắt buộc | 
| tất cả 8s | CÓ | nhiều lần bắt đầu hợp lệ | 
| không có cấu trúc hợp lệ | KHÔNG | trường hợp phủ định chung | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chuỗi chứa một`8`, nhưng có vẻ đã quá muộn để cho phép còn lại 10 ký tự. Ví dụ, trong`11111111118`, duy nhất`8`đang ở vị trí cuối cùng. Mặc dù chữ số bắt buộc tồn tại nhưng không còn ký tự nào để hoàn thành độ dài 11, vì vậy câu trả lời đúng là`NO`. Thuật toán xử lý việc này bằng cách kiểm tra rõ ràng độ dài hậu tố, không chỉ sự hiện diện của`8`. 

Một trường hợp cạnh khác là khi nhiều`8`s tồn tại. hợp lệ đầu tiên`8`đủ để quyết định sự thành công. Quá trình quét xử lý việc này một cách tự nhiên vì nó dừng ở vị trí đầu tiên thỏa mãn điều kiện hậu tố.
