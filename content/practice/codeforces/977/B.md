---
title: "CF 977B - Hai gram"
description: "Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết hoa và chúng ta được yêu cầu xem xét từng cặp ký tự liền kề trong đó. Mỗi cặp liền kề như vậy tạo thành một “mẫu hai chữ cái”, ví dụ: chuỗi “ABAC” chứa “AB”, “BA” và “AC”."
date: "2026-06-17T01:26:27+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 977
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 479 (Div. 3)"
rating: 900
weight: 977
solve_time_s: 75
verified: false
draft: false
---

[CF 977B - Hai gam](https://codeforces.com/problemset/problem/977/B) 

**Xếp hạng:** 900 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết hoa và chúng ta được yêu cầu xem xét từng cặp ký tự liền kề trong đó. Mỗi cặp liền kề như vậy tạo thành một “mẫu hai chữ cái”, ví dụ: chuỗi “ABAC” chứa “AB”, “BA” và “AC”. 

Nhiệm vụ là tìm ra mẫu hai chữ cái nào xuất hiện thường xuyên nhất dưới dạng chuỗi con liền kề. Vì các chuỗi con được xác định bởi các vị trí liên tiếp nên sự chồng chéo được cho phép một cách tự nhiên, do đó, một mẫu như “AAA” chứa “AA” hai lần. 

Đầu ra không phải là số đếm mà là bất kỳ mẫu nào đạt được tần số tối đa. Nếu có nhiều mẫu buộc nhau thì bất kỳ mẫu nào trong số đó đều được chấp nhận. 

Kích thước đầu vào nhỏ, tối đa 100 ký tự. Điều đó ngay lập tức gợi ý rằng lập luận bậc hai là đủ, vì tổng số cặp liền kề chỉ là n−1, nhiều nhất là 99. 

Một điểm tinh tế là chúng ta không tìm kiếm trên tất cả các mẫu 26×26 có thể một cách độc lập. Chúng ta chỉ quan tâm đến những chuỗi thực sự xuất hiện trong chuỗi dưới dạng các chuỗi con liền kề. Điều này loại bỏ mọi sự mơ hồ về các mẫu như “ZZ” có thể không tồn tại trong đầu vào. 

Trường hợp cạnh chủ yếu là về chuỗi ngắn. Khi n = 2, có chính xác một gram và nó phải được trả về. Khi tất cả các ký tự giống hệt nhau, ví dụ “AAAAA”, câu trả lời đúng là “AA” và nó xuất hiện nhiều lần do trùng lặp. Việc triển khai bất cẩn có thể cố gắng loại bỏ các lần xuất hiện trùng lặp hoặc coi chúng là không trùng lặp, điều này sẽ không chính xác. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là lấy mọi cặp vị trí có thể có (i, j) và kiểm tra xem s[i:i+2] có khớp với mọi lần xuất hiện chuỗi con khác hay không. Điều đó sẽ dẫn đến các ứng cử viên O(n²) và đối với mỗi ứng viên sẽ quét O(n) khác, dẫn đến O(n³). Mặc dù n nhỏ nhưng điều này là không cần thiết và vụng về về mặt khái niệm đối với nhiệm vụ đếm trực tiếp như vậy. 

Một cách mạnh mẽ hơn là tạo ra tất cả các chuỗi con có độ dài bằng hai và đếm chúng bằng từ điển. Điều này làm giảm vấn đề về số lượng tần số trên n-1 phần tử. Mỗi trích xuất chuỗi con là O(1), do đó độ phức tạp tổng thể trở thành tuyến tính theo n. 

Quan sát quan trọng là chúng tôi không xây dựng bất cứ thứ gì mang tính toàn cầu hoặc phụ thuộc vào cấu trúc tầm xa. Mọi câu trả lời hợp lệ đều được xác định đầy đủ bởi tính lân cận cục bộ, vì vậy chỉ cần vượt qua một lần là đủ. Điều này làm giảm vấn đề đếm tần số trong một luồng cặp có kích thước cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force + so sánh lại | O(n³) | O(1) | Quá chậm | 
| Số lần trượt của các cặp liền kề | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi từ trái sang phải, xem xét từng chỉ số i từ 0 đến n−2 và trích xuất chuỗi con hai chữ cái s[i]s[i+1]. Bước này chuyển vấn đề thành một luồng quan sát chồng chéo. 
2. Duy trì một bản đồ tần số ghi lại số lần mỗi mẫu hai chữ cái được nhìn thấy cho đến nay. Mỗi chuỗi con mới tăng bộ đếm của nó. 
3. Theo dõi mẫu tốt nhất hiện tại và tần số của nó trong khi quét. Khi tần số của mẫu vượt quá tần số tốt nhất cho đến nay, hãy cập nhật cả mẫu tốt nhất và số lượng tốt nhất. 
4. Sau khi xử lý tất cả các cặp liền kề, xuất ra mẫu tốt nhất được lưu trữ. 

Lý do chúng tôi có thể cập nhật câu trả lời một cách an toàn trong quá trình quét là vì các cập nhật trong tương lai chỉ phụ thuộc vào các cặp trong tương lai và số lượng chỉ tăng lên. Không có trường hợp nào mà cặp tối ưu trước đó trở nên không hợp lệ hoặc giảm tần suất. 

### Tại sao nó hoạt động 

Mỗi lần xuất hiện hai gram là độc lập và chỉ được xác định bởi một cửa sổ cố định có kích thước hai. Thuật toán đếm mỗi cửa sổ như vậy chính xác một lần. Vì bản đồ tần số lưu trữ số lượng chính xác và chúng tôi luôn duy trì mức tối đa trên tất cả các giá trị nhìn thấy nên cặp được lưu trữ cuối cùng phải là một trong các giá trị cực đại toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()

    freq = {}
    best = ""
    best_cnt = 0

    for i in range(n - 1):
        pair = s[i:i+2]
        freq[pair] = freq.get(pair, 0) + 1

        if freq[pair] > best_cnt:
            best_cnt = freq[pair]
            best = pair

    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng một lần truyền qua chuỗi. Việc trích xuất chuỗi con`s[i:i+2]`là thời gian không đổi vì độ dài lát cắt là cố định. Từ điển duy trì tần số chính xác và cặp tốt nhất chỉ được cập nhật khi quan sát thấy số lượng lớn hơn, đảm bảo hành vi xác định mà không cần chuyển lần thứ hai. 

Một lỗi phổ biến là lặp lại tới n thay vì n−1, điều này sẽ cố gắng truy cập chuỗi con ngoài phạm vi. Một điều nữa là quên rằng các cặp chồng chéo là hợp lệ, điều này được xử lý một cách tự nhiên ở đây vì mỗi chỉ mục đóng góp chính xác một cặp bắt đầu từ nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
ABACABA
```Chúng tôi theo dõi các cặp từng bước. 

| tôi | cặp | tần số sau khi cập nhật | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | AB | AB:1 | AB | 
| 1 | BA | BA:1, AB:1 | AB | 
| 2 | AC | AC:1, AB:1, BA:1 | AB | 
| 3 | CA | CA:1, ... | AB | 
| 4 | AB | AB:2 | AB | 
| 5 | BA | BA:2 | AB hoặc BA (hòa) | 

Câu trả lời cuối cùng là “AB”, mặc dù “BA” cũng có giá trị tùy thuộc vào cách xử lý hòa. 

Dấu vết này cho thấy số lần xuất hiện chồng chéo được tích lũy một cách tự nhiên như thế nào và mức tối đa xuất hiện tăng dần như thế nào. 

### Ví dụ 2 

đầu vào:```
5
AAAAA
```| tôi | cặp | tần số sau khi cập nhật | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | AA | AA:1 | AA | 
| 1 | AA | AA:2 | AA | 
| 2 | AA | AA:3 | AA | 
| 3 | AA | AA:4 | AA | 

Thuật toán ghi lại sự chồng chéo một cách chính xác vì mỗi vị trí liền kề đều đóng góp một lần xuất hiện hợp lệ mới. 

Điều này chứng tỏ rằng các chữ cái giống nhau lặp đi lặp lại không cần xử lý đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua n−1 cặp liền kề, công việc không đổi trên mỗi bước | 
| Không gian | O(1) | Tối đa 26×26 cặp có thể, hằng số giới hạn hiệu quả | 

Giới hạn ràng buộc n ở mức 100, nhưng giải pháp tuyến tính đã hoạt động thoải mái trong giới hạn ngay cả đối với đầu vào lớn hơn nhiều. Việc sử dụng bộ nhớ là không đổi vì bảng chữ cái là cố định và số lượng hai gam có thể có là bị giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys as _sys

    # capture output
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdout = old_stdout

# provided sample
assert run("7\nABACABA\n") in ["AB", "BA"]

# minimum length
assert run("2\nAB\n") == "AB"

# all same characters
assert run("5\nAAAAA\n") == "AA"

# clear dominant pattern
assert run("6\nABCABC\n") in ["AB", "BC", "CA"]

# tie case
assert run("4\nAABB\n") in ["AA", "AB", "BB"]

# alternating pattern
assert run("6\nABABAB\n") in ["AB", "BA"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 AB | AB | trường hợp cạnh tối thiểu | 
| AAAAA | AA | đếm chồng chéo | 
| ABCABC | AB/BC/CA | phân phối theo chu kỳ | 
| ABABAB | AB/BA | hành vi buộc xen kẽ | 

## Vỏ cạnh 

Đối với chuỗi có độ dài hai, chẳng hạn như “XY”, vòng lặp sẽ chạy đúng một lần. Cặp duy nhất là “XY” nên nó trở thành đáp án ngay lập tức với tần số 1. 

Đối với các ký tự lặp lại như “AAAAA”, mỗi chỉ mục đóng góp “AA”, do đó số lượng tích lũy thành 4. Thuật toán không bao giờ cố gắng loại bỏ các lần xuất hiện trùng lặp; nó chỉ đơn giản là tăng từng cửa sổ được quan sát. 

Đối với các chuỗi xen kẽ như “ABABAB”, cả “AB” và “BA” đều xuất hiện nhiều lần. Thuật toán cập nhật giá trị tốt nhất bất cứ khi nào xảy ra mức tăng nghiêm ngặt, do đó cặp đầu tiên đạt mức tối đa vẫn là đầu ra hợp lệ.
