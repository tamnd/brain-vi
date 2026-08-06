---
title: "CF 102535H - Bíp Bop Boop"
description: "Nhiệm vụ là phân loại từng sinh vật được gắn thẻ chỉ bằng cách sử dụng những âm thanh được ghi lại bởi thẻ của nó. Một sinh vật được gọi là bop nếu mọi âm thanh nó tạo ra đều thuộc về tập hợp đặc biệt gồm hai âm thanh bop hợp lệ: BÍP và BOOP. Bất kỳ âm thanh nào khác ngay lập tức chứng minh rằng sinh vật đó không phải là một con bop."
date: "2026-08-05T15:22:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "H"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 199
verified: true
draft: false
---

[CF 102535H - Beep Bop Boop](https://codeforces.com/problemset/problem/102535/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là phân loại từng sinh vật được gắn thẻ chỉ bằng cách sử dụng những âm thanh được ghi lại bởi thẻ của nó. Một sinh vật được coi là bop nếu mọi âm thanh nó tạo ra đều thuộc về tập hợp đặc biệt gồm hai âm thanh bop hợp lệ:`BEEP`Và`BOOP`. Bất kỳ âm thanh nào khác ngay lập tức chứng minh rằng sinh vật đó không phải là một con bop. 

Đầu vào chứa một số sinh vật. Đối với mỗi sinh vật, chúng ta nhận được số lượng âm thanh được ghi lại theo sau là các chuỗi âm thanh đó. Đầu ra của mỗi sinh vật là một thông báo cố định tùy thuộc vào việc liệu tất cả âm thanh được ghi của nó có phải là âm thanh bop hợp lệ hay không. 

Các giới hạn đủ nhỏ để chúng ta có thể kiểm tra trực tiếp mọi âm thanh. Có thể có tối đa 350 sinh vật và mỗi sinh vật có tối đa 350 âm thanh nên tổng số lần kiểm tra âm thanh nhiều nhất là 122.500. Ngay cả việc quét tuyến tính đơn giản trên tất cả các âm thanh cũng có thể dễ dàng thực hiện trong thời gian giới hạn. Những ràng buộc này loại trừ nhu cầu về cấu trúc dữ liệu phức tạp hoặc tiền xử lý. Yêu cầu chính là tránh sai sót trong điều kiện phân loại. 

Một sai lầm phổ biến là tìm kiếm sự hiện diện của`BEEP`hoặc`BOOP`thay vì kiểm tra xem mọi âm thanh có phải là một trong số chúng không. Ví dụ:```
1
3
BEEP
BOOP
QUACK
```Đầu ra đúng là:```
IT'S NOT A BOP!
```Việc triển khai bất cẩn chỉ kiểm tra xem có xuất hiện ít nhất một âm thanh bop hợp lệ hay không sẽ chấp nhận sinh vật này một cách sai lầm. 

Một trường hợp khác là sinh vật lặp lại cùng một âm thanh hợp lệ nhiều lần:```
1
4
BOOP
BOOP
BOOP
BOOP
```Đầu ra đúng là:```
IT'S A BOP!
```Số lần xuất hiện không quan trọng. Điều kiện chỉ phụ thuộc vào việc mọi âm thanh được ghi có thuộc tập hợp được phép hay không. 

Trường hợp thứ ba là một sinh vật có một âm thanh:```
1
1
BEEP
```Đầu ra đúng là:```
IT'S A BOP!
```Việc triển khai vô tình khởi tạo biến kiểm tra không chính xác hoặc chỉ kiểm tra sau khi đọc nhiều âm thanh có thể không thành công trên đầu vào nhỏ nhất này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là kiểm tra mọi âm thanh của mọi sinh vật. Đối với mỗi sinh vật, chúng tôi bắt đầu bằng cách giả định đó là bop, sau đó kiểm tra từng âm thanh được ghi lại. Nếu một âm thanh là bất cứ điều gì khác hơn`BEEP`hoặc`BOOP`, chúng tôi đánh dấu sinh vật này là không hợp lệ. Điều này có tác dụng vì định nghĩa của bop chính xác là tất cả âm thanh được ghi phải nằm trong tập hợp hai phần tử đó. 

Cách tiếp cận bạo lực vốn đã tuyến tính vì bản thân đầu vào chứa tất cả thông tin chúng ta cần. Trong trường hợp xấu nhất, nó sẽ kiểm tra mọi âm thanh có thể, đưa ra 350 × 350 = 122.500 so sánh. Đây không phải là vấn đề về hiệu suất nên ý tưởng tương tự cũng là giải pháp tối ưu. 

Nhận xét quan trọng là không có mối quan hệ nào giữa các sinh vật khác nhau và không cần phải so sánh âm thanh với nhau. Mỗi sinh vật có thể được phân loại độc lập bằng cách xác nhận danh sách âm thanh của riêng nó. Nhận xét rằng điều kiện là một phép kiểm tra thành viên đơn giản cho phép chúng ta giảm vấn đề xuống còn một lần chuyển qua đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C × N) | O(1) | Đã chấp nhận | 
| Tối ưu | O(C × N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng sinh vật. Mỗi sinh vật sẽ được xử lý độc lập vì âm thanh của sinh vật này không ảnh hưởng đến sinh vật khác. 
2. Đối với mỗi sinh vật, hãy đọc số lượng âm thanh và đặt cờ cho biết sinh vật đó hiện được coi là bop. Giả định ban đầu là hợp lệ vì một sinh vật chỉ trở nên không hợp lệ sau khi tìm thấy âm thanh bị cấm. 
3. Đọc từng âm và kiểm tra xem nó có chính xác không`BEEP`hoặc`BOOP`. Nếu không, hãy thay đổi cờ để cho biết sinh vật đó không phải là bop. 
4. Sau khi tất cả âm thanh của sinh vật hiện tại đã được xử lý, hãy in kết quả dựa trên lá cờ. Chúng tôi đợi cho đến khi kết thúc vì âm thanh sau đó có thể vô hiệu hóa sinh vật có vẻ hợp lệ trước đó. 

Tại sao nó hoạt động: Trong quá trình quét âm thanh của sinh vật, điều bất biến là cờ vẫn đúng chính xác khi tất cả âm thanh nhìn thấy cho đến nay đều là âm thanh bop hợp lệ. Âm thanh hợp lệ sẽ giữ nguyên thuộc tính này, trong khi âm thanh không hợp lệ sẽ phá vỡ thuộc tính này vĩnh viễn. Sau khi âm thanh cuối cùng được xử lý, cờ biểu thị liệu mọi âm thanh được ghi có thỏa mãn quy tắc bop hay không, đây chính xác là điều kiện bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    c = int(input())
    ans = []

    for _ in range(c):
        n = int(input())
        is_bop = True

        for _ in range(n):
            sound = input().strip()
            if sound != "BEEP" and sound != "BOOP":
                is_bop = False

        if is_bop:
            ans.append("IT'S A BOP!")
        else:
            ans.append("IT'S NOT A BOP!")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Biến`is_bop`đại diện cho sự phân loại hiện tại của sinh vật. Nó bắt đầu như`True`bởi vì chưa có âm thanh nào mâu thuẫn với quy tắc bop. Mọi âm thanh đều được kiểm tra ngay sau khi đọc nên không cần lưu trữ thêm. 

Việc so sánh sử dụng đẳng thức chuỗi chính xác vì chỉ có hai chuỗi hoàn chỉnh`BEEP`Và`BOOP`được chấp nhận. Kiểm tra tiền tố hoặc kiểm tra chuỗi con sẽ chấp nhận không chính xác các giá trị như`BEEPS`hoặc`XBOOP`. 

Mã xử lý âm thanh khi chúng đến và chỉ lưu trữ các thông báo đầu ra cuối cùng. Vì giới hạn đầu vào nhỏ nên việc xử lý số nguyên thông thường là đủ và không có vấn đề tràn. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, sinh vật đầu tiên chỉ có âm thanh hợp lệ. 

| Sinh vật | Đọc âm thanh | is_bop sau âm thanh | Kết quả | 
| --- | --- | --- | --- | 
| 1 | BÍP | Đúng | Đang chờ xử lý | 
| 1 | BÚP | Đúng | Đang chờ xử lý | 
| 1 | BÚP | Đúng | ĐÓ LÀ BOP! | 
| 2 | BÚP | Đúng | Đang chờ xử lý | 
| 2 | BÍP | Đúng | Đang chờ xử lý | 
| 2 | BÍP | Đúng | Đang chờ xử lý | 
| 2 | BÚP | Đúng | ĐÓ LÀ BOP! | 
| 3 | BIP | Sai | Đang chờ xử lý | 
| 3 | BÚP | Sai | Đang chờ xử lý | 
| 3 | QUACK | Sai | Đang chờ xử lý | 
| 3 | BOO | Sai | NÓ KHÔNG PHẢI LÀ BOP! | 

Dấu vết này cho thấy những âm thanh hợp lệ được lặp lại không bao giờ thay đổi cách phân loại, trong khi âm thanh không hợp lệ đầu tiên sẽ thay đổi vĩnh viễn sự phân loại đó. 

Đối với Mẫu 2, sinh vật thứ hai và thứ ba chứa âm thanh nằm ngoài tập hợp cho phép. 

| Sinh vật | Đọc âm thanh | is_bop sau âm thanh | Kết quả | 
| --- | --- | --- | --- | 
| 1 | BÍP | Đúng | Đang chờ xử lý | 
| 1 | BÚP | Đúng | Đang chờ xử lý | 
| 1 | BÍP | Đúng | Đang chờ xử lý | 
| 1 | BÚP | Đúng | Đang chờ xử lý | 
| 1 | BÚP | Đúng | Đang chờ xử lý | 
| 1 | BÚP | Đúng | Đang chờ xử lý | 
| 1 | BÍP | Đúng | ĐÓ LÀ BOP! | 
| 2 | QUACK | Sai | Đang chờ xử lý | 
| 2 | KWAK | Sai | Đang chờ xử lý | 
| 2 | QUACK | Sai | Đang chờ xử lý | 
| 2 | KWAKK | Sai | Đang chờ xử lý | 
| 2 | QUAKK | Sai | NÓ KHÔNG PHẢI LÀ BOP! | 
| 3 | ARF | Sai | Đang chờ xử lý | 
| 3 | GỖ | Sai | Đang chờ xử lý | 
| 3 | ARFF | Sai | NÓ KHÔNG PHẢI LÀ BOP! | 

Điều này xác nhận rằng thuật toán không yêu cầu tất cả các âm thanh phải khác nhau. Nó chỉ kiểm tra tư cách thành viên trong bộ âm thanh được phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng số âm thanh) | Mỗi âm thanh được ghi đều được kiểm tra chính xác một lần. | 
| Không gian | O(1) | Chỉ duy trì trạng thái và lưu trữ đầu ra của sinh vật hiện tại. | 

Số lần kiểm tra âm thanh tối đa là 122.500, thấp hơn nhiều so với mức cần thiết cho giới hạn 2 giây. Việc sử dụng bộ nhớ liên tục cũng phù hợp thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    c = int(input())
    ans = []

    for _ in range(c):
        n = int(input())
        is_bop = True

        for _ in range(n):
            sound = input().strip()
            if sound != "BEEP" and sound != "BOOP":
                is_bop = False

        ans.append("IT'S A BOP!" if is_bop else "IT'S NOT A BOP!")

    sys.stdin = old_stdin
    return "\n".join(ans)

assert solution("""3
3
BEEP
BOOP
BOOP
4
BOOP
BEEP
BEEP
BOOP
4
BIP
BUP
QUACK
BOO
""") == """IT'S A BOP!
IT'S A BOP!
IT'S NOT A BOP!""", "sample 1"

assert solution("""3
7
BEEP
BOOP
BEEP
BOOP
BOOP
BOOP
BEEP
5
QUACK
KWAK
QUACK
KWAKK
QUAKK
3
ARF
WOOF
ARFF
""") == """IT'S A BOP!
IT'S NOT A BOP!
IT'S NOT A BOP!""", "sample 2"

assert solution("""1
1
BEEP
""") == "IT'S A BOP!", "minimum valid case"

assert solution("""1
1
HELLO
""") == "IT'S NOT A BOP!", "minimum invalid case"

assert solution("""2
5
BOOP
BOOP
BOOP
BOOP
BOOP
3
BEEP
BOOP
NOPE
""") == """IT'S A BOP!
IT'S NOT A BOP!""", "repeated sounds and late failure"

assert solution("""1
350
""" + "BEEP\n" * 350) == "IT'S A BOP!", "maximum size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn`BEEP`âm thanh |`IT'S A BOP!`| Độ chính xác khởi tạo và đầu vào hợp lệ nhỏ nhất | 
| Âm thanh đơn không hợp lệ |`IT'S NOT A BOP!`| Từ chối ngay lập tức các âm thanh bị cấm | 
| lặp đi lặp lại`BOOP`giá trị với một âm thanh xấu sau đó | Kết quả hỗn hợp | Thuật toán tiếp tục quét sau khi tìm thấy âm thanh không hợp lệ | 
| 350 âm thanh hợp lệ |`IT'S A BOP!`| Kích thước sinh vật tối đa được phép | 

## Vỏ cạnh 

Trường hợp chứa cả âm thanh hợp lệ và không hợp lệ sẽ được xử lý vì cờ chỉ thay đổi khi xuất hiện âm thanh nằm ngoài tập hợp cho phép. Đối với đầu vào:```
1
3
BEEP
BOOP
QUACK
```hai âm thanh đầu tiên rời đi`is_bop`BẰNG`True`, sau đó`QUACK`thay đổi nó thành`False`, sản xuất:```
IT'S NOT A BOP!
```Trường hợp âm thanh lặp lại hoạt động vì thuật toán không tính các âm thanh hoặc yêu cầu sự đa dạng. Vì:```
1
4
BOOP
BOOP
BOOP
BOOP
```mỗi so sánh thành công, do đó cờ vẫn đúng và đầu ra là:```
IT'S A BOP!
```Trường hợp ranh giới âm đơn cũng được xử lý một cách tự nhiên. Với:```
1
1
BEEP
```vòng lặp thực hiện một lần, xác nhận âm thanh duy nhất là hợp lệ và in:```
IT'S A BOP!
```Thuật toán phản ánh trực tiếp định nghĩa của bop, vì vậy các trường hợp biên này không yêu cầu xử lý đặc biệt.
