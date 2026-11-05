---
title: "CF 1189A - Keanu Reeves"
description: "Chúng ta được cấp một chuỗi nhị phân, là một chuỗi các ký tự chỉ chứa 0 và 1. Một chuỗi được xác định là tốt nếu số lượng số 0 khác với số lượng số một."
date: "2026-06-12T00:36:39+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 1189
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 572 (Div. 2)"
rating: 800
weight: 1189
solve_time_s: 174
verified: true
draft: false
---

[CF 1189A - Keanu Reeves](https://codeforces.com/problemset/problem/1189/A) 

**Đánh giá:** 800 
**Thẻ:** chuỗi 
**Thời gian giải:** 2m 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân, là một chuỗi các ký tự chỉ chứa`0`Và`1`. Một chuỗi được định nghĩa là tốt nếu số lượng số 0 khác với số lượng số một. Nhiệm vụ là chia chuỗi gốc thành số chuỗi con liền kề nhỏ nhất sao cho mọi chuỗi con đều tốt. Đầu ra trước tiên phải nêu số lượng chuỗi con, sau đó là chính các chuỗi con, cách nhau bằng dấu cách. 

Các ràng buộc nhỏ: độ dài chuỗi$n$nhiều nhất là 100. Điều này cho phép các thuật toán chạy trong$O(n^2)$một cách thoải mái, mặc dù tồn tại nghiệm tuyến tính. Vì chúng ta đang xử lý các chuỗi nhị phân nhỏ nên độ phức tạp bị chi phối nhiều hơn bởi tính đơn giản của logic phân tách hơn là khả năng xử lý. 

Các trường hợp đặc biệt không rõ ràng bao gồm các chuỗi vốn đã tốt và các chuỗi trong đó tất cả các ký tự đều giống hệt nhau. Ví dụ, nếu đầu vào là`111`hoặc`0`, số chuỗi con tối thiểu là một vì bản thân chuỗi đó đã tốt rồi. Ngược lại, các chuỗi như`10`hoặc`01`không tốt nên mỗi ký tự phải được chia thành các chuỗi con riêng lẻ. Một giải pháp bất cẩn có thể cố gắng chia chuỗi thành hai nửa hoặc chỉ chia một cách tham lam khi các ký tự liền kề khác nhau, điều này sẽ thất bại đối với`111000`hoặc`1000`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách phân chia chuỗi có thể thành các chuỗi con liền kề và kiểm tra xem sự kết hợp nào chỉ tạo ra các chuỗi con tốt với số lượng tối thiểu. Có nhiều sự phân chia theo cấp số nhân, đặc biệt$2^{n-1}$, vì vậy điều này là không thực tế ngay cả đối với$n = 100$. 

Quan sát quan trọng là đặc tính tốt chỉ phụ thuộc vào số lượng`0`Và`1`. Một chuỗi là tốt nếu tất cả các ký tự đều giống nhau hoặc nếu nó không hoàn toàn cân bằng. Điều này làm giảm bớt vấn đề: một chuỗi đã tốt có thể vẫn còn nguyên vẹn. Chỉ các chuỗi cân bằng hoàn hảo với số lượng bằng nhau`0`Và`1`cần phải chia nhỏ. Đối với một chuỗi nhị phân, điều này tương đương với việc kiểm tra xem chuỗi đó có bao gồm chính xác một`0`và một`1`có chiều dài bằng hai hoặc lớn hơn. Do đó, trong thực tế, số lượng chuỗi con tối thiểu hầu như luôn là một, ngoại trừ khi chuỗi có số 0 và số 1 chính xác bằng nhau. Trong trường hợp đó, chúng ta có thể chia chuỗi thành hai phần: một phần chứa một`0`hoặc`1`và phần còn lại, đảm bảo cả hai chuỗi con đều tốt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tổng số số 0 và số 1 trong chuỗi đầu vào. Điều này cho phép chúng tôi xác định xem toàn bộ chuỗi đã tốt chưa. 
2. Nếu số lượng khác nhau thì toàn bộ chuỗi là tốt. đầu ra`1`theo sau là chuỗi gốc. 
3. Nếu số lượng bằng nhau thì phải chia. Chiến lược đơn giản nhất là lấy ký tự đầu tiên làm một chuỗi con và phần còn lại làm chuỗi con thứ hai. Điều này đảm bảo cả hai chuỗi con đều tốt vì chuỗi con đầu tiên có số lượng một cho một ký tự và chuỗi thứ hai có số lượng không bằng nhau sau khi loại bỏ một ký tự thuộc một trong hai loại. 
4. In ra số chuỗi con là`1`nếu chuỗi tốt, hoặc`2`nếu cần phân tách, sau đó in các chuỗi con. 

Tại sao nó hoạt động: Bằng cách duy trì số lượng số 0 và số 1, chúng ta có thể xác định ngay xem toàn bộ chuỗi có thỏa mãn điều kiện tốt hay không. Việc tách một ký tự khi số lượng bằng nhau sẽ đảm bảo các chuỗi con kết quả là tốt, vì phần còn lại bây giờ sẽ có số lượng không bằng nhau. Không có sự phân chia nào khác tạo ra ít chuỗi con hơn, đảm bảo tính tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()

zeros = s.count('0')
ones = s.count('1')

if zeros != ones:
    print(1)
    print(s)
else:
    # counts are equal, split into first character and the rest
    print(2)
    print(s[0], s[1:])
```Đầu tiên, mã sẽ đếm các số 0 và số 1 trong chuỗi, điều này cần thiết để xác định xem chuỗi đó có tốt hay không. Nếu số lượng khác nhau, toàn bộ chuỗi được in dưới dạng một chuỗi con. Ngược lại, chúng ta chia ở ký tự đầu tiên. Việc sử dụng`s.strip()`đảm bảo chúng tôi loại bỏ mọi dòng mới ở cuối khỏi đầu vào, tránh các lỗi tinh vi khi in chuỗi con. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
1
1
```| Bước | số không | những cái | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đếm | 0 | 1 | 0 != 1 | 1 chuỗi con: 1 | 

Chuỗi này đã tốt rồi nên chuỗi con tối thiểu là một. 

**Ví dụ 2** 

đầu vào:```
2
10
```| Bước | số không | những cái | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đếm | 1 | 1 | 1 == 1 | Chia thành '1' và '0' | 

Số lượng bằng nhau nên chúng ta chia thành ký tự đầu tiên và phần còn lại. Cả hai chuỗi con đều tốt. 

**Ví dụ 3** 

đầu vào:```
6
100011
```| Bước | số không | những cái | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đếm | 3 | 3 | 3 == 3 | Chia thành '1' và '00011' | 

Việc tách ký tự đầu tiên đảm bảo chuỗi con còn lại có các số 0 và số 1 không bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm số 0 và số 1 yêu cầu phải vượt qua toàn bộ chuỗi. | 
| Không gian | O(n) | Lưu trữ chuỗi và chuỗi con. | 

Được cho$n \le 100$, giải pháp tuyến tính này cực kỳ nhanh và phù hợp với giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    s = input().strip()
    zeros = s.count('0')
    ones = s.count('1')
    if zeros != ones:
        return f"1\n{s}"
    else:
        return f"2\n{s[0]} {s[1:]}"

# Provided samples
assert run("1\n1\n") == "1\n1", "sample 1"
assert run("2\n10\n") == "2\n1 0", "sample 2"
assert run("6\n100011\n") == "2\n1 00011", "sample 3"

# Custom cases
assert run("3\n000\n") == "1\n000", "all zeros"
assert run("3\n111\n") == "1\n111", "all ones"
assert run("4\n0101\n") == "2\n0 101", "equal zeros and ones"
assert run("5\n00100\n") == "1\n00100", "unequal counts"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n000 | 1\n000 | Chuỗi số 0 giống nhau là tốt | 
| 3\n111 | 1\n111 | Chuỗi giống hệt nhau là tốt | 
| 4\n0101 | 2\n0 101 | Số lượng số 0 và số 1 bằng nhau kích hoạt phân chia | 
| 5\n00100 | 1\n00100 | Số lượng không bằng nhau cho phép chuỗi con đơn | 

## Vỏ cạnh 

Đầu vào tối thiểu`1`với chuỗi`1`hoặc`0`được xử lý chính xác: số lượng khác nhau theo mặc định, do đó không xảy ra sự phân chia. Các chuỗi có độ dài`2`với số lượng bằng nhau như`01`hoặc`10`tạo ra chính xác hai chuỗi con. Các chuỗi có tất cả các ký tự giống hệt nhau, chẳng hạn như`000`hoặc`111`, được trả về dưới dạng một chuỗi con. Thuật toán đảm bảo số lượng chuỗi con tối thiểu trong tất cả các trường hợp, vì trường hợp duy nhất yêu cầu phân tách là khi số lượng bằng nhau và không thể cải thiện thêm.
