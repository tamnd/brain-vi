---
title: "CF 1453A - Hủy chuyến tàu"
description: "Chúng tôi được yêu cầu quản lý hệ thống tàu trên lưới 100 × 100. Xe lửa có thể di chuyển theo chiều dọc từ dưới lên trên hoặc theo chiều ngang từ trái sang phải."
date: "2026-06-11T03:02:16+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1453
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 688 (Div. 2)"
rating: 800
weight: 1453
solve_time_s: 85
verified: true
draft: false
---

[CF 1453A - Hủy chuyến tàu](https://codeforces.com/problemset/problem/1453/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu quản lý hệ thống tàu trên lưới 100 × 100. Xe lửa có thể di chuyển theo chiều dọc từ dưới lên trên hoặc theo chiều ngang từ trái sang phải. Mỗi đoàn tàu có một số duy nhất từ ​​1 đến 100 tùy thuộc vào điểm xuất phát và tất cả các đoàn tàu đều di chuyển với cùng một tốc độ không đổi. Tại một thời điểm nhất định, một số đoàn tàu dự kiến ​​khởi hành từ phía dưới và một số từ bên trái. Nếu đoàn tàu thẳng đứng và đoàn tàu ngang chiếm cùng một tọa độ trong cùng một thời điểm thì xảy ra va chạm. 

Mục đích là xác định số lượng tàu tối thiểu cần hủy để tránh mọi va chạm. Mỗi trường hợp thử nghiệm chỉ định có bao nhiêu chuyến tàu dọc và ngang được lên lịch, cùng với số lượng tàu của chúng. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: số lần hủy nhỏ nhất cần thiết. 

Các ràng buộc rất nhỏ: tối đa 100 chuyến tàu ở mỗi bên và tối đa 100 trường hợp thử nghiệm. Điều này ngay lập tức ngụ ý rằng bất kỳ thuật toán nào có khoảng O(n·m) hoạt động cho mỗi trường hợp thử nghiệm, trong đó n và m là số lượng đoàn tàu, sẽ đủ nhanh. Mỗi số tàu nằm trong khoảng từ 1 đến 100, vì vậy chúng ta có thể biểu diễn lịch trình tàu một cách hiệu quả bằng các mảng hoặc bộ. 

Một trường hợp cần thận trọng là khi số tàu nhỏ nhất ở phía dưới khớp với số lớn nhất ở bên trái. Ví dụ: nếu các đoàn tàu phía dưới = [1] và các đoàn tàu bên trái = [1], một cách tiếp cận đơn giản có thể cho rằng không có sự cố nào xảy ra vì các con số là các danh sách khác nhau, nhưng trên thực tế, vị trí lưới trùng nhau ở (1,1). Một trường hợp khác là khi n=1 hoặc m=1; chúng tôi phải đảm bảo rằng chúng tôi không cố gắng so sánh các yếu tố không tồn tại. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực sẽ mô phỏng quỹ đạo của mọi đoàn tàu và kiểm tra va chạm ở mọi tọa độ có thể. Đối với mỗi đoàn tàu phía dưới, bạn sẽ so sánh đường đi của nó với mọi đoàn tàu bên trái. Vì một đoàn tàu di chuyển từ (i,0) đến (i,101) và đoàn tàu kia đi từ (0,j) đến (101,j) nên đường đi của chúng chỉ cắt nhau tại điểm (i,j). Điều này làm giảm vấn đề: va chạm xảy ra nếu đoàn tàu ở phía dưới có cùng số hiệu với đoàn tàu ở bên trái. Sử dụng thông tin chi tiết này, chúng tôi có thể đếm số lượng tàu xuất hiện trong cả hai danh sách; đó là số lượng va chạm tiềm năng tối đa. Việc hủy một chuyến tàu từ mỗi cặp xung đột là đủ để tránh va chạm. 

Nhận xét quan trọng là chỉ có giao điểm của số tàu mới quan trọng. Nếu một con số xuất hiện ở cả lịch trình phía dưới và bên trái, hai đoàn tàu đó sẽ va chạm nhau. Vì chúng ta có thể hủy bất kỳ một trong các đoàn tàu xung đột nào để giải quyết va chạm, nên số lần hủy tối thiểu được yêu cầu là số lượng nhỏ hơn trong số các đoàn tàu dọc hoặc ngang liên quan đến va chạm. Tuy nhiên, vì mỗi số là duy nhất nên nhiều nhất một đoàn tàu dọc và một đoàn tàu ngang có chung một số nên số lần hủy tối thiểu chính xác bằng kích thước của giao lộ. 

Thông tin chi tiết này làm giảm thuật toán từ mô phỏng O(n·m) tiềm năng thành tính toán giao điểm tập hợp đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng mọi đường dẫn) | O(n·m) | O(n·m) | Hoạt động với n, m nhỏ, chấp nhận được ở đây nhưng không cần thiết | 
| Tối ưu (đặt giao lộ) | O(n+m) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số lượng đoàn tàu ở đáy`n`và rời chuyến tàu`m`. 
2. Đọc danh sách các số tàu đã lên lịch và lưu thành bộ để tra cứu nhanh. 
3. Đọc danh sách các số tàu còn lại đã lên lịch và lưu chúng vào một bộ khác. 
4. Tính giao điểm của hai tập hợp. Điều này đưa ra tất cả các số tàu được lên lịch trên cả hai trục sẽ va chạm. 
5. Kích thước của nút giao là số lượng tàu tối thiểu phải hủy. In số này. 

Tại sao nó hoạt động: mọi va chạm có thể xảy ra đều được biểu thị bằng số tàu xuất hiện trong cả hai danh sách. Việc hủy một đoàn tàu khỏi mỗi cặp xung đột sẽ loại bỏ va chạm. Vì mỗi số tàu xảy ra nhiều nhất một lần ở mỗi bên nên có một ánh xạ một-một giữa các số tàu giao nhau và các vụ va chạm. Điều này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        bottom_trains = set(map(int, input().split()))
        left_trains = set(map(int, input().split()))
        collisions = bottom_trains & left_trains
        print(len(collisions))

if __name__ == "__main__":
    main()
```Giải pháp đọc số lượng trường hợp thử nghiệm đầu tiên. Mỗi trường hợp thử nghiệm được xử lý bằng cách đọc hai danh sách số tàu. Việc chuyển đổi danh sách thành tập hợp cho phép kiểm tra thành viên trung bình O(1) và tính toán trực tiếp các nút giao. sử dụng`&`tính toán giao lộ đã thiết lập một cách hiệu quả. Độ dài của giao lộ cho số lần hủy tối thiểu cần thiết. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
1 2
1
3 4
```| đáy_trains | left_trains | ngã tư | kết quả | 
| --- | --- | --- | --- | 
| {1} | {3,4} | {} | 0 | 

Không có số tàu chung nên không cần hủy chuyến. 

### Đầu vào mẫu 2```
3 2
1 3 4
2 4
```| đáy_trains | left_trains | ngã tư | kết quả | 
| --- | --- | --- | --- | 
| {1,3,4} | {2,4} | {4} | 1 | 

Giao lộ có đoàn tàu số 4 sẽ va chạm. Việc hủy một trong các chuyến tàu sẽ giải quyết được vụ va chạm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Chuyển đổi danh sách thành tập hợp và tính toán giao lộ là tuyến tính trong tổng số chuyến tàu | 
| Không gian | O(n + m) cho mỗi trường hợp thử nghiệm | Lưu trữ đoàn tàu | 

Với các ràng buộc (n,m ≤ 100, t ≤ 100), tổng số thao tác tối đa là 100×(100+100) = 20.000, một con số không đáng kể đối với các bộ xử lý hiện đại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided samples
assert run("3\n1 2\n1\n3 4\n3 2\n1 3 4\n2 4\n9 14\n2 7 16 28 33 57 59 86 99\n3 9 14 19 25 26 28 35 41 59 85 87 99 100\n") == "0\n1\n3", "Sample 1"

# Custom test cases
assert run("1\n1 1\n1\n1\n") == "1", "single collision"
assert run("1\n2 2\n1 2\n3 4\n") == "0", "no collision"
assert run("1\n3 3\n1 2 3\n1 2 3\n") == "3", "all collide"
assert run("1\n1 3\n100\n98 99 100\n") == "1", "boundary numbers"
assert run("1\n2 1\n50 60\n60\n") == "1", "one collision in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 / 1 / 1 | 1 | Va chạm tàu ​​đơn | 
| 2 2 / 1 2 / 3 4 | 0 | Không có trường hợp va chạm | 
| 3 3 / 1 2 3 / 1 2 3 | 3 | Tất cả các đoàn tàu va chạm | 
| 1 3/100/98 99 100 | 1 | Va chạm số cạnh | 
| 2 1/50 60/60 | 1 | Va chạm ở giữa | 

## Vỏ cạnh 

Nếu danh sách dưới cùng hoặc bên trái chỉ chứa một đoàn tàu thì thuật toán vẫn hoạt động. Đối với đầu vào`1 1 / 1 / 2`, các bộ là`{1}`Và`{2}`, giao điểm của chúng trống và đầu ra là 0. Đối với đầu vào`1 1 / 1 / 1`, cả hai bộ đều là`{1}`, kích thước giao lộ là 1, xác định chính xác một vụ va chạm. Thuật toán xử lý các số tàu nhỏ nhất và lớn nhất một cách tự nhiên mà không cần cách viết đặc biệt.
