---
title: "CF 1971E - Tìm Xe"
description: "Cho một ô tô chuyển động thẳng từ vị trí 0 đến vị trí n. Chúng tôi biết chính xác thời điểm chiếc xe đi qua một số trạm kiểm soát. Các checkpoint này được sắp xếp theo vị trí: 0, a1, a2, ..., ak và thời gian tương ứng cũng tăng dần theo đúng: 0, b1, b2, ."
date: "2026-06-08T17:21:49+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1971
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 944 (Div. 4)"
rating: 1500
weight: 1971
solve_time_s: 141
verified: true
draft: false
---

[CF 1971E - Tìm xe](https://codeforces.com/problemset/problem/1971/E) 

**Đánh giá:** 1500 
**Tags:** binary search, math, sortings
 **Thời gian giải:** 2m 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta được cho một ô tô chuyển động thẳng từ vị trí`0`để định vị`n`. Chúng tôi biết chính xác thời điểm chiếc xe đi qua một số trạm kiểm soát. Các điểm kiểm tra này được sắp xếp theo vị trí:`0, a1, a2, ..., ak`và số lần tương ứng cũng tăng dần:`0, b1, b2, ..., bk`, với điểm kiểm tra cuối cùng ở vị trí`n`. 

Giữa hai điểm kiểm tra liên tiếp bất kỳ đã biết, ô tô chuyển động với tốc độ không đổi. Điều đó có nghĩa là chuyển động tuyến tính từng đoạn theo thời gian: vị trí tăng tuyến tính theo thời gian bên trong mỗi đoạn, nhưng tốc độ có thể thay đổi tại ranh giới của các điểm đã cho. 

Đối với mỗi vị trí truy vấn`d`, ta phải tính thời gian ô tô đến`d`và xuất ra phần nguyên của thời gian đó. 

Khó khăn chính đó là`n`, tọa độ có thể lớn tới 1e9 nên chúng ta không thể mô phỏng chuyển động từng bước. Tuy nhiên, số lượng phân đoạn đã biết`k`tối đa là 1e5 cho mỗi thử nghiệm và tổng số qua các thử nghiệm cũng là 1e5, vì vậy chúng tôi có thể đủ khả năng xử lý trước logarit hoặc tuyến tính cho mỗi trường hợp thử nghiệm. 

Một sai lầm ngây thơ xuất phát từ sự hiểu lầm về cách hoạt động của phép nội suy giữa các điểm đã biết. Thời gian không tỷ lệ thuận với chỉ số mà tỷ lệ thuận với khoảng cách bên trong một đoạn đường. 

Ví dụ: nếu chúng ta có điểm`(0, 0)`Và`(10, 10)`và một truy vấn`d = 5`, câu trả lời là`5`. Nhưng nếu chúng ta thay đổi điểm thứ hai thành`(10, 20)`, tốc độ tăng gấp đôi, vì vậy`d = 5`tương ứng với thời gian`10`. Bất kỳ giải pháp nào giả định tốc độ toàn cầu không đổi sẽ thất bại. 

Một trường hợp phức tạp khác là truy vấn chính xác tại một điểm kiểm tra đã biết. Nếu như`d = a[i]`, đáp án phải chính xác`b[i]`. Bất kỳ công thức nội suy nào cũng phải bảo toàn tính nhất quán biên. 

Cuối cùng, các truy vấn có thể đến trước điểm kiểm tra đầu tiên sau 0 hoặc bên trong bất kỳ phân đoạn nào, vì vậy chúng ta phải xác định chính xác phân đoạn đó cho mỗi truy vấn. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xử lý từng truy vấn một cách độc lập. Đối với một vị trí truy vấn`d`, chúng tôi quét qua các phân đoạn`[a[i], a[i+1]]`, tìm đâu`d`nằm và tính thời gian bằng cách sử dụng phép nội suy tuyến tính. 

Trong một đoạn đường, nếu ô tô chuyển động từ`(a[i], b[i])`ĐẾN`(a[i+1], b[i+1])`, thì tốc độ là`(a[i+1] - a[i]) / (b[i+1] - b[i])`. Chúng tôi đảo ngược điều này để tính toán thời gian tại`d`:```
t = b[i] + (d - a[i]) * (b[i+1] - b[i]) / (a[i+1] - a[i])
```Điều này đúng, nhưng với mỗi truy vấn, chúng tôi có thể quét tới`k`phân đoạn, làm cho sự phức tạp`O(kq)`mỗi trường hợp thử nghiệm. Với`k, q`lên đến 1e5, điều này trở thành hoạt động 1e10 trong trường hợp xấu nhất, quá chậm. 

Quan sát quan trọng là cả hai`a`Và`b`được sắp xếp và ánh xạ từ vị trí này sang thời gian khác là đơn điệu và tuyến tính từng phần. Khi chúng tôi xác định được phân đoạn chính xác cho vị trí truy vấn`d`, câu trả lời là xác định. Điều này làm giảm vấn đề về vị trí phân khúc hiệu quả. 

Chúng ta có thể tìm kiếm nhị phân trên`a`mảng để tìm điểm kiểm tra cuối cùng`a[i] ≤ d`. Sau đó, chúng tôi tính toán câu trả lời bằng công thức nội suy trong thời gian không đổi. 

Vì vậy, mỗi truy vấn sẽ trở thành`O(log k)`thay vì`O(k)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(kq) | O(k) | Quá chậm | 
| Tìm kiếm nhị phân + Nội suy | O(q log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trước từng trường hợp thử nghiệm bằng cách lưu trữ mảng`a`Và`b`. 

1. Đọc mảng`a`Và`b`đại diện cho các điểm kiểm tra vị trí-thời gian. Chúng xác định một hàm tuyến tính từng phần theo từng vị trí. 
2. Đối với mỗi truy vấn`d`, chúng tôi xác định chỉ số lớn nhất`i`như vậy`a[i] ≤ d`. Điều này xác định phân đoạn có chứa`d`vì hàm này đơn điệu ở vị trí. 
3. Nếu`d`tương đương với một điểm kiểm tra`a[i]`, chúng tôi trực tiếp trở lại`b[i]`. Điều này tránh được lỗi dấu phẩy động và bảo toàn các giá trị chính xác tại các điểm đã biết. 
4. Mặt khác, chúng ta tính phần quãng đường đã đi trong đoạn đường đó: 

vị trí tương đối bên trong đoạn thẳng là`(d - a[i]) / (a[i+1] - a[i])`. 
5. Chúng ta ánh xạ phân số đó vào không gian thời gian bằng phép nội suy tuyến tính:`t = b[i] + (d - a[i]) * (b[i+1] - b[i]) // (a[i+1] - a[i])`. 

Phép chia số nguyên được sử dụng vì đầu ra yêu cầu giá trị sàn của thời gian. 
6. In thời gian tính toán. 

### Tại sao nó hoạt động 

Trong mỗi đoạn, vị trí là một hàm tuyến tính của thời gian vì tốc độ không đổi. Điều đó ngụ ý ánh xạ nghịch đảo từ vị trí này sang thời gian khác cũng tuyến tính trên cùng một khoảng. Từ`a`đang tăng lên một cách nghiêm ngặt, mọi vị trí`d`nằm trong chính xác một phân đoạn và tìm kiếm nhị phân xác định chính xác phân đoạn đó. Công thức nội suy tái tạo chính xác hàm tuyến tính nghịch đảo được giới hạn trong phân đoạn đó, do đó thời gian tính toán khớp với thời gian liên tục thực sự và làm sàn nó mang lại câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k, q = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        for _ in range(q):
            d = int(input())

            if d <= a[0]:
                if a[0] == 0:
                    print(0)
                else:
                    # segment from 0 to a[0]
                    dt = b[0] * d // a[0]
                    print(dt)
                continue

            if d >= a[-1]:
                print(b[-1])
                continue

            lo, hi = 0, k - 1
            while lo < hi:
                mid = (lo + hi) // 2
                if a[mid] <= d:
                    lo = mid + 1
                else:
                    hi = mid
            i = lo - 1

            if a[i] == d:
                print(b[i])
            else:
                dt = (b[i+1] - b[i]) * (d - a[i]) // (a[i+1] - a[i])
                print(b[i] + dt)

solve()
```Việc triển khai dựa vào tìm kiếm nhị phân để tìm phân đoạn chính xác một cách hiệu quả. Trường hợp cạnh`d <= a[0]`được xử lý riêng vì phân đoạn đầu tiên bắt đầu từ số 0. Đối với các truy vấn ngoài điểm kiểm tra cuối cùng, câu trả lời là trực tiếp`b[k-1]`vì xe đã tới đích rồi. 

Phép nội suy được thực hiện hoàn toàn bằng số nguyên và phép nhân được thực hiện trước khi chia để duy trì độ chính xác. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một trường hợp được xây dựng nhỏ: 

đầu vào:```
n=10, k=2
a = [4, 10]
b = [4, 7]
queries: d = 6, 4, 2, 7
```### Dấu vết truy vấn 

| d | chỉ số phân khúc i | tính toán | kết quả | 
| --- | --- | --- | --- | 
| 6 | 0 | 4 + (2 * 3 // 6) | 5 | 
| 4 | 0 | khớp chính xác | 4 | 
| 2 | trước đầu tiên | 2*4 // 4 | 2 | 
| 7 | 0 | 4 + (3 * 3 // 6) | 5 | 

Dấu vết cho thấy các vùng khác nhau hoạt động như thế nào: nội suy phân đoạn bên trong, các lần truy cập điểm kiểm tra chính xác và ngoại suy trước phân đoạn đã biết đầu tiên. 

Điều này xác nhận rằng tìm kiếm nhị phân chọn đúng phân đoạn một cách nhất quán và phép nội suy tuyến tính phù hợp với mô hình chuyển động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log k) trên mỗi trường hợp thử nghiệm | mỗi truy vấn sử dụng tìm kiếm nhị phân trên các điểm kiểm tra | 
| Không gian | O(k) | lưu trữ cho mảng điểm kiểm tra | 

Tổng các ràng buộc tổng cộng là 1e5 trong tất cả các trường hợp thử nghiệm, do đó độ phức tạp này dễ dàng đủ nhanh. Mỗi phép toán được tính logarit trên tối đa 1e5 phần tử, giữ tổng số phép toán trong khoảng vài triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from typing import List

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, k, q = map(int, input().split())
            a = list(map(int, input().split()))
            b = list(map(int, input().split()))

            for _ in range(q):
                d = int(input())

                if d <= a[0]:
                    if a[0] == 0:
                        out.append("0")
                    else:
                        out.append(str(b[0] * d // a[0]))
                    continue

                if d >= a[-1]:
                    out.append(str(b[-1]))
                    continue

                lo, hi = 0, k - 1
                while lo < hi:
                    mid = (lo + hi) // 2
                    if a[mid] <= d:
                        lo = mid + 1
                    else:
                        hi = mid
                i = lo - 1

                if a[i] == d:
                    out.append(str(b[i]))
                else:
                    dt = (b[i+1] - b[i]) * (d - a[i]) // (a[i+1] - a[i])
                    out.append(str(b[i] + dt))

        return "\n".join(out)

    return solve()

# provided sample (abbreviated format check only)
assert run("1\n10 1 3\n10\n10\n0\n6\n7\n") == "0\n6\n7"

# custom: single segment linear
assert run("1\n10 1 3\n10\n10\n2\n5\n9\n") == "2\n5\n9"

# custom: two segments
assert run("1\n10 2 4\n4 10\n4 7\n6\n4\n2\n7\n") == "5\n4\n2\n5"

# custom: boundary
assert run("1\n100 1 2\n100\n100\n0\n100\n") == "0\n100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phân đoạn đơn | ánh xạ tuyến tính | độ chính xác nội suy cơ bản | 
| hai phân đoạn | truy vấn hỗn hợp | chuyển mạch phân đoạn | 
| ranh giới | điểm cuối | xử lý các cạnh đúng cách | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi truy vấn nằm trước điểm kiểm tra được biết đầu tiên. Trong khu vực đó, chúng tôi nội suy một cách hiệu quả từ`(0, 0)`ĐẾN`(a[0], b[0])`. Ví dụ, nếu`a[0] = 10`Và`b[0] = 5`, sau đó`d = 2`cho`t = 5 * 2 // 10 = 1`. Việc triển khai xử lý việc này một cách rõ ràng mà không cần nhập tìm kiếm nhị phân vì không có phân đoạn trước đó. 

Một trường hợp cạnh khác là khớp chính xác tại một điểm kiểm tra. Nếu như`d = a[i]`, phép nội suy vẫn hoạt động ở dạng số, nhưng nó có nguy cơ xảy ra lỗi làm tròn và tính toán không cần thiết. Trực tiếp trở về`b[i]`đảm bảo tính chính xác và tránh hiện tượng phân chia. 

Trường hợp cạnh cuối cùng đang truy vấn ngoài điểm kiểm tra cuối cùng. Từ`a[k-1] = n`, bất kì`d ≥ n`phải quay lại`b[k-1]`. Giải pháp xử lý vấn đề này một cách trực tiếp, phản ánh rằng chuyển động không tiếp tục vượt quá thời điểm đã biết cuối cùng.
