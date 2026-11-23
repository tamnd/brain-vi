---
title: "CF 44C - Ngày Lễ"
description: "Chúng tôi được yêu cầu xác minh lịch tưới nước cho hoa trong một số ngày nghỉ lễ liên tiếp. Mỗi ngày phải tưới nước đúng một lần. Lịch trình quy định cụ thể cho từng người khoảng thời gian họ được phân công tưới hoa."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 44
codeforces_index: "C"
codeforces_contest_name: "School Team Contest 2 (Winter Computer School 2010/11)"
rating: 1300
weight: 44
solve_time_s: 84
verified: true
draft: false
---

[CF 44C - Ngày lễ](https://codeforces.com/problemset/problem/44/C) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác minh lịch tưới nước cho hoa trong một số ngày nghỉ lễ liên tiếp. Mỗi ngày phải tưới nước đúng một lần. Lịch trình quy định cụ thể cho từng người khoảng thời gian họ được phân công tưới hoa. Mục đích là để phát hiện xem có sai sót nào không: ngày không có ai tưới hoa hoặc ngày có nhiều người tưới hoa. Nếu có sai sót, chúng tôi phải báo cáo ngày sớm nhất có số lần tưới không chính xác và số lần tưới hoa trong ngày đó. Nếu không, chúng tôi chỉ báo "OK". 

Dữ liệu đầu vào bao gồm hai số nguyên _n_ và _m_, biểu thị số ngày và số người. Sau đó theo dõi _m_ cặp số nguyên chỉ ngày bắt đầu và ngày kết thúc nhiệm vụ tưới nước của mỗi người. Mỗi phạm vi phân công nằm trong tổng số ngày. Các ràng buộc nhỏ: _n_ và _m_ đều có nhiều nhất là 100. Điều này cho phép các thuật toán có độ phức tạp về thời gian lên đến O(n·m) chạy hiệu quả trong giới hạn thời gian. 

Việc triển khai đơn giản có thể chỉ cần kiểm tra từng ngày một và đếm xem có bao nhiêu nhiệm vụ được thực hiện. Trường hợp ranh giới tinh tế phát sinh khi hai người có phạm vi liên tiếp hoặc chồng chéo. Ví dụ: nếu hai người được chỉ định [2, 4] và [4, 6] thì ngày thứ 4 sẽ được thực hiện hai lần. Một trường hợp khó khăn khác là khi có khoảng trống trong lịch trình, ví dụ: bài tập [1, 2] và [4, 5], khiến ngày thứ 3 không được bổ sung. Một giải pháp bất cẩn chỉ nhìn vào ngày bắt đầu hoặc ngày kết thúc có thể bỏ sót những vấn đề này. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: với mỗi ngày từ 1 đến _n_, hãy đếm xem có bao nhiêu bài tập được thực hiện bằng cách lặp qua tất cả _m_ người. Điều này hiệu quả vì mỗi ngày chúng tôi kiểm tra các khoảng thời gian _m_, dẫn đến các hoạt động O(n·m). Với n và m lên tới 100 thì nhiều nhất là 10.000 phép tính, có thể chấp nhận được. Hạn chế mang tính khái niệm: chúng tôi liên tục kiểm tra sự chồng chéo, điều này là không cần thiết nếu chúng tôi theo dõi trực tiếp số lần tưới nước. 

Một cách tiếp cận hiệu quả hơn là sử dụng một mảng có kích thước _n_, được khởi tạo bằng số 0, để lưu trữ số lần tưới nước mỗi ngày. Đối với mỗi phạm vi gán [a_i, b_i], chúng tôi tăng tất cả các ngày trong phạm vi đó. Sau khi xử lý tất cả các phép gán, chúng tôi quét mảng để tìm bất kỳ ngày nào có giá trị không phải là 1. Điều này đảm bảo tính chính xác vì chúng tôi đếm rõ ràng số lần tưới nước mỗi ngày và cho phép xác định ngay các lỗi. Hiệu quả có thể chấp nhận được với những hạn chế và việc triển khai đơn giản hơn và ít xảy ra lỗi hơn so với việc kiểm tra nhiều lần mỗi ngày. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m) | O(1) | Đã chấp nhận | 
| Mảng đếm phạm vi | O(n·m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`watered`có độ dài _n_ chứa đầy số không. Mỗi phần tử sẽ đại diện cho số lần tưới nước trong ngày đó. Điều này mô hình rõ ràng bất biến chính:`watered[i]`bằng số lượng bài tập của ngày thứ i+1. 
2. Lặp lại nhiệm vụ của mỗi người. Đối với mỗi phạm vi [a_i, b_i], hãy tăng dần`watered[j]`cho tất cả j từ a_i-1 đến b_i-1. Lý do trừ 1 là để chuyển đổi số ngày dựa trên 1 trong đầu vào thành chỉ số mảng dựa trên 0 trong Python. 
3. Sau khi tất cả các bài tập được xử lý, hãy lặp lại`watered`từ ngày đầu tiên đến ngày cuối cùng. Mỗi ngày, hãy kiểm tra xem giá trị có chính xác bằng 1 hay không. Nếu đúng, hãy tiếp tục. Nếu không thì đây là ngày đầu tiên mắc sai lầm. In số ngày (chỉ số +1) và giá trị trong`watered`for that day, then terminate. Điều này đảm bảo chúng tôi báo cáo ngày tối thiểu có số lần tưới không chính xác. 
4. Nếu vòng lặp hoàn tất mà không tìm thấy ngày nào sai, hãy in "OK". 

Thuật toán này hoạt động vì chúng tôi duy trì một bất biến: ở cuối bước 2,`watered[i]`đếm chính xác số lần ngày i+1 được lên lịch tưới nước. Bước 3 đảm bảo rằng chúng tôi phát hiện hành vi vi phạm đầu tiên đối với quy tắc rằng mỗi ngày phải tưới nước đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
watered = [0] * n

for _ in range(m):
    a, b = map(int, input().split())
    for day in range(a - 1, b):
        watered[day] += 1

for i in range(n):
    if watered[i] != 1:
        print(i + 1, watered[i])
        break
else:
    print("OK")
```Mảng`watered`mô hình hóa số lần tưới chính xác mỗi ngày. Chúng tôi cẩn thận chuyển đổi các chỉ số đầu vào dựa trên 1 thành các chỉ mục mảng Python dựa trên 0 để tránh các lỗi riêng lẻ. Vòng lặp kết thúc`i`đảm bảo số ngày tối thiểu có lỗi được tìm thấy. sử dụng`else`trên vòng lặp đảm bảo rằng nếu không có lỗi nào thì "OK" sẽ được in. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
10 5
1 2
3 3
4 6
7 7
8 10
```| Ngày | tưới nước | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 1 | 
| 6 | 1 | 
| 7 | 1 | 
| 8 | 1 | 
| 9 | 1 | 
| 10 | 1 | 

Tất cả các ngày đều được tưới đúng một lần nên kết quả là "OK". 

**Mẫu 2:** 

đầu vào:```
5 2
1 2
4 5
```| Ngày | tưới nước | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 0 | 
| 4 | 1 | 
| 5 | 1 | 

Ngày thứ 3 không được tưới nước nên lỗi đầu tiên xảy ra vào ngày thứ 3, xuất ra “3 0”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) | Đối với mỗi người trong số m người, chúng tôi tăng lên n ngày trong phạm vi của họ. Với n, m ≤ 100, tổng số phép toán ≤ 10.000. | 
| Không gian | O(n) | Mảng`watered`of length n stores counts for each day. | 

Với những hạn chế, giải pháp dễ dàng chạy trong giới hạn thời gian 2 giây và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    n, m = map(int, input().split())
    watered = [0] * n
    for _ in range(m):
        a, b = map(int, input().split())
        for day in range(a - 1, b):
            watered[day] += 1
    for i in range(n):
        if watered[i] != 1:
            print(i + 1, watered[i])
            break
    else:
        print("OK")
    return output.getvalue().strip()

# provided samples
assert run("10 5\n1 2\n3 3\n4 6\n7 7\n8 10\n") == "OK", "sample 1"
assert run("5 2\n1 2\n4 5\n") == "3 0", "sample 2"

# custom cases
assert run("1 1\n1 1\n") == "OK", "single day watered correctly"
assert run("2 1\n1 2\n") == "OK", "two days watered by one person"
assert run("3 2\n1 2\n2 3\n") == "2 2", "overlapping day"
assert run("4 1\n2 3\n") == "1 0", "first day not watered"
assert run("5 3\n1 1\n3 3\n5 5\n") == "2 0", "middle day not watered"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1 1 | được | Kích thước đầu vào tối thiểu hoạt động | 
| 2 1\n1 2 | được | Người độc thân bao gồm nhiều ngày | 
| 3 2\n1 2\n2 3 | 2 2 | Phát hiện ngày trùng lặp | 
| 4 1\n2 3 | 1 0 | Ngày đầu tiên không tưới nước | 
| 5 3\n1 1\n3 3\n5 5 | 2 0 | Phát hiện ngày không tưới ở giữa | 

## Vỏ cạnh 

Trường hợp các phép gán chồng chéo, ví dụ: [1, 2] và [2, 3], được xử lý vì số gia của mảng được tính cho tất cả các ngày được tính. Ngày thứ 2 sẽ có`watered[1] = 2`và thuật toán đưa ra "2 2" một cách chính xác. Nếu ngày đầu tiên bị thiếu trong các bài tập, ví dụ: bài tập [2, 3], thì ngày 1 vẫn là 0 và thuật toán xuất ra "1 0". Trong mọi trường hợp, hệ thống đếm chính xác số lần tưới nước và đảm bảo báo cáo lỗi sớm nhất.
