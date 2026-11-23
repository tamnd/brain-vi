---
title: "CF 470B - Hexakosioihexekontahexaphobia"
description: "Chúng ta được cung cấp một chuỗi chỉ bao gồm các chữ số và nhiệm vụ là xác định xem số \"666\" có xuất hiện ở bất kỳ đâu dưới dạng một chuỗi liền kề bên trong chuỗi đó hay không. Kết quả đầu ra phải là \"CÓ\" nếu có, và \"KHÔNG\" nếu ngược lại."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 470
codeforces_index: "B"
codeforces_contest_name: "Surprise Language Round 7"
rating: 1800
weight: 470
solve_time_s: 66
verified: true
draft: false
---

[CF 470B - Hexakosioihexekontahexaphobia](https://codeforces.com/problemset/problem/470/B) 

**Đánh giá:** 1800 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chỉ bao gồm các chữ số và nhiệm vụ là xác định xem số "666" có xuất hiện ở bất kỳ đâu dưới dạng một chuỗi liền kề bên trong chuỗi đó hay không. Đầu ra phải là "CÓ" nếu có và "KHÔNG" nếu ngược lại. Đây là vấn đề phát hiện mẫu chứ không phải là vấn đề tính toán số, do đó kích thước thực tế của các con số là không liên quan; chúng tôi coi đầu vào hoàn toàn là một chuỗi ký tự. 

Chuỗi có thể ngắn bằng một chữ số hoặc dài tới 100 chữ số. Vì 100 là một con số rất nhỏ đối với giới hạn tính toán hiện đại nên chúng ta không cần những thuật toán quá phức tạp. Ngay cả cách tiếp cận kiểm tra mọi chuỗi con có thể có độ dài 3 cũng sẽ chạy hiệu quả vì số lượng chuỗi con tối đa như vậy là khoảng 98. 

Các trường hợp cạnh không rõ ràng liên quan đến các chuỗi ngắn hơn ba chữ số, trong đó không thể có "666" làm chuỗi con. Ví dụ: đầu vào "66" phải xuất ra "KHÔNG". Một trường hợp cạnh khác là khi "666" xuất hiện ở đầu hoặc cuối chuỗi, chẳng hạn như "666123" hoặc "123666". Việc triển khai bất cẩn chỉ quét phần giữa của chuỗi có thể bỏ lỡ những lần xuất hiện này. Các chuỗi chứa các chữ số lặp lại dài hơn ba, như "6666", vẫn phải mang lại "CÓ" vì tồn tại một chuỗi con liền kề có độ dài ba trong chuỗi lặp lại. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực đơn giản sẽ liên quan đến việc lặp lại mọi chuỗi con có độ dài ba trong chuỗi và kiểm tra xem nó có khớp với "666" hay không. Cách tiếp cận này đúng vì nó kiểm tra rõ ràng mọi ứng cử viên, nhưng nó sẽ không hiệu quả nếu chuỗi dài hơn nhiều so với giới hạn của chúng ta cho phép. Số lần kiểm tra trong trường hợp xấu nhất là xấp xỉ`n-2`so sánh ở đâu`n`là độ dài chuỗi. Với độ dài tối đa là 100, đây chỉ là 98 lần kiểm tra, điều này không đáng kể. 

Cách tiếp cận tối ưu dựa vào tìm kiếm chuỗi con tích hợp sẵn của Python. các`in`toán tử có thể kiểm tra xem "666" có nằm trong chuỗi theo thời gian tuyến tính tương ứng với độ dài chuỗi hay không. Điều này tránh việc lập chỉ mục thủ công, giảm mã soạn sẵn và xử lý chính xác tất cả các trường hợp cạnh. Thông tin chi tiết quan trọng là vấn đề này giảm xuống còn việc kiểm tra tư cách thành viên mẫu đơn giản và Python cung cấp cơ chế được tối ưu hóa cao cho việc đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu (tìm kiếm chuỗi con) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào và loại bỏ mọi ký tự dòng mới hoặc khoảng trắng ở cuối. Điều này đảm bảo chúng ta chỉ làm việc với chính các chữ số. 
2. Kiểm tra xem chuỗi "666" có được chứa ở bất kỳ đâu trong chuỗi đầu vào hay không bằng cách sử dụng`in`nhà điều hành. Điều này quét chuỗi một cách hiệu quả trong một lần. 
3. Nếu tìm thấy "666", hãy in "CÓ". Nếu không thì in ra "NO". 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà chúng tôi đã quét chuỗi để tìm mẫu cụ thể "666". Vì chúng tôi kiểm tra toàn bộ chuỗi trong quá trình quét tuyến tính nên không có khả năng thiếu chuỗi con hợp lệ. Tính chính xác bắt nguồn từ thực tế là tư cách thành viên chuỗi con trong Python là chính xác và tôn trọng tính liền kề, phù hợp với yêu cầu của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

p = input().strip()

if "666" in p:
    print("YES")
else:
    print("NO")
```Dòng đầu tiên nhập`sys`và buộc lại`input`ĐẾN`sys.stdin.readline`để đọc đầu vào nhanh chóng. Chúng tôi ngay lập tức loại bỏ mọi khoảng trắng ở cuối để tránh kết quả âm tính giả. các`in`toán tử thực hiện quét tuyến tính trên chuỗi để kiểm tra chuỗi liền kề "666". Cuối cùng, chúng tôi in phản hồi chính xác dựa trên việc tìm thấy chuỗi con hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`123098`| Bước | p | "666" trong trang | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | "123098" | Sai | "KHÔNG" | 

Giải thích: Chuỗi không chứa chuỗi ba số sáu liên tiếp. Quá trình quét xác nhận rằng chuỗi con không tồn tại. 

### Ví dụ 2 

đầu vào:`456666789`| Bước | p | "666" trong trang | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | "456666789" | Đúng | "CÓ" | 

Giải thích: Dãy số "666" xuất hiện bắt đầu từ ký tự thứ tư. Thuật toán xác định chính xác nó và xuất ra "CÓ". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | các`in`toán tử kiểm tra từng vị trí của chuỗi nhiều nhất một lần để tìm chuỗi con có độ dài ba. | 
| Không gian | O(1) | Chỉ chuỗi đầu vào được lưu trữ; không có bộ nhớ bổ sung tỷ lệ với n được phân bổ. | 

Với n tối đa là 100, quá trình quét tuyến tính này cực kỳ nhanh và vừa vặn trong giới hạn 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    p = input().strip()
    if "666" in p:
        print("YES")
    else:
        print("NO")
    return output.getvalue().strip()

# Provided samples
assert run("123098\n") == "NO", "sample 1"

# Custom cases
assert run("666\n") == "YES", "exact match"
assert run("66\n") == "NO", "too short"
assert run("6666\n") == "YES", "longer repeated sixes"
assert run("123666789\n") == "YES", "in the middle"
assert run("0000000\n") == "NO", "no sixes at all"
assert run("6\n") == "NO", "single character"
assert run("123456789012345678901234567890666\n") == "YES", "end of long string"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "666" | CÓ | Kết hợp chính xác chuỗi con | 
| "66" | KHÔNG | Chuỗi quá ngắn để chứa 666 | 
| "6666" | CÓ | Các trường hợp chồng chéo được xử lý | 
| "123666789" | CÓ | Chuỗi con ở giữa | 
| "0000000" | KHÔNG | Không có số sáu nào cả | 
| "6" | KHÔNG | Nhập ký tự đơn | 
| "123456789012345678901234567890666" | CÓ | Chuỗi con ở cuối chuỗi dài | 

## Vỏ cạnh 

Một chuỗi ngắn hơn ba ký tự không thể chứa "666", chẳng hạn như "66". Thuật toán trả về "KHÔNG" vì`in`toán tử kiểm tra tất cả các chuỗi con liền kề có độ dài bằng ba và không tìm thấy chuỗi nào. Đối với các số sáu lặp lại dài hơn ba, chẳng hạn như "6666", thuật toán vẫn trả về "CÓ" vì chuỗi con "666" tồn tại bắt đầu từ nhiều vị trí. Các chuỗi con ở đầu hoặc cuối, chẳng hạn như "666123" hoặc "123666", được phát hiện chính xác vì tìm kiếm sẽ quét toàn bộ chuỗi mà không bỏ qua bất kỳ vị trí nào.
