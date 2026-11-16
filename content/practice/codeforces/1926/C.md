---
title: "CF 1926C - Vlad và Tổng các chữ số"
description: "Đối với mỗi trường hợp thử nghiệm, chúng tôi được cấp một số n. Vlad viết tất cả các số nguyên từ 1 đến n lên bảng. Sau đó, mỗi số nguyên được thay thế bằng tổng các chữ số thập phân của nó. Nhiệm vụ là tính $$sum{i=1}^{n} text{digitSum}(i)$$ trong đó DigitSum(i) là tổng các chữ số của i."
date: "2026-06-08T18:59:20+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1926
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 928 (Div. 4)"
rating: 1200
weight: 1926
solve_time_s: 119
verified: true
draft: false
---

[CF 1926C - Vlad và tổng các chữ số](https://codeforces.com/problemset/problem/1926/C) 

**Đánh giá:** 1200 
**Tags:** dp, triển khai 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một số`n`. Vlad viết tất cả các số nguyên từ`1`bởi vì`n`trên bảng. Sau đó, mỗi số nguyên được thay thế bằng tổng các chữ số thập phân của nó. 

Nhiệm vụ là tính toán$$\sum_{i=1}^{n} \text{digitSum}(i)$$Ở đâu`digitSum(i)`là tổng các chữ số của`i`. 

Ví dụ, khi`n = 12`, tổng các chữ số là:$$1,2,3,4,5,6,7,8,9,1,2,3$$và tổng của chúng là`51`. 

Những hạn chế là quan sát quan trọng. Có thể có tới`10^4`các trường hợp thử nghiệm và mỗi trường hợp`n`có thể lớn như`200000`. Tính toán câu trả lời độc lập cho từng trường hợp kiểm thử bằng cách lặp lại từ`1`ĐẾN`n`sẽ lặp lại cùng một công việc nhiều lần. Mặc dù`200000`bản thân nó không lớn, làm điều đó cho`10000`các trường hợp thử nghiệm sẽ yêu cầu khoảng hai tỷ thao tác, vượt xa những gì phù hợp với một`0.5`giới hạn lần thứ hai. 

Giá trị tối đa của`n`chỉ là`200000`, điều này gợi ý một chiến lược khác. Vì mọi truy vấn đều yêu cầu tổng tiền tố trên cùng một phạm vi số nên chúng ta có thể xử lý trước các câu trả lời cho tất cả các giá trị từ`1`ĐẾN`200000`một lần, sau đó trả lời từng trường hợp kiểm tra trong thời gian không đổi. 

Một vài trường hợp đáng chú ý. 

Coi như`n = 1`. 

đầu vào:```
1
1
```Câu trả lời là:```
1
```Mảng tiền tố phải được khởi tạo chính xác để giá trị đầu tiên không bị bỏ qua. 

Coi như`n = 10`. 

Tổng các chữ số là:$$1+2+3+4+5+6+7+8+9+1 = 46$$Một sai lầm phổ biến là quên điều đó`digitSum(10) = 1`, không`10`. 

Coi như`n = 99999`Và`n = 100000`. 

Tổng các chữ số giảm mạnh khi vượt qua lũy thừa mười:$$digitSum(99999)=45,\qquad digitSum(100000)=1$$Bất kỳ nỗ lực nào để rút ra các giá trị bằng cách sử dụng các mẫu số học đơn giản mà không xử lý đúng cách sẽ thất bại xung quanh các ranh giới đó. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xử lý từng trường hợp thử nghiệm một cách độc lập. Đối với một nhất định`n`, lặp qua tất cả các số từ`1`ĐẾN`n`, tính tổng từng chữ số và tích lũy kết quả. 

Điều này có hiệu quả vì câu trả lời chính xác là tổng của tất cả các tổng chữ số trong phạm vi đó. Vấn đề là chi phí. Tính tổng các chữ số mất khoảng`O(log n)`hoạt động, và chúng tôi phải làm điều đó cho mọi số lên đến`n`. Đối với một trường hợp thử nghiệm, độ phức tạp là khoảng`O(n log n)`. Với`n = 200000`Và`10000`trường hợp thử nghiệm, khối lượng công việc trở nên rất lớn. 

Quan sát quan trọng là mọi trường hợp thử nghiệm đều yêu cầu cùng một loại truy vấn tiền tố:$$\text{answer}(n)=\sum_{i=1}^{n}\text{digitSum}(i)$$Thay vì tính toán lại các khoản tiền này nhiều lần, chúng ta có thể xây dựng chúng một lần. 

Cho phép$$dp[i] = \text{digitSum}(i)$$Và$$pref[i] = \sum_{j=1}^{i} dp[j]$$Sau đó, câu trả lời cho bất kỳ trường hợp thử nghiệm nào chỉ đơn giản là`pref[n]`. 

Vì lớn nhất có thể`n`chỉ là`200000`, chúng ta có thể tính toán trước tất cả các tổng chữ số và tất cả các tổng tiền tố đạt đến giới hạn đó trước khi đọc các truy vấn. 

Có một quan sát hữu ích khác. Tổng các chữ số của một số có thể được tính đệ quy:$$digitSum(i)=digitSum(i//10)+(i\bmod 10)$$Điều này cho phép tất cả các tổng chữ số từ`1`ĐẾN`200000`được tính toán hiệu quả trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tối ưu | Tiền xử lý O(MAXN) + O(1) cho mỗi truy vấn | O(MAXN) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các trường hợp thử nghiệm và lưu trữ giá trị của chúng. 
2. Tìm giá trị tối đa có thể cần cho quá trình tiền xử lý. Trong vấn đề này luôn luôn`200000`, nhưng việc sử dụng giá trị truy vấn lớn nhất cũng có tác dụng. 
3. Tạo một mảng`digit`Ở đâu`digit[i]`lưu trữ tổng các chữ số của`i`. 
4. Tính tổng các chữ số bằng phép truy hồi$$digit[i] = digit[i // 10] + (i \% 10)$$Chữ số cuối cùng góp phần`i % 10`, các chữ số còn lại chính xác`i // 10`. 
5. Tạo mảng tổng tiền tố`pref`. 
6. Đối với mỗi`i`, tính toán$$pref[i] = pref[i-1] + digit[i]$$Sau bước này,`pref[i]`tương đương với câu trả lời cần thiết cho`i`. 
7. Với mỗi test case, xuất ra`pref[n]`. 

### Tại sao nó hoạt động 

Sự tái phát cho`digit[i]`đúng vì mọi số thập phân đều bao gồm chữ số cuối cùng của nó cộng với tất cả các chữ số trước đó. Chia một số thành`i // 10`Và`i % 10`tách biệt hai phần đó một cách chính xác. 

Mảng tiền tố duy trì bất biến sau khi xử lý chỉ mục`i`,$$pref[i]=\sum_{k=1}^{i} digit[k]$$Điều này ban đầu đúng đối với`i=0`và mỗi bước cộng chính xác một tổng chữ số mới. Do đó`pref[n]`bằng tổng các chữ số từ`1`bởi vì`n`, đó chính xác là đại lượng mà bài toán yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    queries = [int(input()) for _ in range(t)]

    mx = max(queries)

    digit = [0] * (mx + 1)
    pref = [0] * (mx + 1)

    for i in range(1, mx + 1):
        digit[i] = digit[i // 10] + (i % 10)
        pref[i] = pref[i - 1] + digit[i]

    out = [str(pref[n]) for n in queries]
    sys.stdout.write("\n".join(out))

solve()
```Bước đầu tiên đọc mọi truy vấn để chúng tôi biết quá trình xử lý trước phải đi bao xa. Không có lý do gì để xây dựng mảng lên đến`200000`nếu tất cả các truy vấn nhỏ hơn. 

các`digit`mảng sử dụng sự lặp lại```
digit[i] = digit[i // 10] + (i % 10)
```Bởi vì`i // 10`luôn nhỏ hơn`i`, giá trị cần thiết đã được tính toán. 

Mảng tiền tố được xây dựng đồng thời. Sau khi tính tổng các chữ số của`i`, chúng tôi ngay lập tức mở rộng tổng số đang chạy. 

Số nguyên Python là đủ vì câu trả lời lớn nhất chỉ là vài triệu. Không cần xử lý đặc biệt khi tràn. 

Một lỗi triển khai phổ biến là bắt đầu vòng lặp tại`0`. Sự lặp lại cũng hợp lệ ở đó, nhưng định nghĩa tiền tố rõ ràng hơn khi`pref[0] = 0`và quá trình xử lý bắt đầu từ`1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
12
```| tôi | chữ số[i] | trước[i] | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 3 | 
| 3 | 3 | 6 | 
| 4 | 4 | 10 | 
| 5 | 5 | 15 | 
| 6 | 6 | 21 | 
| 7 | 7 | 28 | 
| 8 | 8 | 36 | 
| 9 | 9 | 45 | 
| 10 | 1 | 46 | 
| 11 | 2 | 48 | 
| 12 | 3 | 51 | 

Giá trị cuối cùng là`pref[12] = 51`, phù hợp với mẫu. Dấu vết này cho thấy tổng tiền tố tích lũy chính xác tổng các chữ số của tất cả các số lên đến`12`. 

### Ví dụ 2 

đầu vào:```
2024
```Gần cuối quá trình tiền xử lý: 

| tôi | chữ số[i] | 
| --- | --- | 
| 2020 | 4 | 
| 2021 | 5 | 
| 2022 | 6 | 
| 2023 | 7 | 
| 2024 | 8 | 

Câu trả lời có được trực tiếp như`pref[2024]`, bằng`28170`. 

Ví dụ này chứng tỏ rằng khi quá trình tiền xử lý hoàn tất, việc trả lời một truy vấn chỉ yêu cầu tra cứu một mảng duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M + t) |`M`là giá trị truy vấn lớn nhất, tiền xử lý là tuyến tính và mỗi truy vấn là O(1) | 
| Không gian | O(M) | Mảng tính tổng các chữ số và tổng tiền tố | 

Với`M ≤ 200000`, quá trình tiền xử lý chỉ thực hiện vài trăm nghìn thao tác. Sau đó, mỗi test case sẽ được trả lời ngay lập tức. Điều này dễ dàng phù hợp với cả giới hạn bộ nhớ và giới hạn thời gian nghiêm ngặt khác thường. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    queries = [int(input()) for _ in range(t)]

    mx = max(queries)

    digit = [0] * (mx + 1)
    pref = [0] * (mx + 1)

    for i in range(1, mx + 1):
        digit[i] = digit[i // 10] + (i % 10)
        pref[i] = pref[i - 1] + digit[i]

    return "\n".join(str(pref[n]) for n in queries)

# provided sample
assert run(
"""7
12
1
2
3
1434
2024
200000
"""
) == (
"""51
1
3
6
18465
28170
4600002"""
), "sample"

# minimum value
assert run(
"""1
1
"""
) == "1", "minimum n"

# crossing 10
assert run(
"""1
10
"""
) == "46", "digit carry boundary"

# small prefix
assert run(
"""1
9
"""
) == "45", "sum of first nine digits"

# power of ten boundary
assert run(
"""2
99999
100000
"""
) == "2250000\n2250001", "carry transition"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n = 1`|`1`| Đầu vào hợp lệ nhỏ nhất | 
|`n = 9`|`45`| Phạm vi một chữ số thuần túy | 
|`n = 10`|`46`| Chuyển từ một chữ số sang hai chữ số | 
|`99999, 100000`|`2250000, 2250001`| Xử lý đúng dây chuyền vận chuyển lớn | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
1
```Vòng tiền xử lý tính toán:```
digit[1] = digit[0] + 1 = 1
pref[1] = pref[0] + 1 = 1
```Đầu ra của thuật toán`pref[1] = 1`, điều đó đúng. 

Hãy xem xét ranh giới mang đầu tiên:```
1
10
```Các giá trị được tính toán là:```
digit[9] = 9
digit[10] = digit[1] + 0 = 1
```Tổng tiền tố trở thành:```
45 + 1 = 46
```Câu trả lời đúng vì tổng các chữ số của`10`là`1`, không`10`. 

Hãy xem xét một chuỗi mang dài:```
1
100000
```Sự tái phát mang lại:```
digit[100000]
= digit[10000] + 0
= digit[1000] + 0
= digit[100] + 0
= digit[10] + 0
= digit[1] + 0
= 1
```Thuật toán xử lý việc này một cách tự nhiên mà không có bất kỳ trường hợp đặc biệt nào. Tổng tiền tố đã chứa phần đóng góp của tất cả các số trước đó, vì vậy việc cộng giá trị cuối cùng này sẽ tạo ra kết quả chính xác.
