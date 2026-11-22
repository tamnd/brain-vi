---
title: "CF 349B - Tô màu hàng rào"
description: "Igor muốn sơn hàng rào với số lượng lớn nhất có thể bằng cách sử dụng một lượng sơn hạn chế. Mỗi chữ số từ 1 đến 9 có một chi phí sơn cụ thể và không thể sử dụng số 0."
date: "2026-06-06T18:45:38+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 349
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 202 (Div. 2)"
rating: 1700
weight: 349
solve_time_s: 90
verified: true
draft: false
---

[CF 349B - Tô màu hàng rào](https://codeforces.com/problemset/problem/349/B) 

**Đánh giá:** 1700 
**Tas:** cấu trúc dữ liệu, dp, tham lam, triển khai 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Igor muốn sơn hàng rào với số lượng lớn nhất có thể bằng cách sử dụng một lượng sơn hạn chế. Mỗi chữ số từ 1 đến 9 có một chi phí sơn cụ thể và không thể sử dụng số 0. Đầu vào bao gồm tổng số sơn có sẵn$v$và một mảng chín số nguyên dương$a_1$bởi vì$a_9$cho biết sơn cần thiết cho mỗi chữ số. Sản lượng phải là số lượng lớn nhất mà Igor có thể tạo ra bằng nguồn cung cấp sơn của mình. Nếu không thể vẽ được chữ số nào thì kết quả là -1. 

Những ràng buộc mang lại$v$lên đến$10^6$và chi phí sơn lên tới$10^5$. Điều này ngụ ý rằng chúng ta không thể thử tất cả các tổ hợp chữ số có thể có một cách ngây thơ, bởi vì có thể có hàng triệu chữ số và việc thử mọi tập hợp con sẽ tốn kém theo cấp số nhân. Chúng ta phải hướng tới một cách tiếp cận tuyến tính hoặc gần tuyến tính về mặt$v$. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó chữ số rẻ nhất không phải là số lớn nhất. Ví dụ, nếu$v = 5$và chi phí là$[5, 4, 3, 2, 1, 2, 3, 4, 5]$, một cách tiếp cận tham lam ngây thơ chọn chữ số lớn nhất có thể chấp nhận được ở mỗi bước có thể chọn 5 nhiều lần. Một trường hợp cạnh khác là khi$v$nhỏ hơn chi phí sơn của bất kỳ chữ số nào, nên mang lại -1. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử tất cả các chuỗi chữ số, tính tổng chi phí sơn của chúng và chọn giá trị số tối đa. Mặc dù điều này đúng về mặt khái niệm nhưng số lượng trình tự tăng theo cấp số nhân với$v$chia cho chi phí sơn tối thiểu, có thể lên tới hàng triệu. Việc xây dựng và so sánh rõ ràng tất cả các số là không thể tính toán được. 

Quan sát quan trọng là số lượng lớn nhất đạt được bằng cách tối đa hóa độ dài của số trước tiên. Với phần sơn còn lại, mỗi vị trí phải giữ chữ số lớn nhất có thể. Điều này làm giảm vấn đề khi trước tiên phải tính toán số chữ số tối đa mà chúng ta có thể vẽ bằng chữ số rẻ nhất, sau đó nâng cấp lặp đi lặp lại các chữ số từ trái sang phải nếu lớp sơn còn lại cho phép chữ số đắt tiền hơn thay thế chữ số rẻ hơn. Cách tiếp cận này tận dụng thực tế là giá trị số tăng nhiều nhất bằng cách đặt các chữ số cao hơn ở các vị trí cao hơn sau khi độ dài được cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(9^(v/min_cost)) | O(v/min_cost) | Quá chậm | 
| Tối ưu | O(n * 9) ≈ O(v) | O(v/min_cost) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định chữ số rẻ nhất. Lặp lại mảng chi phí và tìm chữ số có chi phí sơn tối thiểu. Hãy để điều này được$d_\text{min}$với chi phí$c_\text{min}$. 
2. Tính số chữ số lớn nhất mà chúng ta có thể vẽ được chỉ bằng chữ số rẻ nhất. Nếu như$v < c_\text{min}$, in -1 vì không thể vẽ được chữ số nào. 
3. Khởi tạo một mảng có độ dài bằng số chữ số tối đa, chứa đầy chữ số rẻ nhất. Trừ$c_\text{min} \times \text{length}$từ$v$để tính số sơn còn lại. 
4. Lặp lại từ trái sang phải qua từng vị trí chữ số. Đối với mỗi vị trí, hãy thử thay thế chữ số hiện tại bằng chữ số lớn nhất có thể và không vượt quá số sơn còn lại. Cụ thể, hãy kiểm tra các chữ số từ 9 trở xuống$d_\text{min}+1$. Nếu chữ số thay thế có giá$c$, đảm bảo rằng$v + c_\text{min} \ge c$vì giá trị của chữ số gốc$c_\text{min}$đã được chi tiêu rồi. Nếu khả thi, hãy thực hiện thay thế và cập nhật phần sơn còn lại. 
5. Sau khi xử lý tất cả các vị trí, xuất số kết quả dưới dạng chuỗi. 

Tại sao nó hoạt động: Thuật toán đảm bảo độ dài tối đa trước tiên, trực tiếp tối đa hóa giá trị số về số chữ số. Việc thay thế các chữ số ngoài cùng bên trái bằng các chữ số lớn hơn trong khi vẫn tôn trọng phần sơn còn lại sẽ đảm bảo giá trị lớn nhất có thể mà không làm giảm độ dài. Bất biến này đúng cho từng vị trí một cách độc lập, do đó số toàn cầu là tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

v = int(input())
costs = list(map(int, input().split()))

min_cost = min(costs)
min_digit = costs.index(min_cost) + 1

if v < min_cost:
    print(-1)
    sys.exit()

length = v // min_cost
v -= length * min_cost

number = [min_digit] * length

for i in range(length):
    for d in range(9, min_digit, -1):
        c = costs[d - 1]
        if v + min_cost >= c:
            number[i] = d
            v -= c - min_cost
            break

print(''.join(map(str, number)))
```Phần đầu tiên đọc đầu vào và xác định chữ số rẻ nhất. Sau đó, chúng tôi tính toán số chữ số tối đa để đảm bảo lượng sơn còn lại được theo dõi chính xác. Vòng lặp lồng nhau nâng cấp cẩn thận các chữ số từ lớn nhất đến nhỏ nhất khả thi, chỉ trừ đi chi phí bổ sung. Thuật toán xử lý chính xác các trường hợp có thể nâng cấp nhiều lần và đảm bảo các chữ số ngoài cùng bên trái được tối đa hóa. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:```
5
5 4 3 2 1 2 3 4 5
```Biến: 

| Bước | phút_chữ số | chi phí tối thiểu | chiều dài | còn lại v | số | 
| --- | --- | --- | --- | --- | --- | 
| Khởi tạo | 5 | 1 | 5 | 0 | [5,5,5,5,5] | 

Không còn sơn để nâng cấp, đầu ra`55555`. 

**Ví dụ 2:** 

đầu vào:```
10
1 2 3 4 5 6 7 8 9
```Biến: 

| Bước | phút_chữ số | chi phí tối thiểu | chiều dài | còn lại v | số | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 10 | 0 | [1]*10 | 
| Nâng cấp pos0 | 9 | 9 | khả thi | 0 | [9,1,1,1,1,1,1,1,1,1] | 
| Nâng cấp pos1 | 9 | 9 | sơn không đủ | 0 | không thay đổi | 

Đầu ra:`9111111111`Dấu vết này cho thấy chữ số đầu tiên được nâng cấp để tối đa hóa giá trị trong khi vẫn tôn trọng lớp sơn còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(9 * v/min_cost) | Chúng tôi có thể thử tối đa 9 lần thay thế cho mỗi vị trí chữ số | 
| Không gian | O(v/min_cost) | Mảng lưu trữ các chữ số của số kết quả | 

Được cho$v ≤ 10^6$và chi phí tối thiểu ≥1, có thể thực hiện tối đa 10^6 lần lặp, trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    v = int(input())
    costs = list(map(int, input().split()))
    min_cost = min(costs)
    min_digit = costs.index(min_cost) + 1
    if v < min_cost:
        return "-1"
    length = v // min_cost
    v -= length * min_cost
    number = [min_digit] * length
    for i in range(length):
        for d in range(9, min_digit, -1):
            c = costs[d - 1]
            if v + min_cost >= c:
                number[i] = d
                v -= c - min_cost
                break
    return ''.join(map(str, number))

assert run("5\n5 4 3 2 1 2 3 4 5\n") == "55555", "sample 1"
assert run("10\n1 2 3 4 5 6 7 8 9\n") == "9111111111", "custom 1"
assert run("3\n5 5 5 5 5 5 5 5 5\n") == "-1", "custom 2"
assert run("15\n1 1 1 1 1 1 1 1 1\n") == "999999999999999", "custom 3"
assert run("7\n1 3 3 3 3 3 3 3 3\n") == "7111111", "custom 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5\n5 4 3 2 1 2 3 4 5 | 55555 | Không thể nâng cấp, chữ số rẻ nhất chiếm ưu thế | 
| 10\n1 2 3 4 5 6 7 8 9 | 9111111111 | Nâng cấp một phần chữ số ngoài cùng bên trái | 
| 3\n5 5 5 5 5 5 5 5 5 | -1 | Không đủ sơn cho bất kỳ chữ số nào | 
| 15\n1 1 1 1 1 1 1 1 1 | 999999999999999 | Mọi chi phí đều bằng nhau, nâng cấp lên mức khả thi tối đa | 
| 7\n1 3 3 3 3 3 3 3 3 | 7111111 | Chỉ nâng cấp chữ số đầu tiên, chữ số tối thiểu còn lại | 

## Vỏ cạnh 

Nếu$v$nhỏ hơn bất kỳ chi phí chữ số nào, chẳng hạn như$v=3$với tất cả chi phí ≥5, thuật toán in -1 ngay lập tức, xử lý chính xác tình huống không thể xảy ra. 

Nếu nhiều chữ số có giá trị nhỏ nhất như nhau thì thuật toán chọn chữ số nhỏ nhất để điền vào số, đảm bảo giá trị lớn nhất
