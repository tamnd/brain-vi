---
title: "CF 320A - Những con số kỳ diệu"
description: "Chúng ta có một số nguyên duy nhất được viết dưới dạng thập phân và chúng ta phải quyết định xem nó có thể được xây dựng hay không bằng cách liên tục đặt một trong ba khối cố định cạnh nhau: 1, 14 và 144."
date: "2026-06-06T02:17:55+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 320
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 189 (Div. 2)"
rating: 900
weight: 320
solve_time_s: 66
verified: true
draft: false
---

[CF 320A - Những con số kỳ diệu](https://codeforces.com/problemset/problem/320/A) 

**Xếp hạng:** 900 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số nguyên duy nhất được viết dưới dạng thập phân và chúng ta phải quyết định xem nó có thể được xây dựng hay không bằng cách liên tục đặt một trong ba khối cố định cạnh nhau:`1`,`14`, Và`144`. Số phải được tạo thành chính xác, không có chữ số thừa và không được sắp xếp lại, chỉ ghép các mẫu được phép này. 

Điều này biến nhiệm vụ thành một vấn đề xây dựng chuỗi được ngụy trang. Mặc dù đầu vào là một số nguyên lên tới 10^9, nhưng điều thực sự quan trọng là biểu diễn thập phân của nó, bởi vì phép nối được xác định ở cấp độ chữ số. 

Ràng buộc`n ≤ 10^9`có nghĩa là đầu vào có tối đa 10 chữ số. Điều đó ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nặng hoặc tìm kiếm theo cấp số nhân. Bất kỳ giải pháp nào xử lý số một cách tuyến tính theo số chữ số hoặc thậm chí thử tìm kiếm giới hạn nhỏ sẽ đủ nhanh. 

Một số trường hợp khó khăn xuất hiện một cách tự nhiên khi nghĩ đến việc phân tích cú pháp tham lam. Một chế độ lỗi là diễn giải các chữ số một cách độc lập. Ví dụ, điều trị`144`BẰNG`1 + 4 + 4`sẽ gợi ý không chính xác rằng nhiều số không hợp lệ là hợp lệ. Một vấn đề khác là sự mơ hồ tham lam. Ví dụ, trong một chuỗi như`14144`, đang chọn`1 | 4 | 1 | 4 | 4`không hợp lệ, nhưng vẫn tồn tại một phân đoạn chính xác dưới dạng`14 | 144`. Một sự lựa chọn tham lam ngây thơ từ trái sang phải đối với đơn vị nhỏ nhất sẽ phá vỡ tính đúng đắn. 

Chúng ta cũng cần cẩn thận với xung đột tiền tố. Các mẫu chồng lên nhau:`1`là tiền tố của`14`, Và`14`là tiền tố của`144`. Điều này có nghĩa là cách tiếp cận tham lam ngây thơ luôn diễn ra trong thời gian ngắn nhất có thể bị mắc kẹt, trong khi chiến lược tham lam có cấu trúc chặt chẽ hơn trước tiên phải ưu tiên các trận đấu dài hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách có thể để chia chuỗi chữ số thành các phân đoạn, mỗi phân đoạn là`1`,`14`, hoặc`144`. Tại mỗi vị trí, có thể có tối đa ba lựa chọn, dẫn đến đệ quy phân nhánh. Trong trường hợp xấu nhất, điều này trở thành hàm mũ theo số chữ số, khoảng O(3^d), trong đó d là độ dài của số. 

Điều này hoạt động về mặt khái niệm vì nó trực tiếp khám phá mọi phân tách hợp lệ, nhưng nó trở nên không khả thi ngay cả đối với 10 chữ số vì số lượng trạng thái tăng lên nhanh chóng và nhiều đường đi chồng chéo nhau. 

Quan sát quan trọng là cấu trúc bị hạn chế cao: mọi phân đoạn hợp lệ đều bắt đầu bằng một trong hai`1`, sau đó tùy ý mở rộng đến`14`và tùy chọn thêm để`144`. Cấu trúc lồng nhau này có nghĩa là bất cứ khi nào chúng ta nhìn thấy một`1`, trước tiên chúng ta phải luôn cố gắng sử dụng mẫu hợp lệ dài nhất. Nếu thất bại, chúng ta ngầm quay lại bằng cách từ chối toàn bộ cấu hình. 

Điều này biến vấn đề thành một lần quét tuyến tính duy nhất với việc khớp tham lam từ trái sang phải, luôn cố gắng khớp`144`đầu tiên, sau đó`14`, sau đó`1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^d) | O(d) | Quá chậm | 
| Quét tham lam tối ưu | O(d) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển số thành chuỗi để chúng ta có thể kiểm tra các chữ số một cách tuần tự. Làm việc trực tiếp với các chữ số là cần thiết vì các mẫu được xác định trên các chữ số liên tiếp chứ không phải các thuộc tính số học. 
2. Bắt đầu quét từ chữ số ngoài cùng bên trái. Duy trì một chỉ mục`i`theo dõi vị trí hiện tại của chúng tôi. 
3. Tại mỗi vị trí, kiểm tra xem chuỗi con bắt đầu tại`i`bắt đầu bằng`144`. Nếu vậy, hãy sử dụng ba ký tự và di chuyển`i`chuyển tiếp 3. Lựa chọn này đúng vì`144`là mẫu cụ thể và hạn chế nhất, đồng thời việc sử dụng nó sẽ ngăn chặn việc vô tình phân mảnh các cấu trúc hợp lệ. 
4. Nếu`144`không có sẵn, hãy kiểm tra xem chuỗi con có bắt đầu bằng`14`. Nếu có, hãy sử dụng hai ký tự và tiến lên`i`tăng thêm 2. Bước này đảm bảo chúng tôi vẫn ưu tiên các khối hợp lệ dài hơn các khối ngắn hơn. 
5. Nếu không`144`cũng không`14`khớp, kiểm tra xem ký tự hiện tại có phải là`1`. Nếu vậy, hãy sử dụng nó dưới dạng một khối duy nhất và tiến lên`i`bằng 1. 
6. Nếu không có điều kiện nào trong ba điều kiện xảy ra ở bất kỳ vị trí nào thì việc xây dựng không thành công và số đó không phải là số ma thuật hợp lệ. 
7. Nếu quá trình quét kết thúc chính xác ở cuối chuỗi thì số đó hợp lệ. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên cấu trúc lồng nhau của các mẫu hợp lệ: mỗi số hợp lệ là một chuỗi các mã thông báo trong đó mỗi mã thông báo bắt đầu bằng`1`và mã thông báo dài hơn sẽ mở rộng mã thông báo ngắn hơn (`1 ⊂ 14 ⊂ 144`). Do sự ngăn chặn này, bất cứ khi nào có kết quả phù hợp dài hơn, việc chọn kết quả phù hợp đó sẽ không bao giờ chặn giải pháp hợp lệ có thể bắt đầu bằng tiền tố ngắn hơn. Nếu một giải pháp tồn tại, quá trình quét tham lam sẽ không bao giờ loại bỏ nó sớm và bất kỳ lỗi nào trong việc so khớp đều ngụ ý rằng không có phân đoạn hợp lệ nào tồn tại ở vị trí đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

i = 0
n = len(s)

ok = True

while i < n:
    if i + 3 <= n and s[i:i+3] == "144":
        i += 3
    elif i + 2 <= n and s[i:i+2] == "14":
        i += 2
    elif s[i] == "1":
        i += 1
    else:
        ok = False
        break

print("YES" if ok and i == n else "NO")
```Đoạn mã phản ánh chính xác chiến lược tham lam. Thứ tự kiểm tra là rất quan trọng:`144`phải được kiểm tra trước`14`, Và`14`trước`1`, nếu không, chúng ta có nguy cơ sử dụng tiền tố ngăn chặn việc khớp khối hợp lệ dài hơn sau này. 

Điều kiện cuối cùng`i == n`đảm bảo chúng tôi sử dụng toàn bộ chuỗi không có chữ số còn sót lại, điều này cần thiết vì phân tích cú pháp một phần là không đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`114114`Chúng tôi quét từ trái sang phải. 

| Bước | tôi | Chuỗi còn lại | Trận đấu | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 114114 | 1 | tiêu thụ`1`| 
| 2 | 1 | 14114 | 14 | tiêu thụ`14`| 
| 3 | 3 | 114 | 114 không khớp hoàn toàn với 144/14/1 mà bắt đầu bằng 1 | tiêu thụ`1`| 
| 4 | 4 | 14 | 14 | tiêu thụ`14`| 
| 5 | 6 | "" | kết thúc | thành công | 

Quá trình quét sẽ phân tách hoàn toàn chuỗi thành các khối hợp lệ, xác nhận tính chính xác. 

### Ví dụ 2:`1441`| Bước | tôi | Chuỗi còn lại | Trận đấu | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1441 | 144 | tiêu thụ`144`| 
| 2 | 3 | 1 | 1 | tiêu thụ`1`| 
| 3 | 4 | "" | kết thúc | thành công | 

Điều này chứng tỏ tại sao việc ưu tiên`144`là điều cần thiết. Nếu chúng ta đã tham lam chiếm đoạt`1`đầu tiên, chúng tôi vẫn sẽ thành công ở đây, nhưng trong những trường hợp phức tạp hơn như`14144`, việc phân chia sớm không chính xác sẽ thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d) | Mỗi chữ số được xử lý nhiều nhất một lần như một phần của số lần kiểm tra chuỗi con không đổi | 
| Không gian | O(1) | Chỉ một vài con trỏ và chuỗi đầu vào được lưu trữ | 

Kích thước đầu vào tối đa là 10 chữ số, do đó, ngay cả quá trình quét tuyến tính đơn giản nhất cũng nhanh chóng. Giải pháp nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin

    s = stdin.readline().strip()
    i = 0
    n = len(s)
    ok = True

    while i < n:
        if i + 3 <= n and s[i:i+3] == "144":
            i += 3
        elif i + 2 <= n and s[i:i+2] == "14":
            i += 2
        elif s[i] == "1":
            i += 1
        else:
            ok = False
            break

    return "YES" if ok and i == n else "NO"

# provided sample
assert run("114114\n") == "YES"

# custom cases
assert run("1\n") == "YES", "single valid token"
assert run("14\n") == "YES", "two-digit token"
assert run("144\n") == "YES", "three-digit token"
assert run("1414\n") == "YES", "mixed valid composition"
assert run("1444\n") == "NO", "invalid tail digit"
assert run("514\n") == "NO", "invalid prefix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`| CÓ | mã thông báo hợp lệ nhỏ nhất | 
|`1444`| KHÔNG | hậu tố không hợp lệ sau tiền tố hợp lệ | 
|`1414`| CÓ | kết hợp hỗn hợp lặp đi lặp lại | 
|`514`| KHÔNG | chữ số bắt đầu không hợp lệ | 

## Vỏ cạnh 

Một trường hợp phức tạp xuất hiện khi các quyết định tham lam có thể có vẻ như có thể thay thế cho nhau nhưng thực tế không phải vậy. Coi như`14144`. 

Nếu chúng tôi xử lý nó: 

Bắt đầu lúc`1`, sau đó`41...`gây ra vấn đề trừ khi chúng ta căn chỉnh các phân đoạn một cách cẩn thận. Sự phân hủy đúng là`14 | 144`. Thuật toán đảm bảo điều này bằng cách luôn cố gắng`144`Và`14`trước khi rơi trở lại`1`. 

Từng bước một: 

đầu vào:`14144`Chúng tôi kiểm tra: 

Tại i=0,`14`trận đấu, vì vậy chúng tôi tiêu thụ`14`. 

Tại i=2, chuỗi con là`144`, vì vậy chúng tôi tiêu thụ`144`. 

Tại i=5, chúng ta đã hoàn thành. 

Điều này cho thấy thứ tự tham lam ngăn chặn sự phân mảnh sớm và đảm bảo sự phân tách hợp lệ bất cứ khi nào tồn tại.
