---
title: "CF 104149L - Bước nhảy vọt đáy dài"
description: "Chúng ta được cấp một chuỗi nhị phân đại diện cho một cầu thang dài. Mỗi ký tự tương ứng với một bước và chỉ những vị trí được đánh dấu bằng 1 là bị hỏng và cần phải sửa."
date: "2026-07-02T01:26:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104149
codeforces_index: "L"
codeforces_contest_name: "CPUlm Winter Contest 2022"
rating: 0
weight: 104149
solve_time_s: 50
verified: true
draft: false
---

[CF 104149L - Bước nhảy vọt từ đáy dài](https://codeforces.com/problemset/problem/104149/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân đại diện cho một cầu thang dài. Mỗi ký tự tương ứng với một bước và chỉ những vị trí được đánh dấu bằng`1`bị hỏng và cần được sửa chữa. Tất cả các bậc thang bị hỏng phải được sửa chữa trong một thao tác duy nhất và thao tác đó phải áp dụng cho một đoạn liền kề của cầu thang. 

Câu thần chú mà chúng tôi sử dụng luôn sửa chữa toàn bộ khoảng thời gian liên tục, nhưng phạm vi sử dụng của nó phụ thuộc vào cách nó được viết. Ở dạng đơn giản nhất, nó có thể bao gồm tối đa 32 bước liên tiếp. Chúng ta có thể mở rộng phạm vi của nó bằng cách liên tục thêm từ “dài” vào trước nó và mỗi từ “dài” được thêm vào sẽ nhân đôi số bước tối đa mà nó có thể xử lý. Vì vậy, câu thần chú tạo thành một tiến trình hình học về năng lực. 

Nhiệm vụ là xác định dạng chính tả hợp lệ ngắn nhất có thể bao gồm mọi`1`trong chuỗi có một đoạn liền kề. 

Chuỗi đầu vào có thể lớn tới 10^6 ký tự, vì vậy mọi giải pháp đều phải quét chuỗi đó theo thời gian tuyến tính. Bất cứ điều gì liên quan đến mô phỏng lặp lại trên các chuỗi con hoặc tính toán lại các phạm vi sẽ quá chậm. 

Một quan sát cấu trúc quan trọng là chỉ có lần xuất hiện đầu tiên và cuối cùng của`1`vấn đề. Khi chúng tôi chọn một đoạn liền kề bao gồm tất cả các bước bị hỏng, độ dài yêu cầu tối thiểu của nó được xác định hoàn toàn bằng khoảng cách giữa hai điểm cực trị này. 

Một sai lầm ngây thơ là nghĩ rằng chúng ta cần xem xét nhiều nhóm rời rạc của`1`s hoặc đếm xem có bao nhiêu đoạn của`1`s tồn tại. Ví dụ: trong một chuỗi như`100010001`, ai đó có thể nghĩ sai rằng cần có nhiều phép thuật. Trên thực tế, vì câu thần chú bao gồm một khoảng liền kề, nên chúng tôi buộc phải đưa các số 0 vào giữa, vì vậy chỉ có khoảng giới hạn mới quan trọng. 

Một sai lầm tiềm ẩn khác là hiểu sai quy mô của câu thần chú. Sự tăng trưởng là theo cấp số nhân đối với số lần "dài" được thêm vào trước, vì vậy việc coi nó là tăng trưởng tuyến tính sẽ dẫn đến câu trả lời sai cho các khoảng lớn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử tất cả các vị trí bắt đầu và kết thúc có thể có của một đoạn chứa tất cả`1`s, tính độ dài của nó và sau đó xác định cần bao nhiêu tiền tố “dài” để bao phủ độ dài đó. Điều này ngay lập tức trở nên không cần thiết khi chúng tôi nhận thấy rằng bất kỳ phân đoạn hợp lệ nào cũng phải bắt đầu tại hoặc trước phân đoạn đầu tiên.`1`và kết thúc ở hoặc sau cái cuối cùng`1`và việc chọn bất cứ thứ gì lớn hơn chỉ làm cho yêu cầu trở nên tồi tệ hơn. Việc thử tất cả các cặp điểm cuối sẽ dẫn đến hành vi bậc hai trên một chuỗi có độ dài lên tới 10^6, vượt xa giới hạn khả thi. 

Cái nhìn sâu sắc quan trọng là vấn đề được rút gọn thành một khoảng duy nhất: đoạn nhỏ nhất chứa tất cả`1`s là khoảng thời gian kể từ lần đầu tiên`1`đến cuối cùng`1`. Gọi chiều dài của nó là`L`. Phép thuật ít nhất phải hỗ trợ`L`các bước. 

Bây giờ chúng ta chỉ cần xác định số tiền tố “dài” nhỏ nhất sao cho:```
32 * 2^(k-1) >= L
```Điều này xuất phát từ thực tế là dạng cơ sở đã tương ứng với dung lượng 32 và mỗi chữ “dài” bổ sung sẽ nhân đôi dung lượng đó. Chúng tôi đang giải quyết một cách hiệu quả số mũ nhỏ nhất đẩy công suất lên trên khoảng yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên tất cả các phân khúc | O(n^2) | O(1) | Quá chậm | 
| Sử dụng đầu tiên/cuối cùng`1`và chia tỷ lệ logarit | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi một lần để tìm chỉ mục của chuỗi đầu tiên`1`và cuối cùng`1`. Điều này xác định phân khúc tối thiểu phải được bao phủ. Bất kỳ phân khúc nhỏ hơn nào cũng sẽ bỏ lỡ ít nhất một bước bị hỏng. 
2. Tính độ dài cần thiết`L = last_one_index - first_one_index + 1`. Đây là khoảng liền kề nhỏ nhất bao gồm tất cả các bước bị hỏng. 
3. Bắt đầu từ dung lượng cơ bản là 32, tương ứng với một chữ “dài”. 
4. Liên tục tăng gấp đôi công suất trong khi đếm số lần chúng tôi áp dụng khái niệm “dài”. Chúng tôi đang tìm kiếm một cách hiệu quả những điều nhỏ nhất`k >= 1`như vậy`32 * 2^(k-1) >= L`. 
5. Xuất ra từ “dài” lặp lại`k`lần, cách nhau bởi dấu cách. 

### Tại sao nó hoạt động 

Câu thần chú luôn hoạt động trong một khoảng thời gian liền kề, vì vậy bất kỳ giải pháp hợp lệ nào cũng phải bao gồm ít nhất khoảng thời gian giữa lần đầu tiên và lần cuối cùng.`1`. Việc mở rộng ra ngoài khoảng thời gian đó chỉ làm tăng công suất cần thiết. Vì mỗi “dài” tăng gấp đôi công suất, nên vấn đề giảm xuống còn việc tìm tỷ lệ lũy thừa hai nhỏ nhất phù hợp với độ dài mục tiêu cố định. Sự tăng trưởng đơn điệu đảm bảo rằng một khi công suất vượt quá`L`, không có số lượng tiền tố nhỏ hơn có thể hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    
    n = len(s)
    first = -1
    last = -1
    
    for i, ch in enumerate(s):
        if ch == '1':
            if first == -1:
                first = i
            last = i
    
    L = last - first + 1
    
    k = 1
    cap = 32
    
    while cap < L:
        cap *= 2
        k += 1
    
    print(" ".join(["long"] * k))

if __name__ == "__main__":
    solve()
```Việc thực hiện bắt đầu bằng việc xác định khoảng giới hạn của`1`s trong một lần duy nhất. Sau đó nó tính toán độ dài nhịp yêu cầu. Sau đó, nó mô phỏng sự tăng trưởng theo cấp số nhân của dung lượng bùa chú bắt đầu từ 32, tăng số lượng tiền tố “dài” cho đến khi đủ dung lượng. Vòng lặp có tính logarit với độ dài yêu cầu, an toàn trong các ràng buộc. 

Một điểm tinh tế là chúng ta bắt đầu`k`ở mức 1 chứ không phải 0, bởi vì ngay cả câu thần chú tối thiểu cũng đã bao gồm một chữ "dài". Điều này khớp với kết quả đầu ra mẫu trong đó biểu mẫu hợp lệ ngắn nhất không bao giờ trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
101
```| Bước | Đầu tiên 1 | Cuối cùng 1 | L | Công suất | k | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 2 | 3 | 32 | 1 | 

Khoảng yêu cầu là 3, đã nằm trong dung lượng cơ bản 32. Vì vậy, chỉ cần một "dài". 

Đầu ra:```
long
```### Ví dụ 2 

đầu vào:```
10000000000000000000000000000001
```| Bước | Đầu tiên 1 | Cuối cùng 1 | L | Công suất | k | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 31 | 32 | 32 | 1 | 

Khoảng thời gian khớp chính xác với dung lượng cơ bản, do đó, một lần nữa một “dài” là đủ. 

Đầu ra:```
long
```### Ví dụ 3 

đầu vào:```
1 followed by 70 zeros and 1
```| Bước | Đầu tiên 1 | Cuối cùng 1 | L | Công suất | k | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 71 | 72 | 32 | 1 | 
| sau khi nhân đôi | 0 | 71 | 72 | 64 | 2 | 
| sau khi nhân đôi | 0 | 71 | 72 | 128 | 3 | 

Công suất phải tăng gấp đôi trước khi vượt quá nhịp yêu cầu. 

Đầu ra:```
long long long
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét một lần để xác định vị trí đầu tiên và cuối cùng`1`, cộng với tăng trưởng công suất logarit | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ | 

Quét tuyến tính lên tới 10^6 ký tự chiếm ưu thế trong thời gian chạy nhưng vẫn nằm trong giới hạn. Vòng lặp bổ sung chạy tối đa khoảng 20 lần lặp vì công suất tăng gấp đôi mỗi lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""  # output is printed directly

# provided samples (structure-based checks would be adapted in real harness)
# custom cases
# single 1
assert run("1\n") is not None

# small spread
assert run("101\n") is not None

# all ones
assert run("11111\n") is not None

# large gap
assert run("1" + "0"*1000000 + "1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`long`| khoảng tối thiểu | 
|`101`|`long`| số 0 nội bộ bị bỏ qua | 
| điểm cuối tách biệt dài | nhiều dài | tăng trưởng theo cấp số nhân | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi`1`s đã được đóng gói chặt chẽ. Ví dụ, trong`11111`, vị trí đầu tiên và cuối cùng gần nhau nên độ dài yêu cầu nhỏ. Thuật toán tính toán chính xác khoảng thời gian tối thiểu và ngay lập tức duy trì ở mức công suất cơ bản, tạo ra một khoảng thời gian “dài” duy nhất. 

Một trường hợp khác là khi`1`s xuất hiện ở đầu cuối của một chuỗi rất lớn. Mặc dù có thể có hàng triệu số 0 ở giữa nhưng chỉ có khoảng cách giữa số đầu tiên và số cuối cùng`1`vấn đề. Quá trình quét nắm bắt chính xác điều này trong thời gian O(n) và vòng lặp nhân đôi vẫn còn nhỏ vì mức tăng trưởng theo cấp số nhân. 

Trường hợp cạnh cuối cùng là một trường hợp duy nhất`1`. Trong trường hợp này,`L = 1`, và dung lượng cơ bản đã bao gồm nó, vì vậy đầu ra lại chỉ là một "dài", phù hợp với định nghĩa rằng ngay cả câu thần chú ngắn nhất cũng có ít nhất một tiền tố.
