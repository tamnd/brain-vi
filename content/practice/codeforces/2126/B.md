---
title: "CF 2126B - Không có sòng bạc trên núi"
description: "Chúng ta được cung cấp một chuỗi các ngày, mỗi ngày được dán nhãn là mưa (1) hoặc tốt (0). Jean muốn hoàn thành càng nhiều chuyến đi bộ đường dài càng tốt. Mỗi chuyến đi bộ mất đúng k ngày thời tiết tốt liên tục và sau khi kết thúc chuyến đi bộ, anh ta phải nghỉ ngơi ít nhất một ngày trước khi bắt đầu chuyến đi tiếp theo."
date: "2026-06-08T03:21:47+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2126
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1037 (Div. 3)"
rating: 800
weight: 2126
solve_time_s: 89
verified: true
draft: false
---

[CF 2126B - Không có sòng bạc trên núi](https://codeforces.com/problemset/problem/2126/B) 

**Đánh giá:** 800 
**Tags:** dp, tham lam 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các ngày, mỗi ngày được dán nhãn là mưa (1) hoặc tốt (0). Jean muốn hoàn thành càng nhiều chuyến đi bộ đường dài càng tốt. Mỗi lần đi bộ mất chính xác`k`những ngày thời tiết đẹp liên tục và sau khi kết thúc chuyến đi bộ đường dài, anh ta phải nghỉ ngơi ít nhất một ngày trước khi bắt đầu chuyến đi tiếp theo. Bài toán yêu cầu chúng ta xác định số lần đi bộ đường dài tối đa mà anh ta có thể thực hiện trong chuỗi ngày. 

Đầu vào có thể lớn: lên tới`10^5`ngày cho mỗi trường hợp thử nghiệm và lên đến`10^4`các trường hợp thử nghiệm, với tổng số ngày của các trường hợp thử nghiệm không vượt quá`10^5`. Điều này ngụ ý rằng mọi giải pháp đều phải chạy theo thời gian tuyến tính tương ứng với số ngày. Các vòng lặp bậc hai hoặc lồng nhau lặp đi lặp lại trong phạm vi ngày sẽ quá chậm. 

Các trường hợp đặc biệt không rõ ràng bao gồm các chuỗi có những cơn mưa ngắt quãng làm gián đoạn việc đi bộ đường dài, các chuỗi trong đó`k`lớn hơn số ngày tốt liên tiếp và số ngày tất cả các ngày đều mưa hoặc tất cả đều tốt. Ví dụ, nếu`n = 5`,`k = 2`, Và`a = [0, 1, 0, 0, 0]`, một kẻ tham lam ngây thơ nhảy vọt lên mọi nơi`k`những ngày không kiểm tra thời gian nghỉ xen kẽ có thể tính quá mức số lần tăng vọt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng bắt đầu một chuyến đi bộ đường dài hàng ngày`i`và kiểm tra xem tiếp theo`k`ngày đều tốt. Nếu đúng như vậy, chúng tôi sẽ tăng số lần đi bộ đường dài và bỏ qua ngày này`i + k + 1`cho lần tăng giá tiềm năng tiếp theo. Điều này có tác dụng vì nó tôn trọng ngày nghỉ bắt buộc. Tuy nhiên, điều này đòi hỏi phải kiểm tra`k`ngày cho mỗi ngày bắt đầu có thể, dẫn đến sự phức tạp trong trường hợp xấu nhất`O(n * k)`. Đối với lớn`n`Và`k`, điều này có thể đạt tới`10^10`những hoạt động không thể chấp nhận được. 

Điều quan trọng là chúng ta không cần phải kiểm tra lại nhiều lần các phân đoạn chồng chéo. Thay vào đó, chúng ta có thể đếm số ngày tốt liên tiếp trong một lượt. Mỗi lần chúng tôi tìm thấy một khối`length`các số 0 liên tiếp, số lần tăng vọt có thể được thực hiện trong khối đó là`length // k`. Mỗi lần đi bộ đường dài trong khu vực này đương nhiên sẽ tôn trọng ngày nghỉ bắt buộc vì về mặt khái niệm, chúng ta có thể đặt số "1" sau mỗi lần đi bộ đường dài, giúp giảm hiệu quả chiều dài có thể sử dụng. Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính duy nhất của mảng, điều này là tối ưu với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k) | O(1) | Quá chậm đối với n và k lớn | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`count`về 0 để lưu trữ số lần tăng vọt cho trường hợp thử nghiệm hiện tại. Khởi tạo một biến`length`về 0 để theo dõi chuỗi ngày tốt liên tiếp hiện tại. 
2. Lặp lại mỗi ngày trong mảng. Nếu ngày tốt (`a[i] = 0`), tăng`length`. Nếu ngày mưa (`a[i] = 1`), cài lại`length`về không. Điều này duy trì sự bất biến rằng`length`luôn đại diện cho khối ngày tốt lành liền kề hiện tại kết thúc vào lúc`i`. 
3. Bất cứ khi nào`length`đạt tới`k`, chúng ta có thể lên lịch đi bộ đường dài. Tăng`count`bởi một. Sau đó, để thực thi ngày nghỉ bắt buộc, hãy giảm`length`qua`k + 1`. Phép trừ bỏ qua một cách hiệu quả những ngày đi bộ đường dài cộng với ngày nghỉ ngơi, giữ nguyên`length`sẵn sàng đếm lần tăng giá tiềm năng tiếp theo trong những ngày còn lại. 
4. Tiếp tục lặp cho đến hết mảng. Sau khi lặp lại,`count`giữ số lần tăng tối đa cho trường hợp thử nghiệm. 
5. In`count`cho từng trường hợp thử nghiệm. 

Lý do điều này hoạt động là vì thuật toán duy trì độ dài của những ngày tốt liên tiếp và lên lịch tăng vọt bất cứ khi nào có đủ kích thước khối`k`có sẵn. Trừ`k + 1`đảm bảo chuyến đi bộ tiếp theo tôn trọng ngày nghỉ bắt buộc mà không quét lại hoặc chồng chéo các đoạn đường. Không có chuyến đi bộ đường dài hợp lệ nào bị bỏ lỡ và không có chuyến đi bộ đường dài nào vi phạm các quy tắc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    count = 0
    length = 0
    for day in a:
        if day == 0:
            length += 1
            if length >= k:
                count += 1
                length -= k + 1
        else:
            length = 0
    print(count)
```Dòng đầu tiên ghi số lượng test case. Đối với mỗi trường hợp thử nghiệm, chúng tôi đọc`n`Và`k`và sau đó là mảng`a`. Vòng lặp chính lặp lại mỗi ngày, cập nhật chuỗi thời tiết tốt hiện tại. Khi có thể đi bộ đường dài, chúng tôi sẽ tăng`count`và thực thi ngày nghỉ bằng cách giảm`length`. Đặt lại`length`vào những ngày mưa để đảm bảo chúng tôi không bao giờ cố gắng đi bộ đường dài khi thời tiết xấu. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`n = 5, k = 1, a = [0, 1, 0, 0, 0]`| ngày | một [tôi] | chiều dài | đếm | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 1 | 0 | 1 | 
| 3 | 0 | 1 | 2 | 
| 4 | 0 | 1 | 2 | 
| 5 | 0 | 2 | 3 | 

Điều này chứng tỏ rằng có thể lên lịch nhiều chuyến đi bộ đường dài với những ngày nghỉ xen kẽ. Sự giảm giá tham lam của`length`đảm bảo chúng ta bỏ qua ngày nghỉ một cách chính xác. 

**Mẫu 2:** 

đầu vào:`n = 7, k = 3, a = [0, 0, 0, 0, 0, 0, 0]`| ngày | một [tôi] | chiều dài | đếm | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | 
| 2 | 0 | 2 | 0 | 
| 3 | 0 | 3 | 1 | 
| 4 | 0 | 0 | 1 | 
| 5 | 0 | 1 | 1 | 
| 6 | 0 | 2 | 1 | 
| 7 | 0 | 3 | 2 | 

Điều này cho thấy thuật toán lên lịch tăng vọt ở mọi khối có thể, tôn trọng ngày nghỉ cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ngày được truy cập một lần và các hoạt động bên trong vòng lặp là thời gian không đổi. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được sử dụng. Mảng đầu vào được đọc một lần. | 

Điều này phù hợp một cách thoải mái trong giới hạn của bài toán, vì`n`được tổng hợp trên tất cả các trường hợp thử nghiệm nhiều nhất là`10^5`, làm cho các đường chuyền tuyến tính O(n) được chấp nhận trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("5\n5 1\n0 1 0 0 0\n7 3\n0 0 0 0 0 0 0\n3 1\n1 1 1\n4 2\n0 1 0 1\n6 2\n0 0 1 0 0 0\n") == "3\n2\n0\n0\n2", "sample tests"

# Custom edge cases
assert run("1\n1 1\n0\n") == "1", "single good day"
assert run("1\n1 1\n1\n") == "0", "single rainy day"
assert run("1\n5 5\n0 0 0 0 0\n") == "1", "entire array one hike"
assert run("1\n5 2\n0 0 0 0 0\n") == "2", "overlapping block with rest day"
assert run("1\n6 2\n0 0 1 0 0 0\n") == "2", "split by rain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 ngày tốt lành | 1 | đầu vào kích thước tối thiểu | 
| 1 ngày mưa | 0 | đầu vào có kích thước tối thiểu mà không cần tăng vọt | 
| 5 ngày mọi việc đều ổn, k=5 | 1 | chuyến đi bộ đơn phù hợp chính xác | 
| 5 ngày mọi việc đều ổn, k=2 | 2 | tăng vọt nhiều lần và buộc phải có ngày nghỉ | 
| 6 ngày chia cắt bởi mưa, k=2 | 2 | xử lý đúng khối ngày tốt bị hỏng | 

## Vỏ cạnh 

Đối với trường hợp`a = [0]`với`k = 1`, thuật toán tăng dần`length`đến 1, đáp ứng yêu cầu tăng vọt, thêm vào`count`, và trừ`k + 1 = 2`, làm`length`âm, nhưng vòng lặp kết thúc và`count = 1`. Điều này lên lịch chính xác cho một chuyến đi bộ. 

Vì`a = [1]`với`k = 1`,`length`
