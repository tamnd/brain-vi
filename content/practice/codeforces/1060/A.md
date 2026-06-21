---
title: "CF 1060A - Số điện thoại"
description: "Chúng ta được cấp một bộ thẻ chữ số, mỗi thẻ chứa một ký tự từ 0 đến 9. Từ những thẻ này, chúng ta muốn tập hợp càng nhiều số điện thoại hợp lệ càng tốt."
date: "2026-06-15T09:09:35+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1060
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 513 by Barcelona Bootcamp (rated, Div. 1 + Div. 2)"
rating: 800
weight: 1060
solve_time_s: 284
verified: true
draft: false
---

[CF 1060A - Số điện thoại](https://codeforces.com/problemset/problem/1060/A) 

**Đánh giá:** 800 
**Tags:** vũ lực 
**Thời gian giải:** 4 phút 44 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ thẻ chữ số, mỗi thẻ chứa một ký tự từ 0 đến 9. Từ những thẻ này, chúng ta muốn tập hợp càng nhiều số điện thoại hợp lệ càng tốt. Số điện thoại hợp lệ có cấu trúc cố định: gồm đúng 11 chữ số, chữ số đầu tiên phải là 8, 10 vị trí còn lại có thể điền bằng bất kỳ chữ số nào. 

Mỗi thẻ có thể được sử dụng tối đa một lần và về mặt khái niệm, chúng tôi được phép sử dụng lại cùng một mẫu số điện thoại thu được nhiều lần miễn là chúng tôi có đủ thẻ để tạo từng bản sao một cách độc lập. 

Nhiệm vụ là xác định số lượng tối đa các chuỗi 11 chữ số bắt đầu bằng 8 có thể được tạo thành từ các chữ số có sẵn. 

Ràng buộc n 100 đủ nhỏ để ngay cả các phương pháp đếm tham lam đơn giản hoặc bậc hai cũng an toàn. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu hoặc tổ hợp phức tạp. Chúng ta đang giải quyết hoàn toàn vấn đề kế toán tần số. 

Một quan sát quan trọng là yêu cầu đặc biệt duy nhất là số 8 đứng đầu. Mỗi số điện thoại có đúng một lần xuất hiện của chữ số 8 và tổng cộng là 11 thẻ. Mọi thứ khác chỉ là một tập hợp các chữ số phụ. 

Trường hợp cạnh xuất hiện khi không có số 8 nào cả. Trong trường hợp đó, bất kể có bao nhiêu chữ số khác tồn tại, không thể tạo được số điện thoại hợp lệ. Ví dụ, đầu vào`11111111111`mang lại 0. 

Một trường hợp khó phát hiện khác là khi có đủ tổng chữ số nhưng không đủ số 8. Ví dụ,`888`được lặp lại với quá ít chữ số khác vẫn giới hạn câu trả lời theo số 8 có sẵn. 

Cuối cùng, nếu có nhiều số 8 nhưng tổng số có quá ít thì tổng dung lượng chiếm ưu thế: dù mỗi số điện thoại cần số 8 nhưng cũng cần thêm 10 chữ số, nên số 8 còn sót lại sẽ vô dụng nếu không có chữ số hỗ trợ. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là thử xây dựng từng số điện thoại một. Chúng tôi liên tục quét nhiều tập hợp chữ số hiện tại, kiểm tra xem liệu chúng tôi có thể chọn một số 8 và 10 chữ số bất kỳ khác hay không, sau đó xóa các chữ số đó và tiếp tục. Mỗi lần thử đều tốn O(11) lần quét hoặc ghi sổ và chúng tôi có thể lặp lại tới O(n) lần, dẫn đến hành vi O(n²). Với n 100 thì điều này là ổn nhưng không cần thiết. 

Cấu trúc của bài toán cho thấy một hệ thống ràng buộc đơn giản hơn. Mỗi số điện thoại dùng đúng 11 thẻ, trong số 11 thẻ đó có đúng 1 thẻ là số 8. Vậy nếu ta ký hiệu số 8 là c8 và tổng các chữ số là n thì số điện thoại k phải thỏa mãn cả k ≤ c8 và 11k ≤ n. Điều kiện thứ hai xuất phát từ tổng mức tiêu thụ thẻ và điều kiện thứ nhất xuất phát từ yêu cầu cố định là một số 8 cho mỗi số. 

Do đó, giải pháp tối ưu chỉ là mức tối thiểu của hai yếu tố hạn chế này. Điều này làm giảm vấn đề đếm tần số và lấy một giới hạn đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng Brute Force | O(n²) | O(1) | Đã chấp nhận | 
| Đếm + ràng buộc tối thiểu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần chữ số 8 xuất hiện trong đầu vào. Điều này thể hiện số lượng số điện thoại tối đa có thể có từ góc độ của chữ số hàng đầu được yêu cầu. 
2. Tính xem có tổng cộng bao nhiêu nhóm đầy đủ gồm 11 quân bài, là n // 11. Điều này thể hiện giới hạn trên tuyệt đối được áp đặt bởi tổng số quân bài có sẵn. 
3. Câu trả lời là giá trị nhỏ hơn trong hai giá trị này, vì mọi số điện thoại hợp lệ phải đồng thời thỏa mãn cả hai ràng buộc. 
4. Xuất trực tiếp giá trị này. 

### Tại sao nó hoạt động 

Mỗi số điện thoại hợp lệ sử dụng chính xác 11 thẻ riêng biệt và chính xác một trong số chúng phải là số 8. Mọi cấu trúc hợp lệ đều có thể được ánh xạ tới một cặp giữa số điện thoại và các nhóm rời rạc gồm 11 thẻ, mỗi nhóm chứa ít nhất một số 8 ở vị trí đầu tiên. Vì vậy, không có giải pháp nào có thể vượt quá tổng số 8 hoặc số khối 11 lá bài rời nhau. Việc xây dựng tham lam sử dụng thẻ mà không vi phạm các ràng buộc này luôn đạt được giới hạn tối thiểu vì không có cấu trúc bổ sung nào ngoài việc đếm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()
    
    c8 = s.count('8')
    
    print(min(c8, n // 11))

if __name__ == "__main__":
    solve()
```Việc triển khai đầu tiên sẽ đọc số lượng thẻ và chuỗi chữ số. Sau đó, nó đếm số lần xuất hiện của '8', trực tiếp kiểm soát số lượng số điện thoại có thể được hình thành do ràng buộc bắt buộc về chữ số đầu. Ràng buộc thứ hai bắt nguồn từ tổng độ dài: mỗi số điện thoại có 11 chữ số, vì vậy n // 11 là số lượng số điện thoại hoàn chỉnh tối đa có thể bất kể thành phần chữ số. Câu trả lời cuối cùng là mức tối thiểu của hai giới hạn này. 

Không cần có vấn đề đặt hàng hoặc lựa chọn tham lam vì vấn đề không phụ thuộc vào sự sắp xếp mà chỉ phụ thuộc vào số lượng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
11
00000000008
```Chúng ta tính c8 = 1 và n // 11 = 1. 

| Bước | c8 | n // 11 | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | phút(1, 1) = 1 | 

Kết quả là 1, nghĩa là chúng ta có thể tạo chính xác một số điện thoại hợp lệ bằng tất cả các chữ số. 

Điều này xác nhận rằng ngay cả khi hầu hết các chữ số đều là số 0 thì một số 8 là đủ khi tổng chiều dài chỉ hỗ trợ một nhóm. 

### Ví dụ 2 

đầu vào:```
22
8888888888888888888888
```Ở đây c8 = 22 và n // 11 = 2. 

| Bước | c8 | n // 11 | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Ban đầu | 22 | 2 | phút(22, 2) = 2 | 

Mặc dù có nhiều số 8 nhưng tổng độ dài chỉ giới hạn ở 2 số điện thoại. Điều này thể hiện sự thống trị của ràng buộc nhóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét chuỗi một lần để đếm tần số chữ số | 
| Không gian | O(1) | Chỉ có một bộ đếm duy nhất được duy trì | 

Giải pháp dễ dàng thỏa mãn các ràng buộc vì n tối đa là 100 và ngay cả đối với đầu vào lớn hơn nhiều, việc đếm tần số một lần vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(sys.stdin.readline().strip())
    s = sys.stdin.readline().strip()
    
    return str(min(s.count('8'), n // 11))

# provided sample
assert run("11\n00000000008\n") == "1", "sample 1"

# no 8 at all
assert run("11\n11111111111\n") == "0", "no valid phone numbers"

# exactly one full valid set
assert run("11\n81234567890\n") == "1", "single possible number"

# too few digits for even one number
assert run("10\n8888888888\n") == "0", "insufficient length"

# many 8s but limited by length
assert run("22\n8888888888888888888888\n") == "2", "length constraint dominates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 11 chữ số, số 8 | 0 | thiếu chữ số đầu bắt buộc | 
| tập hợp lệ hỗn hợp | 1 | xây dựng bình thường | 
| 10 chữ số 8 | 0 | hạn chế về độ dài ngăn cản sự hình thành | 
| nhiều số 8 | 2 | hành vi tối thiểu(n//11, c8) | 

## Vỏ cạnh 

Đối với đầu vào không có chữ số 8, chẳng hạn như`11111111111`, thuật toán tính c8 = 0 và trả về 0 ngay lập tức. Điều này phù hợp với thực tế là không có số điện thoại hợp lệ nào có thể bắt đầu. 

Đối với đầu vào có n < 11, chẳng hạn như`8888888888`, n // 11 bằng 0 nên kết quả là 0 bất kể tồn tại bao nhiêu số 8. Điều này phản ánh chính xác khả năng không thể hình thành bất kỳ số điện thoại đầy đủ nào. 

Đối với một đầu vào như`8888888888888888888888`với n = 22, c8 = 22 và n // 11 = 2, do đó đầu ra là 2. Thuật toán giới hạn chính xác bằng tổng dung lượng nhóm có sẵn thay vì lạm dụng chữ số 8.
