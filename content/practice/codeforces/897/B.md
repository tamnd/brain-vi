---
title: "CF 897B - Yêu cầu của Chtholly"
description: "Chúng ta được yêu cầu liệt kê các số đặc biệt theo thứ tự tăng dần và tính tổng tiền tố. Một số được coi là hợp lệ nếu nó đọc xuôi và đọc ngược giống nhau theo ký hiệu thập phân và độ dài của nó là số chẵn."
date: "2026-06-17T03:35:48+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 897
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 449 (Div. 2)"
rating: 1300
weight: 897
solve_time_s: 67
verified: true
draft: false
---

[CF 897B - Yêu cầu của Chtholly](https://codeforces.com/problemset/problem/897/B) 

**Đánh giá:** 1300 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu liệt kê các số đặc biệt theo thứ tự tăng dần và tính tổng tiền tố. 

Một số được coi là hợp lệ nếu nó đọc xuôi và đọc ngược giống nhau theo ký hiệu thập phân và độ dài của nó là số chẵn. Điều này có nghĩa là mọi số hợp lệ đều được xác định đầy đủ bởi nửa đầu của nó, vì nửa sau chỉ là tấm gương phản chiếu của nửa đầu. Sau đó, chúng tôi lấy các số hợp lệ nhỏ nhất như vậy, cộng k nhỏ nhất trong số chúng và cuối cùng xuất ra tổng modulo p. 

Đầu vào cung cấp k, số lượng palindrome nhỏ nhất mà chúng ta cần và p, mô đun áp dụng cho tổng cuối cùng. Nhiệm vụ không phải là tìm một palindrome đơn lẻ hay kiểm tra tư cách thành viên mà là tạo ra chuỗi có thứ tự của tất cả các palindrome có độ dài chẵn và tổng hợp k số hạng đầu tiên. 

Ràng buộc k lên tới 100000 sẽ thay đổi đáng kể cấu trúc bài toán. Một cách tiếp cận trực tiếp để kiểm tra mọi số nguyên và kiểm tra xem đó có phải là một palindrome hay không sẽ quá chậm, vì các palindrome trở nên thưa thớt khi số lượng tăng lên và việc quét các số nguyên hợp lệ thứ k có thể dễ dàng vượt quá hàng tỷ lần kiểm tra. Thay vào đó, chúng ta phải khai thác trực tiếp việc xây dựng các palindromes. 

Một cạm bẫy tinh tế là giả định rằng các palindrome nên được tạo theo thứ tự số bằng cách liệt kê bạo lực. Điều đó không thành công vì hầu hết các số nguyên không phải là số palindrome và việc bỏ qua các số không hợp lệ vẫn tốn thời gian tỷ lệ thuận với không gian tìm kiếm. Một vấn đề khác là vô tình bao gồm các palindrome có độ dài lẻ như 12321, rõ ràng là không hợp lệ mặc dù chúng là palindromic. Sai lầm phổ biến thứ ba là quên rằng không được phép sử dụng số 0 đứng đầu, điều này ảnh hưởng đến cách chúng ta diễn giải cách biểu thị "một nửa". 

## Phương pháp tiếp cận 

Một phương pháp đơn giản sẽ lặp lại tất cả các số nguyên bắt đầu từ 1, kiểm tra xem mỗi số có phải là một bảng màu hay không, kiểm tra xem độ dài chữ số của nó có chẵn hay không và thu thập những số thỏa mãn cả hai điều kiện cho đến khi tìm thấy k. Điều này đúng nhưng cực kỳ kém hiệu quả. Trong trường hợp xấu nhất, để tìm ra 100000 palindrome có độ dài chẵn hợp lệ, chúng ta sẽ cần quét nhiều hơn 10^9 số nguyên, vì các palindrome hợp lệ trở nên ít thường xuyên hơn khi số lượng tăng lên. Mỗi lần kiểm tra palindrome tốn O(d), trong đó d là số chữ số, do đó độ phức tạp tổng cộng trở nên lớn đến mức không thể chấp nhận được. 

Quan sát quan trọng là mỗi palindrome có độ dài chẵn được xác định duy nhất bởi nửa đầu của nó. Nếu chúng ta chọn một số x, chúng ta có thể xây dựng một bảng màu bằng cách nối số đảo ngược của x vào chính nó. Điều này biến bài toán thành việc tạo ra tất cả các số nguyên dương được nhóm theo độ dài chữ số của một nửa biểu diễn của chúng. 

Thứ tự cũng được cấu trúc. Tất cả các palindrome 2 chữ số đều xuất hiện trước, sau đó là tất cả các palindrome 4 chữ số, sau đó là các palindrome 6 chữ số, v.v. Trong một nhóm có độ dài cố định, thứ tự của các palindrome tuân theo thứ tự tự nhiên của nửa đầu của chúng. 

Điều này cho phép chúng ta xây dựng chuỗi một cách trực tiếp, từng lớp một mà không cần kiểm tra các vùng không phải palindrome. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force | O(N · d) | O(1) | Quá chậm | 
| Xây dựng từ một nửa số | O(k · d) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chúng tôi lặp lại các nửa độ dài có thể bắt đầu từ nửa có 1 chữ số trở lên. Mỗi nửa chiều dài m tương ứng với các palindrome có chiều dài 2m. Thứ tự này là cần thiết vì độ dài nhỏ hơn luôn tạo ra số nhỏ hơn. 
2. Với mỗi nửa độ dài m, chúng ta tạo ra tất cả các nửa số hợp lệ theo thứ tự tăng dần. Với m = 1, đây là 1 đến 9. Đối với m ≥ 2, đây là từ 10^(m−1) đến 10^m − 1. Điều này đảm bảo không có số 0 đứng đầu nào xuất hiện trong các bảng màu được xây dựng. 
3. Đối với mỗi nửa số, chúng ta xây dựng bảng màu có độ dài chẵn đầy đủ bằng cách ghép số đó với số đảo ngược của nó ở dạng thập phân. Điều này đảm bảo tính đúng đắn của thuộc tính palindrom. 
4. Chúng tôi duy trì một danh sách đang chạy (hoặc tổng chạy) của các palindrome được tạo. Mỗi lần chúng ta tạo ra một palindrome, chúng ta cộng nó vào tổng modulo p. 
5. Chúng ta dừng ngay lập tức khi đã tạo ra k palindromes. Không cần phải tạo thêm độ dài sau khi yêu cầu được thỏa mãn. 

Ý tưởng cốt lõi đằng sau tính đúng đắn là quá trình tạo này liệt kê mọi palindrome có độ dài chẵn hợp lệ chính xác một lần và theo thứ tự tăng dần. 

### Tại sao nó hoạt động 

Mỗi số hợp lệ được xác định duy nhất bởi nửa đầu của nó, do đó có sự song ánh giữa các palindrome hợp lệ có độ dài 2m và các số nguyên trong phạm vi [10^(m−1), 10^m − 1] (hoặc [1, 9] với m = 1). Vì các phạm vi này được duyệt theo thứ tự tăng dần của m và trong mỗi phạm vi theo thứ tự số tăng dần nên chuỗi kết quả được sắp xếp toàn cục. Điều này đảm bảo rằng việc lấy k phần tử đầu tiên của cấu trúc này tương đương với việc lấy k phần tử hợp lệ nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def make_pal(x: int) -> int:
    s = str(x)
    return int(s + s[::-1])

k, p = map(int, input().split())

res = 0
cnt = 0
m = 1

while cnt < k:
    if m == 1:
        start, end = 1, 9
    else:
        start = 10 ** (m - 1)
        end = 10 ** m - 1

    x = start
    while x <= end and cnt < k:
        pal = make_pal(x)
        res = (res + pal) % p
        cnt += 1
        x += 1

    m += 1

print(res)
```Giải pháp xây dựng các palindrome trực tiếp từ một nửa biểu diễn của chúng. Hàm trợ giúp chuyển đổi một số thành bảng màu có độ dài chẵn bằng cách đảo ngược chuỗi, điều này an toàn ở đây vì k tối đa là 100000, do đó tổng số thao tác chuỗi vẫn ở mức nhỏ. 

Vòng lặp bên ngoài kiểm soát độ dài chữ số của một nửa. Vòng lặp bên trong liệt kê tất cả các nửa hợp lệ cho độ dài đó. Điều kiện dừng sớm đảm bảo chúng ta không bao giờ tạo ra các palindrome không cần thiết sau khi k được thu thập. 

Một lỗi triển khai phổ biến là quên tách trường hợp m = 1, trong đó nửa đầu không thể bắt đầu từ 0. Một vấn đề nhỏ khác là tràn số nguyên, nhưng Python xử lý các số nguyên lớn một cách tự nhiên và việc giảm modulo ngăn cản sự tăng trưởng không giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 100
```Chúng tôi tạo ra các palindrome có độ dài chẵn. 

| Bước | m (nửa chiều dài) | x (một nửa) | bảng màu | tổng tích lũy | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 11 | 11 | 
| 2 | 1 | 2 | 22 | 33 | 

Chúng tôi dừng lại sau 2 giá trị. 

Đầu ra:```
33
```Dấu vết này cho thấy chỉ riêng lớp đầu tiên đã cung cấp ít nhất k giá trị khi k nhỏ và thứ tự ngày càng tăng. 

### Ví dụ 2 

đầu vào:```
10 1000
```Chúng tôi liệt kê nhiều giá trị hơn: 

| Bước | m | x | bảng màu | tổng hợp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 11 | 11 | 
| 2 | 1 | 2 | 22 | 33 | 
| ... | 1 | 9 | 99 | 495 | 
| 10 | 2 | 10 | 1001 | 1496 | 

Sau 9 số từ m=1 ta tiếp tục vào m=2. 

Điều này cho thấy cách chuyển tiếp chuỗi giữa các độ dài chữ số trong khi vẫn giữ nguyên thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · d) | Mỗi palindrome được xây dựng từ một nửa số và đảo ngược một lần, với độ dài chữ số d 5 cho k ≤ 1e5 | 
| Không gian | O(1) | Chỉ các bộ đếm và tổng chạy được lưu trữ | 

Các ràng buộc cho phép xây dựng trực tiếp vì k chỉ bằng 100000 và mỗi thao tác đều rẻ. Ngay cả với chi phí đảo ngược chuỗi, tổng công việc vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def make_pal(x: int) -> int:
        s = str(x)
        return int(s + s[::-1])

    k, p = map(int, input().split())
    res = 0
    cnt = 0
    m = 1

    while cnt < k:
        if m == 1:
            start, end = 1, 9
        else:
            start = 10 ** (m - 1)
            end = 10 ** m - 1

        x = start
        while x <= end and cnt < k:
            res = (res + make_pal(x)) % p
            cnt += 1
            x += 1
        m += 1

    return str(res)

# provided sample
assert run("2 100\n") == "33"

# custom cases
assert run("1 100\n") == "11"
assert run("3 100\n") == "66"
assert run("10 1000\n") == str((11+22+33+44+55+66+77+88+99+1001)%1000)
assert run("5 7\n") == str((11+22+33+44+55)%7)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 100 | 11 | palindrome hợp lệ nhỏ nhất | 
| 3 100 | 66 | tính chính xác của tiền tố sớm | 
| 10 1000 | tính toán | chuyển sang độ dài chữ số tiếp theo | 
| 5 7 | hành vi mod | tích lũy mô-đun | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi k đủ nhỏ nên chỉ cần hai nửa có 1 chữ số. Đối với đầu vào như k = 1, thuật toán không được cố gắng tạo ra độ dài chữ số cao hơn. Cấu trúc vòng lặp xử lý việc này một cách tự nhiên vì nó dừng ngay lập tức khi cnt đạt tới k. 

Một trường hợp cạnh khác là khi k trải rộng trên nhiều lớp có độ dài chữ số. Ví dụ: k = 10 buộc thuật toán phải dùng hết tất cả các nửa có 1 chữ số (9 giá trị) và sau đó bắt đầu các nửa có 2 chữ số. Cấu trúc phân lớp đảm bảo rằng thứ tự vẫn chính xác trên ranh giới này vì tất cả các bảng màu 1 chữ số đều nhỏ hơn tất cả các bảng màu 4 chữ số. 

Cuối cùng, modulo p = 1 bị suy biến vì tất cả các đầu ra đều bằng 0. Bản cập nhật modulo đang chạy xử lý việc này một cách an toàn mà không cần vỏ đặc biệt.
