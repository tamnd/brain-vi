---
title: "CF 72H - Đảo ngược nó!"
description: "Nhiệm vụ là đảo ngược một số được cho dưới dạng một chuỗi, chú ý đến các dấu và số 0 đứng đầu. Đầu vào có thể là một số nguyên rất lớn, lên tới 10.000 chữ số, có thể có các số 0 đứng đầu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 72
codeforces_index: "H"
codeforces_contest_name: "Unknown Language Round 2"
rating: 1600
weight: 72
solve_time_s: 208
verified: true
draft: false
---

[CF 72H - Đảo ngược nó!](https://codeforces.com/problemset/problem/72/H) 

**Đánh giá:** 1600 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 3 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là đảo ngược một số được cho dưới dạng một chuỗi, chú ý đến các dấu và số 0 đứng đầu. Đầu vào có thể là một số nguyên rất lớn, lên tới 10.000 chữ số, có thể có các số 0 đứng đầu. Trước tiên, chúng ta cần loại bỏ các số 0 đứng đầu đó, đảo ngược các chữ số còn lại và xuất kết quả dưới dạng chuỗi biểu thị một số nguyên. Nếu là số âm thì số đảo ngược phải giữ nguyên dấu âm. Các số 0 đứng đầu trong số đảo ngược cũng phải được bỏ qua. 

Các ràng buộc ngụ ý rằng chúng ta không thể dựa vào các loại số nguyên tiêu chuẩn, bởi vì một số có 10.000 chữ số vượt quá bất kỳ loại số có sẵn nào. Thay vào đó, chúng ta phải coi số là một chuỗi, thao tác trực tiếp với các ký tự. Vì kích thước đầu vào tối đa là 10.000 ký tự nên giải pháp O(n) là khả thi, nhưng bất cứ điều gì tệ hơn O(n log n) có thể quá chậm. Các trường hợp có biên không rõ ràng bao gồm các số như "0000123", số này sẽ đảo ngược thành "321" hoặc các số âm như "-00120", số này sẽ trở thành "-21". Việc triển khai bất cẩn có thể để lại các số 0 ở cuối sau khi đảo ngược hoặc xử lý sai dấu âm. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng chuyển đổi chuỗi thành một số nguyên, sau đó đảo ngược các chữ số bằng cách chia liên tục cho 10 và thu thập số dư. Điều này hoạt động với các số nhỏ, nhưng không thành công ở đây vì số nguyên Python có thể xử lý kích thước, nhưng trong các ngôn ngữ khác, nó sẽ tràn. Hơn nữa, đầu vào có thể có các số 0 ở đầu bị mất khi chuyển đổi thành số nguyên, khiến cho việc đảo ngược trực tiếp là không thể. 

Quan sát chính là vấn đề này tự nhiên là một vấn đề thao tác chuỗi. Chúng ta có thể cắt bớt các số 0 đứng đầu, kiểm tra dấu âm, đảo ngược chuỗi con đại diện cho các chữ số và sau đó xóa mọi số 0 đứng đầu xuất hiện sau khi đảo ngược. Điều này làm giảm vấn đề thành một phép truyền tuyến tính đơn giản qua chuỗi đầu vào. Dấu âm được xử lý riêng biệt, đảm bảo đầu ra cuối cùng có vị trí dấu chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (chuyển đổi số nguyên) | O(n) | O(n) | Đúng về mặt khái niệm nhưng không thành công với số lượng rất lớn | 
| Thao tác chuỗi | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào biểu thị số và xóa mọi khoảng trắng xung quanh. Điều này đảm bảo chúng tôi không hiểu sai các khoảng trắng ngẫu nhiên là chữ số. 
2. Kiểm tra xem ký tự đầu tiên có phải là dấu âm hay không. Nếu đúng, hãy lưu trữ thông tin này và làm việc trên chuỗi con ngoại trừ dấu trừ. Điều này cho phép xử lý thống nhất các chữ số. 
3. Xóa các số 0 đứng đầu khỏi chuỗi con số. của Python`lstrip('0')`đạt được điều này. Nếu chuỗi trở nên trống, số đó bằng 0. 
4. Đảo ngược chuỗi số đã làm sạch bằng cách cắt`[::-1]`. Đảo ngược một chuỗi tại chỗ là O(n) và giữ đúng thứ tự các chữ số. 
5. Loại bỏ mọi số 0 đứng đầu xuất hiện sau khi đảo ngược, sử dụng phương pháp tương tự như ở bước 3. Điều này rất quan trọng đối với các số như "1000" đảo ngược thành "0001". 
6. Thêm dấu âm vào trước nếu số ban đầu là số âm và chuỗi đảo ngược không trống. Điều này đảm bảo dấu hiệu là chính xác và chúng tôi không tạo ra "-0". 
7. Nếu chuỗi cuối cùng trống, xuất "0" để biểu thị giá trị 0. Ngược lại, in chuỗi đảo ngược. 

Điều bất biến là sau mỗi bước chuyển đổi, chuỗi biểu thị giá trị số hiện tại mà không có số 0 đứng đầu không hợp lệ. Điều này đảm bảo rằng việc đảo ngược và cắt xén sẽ tạo ra biểu diễn số nguyên chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    s = input().strip()
    if not s:
        print(0)
        return

    negative = s[0] == '-'
    if negative:
        s = s[1:]

    s = s.lstrip('0')
    if not s:
        print(0)
        return

    reversed_s = s[::-1].lstrip('0')
    if negative:
        reversed_s = '-' + reversed_s

    print(reversed_s)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách loại bỏ khoảng trắng để tránh lỗi từ các ký tự không liên quan. Việc phát hiện sớm dấu hiệu tiêu cực cho phép chúng ta xử lý đảo chiều một cách thống nhất. Các số 0 đứng đầu được loại bỏ trước và sau khi đảo ngược để xử lý cả đầu vào như "000123" và đầu ra như "1000". Việc kiểm tra chuỗi trống rõ ràng đảm bảo chúng tôi trả về chính xác "0" cho các đầu vào hoàn toàn bằng 0. 

## Ví dụ đã hoạt động 

Đối với đầu vào "23": 

| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc đầu vào | s | "23" | 
| Kiểm tra tiêu cực | tiêu cực | Sai | 
| Dải số 0 đứng đầu | s | "23" | 
| Đảo ngược | đảo ngược_s | "32" | 
| Đầu ra | in | "32" | 

Điều này xác nhận số dương hai chữ số cơ bản được đảo ngược chính xác. 

Đối với đầu vào "-00120": 

| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc đầu vào | s | "-00120" | 
| Kiểm tra tiêu cực | tiêu cực | Đúng | 
| Xóa dấu | s | "00120" | 
| Dải số 0 đứng đầu | s | "120" | 
| Đảo ngược | đảo ngược_s | "021" | 
| Loại bỏ các số 0 đứng đầu sau khi đảo ngược | đảo ngược_s | "21" | 
| Thêm vào trước phủ định | đảo ngược_s | "-21" | 
| Đầu ra | in | "-21" | 

Dấu vết này thể hiện việc xử lý chính xác các số âm và cả số 0 ở đầu và cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập một số lần không đổi để tước, cắt và đảo ngược | 
| Không gian | O(n) | Các thao tác đảo ngược và chuỗi con tạo ra các chuỗi mới tỷ lệ thuận với độ dài đầu vào | 

Cho n ≤ 10.000, các thao tác này hoàn toàn thoải mái trong giới hạn thời gian 4 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("23\n") == "32", "sample 1"
# negative number
assert run("-00120\n") == "-21", "negative number with leading zeros"
# only zeros
assert run("0000\n") == "0", "all zeros"
# large number
assert run("100000000000000000000\n") == "1", "large power-of-ten number"
# single digit
assert run("7\n") == "7", "single digit number"
# negative single digit
assert run("-9\n") == "-9", "negative single digit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "23" | "32" | Đảo ngược số dương cơ bản | 
| "-00120" | "-21" | Số âm có số 0 đứng đầu | 
| "0000" | "0" | Đầu vào của tất cả các số không | 
| "1000000000000000000000" | "1" | Số rất lớn có số 0 ở cuối | 
| "7" | "7" | Đầu vào một chữ số | 
| "-9" | "-9" | Đầu vào âm một chữ số | 

## Vỏ cạnh 

Đối với đầu vào "0000", trước tiên thuật toán sẽ loại bỏ các số 0 đứng đầu, để lại một chuỗi trống. Kiểm tra chuỗi trống sau đó in "0". Đối với đầu vào "-00120", sau khi loại bỏ các số 0 ở đầu, chuỗi sẽ trở thành "120", đảo ngược tạo ra "021" và loại bỏ các số 0 sau khi đảo ngược tạo ra "21", sau đó dấu âm được thêm vào trước để mang lại "-21". Cách tiếp cận xử lý chính xác cả đầu vào và số chỉ có 0 với các mẫu số 0 ở đầu/cuối phức tạp.
