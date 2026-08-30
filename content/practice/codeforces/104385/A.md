---
title: "CF 104385A - Khoan Gỗ Tạo Lửa"
description: "Chúng ta được cung cấp một mô phỏng rất nhỏ được lặp lại nhiều lần. Mỗi trường hợp thử nghiệm mô tả một tình huống trong đó một người đang cố gắng tạo ra lửa bằng cách khoan gỗ."
date: "2026-07-01T02:51:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104385
codeforces_index: "A"
codeforces_contest_name: "2023 (ICPC) Jiangxi Provincial Contest -- Official Contest"
rating: 0
weight: 104385
solve_time_s: 40
verified: true
draft: false
---

[CF 104385A - Khoan gỗ để tạo lửa](https://codeforces.com/problemset/problem/104385/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mô phỏng rất nhỏ được lặp lại nhiều lần. Mỗi trường hợp thử nghiệm mô tả một tình huống trong đó một người đang cố gắng tạo ra lửa bằng cách khoan gỗ. Sự thành công của nỗ lực chỉ phụ thuộc vào ba số nguyên: một giá trị ngưỡng bắt buộc và hai tham số biểu thị mức độ mạnh mẽ của hành động và tốc độ thực hiện của nó. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một giá trị ngưỡng$N$, một sức mạnh$S$, và tốc độ$V$. Quá trình thành công nếu “hiệu quả” kết hợp của việc khoan, được mô hình hóa như sản phẩm$S \times V$, đạt hoặc vượt$N$. Nếu sản phẩm đủ lớn thì gỗ sẽ bốc cháy, nếu không thì không. 

Vì vậy, mỗi truy vấn là độc lập và đầu ra là một chuỗi các quyết định nhị phân: in 1 nếu$S \cdot V \ge N$, ngược lại in ra 0. 

Những hạn chế là cực kỳ nhỏ:$T \le 100$và tất cả các giá trị nhiều nhất là 100. Điều này ngay lập tức ngụ ý rằng ngay cả phép tính đơn giản nhất cũng không đáng kể về mặt hiệu suất. Nhân hai số nguyên cho mỗi trường hợp thử nghiệm là công việc liên tục, do đó, ngay cả một vòng lặp đơn giản cũng đủ. 

Cạm bẫy tinh vi duy nhất đến từ việc diễn giải chính xác điều kiện. Một sai lầm phổ biến là so sánh$S + V$chống lại$N$, hoặc yêu cầu sự bất bình đẳng nghiêm ngặt thay vì cho phép sự bình đẳng. Một lỗi khác có thể xảy ra là tràn trong các ngôn ngữ có số nguyên có chiều rộng cố định, nhưng trong Python thì điều này không liên quan. Vấn đề thứ ba là quên rằng mỗi trường hợp thử nghiệm là độc lập và vô tình mang trạng thái giữa các trường hợp, mặc dù vấn đề này không đưa ra trạng thái một cách tự nhiên. 

## Phương pháp tiếp cận 

Việc giải thích brute-force là trực tiếp: đối với mỗi trường hợp thử nghiệm, hãy tính sản phẩm$S \times V$và so sánh nó với$N$. Điều này đã tối ưu vì không có cấu trúc liên kết các ca kiểm thử và không có quá trình tiền xử lý nào giúp giảm bớt tính toán. Mỗi quyết định chỉ phụ thuộc vào ba số nên bất kỳ thuật toán nào ít nhất cũng phải đọc chúng và thực hiện một phép nhân. 

Cách tiếp cận vũ phu diễn ra$O(T)$, vì mỗi trường hợp kiểm thử yêu cầu thời gian làm việc không đổi. Ngay cả khi chúng ta tưởng tượng logic phức tạp hơn, kích thước đầu vào giới hạn mọi thứ chặt chẽ đến mức không có sự tối ưu hóa nào ngoài việc đánh giá trực tiếp là có ý nghĩa. 

Quan sát quan trọng là vấn đề không nằm ở việc tìm kiếm hoặc tối ưu hóa trên nhiều đầu vào. Nó hoàn toàn là một so sánh ngưỡng trên một giá trị dẫn xuất duy nhất. Khi điều này được nhận ra, lời giải sẽ chuyển thành một phép kiểm tra số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T) | O(1) | Đã chấp nhận | 
| Tối ưu | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$T$. Điều này xác định số lượng kiểm tra độc lập chúng tôi sẽ thực hiện. 
2. Với mỗi test, đọc ba số nguyên$N$,$S$, Và$V$. Những điều này xác định đầy đủ kịch bản. 
3. Tính tích$P = S \times V$. Điều này thể hiện sức mạnh khoan hiệu quả. 
4. So sánh$P$với$N$. Nếu như$P \ge N$, đầu ra 1 vì ngưỡng đánh lửa được đáp ứng hoặc vượt quá. 
5. Nếu không thì xuất ra 0, vì quá trình khoan không đủ để đốt cháy gỗ. 

### Tại sao nó hoạt động 

Mỗi ca kiểm thử rút gọn thành một bất đẳng thức duy nhất chỉ liên quan đến$S$,$V$, Và$N$. Không có tương tác ẩn hoặc hiệu ứng tuần tự giữa các trường hợp. Thuật toán đánh giá trực tiếp điều kiện xác định thành công nên nó khớp chính xác với tiêu chí thành công của bài toán. Vì phép nhân và so sánh là các phép toán chính xác và xác định nên quyết định luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    out = []
    for _ in range(t):
        n, s, v = map(int, input().split())
        out.append("1" if s * v >= n else "0")
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`để xử lý nhiều trường hợp thử nghiệm một cách rõ ràng. Mỗi trường hợp thử nghiệm được xử lý độc lập bên trong một vòng lặp. 

Việc tính toán khóa diễn ra trong một dòng duy nhất:`s * v >= n`. Điều này mã hóa cả phép nhân và so sánh ngưỡng. Kết quả được lưu dưới dạng chuỗi vì định dạng đầu ra yêu cầu một giá trị trên mỗi dòng. 

Việc sử dụng danh sách để thu thập kết quả đầu ra sẽ tránh các lệnh gọi I/O lặp lại bên trong vòng lặp, đây là một phương pháp lập trình cạnh tranh tiêu chuẩn mặc dù các ràng buộc ở đây không yêu cầu nghiêm ngặt về điều đó. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
3
10 2 5
12 3 3
20 4 4
```Chúng tôi theo dõi từng trường hợp thử nghiệm: 

| N | S | V | S×V | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 10 | 2 | 5 | 10 | 1 | 
| 12 | 3 | 3 | 9 | 0 | 
| 20 | 4 | 4 | 16 | 0 | 

Trường hợp đầu tiên thành công chính xác ở ngưỡng, xác nhận rằng đẳng thức được chấp nhận. Trường hợp thứ hai và thứ ba đều thất bại do sản phẩm không đạt giá trị yêu cầu. Điều này cho thấy thuật toán được điều khiển chặt chẽ bởi sự so sánh ngưỡng chứ không phải độ lớn tương đối. 

Một ví dụ thứ hai:```
4
1 1 1
5 2 3
6 2 3
7 3 3
```| N | S | V | S×V | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 
| 5 | 2 | 3 | 6 | 1 | 
| 6 | 2 | 3 | 6 | 1 | 
| 7 | 3 | 3 | 9 | 1 | 

Điều này thể hiện nhiều điều kiện biên, đặc biệt là đẳng thức lặp lại tại$N = 6$, nơi điều kiện vẫn giữ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm yêu cầu một phép nhân và một phép so sánh | 
| Không gian | O(1) | Chỉ một vài biến số nguyên được lưu trữ bất kỳ lúc nào | 

Giới hạn đầu vào đảm bảo rằng ngay cả$T = 100$là tầm thường. Thuật toán thực hiện tối đa 100 thao tác trong thời gian không đổi, thấp hơn nhiều so với bất kỳ giới hạn thực tế nào. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, s, v = map(int, input().split())
        out.append("1" if s * v >= n else "0")
    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample-style tests
assert run("3\n10 2 5\n12 3 3\n20 4 4\n") == "1\n0\n0"

# minimum values
assert run("1\n1 1 1\n") == "1"

# just below threshold
assert run("1\n10 2 4\n") == "0"

# all equal values
assert run("3\n5 2 3\n5 2 3\n5 2 3\n") == "1\n1\n1"

# boundary equality cases
assert run("2\n6 2 3\n7 2 3\n") == "1\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | đầu vào hợp lệ tối thiểu | 
| 10 2 4 | 0 | thất bại nghiêm trọng ngay dưới ngưỡng | 
| trường hợp giống hệt nhau lặp đi lặp lại | tất cả 1 | tính nhất quán giữa các bài kiểm tra | 
| ranh giới bình đẳng | 1 rồi 0 | tính đúng đắn của điều kiện ≥ | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi sản phẩm chính xác bằng ngưỡng. Ví dụ: đầu vào:```
1
6 2 3
```Đây$S \times V = 6$, bằng$N$. Thuật toán tính tích là 6, so sánh với 6 và đưa ra 1. Điều này xác nhận rằng điều kiện là bao gồm, không nghiêm ngặt. 

Một trường hợp khác là khi sản phẩm chỉ thiếu một sản phẩm so với yêu cầu:```
1
7 2 3
```Tích số là 6, nhỏ hơn 7, do đó đầu ra là 0. Thuật toán không thực hiện bất kỳ phép làm tròn hoặc phép tính gần đúng nào, nó trực tiếp đánh giá tích số nguyên. 

Vì tất cả các phép tính đều là số nguyên nhỏ nên không có vấn đề tràn hoặc độ chính xác và mọi trường hợp đều giảm rõ ràng xuống mức so sánh xác định.
