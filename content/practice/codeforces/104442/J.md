---
title: "CF 104442J - Aviones"
description: "Lưới mô tả một hình ảnh đen trắng trong đó hầu hết các ô là nền và các ô còn lại thuộc về một chiếc máy bay. Mỗi ô máy bay được gắn nhãn bằng một chữ số từ 0 đến 9 và tất cả các ô có cùng chữ số sẽ tạo thành một “loại vùng” duy nhất."
date: "2026-06-30T18:08:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104442
codeforces_index: "J"
codeforces_contest_name: "AdaByron Regional Madrid 2023"
rating: 0
weight: 104442
solve_time_s: 52
verified: true
draft: false
---

[CF 104442J - Aviones](https://codeforces.com/problemset/problem/104442/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lưới mô tả một hình ảnh đen trắng trong đó hầu hết các ô là nền và các ô còn lại thuộc về một chiếc máy bay. Mỗi ô máy bay được gắn nhãn bằng một chữ số từ 0 đến 9 và tất cả các ô có cùng chữ số sẽ tạo thành một “loại vùng” duy nhất. Nhiệm vụ là quyết định loại vùng nào cần được gia cố. 

Ngoài hình ảnh, chúng ta được cung cấp danh sách các điểm tác động. Mỗi lần va chạm đều chạm vào một ô máy bay hợp lệ, nghĩa là nó luôn chạm vào một trong các pixel được gắn nhãn chữ số. Đối với mỗi chữ số, chúng tôi đếm xem có bao nhiêu tác động chạm vào pixel của chữ số đó. Chúng ta cũng biết vùng của mỗi chữ số lớn đến mức nào, được đo bằng số lượng ô lưới mang chữ số đó. 

Quy tắc quyết định có tính phân cấp. Đầu tiên ta chọn chữ số có số lần chạm nhỏ nhất. Nếu nhiều chữ số bằng nhau, chúng tôi ưu tiên chữ số có vùng chiếm ít pixel hơn trong ảnh. Nếu vẫn hòa thì ta chọn chữ số có số nhận dạng nhỏ nhất. 

Các ràng buộc rất nhỏ: lưới tối đa là 100 x 100 và có nhiều nhất là 2000 tác động. Điều này có nghĩa là việc quét trực tiếp toàn bộ hình ảnh và việc kiểm đếm đơn giản cho mỗi lần tác động có thể dễ dàng đủ nhanh vì tất cả các hoạt động đều tuyến tính trong kích thước lưới cộng với số lần tác động. 

Một lỗi phổ biến là cho rằng các chữ số luôn xuất hiện trong một thành phần liền kề hoặc được kết nối đơn lẻ, nhưng điều đó không liên quan ở đây. Một cái bẫy tinh vi khác là quên rằng một số chữ số có thể không xuất hiện trong lưới; những thứ đó phải được loại trừ khỏi việc xem xét vì chúng không đại diện cho bất kỳ khu vực nào. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp tính toán hai phần thông tin cho mỗi chữ số từ 0 đến 9: có bao nhiêu ô trong lưới chứa chữ số đó và có bao nhiêu tác động lên nó. Cả hai có thể được tích lũy trong một lần chuyển qua lưới và một lần chuyển qua danh sách tác động. 

Cách mạnh mẽ nhất để trả lời từng tác động là quét toàn bộ lưới và kiểm tra xem nó dừng ở đâu, nhưng điều đó sẽ khiến mỗi truy vấn O(MN), dẫn đến tổng số O(DMN) hoạt động, điều này không cần thiết vì chúng ta có thể lập chỉ mục trực tiếp vào lưới để tìm chữ số tại vị trí tác động. 

Quan sát quan trọng là lưới đã mã hóa mọi thứ chúng ta cần để phân loại. Mỗi tác động chỉ là một bản tra cứu trong nhãn được tính toán trước, vì vậy chúng tôi có thể tích lũy số lượng trong thời gian không đổi cho mỗi sự kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét lưới mỗi lần va chạm) | O(D · M · N) | O(M · N) | Quá chậm | 
| Tối ưu (tính toán trước + đếm) | O(M · N + D) | O(M · N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước lưới và lưu trữ lưới dưới dạng danh sách các chuỗi. Mỗi ô đều chứa '.' hoặc một ký tự chữ số. Chúng tôi chỉ quan tâm đến các ô chữ số vì chúng xác định vùng hợp lệ. 
2. Khởi tạo một mảng có kích thước 10 để đếm xem mỗi chữ số có bao nhiêu ô. Chúng tôi cũng khởi tạo một mảng khác có kích thước 10 để đếm xem có bao nhiêu tác động xảy ra với mỗi chữ số. 
3. Quét từng ô trong lưới. Bất cứ khi nào chúng ta nhìn thấy một ký tự chữ số, hãy chuyển đổi nó thành một số nguyên và tăng bộ đếm kích thước của nó. Điều này xây dựng tổng diện tích của từng loại vùng theo thời gian tuyến tính. 
4. Đọc số lượng tác động, sau đó xử lý từng tọa độ tác động. Đối với mỗi tọa độ, truy cập trực tiếp vào ô lưới và chuyển đổi nó thành một chữ số. Tăng bộ đếm lượt truy cập tương ứng. Điều này hoạt động vì mọi tác động đều được đảm bảo tác động lên một ô chữ số. 
5. Sau khi xử lý tất cả dữ liệu đầu vào, lặp lại các chữ số từ 0 đến 9 và chọn ứng cử viên tốt nhất bằng cách sử dụng quy tắc so sánh: số lần truy cập nhỏ nhất trước tiên, sau đó là kích thước vùng nhỏ nhất, sau đó là giá trị chữ số nhỏ nhất. 
6. Xuất ra chữ số đã chọn. 

### Tại sao nó hoạt động

Mỗi tác động đóng góp độc lập vào chính xác một danh mục chữ số, do đó việc đếm chỉ đơn giản là phân chia các sự kiện trên các nhãn cố định. Quét lưới cung cấp trọng số chính xác cho từng nhãn và quét tác động cung cấp tần số chính xác. Vì cả hai số lượng đều được tổng hợp đầy đủ trên mỗi chữ số, nên việc so sánh các chữ số sẽ giảm xuống mức tối thiểu từ điển đơn giản so với số liệu thống kê được tính toán trước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    M, N = map(int, input().split())
    grid = [input().strip() for _ in range(N)]

    size = [0] * 10
    hits = [0] * 10

    for y in range(N):
        row = grid[y]
        for x in range(M):
            c = row[x]
            if c != '.':
                size[int(c)] += 1

    D = int(input())
    for _ in range(D):
        y, x = map(int, input().split())
        d = grid[y][x]
        hits[int(d)] += 1

    best = None
    for d in range(10):
        if size[d] == 0:
            continue
        cand = (hits[d], size[d], d)
        if best is None or cand < best:
            best = cand

    print(best[2])

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng bảng tần suất cho cả quy mô khu vực và số lượng tác động. Truyền tải lưới là một vòng lặp lồng nhau trực tiếp trên tất cả các ô, đảm bảo tổng diện tích của mỗi chữ số được biết chính xác một lần. Giai đoạn thứ hai xử lý từng tác động trong thời gian không đổi bằng cách lập chỉ mục trực tiếp vào lưới. 

Bước lựa chọn cuối cùng dựa vào so sánh bộ dữ liệu của Python, bước này thực hiện một cách tự nhiên thứ tự ưu tiên cần thiết: giảm thiểu số lần truy cập, sau đó giảm thiểu kích thước, sau đó thu nhỏ chữ số. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới đơn giản hóa: 

đầu vào:```
3 3
0.1
0.2.
111
3
0 0
2 0
2 2
```Ở đây lưới chứa các chữ số 0, 1 và 2. 

Đầu tiên chúng tôi tính toán kích thước: 

| Chữ số | Kích thước | 
| --- | --- | 
| 0 | 2 | 
| 1 | 4 | 
| 2 | 1 | 

Bây giờ xử lý các tác động: 

| Tác động | Tế bào | Chữ số | Lượt truy cập | 
| --- | --- | --- | --- | 
| (0,0) | 0 | 0 | 1 | 
| (2,0) | 1 | 1 | 1 | 
| (2,2) | 1 | 1 | 2 | 

Vì vậy, lần truy cập cuối cùng được tính: 

| Chữ số | Lượt truy cập | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 
| 2 | 0 | 

Chữ số 2 có ít lượt truy cập nhất nên được chọn dù nhỏ. Điều này chứng tỏ rằng số lần truy cập chiếm ưu thế trong kích thước trong hệ thống phân cấp quyết định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MN + D) | Một lần quét toàn bộ lưới cộng với một lần quét qua tất cả các tác động | 
| Không gian | O(MN) | Lưu trữ lưới cộng với các bộ đếm có kích thước không đổi | 

Các ràng buộc đảm bảo rằng tối đa 10 triệu ô lưới và 2000 tác động được xử lý, dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from types import ModuleType

    # assumes solution is defined in same file
    # we re-run solve directly if available
    return _sys.stdout.getvalue().strip()

# custom cases only

def solve_test(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# single digit only
assert solve_test("""5 1
11111
3
0 0
0 1
0 2
""") == "1"

# tie broken by size
assert solve_test("""3 2
0.0
111
2
1 0
1 1
""") == "0"

# no ties, clear minimum hits
assert solve_test("""4 2
00..
1111
1
0 0
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới một chữ số | 1 | tính đúng cơ bản | 
| hộp đựng cà vạt | 0 | tie-break theo kích thước và id | 
| lượt truy cập sai lệch | 1 | ưu tiên chính xác các lượt truy cập | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một chữ số xuất hiện trong lưới nhưng không nhận được kết quả nào. Trong tình huống đó, nó trở thành một ứng cử viên nặng ký vì 0 là số lần truy cập tối thiểu có thể xảy ra. Thuật toán xử lý điều này một cách tự nhiên vì tất cả các chữ số đều được khởi tạo với số lần truy cập bằng 0 và chỉ những số xuất hiện trong lưới mới được xem xét. 

Một trường hợp khác là khi nhiều chữ số không có lần truy cập nào. Ví dụ: nếu cả hai chữ số 2 và 8 đều tồn tại nhưng không nhận được tác động nào thì quyết định sẽ rơi vào kích thước vùng nhỏ nhất. Vòng lựa chọn so sánh rõ ràng kích thước sau số lần truy cập, do đó vùng nhỏ hơn chính xác sẽ được chọn. 

Trường hợp khó phát hiện cuối cùng là khi một số chữ số từ 0 đến 9 không bao giờ xuất hiện trong lưới. Những điều này phải được loại trừ; nếu không chúng sẽ xuất hiện không chính xác dưới dạng vùng có kích thước bằng 0. Séc`if size[d] == 0: continue`đảm bảo rằng chỉ các vùng hợp lệ mới được xem xét.
