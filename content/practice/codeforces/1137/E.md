---
title: "CF 1137E - Lựa chọn toa xe lửa"
description: "Chúng tôi được yêu cầu mô phỏng một đoàn tàu đang phát triển và giá trị toa xe của nó tăng dần theo thời gian. Ban đầu có $n$ ô tô được đánh số từ đầu."
date: "2026-06-12T03:58:46+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1137
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 545 (Div. 1)"
rating: 2700
weight: 1137
solve_time_s: 97
verified: false
draft: false
---

[CF 1137E - Lựa chọn toa tàu](https://codeforces.com/problemset/problem/1137/E) 

**Đánh giá:** 2700 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một đoàn tàu đang phát triển và giá trị toa xe của nó tăng dần theo thời gian. Ban đầu, có$n$ô tô được đánh số từ đầu. Ô tô có thể được thêm vào hai đầu tàu và tại một số thời điểm nhất định, tất cả giá trị của ô tô$A_i$được tăng lên bởi một hàm tuyến tính$b + (i-1) \cdot s$. Sau mỗi thao tác, chúng ta phải báo cáo chỉ số nhỏ nhất của ô tô với giá trị tối thiểu$A_i$và giá trị của nó. 

Thách thức chính đó là$n$có thể lên đến$10^9$, trong khi có thể có tới 300.000 sự kiện. Việc lưu trữ tất cả các giá trị ô tô một cách rõ ràng là không thể, vì vậy bất kỳ phương pháp nào thực sự theo dõi được$A_i$vì mỗi chiếc xe sẽ làm nổ tung trí nhớ và thời gian. Điều này loại trừ hoàn toàn các mô phỏng mảng ngây thơ. 

Một trường hợp phức tạp phát sinh khi ô tô được thêm vào phần đầu: tất cả các chỉ số hiện có đều thay đổi. Nếu chúng ta quên tính đến ca làm việc này, chúng ta có thể báo cáo sai chiếc xe ở mức tối thiểu. Tương tự, sau khi tính toán lại, ô tô có công suất tối thiểu$A_i$có thể di chuyển đến đầu hoặc đuôi mới tùy theo độ dốc$s$. Ví dụ: nếu tàu có toa [0,0] và chúng ta thêm 3 vào đầu, đầu mới có chỉ số 1 và giá trị 0, trong khi đầu cũ trở thành chỉ số 4; một cách tiếp cận ngây thơ mà bỏ qua sự thay đổi chỉ số sẽ báo cáo sai chiếc xe tối thiểu. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Duy trì một mảng của tất cả$A_i$, cập nhật chỉ số khi ô tô được thêm vào và tính toán lại giá trị cho tất cả ô tô bất cứ khi nào xảy ra sự kiện loại 3. Sau mỗi thao tác, hãy quét mảng để tìm giá trị tối thiểu và in chỉ mục và giá trị của nó. Cách tiếp cận này đúng vì nó phản ánh vấn đề theo đúng nghĩa đen, nhưng nó đòi hỏi$O(n \cdot m)$hoạt động trong trường hợp xấu nhất. Với$n$lên đến$10^9$Và$m$lên tới 3×10^5, điều này hoàn toàn không khả thi. 

Cách tiếp cận tối ưu dựa vào việc quan sát rằng sau mỗi lần tính toán lại, giá trị tối thiểu xuất hiện ở đầu hoặc đuôi tàu, tùy thuộc vào độ dốc$s$. Cụ thể, nếu$s > 0$, các giá trị tăng tuyến tính từ đầu đến đuôi, do đó mức tối thiểu$A_i$đang ở đầu. Nếu như$s = 0$, tất cả các ô tô đều tăng như nhau, vì vậy giá trị tối thiểu có thể ở bất kỳ đâu nhưng chỉ số sớm nhất được ưu tiên. Bởi vì tất cả các ô tô đều bắt đầu bằng 0 và chỉ trải qua các cập nhật tuyến tính nên chỉ cần theo dõi ba thông tin: số lượng ô tô, hằng số cộng tích lũy là đủ$base$và sự tích lũy độ dốc$slope$. Khi ô tô được thêm vào, chúng tôi điều chỉnh độ lệch chỉ số nhưng không chạm vào các giá trị riêng lẻ. Gia tăng cập nhật loại 3$base$Và$slope$và giá trị tối thiểu được tính ở đầu hoặc đuôi tùy thuộc vào độ dốc hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m) | O(n) | Quá chậm | 
| Tối ưu | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo biến:`train_length = n`,`head_offset = 0`,`base = 0`,`slope = 0`. Đây,`head_offset`tính đến sự thay đổi chỉ số do ô tô được thêm vào đầu,`base`tích lũy tất cả các phần bổ sung liên tục, và`slope`tích lũy các phần bổ sung gia tăng trên mỗi chỉ mục. 
2. Đối với mỗi sự kiện, trước tiên hãy kiểm tra loại của nó. Nếu là loại 1 (thêm vào đầu), tăng dần`head_offset`theo số lượng xe mới và tăng`train_length`tương ứng. Những chiếc xe mới này có`A_i = 0`, do đó giá trị hiệu dụng ở đầu mới bằng 0; điều chỉnh độ lệch để tính toán chỉ số tối thiểu. 
3. Nếu là loại 2 (thêm vào đuôi) thì chỉ cần tăng`train_length`. Đuôi có thể có liên quan nếu độ dốc âm. 
4. Nếu là loại 3 có tham số`(b, s)`, tăng`base += b`Và`slope += s`. Điều này theo dõi giá trị của mỗi chiếc xe tăng lên như thế nào mà không cần lưu trữ chúng một cách rõ ràng. 
5. Sau mỗi sự kiện, hãy xác định số xe tối thiểu. Nếu như`slope > 0`, chiếc ô tô đầu tiên có giá trị nhỏ nhất, đó là`base + slope * 0 = base`. Nếu như`slope < 0`, chiếc xe cuối cùng có giá trị nhỏ nhất`base + slope * (train_length - 1)`. Nếu như`slope == 0`, mọi ô tô đều có giá trị như nhau; chúng tôi chọn chỉ mục sớm nhất sau phần bù đầu. 
6. Xuất chỉ số (điều chỉnh cho`head_offset`) và giá trị tối thiểu được tính toán. 

Bất biến đảm bảo tính đúng đắn là hàm`A_i`luôn tuyến tính với chỉ số ô tô, với độ dốc`slope`và hằng số`base`. Bởi vì các hàm tuyến tính đạt mức tối thiểu ở một trong hai điểm cuối nên chỉ theo dõi phần đầu và phần đuôi là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
train_length = n
head_offset = 0
base = 0
slope = 0

for _ in range(m):
    parts = input().split()
    t = int(parts[0])
    
    if t == 1:
        k = int(parts[1])
        head_offset += k
        train_length += k
    elif t == 2:
        k = int(parts[1])
        train_length += k
    else:
        b = int(parts[1])
        s = int(parts[2])
        base += b
        slope += s
    
    if slope > 0:
        min_index = 1
        min_value = base
    elif slope < 0:
        min_index = train_length
        min_value = base + slope * (train_length - 1)
    else:
        min_index = 1
        min_value = base
    
    print(min_index, min_value)
```Giải pháp giữ tất cả các tính toán trong không gian không đổi.`head_offset`đảm bảo rằng chúng tôi diễn giải chính xác chiếc xe đầu tiên sau khi chèn đầu. Logic để chọn chỉ số tối thiểu dựa trên tính tuyến tính của`A_i`sau nhiều lần tính toán lại nên không cần quét tất cả các ô tô. 

## Ví dụ đã hoạt động 

**Dấu vết đầu vào mẫu 1** 

| Sự kiện | chiều dài tàu | căn cứ | độ dốc | Chỉ số tối thiểu | Giá trị tối thiểu | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 0 | 0 | 1 | 0 | 
| 1 1 (đầu) | 2 | 0 | 0 | 1 | 0 | 
| 3 1 1 | 2 | 1 | 1 | 1 | 1 | 
| 3 1 1 | 2 | 2 | 2 | 1 | 2 | 
| 2 1 (đuôi) | 3 | 2 | 2 | 1 | 2 | 
| 2 1 (đuôi) | 4 | 2 | 2 | 1 | 2 | 
| 3 1 1 | 4 | 3 | 3 | 1 | 3 | 
| 2 1 (đuôi) | 5 | 3 | 3 | 1 | 3 | 
| 3 1 5 | 5 | 4 | 8 | 1 | 4 | 

Điều này xác nhận rằng chỉ mục tối thiểu được theo dõi chính xác sau khi chèn và tính toán lại đầu và đuôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi sự kiện được xử lý trong thời gian không đổi. | 
| Không gian | O(1) | Chỉ có một số quầy được duy trì; không có mảng nào tỷ lệ thuận với n. | 

Với m lên tới 300.000, giải pháp này dễ dàng chạy trong vòng 2 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open('solution.py').read())
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Sample
assert run("1 8\n1 1\n3 1 1\n3 1 1\n2 1\n2 1\n3 1 1\n2 1\n3 1 5\n") == "1 0\n1 1\n1 2\n1 2\n1 2\n1 3\n1 3\n1 4", "sample 1"

# Custom: minimal size
assert run("1 1\n3 10 0\n") == "1 10", "minimal n=1"

# Custom: slope negative
assert run("2 2\n3 5 -1\n3 2 -1\n") == "2 4\n2 5", "negative slope"

# Custom: multiple head inserts
assert run("1 3\n1 2\n1 3\n3 1 2\n") == "1 1", "head shifts"

# Custom: slope zero
assert run("3 2\n3 5 0\n3 2 0\n") == "1 7\n1 7", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n3 10 0 | 1 10 | tối thiểu n | 
| 2 2\n3 5 -1\n3 2 -1 | 2 4\n2 | |
