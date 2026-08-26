---
title: "CF 104339D - mã hóa base64"
description: "Chúng tôi được cung cấp một luồng byte, đã được cung cấp dưới dạng giá trị thập lục phân và chúng tôi cần chuyển đổi dữ liệu nhị phân thô đó thành chuỗi được mã hóa Base64 bằng bảng chữ cái tiêu chuẩn ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/."
date: "2026-07-01T18:38:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104339
codeforces_index: "D"
codeforces_contest_name: "FAMCS Olympiad for scholars, Qualification (copy)"
rating: 0
weight: 104339
solve_time_s: 62
verified: true
draft: false
---

[CF 104339D - mã hóa base64](https://codeforces.com/problemset/problem/104339/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một luồng byte, đã được cung cấp dưới dạng giá trị thập lục phân và chúng tôi cần chuyển đổi dữ liệu nhị phân thô đó thành chuỗi được mã hóa Base64 bằng bảng chữ cái tiêu chuẩn`ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/`. 

Mã hóa hoạt động bằng cách nhóm các byte đầu vào thành ba phần. Mỗi nhóm ba byte tạo thành một số nguyên 24 bit. Khối 24 bit đó sau đó được chia thành bốn đoạn 6 bit và mỗi đoạn được sử dụng làm chỉ mục trong bảng chữ cái Base64 để tạo ra bốn ký tự. Nếu nhóm cuối cùng có ít hơn ba byte, về mặt khái niệm, chúng ta vẫn xây dựng khối 24 bit bằng cách đệm các byte bị thiếu bằng các bit 0, nhưng đầu ra bị cắt bớt và được đệm bằng`=`để độ dài cuối cùng trở thành bội số của 4 ký tự. 

Đầu ra là một chuỗi Base64 liên tục duy nhất đại diện cho toàn bộ đầu vào. 

Các ràng buộc cho phép lên tới 50.000 byte. Mô phỏng trực tiếp trên mỗi byte hoặc mỗi bit là hoàn toàn tốt vì công việc là tuyến tính ở kích thước đầu vào. Bất cứ điều gì tệ hơn O(n) sẽ là không cần thiết, vì ngay cả một vài triệu thao tác nguyên thủy cũng dễ dàng đủ nhanh trong Python. 

Trường hợp cạnh tinh tế xuất hiện khi độ dài đầu vào không chia hết cho ba. Ví dụ: một đầu vào byte đơn như`0F`phải tạo ra hai ký tự Base64 theo sau là`==`, không phải bốn ký tự được tính toán với cách diễn giải sai các bit đệm. Việc triển khai bất cẩn thường quên ngăn chặn đầu ra bổ sung hoặc xử lý sai việc dịch chuyển phần đệm. 

Một vấn đề khác là nhóm byte ở ranh giới. Ví dụ: với hai byte, thuật toán vẫn tạo thành khối 24 bit bằng cách dịch chuyển về số 0, nhưng chỉ có ba ký tự Base64 hợp lệ trước khi đệm. Việc che giấu không chính xác hoặc không phân biệt được byte “thực” và byte “đệm” sẽ dẫn đến các ký hiệu ở cuối không chính xác. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng trực tiếp các hoạt động bit cho mỗi byte, nối các bit thành một chuỗi hoặc số nguyên đang phát triển, sau đó cắt nó thành các đoạn 6 bit. Điều này hiệu quả vì Base64 về cơ bản là một phép chuyển đổi đóng gói bit. Tuy nhiên, việc xây dựng và cắt các chuỗi bit liên tục tạo ra chi phí tỷ lệ thuận với tổng số bit có hệ số không đổi lớn và việc nối chuỗi đơn giản trong các vòng lặp có thể làm giảm hiệu suất đáng kể đối với 50.000 byte. 

Quan sát quan trọng là Base64 hoạt động trong các khối có kích thước cố định. Cứ 3 byte độc ​​lập tạo ra 4 ký tự. Điều này cho phép chúng tôi xử lý đầu vào theo khối mà không cần duy trì bộ đệm bit ngày càng tăng. Thay vì mô phỏng các bit một cách rõ ràng, chúng tôi tính toán trực tiếp số nguyên 24 bit bằng cách sử dụng các phép dịch chuyển và phép toán OR theo bit, sau đó trích xuất bốn chỉ số 6 bit bằng cách sử dụng các ca. 

Đối với khối một phần cuối cùng, chúng tôi vẫn tính giá trị 24 bit theo cách tương tự, nhưng chúng tôi chỉ phát ra 2 hoặc 3 ký tự đầu tiên tùy thuộc vào việc chúng tôi có 1 hay 2 byte, tiếp theo là phần đệm bắt buộc. 

Điều này làm giảm vấn đề xuống còn quét tuyến tính đơn giản trên mảng byte với quá trình xử lý theo thời gian không đổi trên mỗi khối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng chuỗi bit Brute Force | O(n) với hằng số cao | O(n) | Thực hành quá chậm | 
| Thao tác bit theo khối | O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các byte đầu vào theo nhóm ba. 

1. Đọc tất cả byte dưới dạng số nguyên từ chuỗi thập lục phân. Điều này mang lại cho chúng ta các giá trị số trực tiếp, tránh việc phân tích cú pháp lặp lại sau này. 
2. Lặp lại mảng byte theo các bước ba. Đối với mỗi nhóm đầy đủ: 

Chúng tôi xây dựng một số nguyên 24 bit bằng cách dịch chuyển byte đầu tiên sang trái 16 bit, byte thứ hai 8 bit và thêm nguyên trạng thứ ba. Điều này duy trì trật tự ban đầu của chúng trong một giá trị đóng gói duy nhất. 
3. Từ số nguyên 24 bit này, trích ra bốn đoạn 6 bit. Chúng tôi thực hiện điều này bằng cách dịch chuyển sang phải lần lượt 18, 12, 6 và 0 bit, sau đó che dấu bằng`63`để cô lập 6 bit cuối cùng. Mỗi giá trị lập chỉ mục vào bảng chữ cái Base64. 
4. Nối bốn ký tự vào chuỗi đầu ra. 
5. Khi còn lại ít hơn ba byte, hãy tạo cùng một bộ đệm 24 bit nhưng coi các byte bị thiếu là 0. Nếu chỉ còn lại một byte, chúng tôi tạo hai ký tự Base64 hợp lệ và nối thêm`==`. Nếu vẫn còn hai byte, chúng tôi tạo ba ký tự và nối thêm`=`. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là Base64 là sự kết hợp giữa các khối 24 bit và bốn ký hiệu 6 bit. Mỗi nhóm đầy đủ ba byte đóng góp chính xác 24 bit và việc chia chúng thành các cửa sổ 6 bit cố định sẽ duy trì cấu trúc nhị phân ban đầu mà không bị chồng chéo. Phần đệm không làm thay đổi tiền tố được mã hóa vì các bit bị thiếu được điền bằng 0 một cách nhất quán trong cả định nghĩa và triển khai mã hóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

ALPHABET = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"

def encode_base64(n, data):
    out = []

    i = 0
    while i < n:
        b1 = data[i]
        i += 1

        b2 = data[i] if i < n else None
        i += 1 if i < n else 0

        b3 = data[i] if i < n else None
        i += 1 if i < n else 0

        # build 24-bit buffer
        x = b1 << 16
        if b2 is not None:
            x |= b2 << 8
        if b3 is not None:
            x |= b3

        # always compute full 4 indices
        c1 = ALPHABET[(x >> 18) & 63]
        c2 = ALPHABET[(x >> 12) & 63]
        c3 = ALPHABET[(x >> 6) & 63]
        c4 = ALPHABET[x & 63]

        if b2 is None:
            out.append(c1 + c2 + "==")
        elif b3 is None:
            out.append(c1 + c2 + c3 + "=")
        else:
            out.append(c1 + c2 + c3 + c4)

    return "".join(out)

def main():
    n = int(input().strip())
    data = list(map(lambda x: int(x, 16), input().split()))
    print(encode_base64(n, data))

if __name__ == "__main__":
    main()
```Việc triển khai tiếp tục xử lý nghiêm ngặt theo khối tối đa ba byte. Bước đóng gói bit bằng cách sử dụng ca đảm bảo chúng ta không bao giờ cần quản lý rõ ràng các chuỗi nhị phân. Việc che đậy bằng`& 63`đảm bảo rằng mỗi phân đoạn được trích xuất là chỉ mục Base64 hợp lệ. 

Phải cẩn thận khi xử lý khối cuối cùng. Logic kiểm tra rõ ràng xem byte thứ hai hay thứ ba có tồn tại hay không và quyết định phần đệm dựa trên đó. Một lỗi phổ biến là luôn xuất ra bốn ký tự rồi thêm phần đệm, điều này tạo ra mã hóa không chính xác cho các khối cuối cùng ngắn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
43 61 74
```| Bước | Byte | Giá trị 24-bit | Chỉ số (khối 6 bit) | Đoạn đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 43 61 74 | đóng gói đầy đủ | 16, 34, 25, 52 | Q2F0 | 

Ví dụ này là một khối đầy đủ gồm ba byte, do đó không có phần đệm xảy ra. Thuật toán tạo ra bốn ký tự trực tiếp. 

Dấu vết hiển thị ánh xạ rõ ràng từ việc đóng gói 24 bit đến bốn ký hiệu Base64, xác nhận tính chính xác của các khối hoàn chỉnh. 

### Ví dụ 2 

đầu vào:```
4
0F DD A4 12
```| Bước | Byte | Giá trị 24-bit | Chỉ số | Đoạn đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 0F DD A4 | khối đầy đủ | 3, 61, 40, 36 | D92k | 
| 2 | 12 | khối một phần | 4, 18, 0, 0 | Ví dụ== | 

Nhóm đầu tiên hoạt động giống như mã hóa đầy đủ thông thường. Nhóm thứ hai chỉ có một byte, vì vậy hai byte còn lại được coi là phần đệm bằng 0. Chỉ hai ký tự Base64 đầu tiên có ý nghĩa và kết quả đầu ra kết thúc bằng`==`. 

Dấu vết này nêu bật cách phần đệm ngăn chặn các ký tự được mã hóa dư thừa trong khi vẫn tính toán phân tách 6 bit đầy đủ bên trong. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi byte được xử lý chính xác một lần trong các hoạt động bit có thời gian không đổi | 
| Không gian | O(n) | Chuỗi đầu ra lưu trữ sự mở rộng liên tục của kích thước đầu vào | 

Thuật toán chia tỷ lệ tuyến tính với kích thước đầu vào, tối ưu cho phép chuyển đổi phải đọc từng byte ít nhất một lần. Với 50.000 byte, tổng số thao tác vẫn nằm trong giới hạn của Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return io.StringIO(sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else "")

# helper redefinition for clean runs
def run(inp: str) -> str:
    import sys, io
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    try:
        main()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = backup_stdin
        sys.stdout = backup_stdout

assert run("3\n43 61 74\n") == "Q2F0"
assert run("4\n0F DD A4 12\n") == "D92kEg=="

# single byte
assert run("1\n00\n") == "AA=="

# two bytes
assert run("2\nFF FF\n") == "//8="

# all equal pattern
assert run("3\n00 00 00\n") == "AAAA"

# maximum small repeat pattern
assert run("6\n41 42 43 44 45 46\n") == "QUJDREVG"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 byte số 0 | AA== | đệm một byte | 
| 2 byte FF FF | //8= | trường hợp ranh giới hai byte | 
| 3 byte 0 | AAAA | độ chính xác toàn bộ khối | 
| Byte ABCDEF | QUJDREVG | nhiều khối đầy đủ | 

## Vỏ cạnh 

Đầu vào một byte như`00`thực hiện logic đệm trực tiếp. Thuật toán xây dựng khối 24 bit`0x000000`, trích xuất tất cả các chỉ số bằng 0, tạo ra`AAAA`, và sau đó cắt ngắn thành`AA==`. Điều này xác nhận rằng việc loại bỏ các ký tự phụ xảy ra sau khi trích xuất chứ không phải trước đó. 

Một đầu vào hai byte như`FF FF`tạo ra giá trị 24 bit đầy đủ`0xFFFF00`. Ba ký tự Base64 đầu tiên hợp lệ, nhưng ký tự thứ tư bị loại bỏ và thay thế bằng`=`. Cách tiếp cận shift-and-mask đảm bảo chúng tôi vẫn tính toán giá trị trung gian chính xác mà không cần đặt phép toán bit đặc biệt, chỉ có độ dài đầu ra là khác.
