---
title: "CF 103351B - A+B"
description: "Nhiệm vụ này là khối xây dựng số học cổ điển: chúng ta được cấp các cặp số nguyên và với mỗi cặp chúng ta phải xuất tổng của chúng. Mỗi dòng đầu vào đại diện cho hai số cần được kết hợp trực tiếp, không có cấu trúc bổ sung như biểu đồ hoặc mảng."
date: "2026-07-03T13:30:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103351
codeforces_index: "B"
codeforces_contest_name: "SDU Open 2021 Fall"
rating: 0
weight: 103351
solve_time_s: 52
verified: true
draft: false
---

[CF 103351B - A+B](https://codeforces.com/problemset/problem/103351/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này là khối xây dựng số học cổ điển: chúng ta được cấp các cặp số nguyên và với mỗi cặp chúng ta phải xuất tổng của chúng. Mỗi dòng đầu vào đại diện cho hai số cần được kết hợp trực tiếp, không có cấu trúc bổ sung như biểu đồ hoặc mảng. Đầu ra đơn giản là số nguyên kết quả cho mỗi cặp, được viết theo thứ tự. 

Mặc dù vấn đề có vẻ tầm thường nhưng các ràng buộc vẫn có ý nghĩa quan trọng trong thực tế. Các bài toán thuộc loại này thường cho phép số nguyên rất lớn, thường có giá trị tuyệt đối lên tới 10^18. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào dựa trên các giả định số nguyên có chiều rộng cố định trong các ngôn ngữ có rủi ro tràn. Trong Python, điều này được xử lý một cách tự nhiên, nhưng trong các ngôn ngữ khác, nó buộc phải sử dụng cẩn thận các loại 64-bit. 

Kích thước đầu vào cũng rất quan trọng. Vì mỗi dòng là độc lập nên tổng số cặp có thể lớn, có thể lên tới 10^5 hoặc hơn. Điều này có nghĩa là mọi chi phí cho mỗi lần kiểm tra vượt quá thời gian không đổi trên mỗi dòng, chẳng hạn như logic phân tích cú pháp lặp lại hoặc các cấu trúc dữ liệu không cần thiết, vẫn sẽ vượt qua nhưng về mặt khái niệm nên tránh. 

Có một số trường hợp phức tạp có thể phá vỡ việc triển khai ngây thơ. Người ta giả định chỉ tồn tại một trường hợp thử nghiệm duy nhất. Ví dụ: nếu đầu vào là:```
1 2
3 4
```đầu ra đúng là:```
3
7
```Việc triển khai sai chỉ đọc một dòng sẽ chỉ xuất ra sai`3`. 

Một trường hợp khác là số âm, chẳng hạn như:```
-5 2
```Đầu ra đúng là`-3`. Phương pháp phân tách hoặc phân tích chuỗi dễ vỡ không xử lý chính xác dấu trừ sẽ không thành công ở đây. 

Cuối cùng, các giá trị rất lớn như:```
1000000000000000000 1000000000000000000
```phải được xử lý mà không bị tràn trong các ngôn ngữ số nguyên có kích thước cố định. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu đã là giải pháp được mong đợi: đọc từng cặp số nguyên, tính tổng của chúng và in ra. Không có cấu trúc tổ hợp hoặc vấn đề tối ưu hóa nào ẩn đằng sau nó. Tính đúng đắn xuất phát trực tiếp từ định nghĩa của phép cộng. 

Nếu chúng tôi mô tả một phiên bản “kém tối ưu hơn” thì nó sẽ chỉ khác về cách xử lý đầu vào. Ví dụ: đọc toàn bộ tệp vào danh sách mã thông báo và liên tục chuyển đổi chuỗi con thành số nguyên vẫn đúng, nhưng nó gây ra chi phí không cần thiết. Một biến thể kém hiệu quả khác có thể xây dựng lại chuỗi hoặc thực hiện phân tích cú pháp dư thừa cho mỗi ký tự, tăng hệ số không đổi mà không làm thay đổi độ phức tạp tiệm cận. 

Quan sát quan trọng là mỗi cặp đều độc lập. Không có kết quả nào phụ thuộc vào các tính toán trước đó, vì vậy chúng tôi không bao giờ cần lưu trữ ngoài dòng hiện tại. Điều đó làm giảm vấn đề xuống còn một lần quét tuyến tính trên đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân tích và tổng hợp từng dòng | O(n) | O(1) | Đã chấp nhận | 
| Bộ đệm mã thông báo với chi phí phân tích cú pháp lặp đi lặp lại | O(n) | O(n) | Được chấp nhận nhưng không cần thiết | 

Ở đây n là số dòng đầu vào. 

## Hướng dẫn thuật toán 

1. Đọc đầu vào cho đến cuối tệp, coi mỗi dòng là một cặp số nguyên riêng biệt. Điều này đảm bảo chúng tôi xử lý số lượng trường hợp thử nghiệm tùy ý mà không cần số lượng được xác định trước. 
2. Đối với mỗi dòng, chia nó thành hai mã số nguyên. Cấu trúc cố định nên chúng tôi luôn mong đợi chính xác hai giá trị trên mỗi dòng. 
3. Chuyển đổi cả hai mã thông báo thành số nguyên. Bước này là cần thiết để đảm bảo phép cộng số học thay vì nối chuỗi. 
4. Tính tổng của hai số nguyên ngay sau khi phân tích cú pháp. Vì mỗi dòng là độc lập nên không cần lưu trữ trung gian. 
5. Xuất kết quả trước khi chuyển sang dòng tiếp theo. Điều này giúp duy trì mức sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là mỗi dòng đầu vào xác định một biểu thức số học độc lập. Không có sự phụ thuộc ẩn giữa các dòng, do đó việc tính toán từng tổng riêng biệt sẽ tạo ra giải pháp tổng thể. Thuật toán duy trì tính bất biến là sau khi xử lý k dòng, sẽ xuất ra chính xác k tổng đúng, mỗi tổng tương ứng với cặp đầu vào thứ k. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    for line in sys.stdin:
        line = line.strip()
        if not line:
            continue
        a, b = map(int, line.split())
        print(a + b)

if __name__ == "__main__":
    main()
```Giải pháp xử lý đầu vào theo kiểu truyền phát bằng cách sử dụng`sys.stdin`, giúp tránh lưu trữ toàn bộ tập dữ liệu trong bộ nhớ. Mỗi dòng được tách ra và phân chia chính xác một lần, đảm bảo công việc liên tục cho mỗi trường hợp thử nghiệm. 

Một chi tiết triển khai tinh tế là xử lý các dòng trống. Đầu vào lập trình cạnh tranh đôi khi có thể bao gồm các dòng mới ở cuối, vì vậy việc bỏ qua các chuỗi trống sẽ ngăn ngừa các lỗi phân tích cú pháp ngẫu nhiên. Một chi tiết quan trọng khác là sử dụng`map(int, ...)`trực tiếp, điều này tránh việc xây dựng danh sách trung gian và giữ cho mã ngắn gọn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2
10 20
-5 7
```| Bước | một | b | a + b | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | 
| 2 | 10 | 20 | 30 | 
| 3 | -5 | 7 | 2 | 

Dấu vết này cho thấy mỗi dòng được xử lý độc lập và việc tính toán hoàn toàn mang tính cục bộ. Không có trạng thái nào được thực hiện qua các lần lặp, xác nhận giả định về tính độc lập. 

### Ví dụ 2 

đầu vào:```
1000000000000000000 1
-100 100
0 0
```| Bước | một | b | a + b | 
| --- | --- | --- | --- | 
| 1 | 10^18 | 1 | 10000000000000000001 | 
| 2 | -100 | 100 | 0 | 
| 3 | 0 | 0 | 0 | 

Điều này thể hiện việc xử lý đúng các số nguyên lớn và các trường hợp hủy bỏ. Dòng đầu tiên cũng xác minh rằng số học có độ chính xác tùy ý hoạt động chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi dòng trong số n dòng được phân tích cú pháp và xử lý chính xác một lần | 
| Không gian | O(1) | Chỉ có hai số nguyên được lưu trữ bất cứ lúc nào | 

Thuật toán tuyến tính về số lượng dòng đầu vào, điều này là tối ưu vì mỗi dòng phải được đọc ít nhất một lần. Việc sử dụng bộ nhớ không đổi do không cần tích lũy kết quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO as _StringIO

    out = _StringIO()
    backup = sys.stdout
    sys.stdout = out

    try:
        for line in sys.stdin:
            line = line.strip()
            if not line:
                continue
            a, b = map(int, line.split())
            print(a + b)
    finally:
        sys.stdout = backup

    return out.getvalue().strip()

# provided samples
assert run("1 2\n3 4\n") == "3\n7"

# custom cases
assert run("-1 1\n") == "0", "zero cancellation"
assert run("0 0\n5 5\n") == "0\n10", "zeros and small positives"
assert run("1000000000000000000 1000000000000000000\n") == "2000000000000000000", "large values"
assert run("   2    3   \n") == "5", "extra whitespace handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`-1 1`|`0`| xử lý và hủy bỏ biển báo | 
|`0 0 / 5 5`|`0 / 10`| nhiều dòng và giá trị bằng 0 | 
| số nguyên lớn | số tiền lớn | số học an toàn tràn | 
| đầu vào cách nhau | tổng đúng | độ mạnh của phân tích cú pháp | 

## Vỏ cạnh 

Đối với đầu vào nhiều dòng, thuật toán xử lý chính xác từng cặp một cách độc lập. Ví dụ: 

đầu vào:```
1 2
3 4
```Việc thực thi xử lý dòng đầu tiên, kết quả đầu ra`3`, sau đó chuyển sang dòng tiếp theo và xuất ra`7`. Tính bất biến của vòng lặp được giữ nguyên vì sau mỗi lần lặp, chính xác một dòng được sử dụng và một kết quả được tạo ra. 

Đối với số âm: 

đầu vào:```
-5 2
```Bước phân tích cú pháp chuyển đổi chính xác mã thông báo thành số nguyên có dấu và tổng được tính là`-3`. Không có sự mơ hồ trong cách trình bày vì`int()`giải thích chính xác dấu trừ. 

Đối với các giá trị rất lớn: 

đầu vào:```
1000000000000000000 1000000000000000000
```Kiểu số nguyên của Python tự động mở rộng độ chính xác, do đó tổng được tính chính xác mà không bị tràn. Thuật toán không đưa ra bất kỳ chuyển đổi thu hẹp trung gian nào, do đó tính chính xác được giữ nguyên.
