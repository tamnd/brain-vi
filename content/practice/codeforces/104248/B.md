---
title: "CF 104248B - Trình tự"
description: "Chúng ta được yêu cầu đếm xem có thể tạo ra bao nhiêu chuỗi có độ dài n bằng cách sử dụng m chữ cái viết thường bằng m chữ cái Latinh đầu tiên sao cho thỏa mãn một ràng buộc định kỳ cụ thể. Ràng buộc được xác định bởi độ lệch cố định k."
date: "2026-07-01T22:07:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104248
codeforces_index: "B"
codeforces_contest_name: "Udmurt SU Contest 2010"
rating: 0
weight: 104248
solve_time_s: 46
verified: true
draft: false
---

[CF 104248B - Trình tự](https://codeforces.com/problemset/problem/104248/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có thể tạo ra bao nhiêu chuỗi có độ dài n bằng cách sử dụng m chữ cái viết thường bằng m chữ cái Latinh đầu tiên sao cho thỏa mãn một ràng buộc định kỳ cụ thể. 

Ràng buộc được xác định bởi độ lệch cố định k. Với mọi vị trí i từ 1 đến n − k, ký tự ở vị trí i phải khác ký tự ở vị trí i + k. Nói cách khác, nếu bạn nhìn vào chuỗi và so sánh mọi vị trí với vị trí phía trước chính xác k bước, thì không có cặp nào trong số đó được phép khớp. 

Điều này tạo ra một cấu trúc trong đó các vị trí được liên kết bằng khoảng cách k. Thay vì coi chuỗi như một dòng đơn, sẽ hữu ích hơn nếu coi nó như k cột độc lập được hình thành bằng cách lấy các chỉ số theo modulo k. Mỗi cột như vậy hoạt động giống như một chuỗi trong đó các phần tử liền kề (theo bước k) phải khác nhau. 

Các ràng buộc nhỏ: n 40, m 8, k 8. Điều này ngay lập tức cho chúng ta biết rằng các phương pháp nén trạng thái hoặc hàm mũ đối với các vị trí là khả thi. Ngay cả O(n · m^k) hoặc O(m^n) cũng không được chấp nhận, nhưng bất cứ điều gì phân tách thành k bài toán con độc lập nhỏ hoặc sử dụng DP trên các vị trí và k ký tự trước đó đều khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi k ≥ n. Trong tình huống đó, không có ràng buộc nào cả vì không có chỉ số i sao cho i + k ≤ n. Câu trả lời chỉ đơn giản là m^n. Bất kỳ giải pháp nào áp dụng chuyển đổi một cách mù quáng mà không kiểm tra điều này sẽ vẫn hoạt động, nhưng một số công thức DP có thể vô tình bị tính thiếu nếu chúng cho rằng tồn tại sự phụ thuộc. 

Một góc không rõ ràng khác là khi k = 1. Khi đó mọi cặp liền kề đều phải khác nhau, đó là bài toán tiêu chuẩn “không có hai ký tự bằng nhau liên tiếp”. Bất kỳ phương pháp nào giả định không chính xác tính độc lập giữa các vị trí modulo k mà không xử lý k = 1 cẩn thận vẫn có thể hoạt động, nhưng nó sẽ gây căng thẳng nặng nề cho quá trình chuyển đổi DP và là một biện pháp kiểm tra độ tỉnh táo tốt. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tạo ra mọi chuỗi có thể có độ dài n trên m ký tự và kiểm tra xem nó có thỏa mãn ràng buộc k-difference hay không. Đối với mỗi chuỗi, chúng tôi quét tất cả i từ 1 đến n − k và xác minh rằng s[i] ≠ s[i + k]. Điều này tốn O(m^n · n) thời gian trong trường hợp xấu nhất, vì có m^n chuỗi và mỗi lần xác thực đều tốn thời gian tuyến tính. Với n = 40 và m lên tới 8, điều này lớn về mặt thiên văn và hoàn toàn không khả thi. 

Quan sát chính là các ràng buộc chỉ kết nối các vị trí cách nhau bởi k. Điều này có nghĩa là vị trí i chỉ tương tác với i − k và i + k, do đó cấu trúc tách thành các chuỗi độc lập dựa trên các lớp dư lượng modulo k. Mỗi chuỗi là một chuỗi trong đó các phần tử liên tiếp phải khác nhau. Khi chúng tôi sửa nhiệm vụ cho một chuỗi, nó sẽ không ảnh hưởng đến các chuỗi khác. 

Vì vậy, thay vì suy nghĩ tổng thể về n vị trí, chúng ta nhóm các chỉ số theo i mod k. Đối với mỗi lớp dư lượng r, chúng ta xem xét chuỗi r, r + k, r + 2k, v.v. Mỗi chuỗi như vậy là độc lập và hoạt động giống như một ràng buộc đơn giản “không có liền kề bằng nhau” trên một độ dài ngắn hơn. Sau đó chúng tôi nhân số lượng chuỗi hợp lệ trên tất cả k chuỗi. 

Điều này làm giảm vấn đề đếm, đối với mỗi chuỗi có độ dài L, số cách điền nó bằng m chữ cái sao cho các phần tử liền kề khác nhau. Đây là DP tuyến tính tiêu chuẩn trong đó phần tử đầu tiên có m lựa chọn và mỗi phần tử tiếp theo có (m − 1) lựa chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n · n) | O(n) | Quá chậm | 
| Chuỗi mô-đun DP | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng lớp dư lượng modulo k một cách độc lập và nhân kết quả.

1. Chia các vị trí thành k nhóm dựa trên chỉ số modulo k của chúng. Mỗi nhóm tạo thành một chuỗi như r, r + k, r + 2k, v.v. Điều này hoạt động vì các ràng buộc chỉ kết nối các chỉ số ở khoảng cách chính xác là k, do đó không có tương tác nào xảy ra giữa các nhóm khác nhau. 
2. Với mỗi nhóm, hãy tính độ dài L của nó. Đây là số chỉ số i sao cho i ≤ n và i ≡ r (mod k). Cấu trúc của nhóm trở thành một chuỗi đơn giản có độ dài L. 
3. Đếm xem có bao nhiêu cách điền vào một chuỗi L dài bằng cách sử dụng m chữ cái sao cho các vị trí liền kề khác nhau. Chúng tôi định nghĩa dp[j] là số cách để điền vào j phần tử đầu tiên của chuỗi. 
4. Khởi tạo dp[1] = m, vì vị trí đầu tiên có thể là bất kỳ chữ cái nào trong số m chữ cái. 
5. Với mỗi vị trí tiếp theo j ≥ 2, chọn một chữ cái khác với vị trí trước đó. Nếu vị trí trước đó có m lựa chọn thì mỗi lựa chọn đó sẽ cho phép (m − 1) lựa chọn cho vị trí tiếp theo, do đó dp[j] = dp[j − 1] · (m − 1). 
6. Nhân dp[L] vào kết quả chung của mỗi nhóm. 

Kết quả cuối cùng là tích trên tất cả k nhóm. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mỗi lớp modulo tạo thành một chuỗi độc lập trong đó các ràng buộc chỉ liên quan đến các phần tử liên tiếp trong chuỗi đó. Một khi chúng ta đặt điều kiện vào một phép gán cố định cho một nhóm, thì không có ràng buộc nào được chuyển sang nhóm khác, bởi vì bất kỳ cặp bị cấm nào i và i + k đều nằm trong cùng một lớp dư lượng. Do đó, không gian cấu hình toàn cầu phân tích thành tích Descartes của các cấu hình chuỗi độc lập và việc đếm từng chuỗi riêng biệt rồi nhân lên sẽ thu được chính xác tổng số chuỗi hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    # answer is product over k independent chains
    ans = 1

    for r in range(k):
        # compute length of this residue class
        length = 0
        i = r + 1
        while i <= n:
            length += 1
            i += k

        if length == 0:
            continue

        # first position: m choices, rest: (m-1) choices each
        if length == 1:
            ways = m
        else:
            ways = m * (m - 1) ** (length - 1)

        ans *= ways

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau sự phân hủy thành các lớp dư lượng. Đối với mỗi lớp, chúng tôi tính toán rõ ràng độ dài của nó bằng cách duyệt qua các chỉ số với bước k. Điều này an toàn với các ràng buộc (n ≤ 40) và tránh mọi nhu cầu về công thức số học, mặc dù người ta cũng có thể tính độ dài bằng cách sử dụng phép chia số nguyên. 

Mỗi chuỗi được đánh giá bằng dạng đóng m · (m − 1)^(L − 1), xuất phát từ thực tế là phần tử đầu tiên là tự do và mỗi phần tử tiếp theo chỉ loại trừ giá trị trước đó. 

Một cạm bẫy phổ biến là quên rằng các lớp dư lượng khác nhau là độc lập. Một cách khác là áp dụng sai công thức m · (m − 1)^(n − 1) cho toàn bộ chuỗi, sai vì tính kề chỉ tồn tại trong các chuỗi chứ không tồn tại trên toàn bộ chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 2
```Ta có các vị trí 1, 2, 3 và k = 2 nên ta tạo thành hai chuỗi: 

chuỗi 0: vị trí 1, 3 (chiều dài 2) 

chuỗi 1: vị trí 2 (chiều dài 1) 

| Chuỗi | Chiều dài L | Cách tính toán | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 2 | 2 · 1^1 | 2 | 
| 1 | 1 | 2 | 2 | 

Phép nhân cho 4 chuỗi hợp lệ. 

Điều này phù hợp với ý tưởng rằng mỗi cặp (1,3) phải khác nhau trong khi vị trí 2 là miễn phí. 

### Ví dụ 2 

đầu vào:```
4 3 1
```Ở đây k = 1, do đó mỗi cặp liền kề phải khác nhau, tạo thành một chuỗi có độ dài 4. 

| Chuỗi | Chiều dài L | Cách tính toán | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 4 | 3 · 2^3 | 24 | 

Điều này tương ứng với các chuỗi tiêu chuẩn trong đó không có hai ký tự liền kề nào bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét các vị trí theo k cấp số cộng, mỗi cấp có độ dài lên tới n/k | 
| Không gian | O(1) | Chỉ có một vài bộ đếm và kết quả cuối cùng được lưu trữ | 

Thuật toán dễ dàng nằm trong giới hạn vì n nhiều nhất là 40, do đó, ngay cả mô phỏng trực tiếp hoặc DP cũng sẽ nhanh, nhưng cách tiếp cận này làm giảm nó thành số học theo thời gian không đổi cho mỗi nhóm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    data = sys.stdin.read().strip().split()
    n, m, k = map(int, data)

    ans = 1
    for r in range(k):
        length = 0
        i = r + 1
        while i <= n:
            length += 1
            i += k

        if length == 0:
            continue
        if length == 1:
            ways = m
        else:
            ways = m * (m - 1) ** (length - 1)
        ans *= ways

    return str(ans)

# provided sample
assert run("3 2 2") == "4"

# minimum case
assert run("1 5 1") == "5"

# k > n case
assert run("3 2 5") == "8"

# all equal letters impossible but counted via constraints
assert run("2 2 1") == "2"

# larger chain
assert run("4 3 2") == "36"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 1 | 5 | chuỗi ký tự đơn | 
| 3 2 5 | 8 | không có ràng buộc khi k > n | 
| 4 3 2 | 36 | nhiều chuỗi độc lập | 

## Vỏ cạnh 

Đối với k ≥ n, mọi lớp dư lượng ngoại trừ một lớp có thể có độ dài 0 hoặc 1. Thuật toán xử lý điều này một cách tự nhiên vì mỗi chuỗi đóng góp m hoặc 1 tùy thuộc vào độ dài và không có ràng buộc không hợp lệ nào được đưa ra. 

Với k = 1, toàn bộ chuỗi trở thành một chuỗi duy nhất và công thức rút gọn thành m · (m − 1)^(n − 1). Thuật toán không xử lý vấn đề này một cách đặc biệt, nó xuất hiện trực tiếp từ việc xây dựng lớp dư lượng. 

Với m = 1, bất kỳ chuỗi nào có độ dài lớn hơn 1 đều cho ra 0 cách vì (m − 1) = 0. Điều này đúng vì không có hai vị trí nào ở khoảng cách k có thể bằng nhau, nên bất kỳ độ dài nào ≥ 2 đều không thể xảy ra.
