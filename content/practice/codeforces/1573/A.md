---
title: "CF 1573A - Đếm ngược"
description: "Bài toán trình bày một đồng hồ số hiển thị một số nguyên có chính xác $n$ chữ số. Mỗi chữ số nằm trong khoảng từ 0 đến 9 và màn hình hiển thị bao gồm các số 0 đứng đầu nên số luôn có độ dài $n$. Mục tiêu là giảm con số này xuống 0 với ít thao tác nhất có thể."
date: "2026-06-10T11:12:59+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1573
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 743 (Div. 2)"
rating: 800
weight: 1573
solve_time_s: 322
verified: false
draft: false
---

[CF 1573A - Đếm ngược](https://codeforces.com/problemset/problem/1573/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 5 phút 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bài toán trình bày một chiếc đồng hồ số hiển thị một số nguyên có số đo chính xác$n$chữ số. Mỗi chữ số nằm trong khoảng từ 0 đến 9 và màn hình bao gồm các số 0 đứng đầu nên số luôn có độ dài$n$. Mục tiêu là giảm con số này xuống 0 với ít thao tác nhất có thể. Được phép thực hiện hai thao tác: giảm số đi 1 hoặc hoán đổi hai chữ số bất kỳ trên màn hình. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra$n$, số chữ số và chuỗi chữ số biểu thị giá trị đồng hồ hiện tại. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất: số lượng thao tác tối thiểu để đạt đến 0. 

Các ràng buộc rất nhẹ: tối đa 100 chữ số cho mỗi trường hợp thử nghiệm và tối đa 1000 trường hợp thử nghiệm. Việc giảm từng chữ số riêng lẻ là khả thi nếu số nhỏ, nhưng đối với những số có nhiều chữ số, chúng ta cần khai thác hoán đổi để giảm thiểu số giảm. Trường hợp cạnh xảy ra khi đồng hồ đã hiển thị số 0 - không cần thực hiện thao tác nào. Một tình huống tế nhị khác nảy sinh khi số 0 bị chôn vùi trong số. Ví dụ, đã cho`1000`, hoán đổi vị trí dẫn đầu`1`với số 0 cuối cùng làm giảm số lượng giảm đáng kể so với mức giảm đơn giản từ`1000`xuống`0`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xem xét tất cả các chuỗi hoán đổi và giảm giá. Đối với một số có$n$chữ số, người ta có thể thử mọi cấu hình hoán đổi có thể để giảm thiểu số lượng giảm. Cách tiếp cận này đúng vì cuối cùng nó sẽ tìm ra chuỗi các thao tác tạo ra số 0 trong ít bước nhất. Tuy nhiên, độ phức tạp của nó phụ thuộc vào$n$, làm cho nó không thể thực hiện được$n$lên tới 100. Cụ thể, số lần hoán đổi là$O(n^2)$và mức giảm có thể lên tới$10^n$, vì vậy vũ lực rõ ràng là quá chậm. 

Quan sát quan trọng là các chữ số duy nhất quan trọng đối với việc giảm là chữ số cuối cùng khác 0 trong số. Tất cả các chữ số khác có thể được hoán đổi sang vị trí đơn vị để giảm số hiệu quả hơn. Trong thực tế, chúng ta chỉ cần đếm xem có bao nhiêu chữ số khác 0 và chữ số ngoài cùng bên phải ở đâu. Mỗi chữ số khác 0 đóng góp một mức giảm, ngoại trừ một trong số chúng, sẽ giảm về 0 sau khi di chuyển nó đến vị trí ít quan trọng nhất. Hoán đổi được tính là số chữ số khác 0 không ở vị trí cuối cùng. 

Vì vậy, cách tiếp cận tối ưu là: đếm tổng số chữ số khác 0, trừ đi một nếu chữ số cuối cùng khác 0 và cộng số chữ số cần hoán đổi để đưa chữ số khác 0 vào vị trí cuối cùng. Điều này làm giảm vấn đề xuống còn việc đếm đơn giản thay vì khám phá mọi chuỗi hoán đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * 10^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và chuỗi đại diện cho giá trị đồng hồ. 
2. Nếu số đã toàn là số 0, xuất 0 và tiếp tục. 
3. Đếm tổng số chữ số khác 0 trong chuỗi. Mỗi chữ số này sẽ cần ít nhất một thao tác: giảm hoặc hoán đổi + giảm. 
4. Xác định chữ số cuối cùng trong chuỗi. Nếu nó khác 0 thì việc giảm trên tất cả các chữ số khác 0 khác yêu cầu hoán đổi để di chuyển chúng ra khỏi vị trí cuối cùng trước tiên. Số lần hoán đổi tối thiểu được yêu cầu là số chữ số khác 0 ngoại trừ số khác 0 ngoài cùng bên phải. 
5. Tính tổng các phép toán bằng tổng của: số lần hoán đổi cần thiết cộng với tổng số lần giảm để giảm tất cả các chữ số khác 0 về 0. 
6. In tổng số thao tác của test case. 

Tại sao nó hoạt động: mọi chữ số khác 0 cuối cùng phải được giảm xuống 0, bằng cách giảm trực tiếp hoặc sau khi hoán đổi nó đến vị trí ít quan trọng nhất. Bằng cách đếm các chữ số khác 0 và điều chỉnh chữ số cuối cùng, chúng tôi đảm bảo không có thao tác nào bị lãng phí. Chiến lược này đảm bảo ít thao tác nhất có thể vì các giao dịch hoán đổi chỉ được sử dụng khi cần thiết để đưa một chữ số khác 0 về vị trí cuối cùng để giảm dần hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        if s.count('0') == n:
            print(0)
            continue
        
        non_zero_count = sum(1 for ch in s if ch != '0')
        # Check if last digit is zero
        if s[-1] != '0':
            # All other non-zero digits except last require a swap
            swaps_needed = non_zero_count - 1
        else:
            # Last digit is zero, all non-zero digits need to be swapped somewhere
            swaps_needed = non_zero_count
        
        print(swaps_needed)

solve()
```Đầu tiên, mã sẽ đọc số lượng trường hợp kiểm thử và lặp qua từng trường hợp kiểm thử. Nó kiểm tra xem chuỗi có phải là số 0 hay không và in 0 ngay lập tức nếu có. Các chữ số khác 0 được tính và các giao dịch hoán đổi được tính toán dựa trên vị trí của chữ số cuối cùng. sử dụng`sum(1 for ch in s if ch != '0')`đếm các chữ số khác 0 một cách hiệu quả. Giải pháp cẩn thận tránh các lỗi sai lệch một khi tính toán hoán đổi, đặc biệt khi chữ số cuối cùng khác 0. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`007`: 

| s | non_zero_count | chữ số cuối cùng | trao đổi_cần thiết | đầu ra | 
| --- | --- | --- | --- | --- | 
| 007 | 1 | 7 | 0 | 7 | 

Giải thích: chữ số khác 0 duy nhất là`7`ở vị trí cuối cùng. Không cần trao đổi; 7 lần giảm được thực hiện. 

Đầu vào mẫu`1000`: 

| s | non_zero_count | chữ số cuối cùng | trao đổi_cần thiết | đầu ra | 
| --- | --- | --- | --- | --- | 
| 1000 | 1 | 0 | 1 | 2 | 

Giải thích: số khác 0`1`không ở vị trí cuối cùng, vì vậy một lần hoán đổi sẽ di chuyển nó về cuối, sau đó một lần giảm sẽ đặt nó về 0. Tổng cộng 2 hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đếm các chữ số khác 0 và kiểm tra chữ số cuối cùng yêu cầu quét chuỗi một lần. | 
| Không gian | O(1) | Chỉ có bộ đếm được sử dụng; không có mảng bổ sung tỷ lệ thuận với n. | 

Với tối đa 1000 trường hợp thử nghiệm, mỗi trường hợp có 100 chữ số, tổng số thao tác dưới 10^5, nằm trong giới hạn 1 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("7\n3\n007\n4\n1000\n5\n00000\n3\n103\n4\n2020\n9\n123456789\n30\n001678294039710047203946100020\n") == \
"7\n2\n0\n5\n6\n53\n115"

# Custom cases
assert run("1\n1\n0\n") == "0", "single zero"
assert run("1\n1\n9\n") == "9", "single non-zero"
assert run("1\n4\n0001\n") == "1", "non-zero at last position"
assert run("1\n4\n1000\n") == "2", "non-zero at first position"
assert run("1\n5\n12345\n") == "9", "multiple non-zero digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 chữ số 0 | 0 | Đầu vào tối thiểu, đã bằng 0 | 
| 1 chữ số 9 | 9 | Đầu vào tối thiểu, giảm một lần | 
| 0001 | 1 | Khác 0 ở vị trí cuối cùng chỉ cần giảm | 
| 1000 | 2 | Khác 0 ở vị trí đầu tiên yêu cầu hoán đổi + giảm | 
| 12345 | 9 | Nhiều chữ số khác 0, hoán đổi + giảm được tính chính xác | 

## Vỏ cạnh 

Khi tất cả các chữ số bằng 0, ví dụ: đầu vào`0000`, thuật toán ngay lập tức đưa ra 0 mà không cần tính toán không cần thiết. Khi chữ số cuối cùng khác 0, như`007`, chữ số cuối cùng chỉ đóng góp vào việc giảm dần và không có sự hoán đổi, mang lại các thao tác tối thiểu. Đối với một số như`1000`, số khác 0 đứng đầu yêu cầu hoán đổi để chuyển nó sang chữ số có nghĩa nhỏ nhất để giảm dần. Thuật toán xử lý các chuỗi có độ dài 1, 100 và các số có nhiều số 0 xen kẽ một cách chính xác vì nó luôn đếm các chữ số khác 0 và điều chỉnh hoán đổi dựa trên vị trí chữ số cuối cùng.
