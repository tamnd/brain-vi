---
title: "CF 106486L - \u7528\u6237\u540d\u957f\u5ea6\u5e94\u5728 5 \u4f4d\u5230 12 \u4f4d\u4e4b\u95f4"
description: "Nhiệm vụ tập trung vào việc xác thực tên người dùng hoàn toàn dựa trên độ dài của nó. Chúng tôi được cấp một mã định danh duy nhất và yêu cầu duy nhất là kiểm tra xem độ dài của nó có nằm trong phạm vi bao gồm cố định hay không."
date: "2026-06-19T15:15:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106486
codeforces_index: "L"
codeforces_contest_name: "Dalian University of Technology, Software College 2025 Freshman Contest"
rating: 0
weight: 106486
solve_time_s: 42
verified: true
draft: false
---

[CF 106486L - \u7528\u6237\u540d\u957f\u5ea6\u5e94\u5728 5 \u4f4d\u5230 12 \u4f4d\u4e4b\u95f4](https://codeforces.com/problemset/problem/106486/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ tập trung vào việc xác thực tên người dùng hoàn toàn dựa trên độ dài của nó. Chúng tôi được cấp một mã định danh duy nhất và yêu cầu duy nhất là kiểm tra xem độ dài của nó có nằm trong phạm vi bao gồm cố định hay không. Nếu độ dài thỏa mãn ràng buộc thì chúng ta chấp nhận nó, nếu không thì chúng ta sẽ từ chối nó. 

Từ góc độ dữ liệu, đầu vào chỉ là một chuỗi đại diện cho tên người dùng. Không có tham số bổ sung, không có nhiều trường hợp thử nghiệm và không có cấu trúc ẩn như biểu đồ hoặc mảng. Đầu ra là một phán đoán đơn giản dựa trên việc độ dài chuỗi có nằm trong giới hạn cho phép hay không. 

Các ràng buộc là cực kỳ nhỏ về mặt tính toán. Ngay cả khi chúng ta giả sử chuỗi đầu vào có thể lên tới vài triệu ký tự thì chỉ cần quét tuyến tính một lần trên chuỗi là đủ. Điều đó có nghĩa là mọi giải pháp chạy trong thời gian O(n) đều đã tối ưu vì ít nhất chúng ta phải đọc đầu vào một lần. Bất cứ điều gì phức tạp hơn thế sẽ là chi phí không cần thiết. 

Các trường hợp cạnh chính đều liên quan đến hành vi độ dài biên. Tên người dùng có độ dài chính xác là 5 sẽ được chấp nhận và tên người dùng có độ dài chính xác là 12 cũng nên được chấp nhận. Một chuỗi ngắn hơn 5, chẳng hạn như "abc" có độ dài 3, phải bị từ chối. Tương tự, một chuỗi dài hơn 12, chẳng hạn như "abcdefghijklmnop", phải bị từ chối. 

Một cạm bẫy tinh vi đến từ việc định dạng đầu vào. Nếu đầu vào chứa các ký tự dòng mới hoặc dấu cách, việc kiểm tra độ dài đơn giản mà không loại bỏ có thể tính sai độ dài tên người dùng hiệu quả. Ví dụ: đọc dữ liệu đầu vào thô và quên xóa dòng mới có thể dịch chuyển độ dài hợp lệ đi một ký tự. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trong bối cảnh này không thể phân biệt được với cách tiếp cận tối ưu vì bài toán không có cấu trúc ngoài một phép đo duy nhất. Chúng tôi chỉ cần tính độ dài của chuỗi và so sánh nó với phạm vi cho phép. Tính chính xác xuất phát từ định nghĩa của chính nhiệm vụ: tính hợp lệ hoàn toàn được xác định bởi liệu độ dài có nằm trong một khoảng cố định hay không. 

Một sự hiểu lầm ngây thơ hơn có thể liên quan đến việc lặp qua chuỗi và thực hiện các kiểm tra không cần thiết cho mỗi ký tự hoặc cố gắng mô phỏng từng ký tự quy tắc xác thực. Đó vẫn sẽ là O (n), nhưng nó không mang lại lợi ích gì vì không tồn tại ràng buộc cấp độ ký tự. 

Quan sát quan trọng là toàn bộ vấn đề quy về một thuộc tính vô hướng duy nhất của đầu vào. Một khi điều đó được nhận ra, giải pháp sẽ trở thành một so sánh trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xác thực từng ký tự) | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu (kiểm tra độ dài trực tiếp) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Phương pháp tối ưu 

1. Đọc chuỗi đầu vào đại diện cho tên người dùng. Chuỗi có thể bao gồm một dòng mới ở cuối tùy thuộc vào việc xử lý đầu vào, vì vậy điều quan trọng là phải xử lý nó một cách nhất quán. 
2. Xóa các ký tự dòng mới hoặc khoảng trắng ở cuối nếu chúng không phải là một phần của tên người dùng thực tế. Điều này đảm bảo độ dài đo được tương ứng chính xác với tên người dùng hợp lý. 
3. Tính độ dài của sợi dây đã được làm sạch. 
4. So sánh độ dài với phạm vi hợp lệ [5, 12]. 
5. Xuất "YES" nếu độ dài nằm trong phạm vi, nếu không thì xuất "NO". 

### Tại sao nó hoạt động 

Điều kiện hợp lệ chỉ phụ thuộc vào một thuộc tính số duy nhất của đầu vào, độ dài của nó. Không có thuộc tính nào khác của chuỗi ảnh hưởng đến kết quả. Bởi vì độ dài là bất biến khi sắp xếp lại các ký tự và không phụ thuộc vào nội dung nên việc kiểm tra trực tiếp là cần thiết và đủ. Bất kỳ giải pháp đúng nào cũng phải tính toán hoặc suy ra độ dài này và một khi đã biết nó, quyết định sẽ mang tính quyết định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().rstrip("\n")

if 5 <= len(s) <= 12:
    print("YES")
else:
    print("NO")
```Việc triển khai đọc tên người dùng trong một thao tác duy nhất và ngay lập tức bình thường hóa nó bằng cách xóa dòng mới ở cuối. Điều này tránh được một lỗi phổ biến trong đó ký tự dòng mới vô tình được tính là một phần của tên người dùng. 

Logic quyết định là kiểm tra khoảng thời gian trực tiếp. Các giới hạn đều mang tính bao hàm nên cả hai điểm cuối 5 và 12 đều được cho phép rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
abcde
```| Bước | Chuỗi | Chiều dài | Tình trạng | 
| --- | --- | --- | --- | 
| Đọc đầu vào | abcde | - | - | 
| Tách dòng mới | abcde | - | - | 
| Tính chiều dài | abcde | 5 | - | 
| Kiểm tra phạm vi | - | 5 | 5 ₫ 5 ₫ 12 đúng | 

Đầu ra:```
YES
```Trường hợp này chứng tỏ ranh giới dưới được chấp nhận. Một chuỗi có chính xác năm ký tự là hợp lệ. 

### Ví dụ 2 

đầu vào:```
abcdefghijklmn
```| Bước | Chuỗi | Chiều dài | Tình trạng | 
| --- | --- | --- | --- | 
| Đọc đầu vào | abcdefghijklmn | - | - | 
| Tách dòng mới | abcdefghijklmn | - | - | 
| Tính chiều dài | abcdefghijklmn | 14 | - | 
| Kiểm tra phạm vi | - | 14 | 5  14  12 sai | 

Đầu ra:```
NO
```Trường hợp này thể hiện sự từ chối khi độ dài vượt quá giới hạn trên. Mặc dù đầu vào được định dạng đúng, chỉ riêng độ dài sẽ xác định tính không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đọc đầu vào yêu cầu quét tất cả các ký tự một lần | 
| Không gian | O(1) | Chỉ một chuỗi duy nhất được lưu trữ | 

Độ phức tạp về thời gian là tối ưu vì mọi giải pháp đều phải đọc toàn bộ đầu vào ít nhất một lần. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ chính chuỗi đầu vào, điều này là không thể tránh khỏi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    input = sys.stdin.readline

    s = input().rstrip("\n")
    if 5 <= len(s) <= 12:
        return "YES"
    else:
        return "NO"

# boundary cases
assert run("abcde\n") == "YES", "lower bound"
assert run("abcd\n") == "NO", "below lower bound"
assert run("abcdefghijkl\n") == "YES", "upper bound"
assert run("abcdefghijklm\n") == "NO", "above upper bound"

# minimal and empty-like cases
assert run("\n") == "NO", "empty string"

# typical case
assert run("hello123\n") == "YES", "valid middle case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "abcde" | CÓ | Chấp nhận ranh giới dưới | 
| "abcd" | KHÔNG | Ngay dưới độ dài tối thiểu | 
| "abcdefghijkl" | CÓ | Chấp nhận ranh giới trên | 
| "abcdefghijklm" | KHÔNG | Ngay trên chiều dài tối đa | 
| "" | KHÔNG | Xử lý đầu vào trống | 
| "xin chào123" | CÓ | Tên người dùng hợp lệ điển hình | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi đầu vào chỉ bao gồm một ký tự dòng mới. Trong trường hợp đó, chuỗi đầu vào thô có thể trống sau khi loại bỏ, dẫn đến độ dài bằng 0. Thuật toán từ chối nó một cách chính xác vì số 0 nằm ngoài khoảng hợp lệ [5, 12]. 

Một trường hợp khác là khi tên người dùng nằm chính xác ở ranh giới. Ví dụ: "abcde" có độ dài 5. Thuật toán tính độ dài là 5 và so sánh trực tiếp nó với giới hạn dưới, chấp nhận chính xác nó mà không cần bất kỳ cách viết hoa đặc biệt nào. 

Trường hợp thứ ba là tên người dùng quá dài. Ví dụ: "abcdefghijklmnop" có độ dài 16. Sau khi tính toán độ dài, phép so sánh không thành công so với giới hạn trên 12 và thuật toán sẽ từ chối nó ngay lập tức. Không cần logic bổ sung vì điều kiện đã nắm bắt đầy đủ tính hợp lệ.
