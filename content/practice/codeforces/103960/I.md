---
title: "CF 103960I - Chặn thông tin"
description: "Hệ thống đang đọc một byte đơn được truyền dưới dạng tám tín hiệu riêng biệt. Mỗi vị trí được coi là một chữ số nhị phân, vì vậy thông thường mọi vị trí phải chứa 0 hoặc 1."
date: "2026-07-02T06:45:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103960
codeforces_index: "I"
codeforces_contest_name: "2022-2023 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 103960
solve_time_s: 38
verified: true
draft: false
---

[CF 103960I - Chặn thông tin](https://codeforces.com/problemset/problem/103960/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hệ thống đang đọc một byte đơn được truyền dưới dạng tám tín hiệu riêng biệt. Mỗi vị trí được coi là một chữ số nhị phân, vì vậy thông thường mọi khe phải chứa 0 hoặc 1. Tuy nhiên, thiết bị đọc không đáng tin cậy: khi xảy ra nhiễu tại một vị trí, nó sẽ ghi giá trị 9 thay vì bit hợp lệ. 

Nhiệm vụ không phải là tái tạo lại byte hay sửa lỗi mà chỉ đơn giản là quyết định xem toàn bộ quá trình đọc 8 bit có sạch hay không. Nếu mỗi một trong tám vị trí đều là 0 hoặc 1 thì quá trình truyền được coi là thành công. Nếu có ít nhất một vị trí chứa số 9 thì quá trình truyền không thành công. 

Vì vậy, đầu vào chỉ là tám số nguyên liên tiếp biểu thị các bit được quan sát. Đầu ra là một ký tự đơn: “S” nếu không có nhiễu ở bất kỳ đâu và “F” nếu không. 

Cấu trúc ràng buộc là cực kỳ nhỏ. Chỉ với tám giá trị, ngay cả một lần quét đơn giản cũng có thời gian không đổi, vì vậy vấn đề này hoàn toàn là về việc diễn giải chính xác tình trạng hơn là tối ưu hóa độ phức tạp. Bất kỳ cách tiếp cận nào từ kiểm tra trực tiếp đến chấm dứt sớm đều đủ trong các giới hạn điển hình. 

Các trường hợp lỗi chính xảy ra do quên rằng chỉ có giá trị 9 đóng vai trò là chỉ báo lỗi. Một sai lầm ngây thơ là coi bất kỳ giá trị nào khác 0 là lỗi, điều này sẽ từ chối không chính xác các bit hợp lệ như 1. Ví dụ: đầu vào “0 0 1 1 0 1 0 1” sẽ tạo ra “S”, nhưng một quy tắc nhầm lẫn như “nếu giá trị != 0 thì không thành công” sẽ trả về “F” không chính xác. 

Một sai lầm tinh vi khác là dừng lại quá sớm mà không kiểm tra tất cả các vị trí. Ví dụ: nếu một người viết logic chỉ kiểm tra một vài bit đầu tiên hoặc ngắt không chính xác, thì đầu vào như “0 0 1 1 0 1 0 9” vẫn phải được phát hiện là lỗi ngay cả khi số 9 xuất hiện muộn. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực đã là cách giải thích tối ưu: đọc tất cả tám số nguyên và xác minh xem có số nào trong số chúng bằng 9 hay không. Độ chính xác là ngay lập tức vì định nghĩa về lỗi mang tính cục bộ đối với từng vị trí và độc lập giữa các vị trí. 

Một cách bắt buộc chính thức hơn một chút là xem xét tất cả 8 vị trí và kiểm tra các ràng buộc về tính hợp lệ trên mỗi vị trí, nhưng vì mỗi lần kiểm tra là O(1), nên tổng công việc là không đổi. Không có trạng thái có ý nghĩa nào để duy trì và không có cấu trúc như tổng tiền tố, biểu đồ hoặc sắp xếp để khai thác. 

Quan sát quan trọng là vấn đề giảm xuống còn việc phát hiện thành viên của một giá trị bị cấm duy nhất trong một chuỗi có kích thước cố định nhỏ. Điều đó có nghĩa là giải pháp chỉ là quét tuyến tính với tùy chọn thoát sớm và không có gì phức tạp hơn là hợp lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(8) | O(1) | Đã chấp nhận | 
| Quét tối ưu với thoát sớm | O(8) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng giá trị một và quyết định xem có bất kỳ giá trị nào trong số đó cho thấy có nhiễu hay không. 

1. Đọc tám số nguyên đại diện cho byte nhận được. Chúng tôi giữ chúng trong một luồng thay vì lưu trữ chúng vì chúng tôi chỉ cần kiểm tra chúng một lần. 
2. Khởi tạo cờ giả định thành công. Điều này thể hiện giả định rằng tất cả các bit đều hợp lệ cho đến khi được chứng minh ngược lại. 
3. Lặp lại từng giá trị trong số tám giá trị. Đối với mỗi giá trị, hãy kiểm tra xem nó có bằng 9 hay không. Nếu đúng như vậy, chúng tôi ngay lập tức đánh dấu kết quả là không thành công. 
4. Chỉ tiếp tục quét các giá trị còn lại nếu chúng ta muốn vượt qua hoàn toàn, nhưng vì câu trả lời đã được xác định khi nhìn thấy số 9 nên chúng ta có thể dừng sớm. 
5. Sau khi xử lý, xuất ra “S” nếu không gặp số 9, nếu không thì xuất ra “F”. 

### Tại sao nó hoạt động

Mỗi vị trí độc lập và chỉ đóng góp một điều kiện nhị phân: hợp lệ (0 hoặc 1) hoặc không hợp lệ (9). Việc truyền tổng thể là hợp lệ khi và chỉ khi tất cả các vị trí đều thỏa mãn điều kiện hợp lệ. Thuật toán trực tiếp thực thi điều kiện chung này bằng cách kiểm tra mọi vị trí và từ chối ngay khi gặp vi phạm. Bởi vì không có vị trí nào sau này có thể “sửa chữa” một lần đọc không hợp lệ trước đó, nên việc chấm dứt sớm không làm thay đổi tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

vals = list(map(int, input().split()))

ok = True
for x in vals:
    if x == 9:
        ok = False
        break

print("S" if ok else "F")
```Mã đọc toàn bộ dòng gồm tám số nguyên và lưu chúng vào một danh sách. Sau đó, nó quét qua chúng và lật cờ boolean nếu có số 9 xuất hiện. Việc nghỉ sớm là một cải tiến nhỏ về hiệu quả, mặc dù không liên quan do kích thước đầu vào không đổi. 

Điểm tinh tế duy nhất là đảm bảo rằng 1 được coi là hợp lệ. Việc kiểm tra được thực hiện nghiêm ngặt`x == 9`, không`x != 0`hoặc`x == 1`, vì cả 0 và 1 đều là giá trị tín hiệu hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 1 1 0 1 0 1
```| Bước | Giá trị | Cờ (có giá trị cho đến nay) | 
| --- | --- | --- | 
| 1 | 0 | Đúng | 
| 2 | 0 | Đúng | 
| 3 | 1 | Đúng | 
| 4 | 1 | Đúng | 
| 5 | 0 | Đúng | 
| 6 | 1 | Đúng | 
| 7 | 0 | Đúng | 
| 8 | 1 | Đúng | 

Không có giá trị không hợp lệ nào xuất hiện nên kết quả đầu ra là “S”. 

Điều này xác nhận tính bất biến rằng một chuỗi hoàn toàn sạch vẫn được chấp nhận trong suốt quá trình quét. 

### Ví dụ 2 

đầu vào:```
0 0 1 9 0 1 0 1
```| Bước | Giá trị | Cờ (có giá trị cho đến nay) | 
| --- | --- | --- | 
| 1 | 0 | Đúng | 
| 2 | 0 | Đúng | 
| 3 | 1 | Đúng | 
| 4 | 9 | Sai (dừng) | 

Ở vị trí thứ tư, điểm đánh dấu không hợp lệ xuất hiện. Sau khi được phát hiện, phần còn lại của chuỗi không liên quan. 

Điều này chứng tỏ rằng thuật toán xác định chính xác lỗi ngay cả khi xảy ra nhiễu ở giữa byte. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(8) | Số lần kiểm tra không đổi trên một byte có kích thước cố định | 
| Không gian | O(1) | Chỉ một tập hợp hằng số nhỏ được sử dụng | 

Các ràng buộc được cố định ở tám số nguyên cho mỗi bài kiểm tra, do đó, giải pháp có hiệu quả là thời gian không đổi và phù hợp thoải mái trong mọi giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    vals = list(map(int, input().split()))
    ok = True
    for x in vals:
        if x == 9:
            ok = False
            break
    return "S" if ok else "F"

# provided samples
assert run("0 0 1 1 0 1 0 1") == "S", "sample 1"
assert run("0 0 1 9 0 1 0 1") == "F", "sample 2"

# custom cases
assert run("9 0 0 0 0 0 0 0") == "F", "failure at start"
assert run("0 0 0 0 0 0 0 9") == "F", "failure at end"
assert run("1 1 1 1 1 1 1 1") == "S", "all ones valid"
assert run("0 0 0 0 0 0 0 0") == "S", "all zeros valid"
assert run("0 9 1 9 0 9 1 0") == "F", "multiple failures"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 9 0 0 0 0 0 0 0 | F | xử lý sự cố sớm | 
| 0 0 0 0 0 0 0 9 | F | phát hiện lỗi muộn | 
| 1 1 1 1 1 1 1 1 | S | ranh giới trên hoàn toàn hợp lệ | 
| 0 0 0 0 0 0 0 0 | S | đường cơ sở hoàn toàn bằng không | 
| 0 9 1 9 0 9 1 0 | F | nhiều trường hợp nhiễu | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi giao thoa xuất hiện ngay tại vị trí đầu tiên. Đầu vào “9 0 0 0 0 0 0 0” ngay lập tức gây ra lỗi trong lần kiểm tra đầu tiên và thuật toán dừng sớm một cách chính xác và xuất ra “F”. Quá trình quét không phụ thuộc vào thứ tự vị trí nên lỗi tải trước được xử lý một cách tự nhiên. 

Trường hợp cạnh thứ hai là khi giao thoa chỉ xuất hiện ở vị trí cuối cùng. Trong “0 0 0 0 0 0 0 9”, thuật toán xử lý tất cả bảy bit hợp lệ trước khi gặp 9 bit cuối cùng. Kết quả vẫn chính xác vì quá trình quét không kết thúc sớm. 

Trường hợp cạnh thứ ba là một byte hoàn toàn hợp lệ như “1 1 1 1 1 1 1 1”. Vì không có giá trị nào bằng 9 nên cờ không thay đổi và đầu ra là “S”, xác nhận rằng các bit 1 hợp lệ không bị phân loại sai. 

Trường hợp cạnh cuối cùng là nhiều vị trí giao thoa, ví dụ “0 9 1 9 0 9 1 0”. Thuật toán chỉ cần phát hiện sự tồn tại của ít nhất một ký hiệu không hợp lệ và thu gọn tất cả các trường hợp đó thành một kết quả lỗi duy nhất mà không cần đếm hoặc theo dõi tần số.
