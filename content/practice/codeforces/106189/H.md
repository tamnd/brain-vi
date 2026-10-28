---
title: "CF 106189H - IPv6"
description: "Nhiệm vụ là chuẩn hóa cách biểu diễn địa chỉ IPv6. Đầu vào là một chuỗi đơn bao gồm các nhóm thập lục phân được phân tách bằng dấu hai chấm."
date: "2026-06-20T04:20:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106189
codeforces_index: "H"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2025"
rating: 0
weight: 106189
solve_time_s: 51
verified: true
draft: false
---

[CF 106189H - IP v6](https://codeforces.com/problemset/problem/106189/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là chuẩn hóa cách biểu diễn địa chỉ IPv6. Đầu vào là một chuỗi đơn bao gồm các nhóm thập lục phân được phân tách bằng dấu hai chấm. Trong dạng IPv6 mở rộng hợp lệ phải luôn có chính xác tám nhóm, mỗi nhóm biểu thị một giá trị 16 bit được viết dưới dạng tối đa bốn chữ số thập lục phân. Tuy nhiên, đầu vào có thể sử dụng dạng nén trong đó một hoặc nhiều nhóm liên tiếp bị bỏ qua, được biểu thị bằng dấu hai chấm kép hoặc các phân đoạn bị thiếu do các dấu phân cách liền kề gây ra. 

Mục đích là để xây dựng lại hình thức kinh điển đầy đủ. Điều đó có nghĩa là chúng ta phải giải thích các dấu phân cách, khôi phục các nhóm bị thiếu bằng số 0 và đảm bảo rằng biểu diễn cuối cùng có chính xác tám thành phần. Mỗi thành phần phải được viết dưới dạng chuỗi thập lục phân gồm bốn ký tự, được đệm bằng các số 0 đứng đầu nếu cần. 

Ràng buộc kích thước đầu vào không lớn một cách rõ ràng trong câu lệnh, nhưng loại vấn đề này luôn được thiết kế để phân tích cú pháp tuyến tính. Bất kỳ giải pháp nào liên tục phân tách, chèn hoặc dịch chuyển mảng không hiệu quả vẫn sẽ đạt, nhưng có thể tránh được hành vi bậc hai không cần thiết. 

Ví dụ: trường hợp cạnh tinh tế xuất hiện khi địa chỉ bắt đầu hoặc kết thúc bằng cách nén`"::1"`hoặc`"2001::"`. Một trường hợp góc khác là nhiều phần bị thiếu liên tiếp, trong đó có nhiều hơn một nhóm được coi là bằng 0. Một cách tiếp cận dựa trên sự phân tách đơn giản giả định rằng mọi chuỗi con trống tương ứng với chính xác một nhóm sẽ không thành công khi quá trình nén biểu thị nhiều phân đoạn bị thiếu. 

Ví dụ,`"1::1"`nên trở thành bảy nhóm 0 giữa hai nhóm, không chỉ là một nhóm 0 duy nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận giải thích trực tiếp là tách chuỗi theo ký tự dấu hai chấm và coi mỗi phân đoạn kết quả là một nhóm. Vấn đề là việc nén IPv6 phá vỡ giả định này. Khi hai dấu hai chấm xuất hiện liên tiếp, phần phân tách tạo ra một chuỗi trống, nhưng chuỗi trống đó không đại diện cho một nhóm 0, nó đại diện cho toàn bộ khối các nhóm bị thiếu có kích thước phụ thuộc vào số lượng phân đoạn đã có ở nơi khác trong địa chỉ. 

Lý do đúng là coi địa chỉ có tổng cộng tám phân đoạn cố định. Sau khi tách, chúng tôi đếm có bao nhiêu nhóm không trống. Sự khác biệt giữa tám và số lượng này là số lượng nhóm 0 phải được chèn vào vị trí xuất hiện đoạn trống. 

Khi cấu trúc được khôi phục, mỗi phân đoạn sẽ được chuẩn hóa một cách độc lập. Mỗi nhóm phải được chuyển đổi thành số nguyên từ hệ thập lục phân và được in với chính xác bốn chữ số, giữ nguyên các số 0 đứng đầu. 

Quan sát quan trọng là quá trình nén xảy ra nhiều nhất một lần trong một địa chỉ IPv6 hợp lệ, do đó chỉ có nhiều nhất một vùng cần mở rộng. Điều đó đảm bảo một điểm chèn duy nhất cho các khối bằng 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân chia trực tiếp mà không cần xử lý nén | O(n) | O(n) | Sai | 
| Tách + xây dựng lại mà không cần mở rộng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia chuỗi đầu vào bằng ký tự dấu hai chấm thành các đoạn thô. Điều này bảo tồn các chuỗi trống tương ứng với các điểm nén. Cấu trúc của mảng phân chia mã hóa nơi xảy ra các khối bị thiếu. 
2. Đếm xem có bao nhiêu đoạn không trống. Điều này cho chúng tôi biết có bao nhiêu nhóm rõ ràng đã được cung cấp trong địa chỉ. 
3. Xác định có bao nhiêu nhóm bị thiếu bằng cách lấy 8 trừ đi số nhóm rõ ràng. Giá trị này là số nhóm 0 phải được chèn vào. 
4. Xây dựng danh sách cuối cùng bằng cách quét các phân đoạn được chia. Bất cứ khi nào gặp một đoạn trống, nó đại diện cho điểm nén. Thay thế vị trí đó bằng số lượng yêu cầu`"0000"`các nhóm. 
5. Đối với mọi phân đoạn không trống, hãy chuẩn hóa nó bằng cách đệm trái bằng các số 0 cho đến khi nó dài bốn ký tự. Điều này đảm bảo sự biểu diễn thống nhất của từng khối 16 bit. 
6. Sau khi xây dựng lại, đảm bảo kết quả chứa chính xác tám nhóm. Hãy nối chúng bằng dấu hai chấm để tạo thành địa chỉ IPv6 cuối cùng. 

Ý tưởng cấu trúc quan trọng là dạng nén chỉ loại bỏ một khối liền kề gồm 0 nhóm, do đó tất cả thông tin bị thiếu sẽ được bản địa hóa và có thể được khôi phục một cách xác định. 

### Tại sao nó hoạt động 

Quy tắc nén IPv6 đảm bảo rằng nhiều nhất một chuỗi các nhóm 0 liên tiếp được thay thế bằng dấu hai chấm kép. Điều này có nghĩa là biểu diễn phân tách chứa chính xác một vị trí cần mở rộng. Vì tổng số nhóm được cố định là 8 nên độ dài còn thiếu được xác định duy nhất. Mọi hoạt động tái tạo hợp lệ phải bảo toàn các nhóm rõ ràng theo thứ tự và chỉ vùng nén là linh hoạt, do đó, việc lấp đầy nó bằng số 0 được tính toán sẽ mang lại một dạng chính tắc duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def normalize(group: str) -> str:
    return group.zfill(4)

s = input().strip()
parts = s.split(':')

# count explicit groups
explicit = sum(1 for x in parts if x != '')

missing = 8 - explicit

res = []
i = 0
n = len(parts)

while i < n:
    if parts[i] == '':
        if missing > 0:
            res.extend(['0000'] * missing)
            missing = 0
        i += 1
    else:
        res.append(normalize(parts[i]))
        i += 1

print(':'.join(res))
```Giải pháp này hoạt động bằng cách trước tiên phân tích cú pháp cấu trúc của các dải phân cách, sau đó chèn các khối 0 bị thiếu chính xác một lần vào điểm nén. Bước chuẩn hóa đảm bảo mỗi nhóm có bốn chữ số thập lục phân. Một điểm tinh tế là chúng ta chỉ được sử dụng phân đoạn trống một lần; nhiều mục trống có thể xuất hiện tùy thuộc vào cách nén được đặt, nhưng chỉ một trong số chúng đại diện cho trang mở rộng. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`"2001:db8::1"`. 

Chúng tôi chia thành`["2001", "db8", "", "1"]`. Có ba nhóm rõ ràng, vì vậy năm nhóm bị thiếu. Mục nhập trống sẽ kích hoạt việc chèn năm`"0000"`các nhóm. Trình tự cuối cùng trở thành: 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | đọc 2001 | [2001] | 
| 2 | đọc db8 | [2001, 0db8] | 
| 3 | khối trống mở rộng | [2001, 0db8, 0000, 0000, 0000, 0000, 0000] | 
| 4 | đọc 1 | [2001, 0db8, 0000, 0000, 0000, 0000, 0001] | 

Đầu ra cuối cùng là`"2001:0db8:0000:0000:0000:0000:0000:0001"`. 

Bây giờ hãy xem xét`"::1"`. 

Chia tách mang lại`["", "", "1"]`. Có một nhóm rõ ràng nên bảy nhóm bị thiếu. Vùng trống mở rộng thành bảy khối 0 và sau đó là khối cuối cùng`"1"`được bình thường hóa. 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | trống mở rộng | bảy 0000 khối | 
| 2 | đọc 1 | nối thêm 0001 | 

Kết quả trở thành`"0000:0000:0000:0000:0000:0000:0000:0001"`. 

Những dấu vết này xác nhận rằng đoạn trống luôn đóng vai trò giữ chỗ cho toàn bộ vùng nén. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | IPv6 đã cố định tối đa 8 phân đoạn nên việc xử lý bị giới hạn không đổi | 
| Không gian | O(1) | chỉ một số nhóm cố định được lưu trữ | 

Thuật toán dễ dàng phù hợp với mọi giới hạn hợp lý vì kích thước cấu trúc không bao giờ tăng vượt quá giới hạn không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    s = input().strip()
    parts = s.split(':')

    explicit = sum(1 for x in parts if x != '')
    missing = 8 - explicit

    res = []
    i = 0
    while i < len(parts):
        if parts[i] == '':
            if missing > 0:
                res.extend(['0000'] * missing)
                missing = 0
            i += 1
        else:
            res.append(parts[i].zfill(4))
            i += 1

    return ':'.join(res)

# basic
assert run("1::1\n") == "0001:0000:0000:0000:0000:0000:0000:0001"
# already full
assert run("abcd:0:0:0:0:0:0:1\n") == "abcd:0000:0000:0000:0000:0000:0000:0001"
# leading compression
assert run("::\n") == "0000:0000:0000:0000:0000:0000:0000:0000"
# trailing compression
assert run("2001:db8::\n") == "2001:0db8:0000:0000:0000:0000:0000:0000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1::1`| khối giữa mở rộng | xử lý nén trung tâm | 
| địa chỉ đầy đủ | nhóm đệm | độ chính xác chuẩn hóa | 
|`::`| tất cả số không | trường hợp nén đầy đủ | 
| theo sau`::`| mở rộng hậu tố | xử lý vị trí cạnh | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi quá trình nén xuất hiện ở đầu chuỗi, tạo ra đoạn trống ban đầu sau khi tách. Trong trường hợp này, thuật toán phải đảm bảo rằng việc mở rộng xảy ra trước khi bất kỳ nhóm thực nào được xử lý. Logic xây dựng xử lý việc này vì phân đoạn trống đầu tiên sẽ kích hoạt việc chèn ngay các khối 0 bị thiếu. 

Một trường hợp khác là khi đầu vào đã chứa tám nhóm rõ ràng mà không cần nén. Ở đây không có phân đoạn trống nào tồn tại, do đó số lượng bị thiếu trở thành 0 và không xảy ra sự mở rộng. Thuật toán chỉ đơn giản là chuẩn hóa từng nhóm một cách độc lập, duy trì tính chính xác mà không cần sửa đổi.
