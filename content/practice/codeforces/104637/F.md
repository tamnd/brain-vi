---
title: "CF 104637F - Phép trừ"
description: "Chúng ta được cho một số cặp số nguyên dương độc lập. Đối với mỗi cặp, chúng tôi liên tục áp dụng một phép biến đổi xác định: xác định số lớn hơn trong hai số và trừ đi số nhỏ hơn từ số đó."
date: "2026-06-29T17:01:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104637
codeforces_index: "F"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u0431\u0430\u0437\u043e\u0432\u0430\u044f \u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u043a\u0430, \u0443\u0441\u043b\u043e\u0432\u0438\u044f, \u0446\u0438\u043a\u043b\u044b"
rating: 0
weight: 104637
solve_time_s: 76
verified: false
draft: false
---

[CF 104637F - Phép trừ](https://codeforces.com/problemset/problem/104637/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số cặp số nguyên dương độc lập. Đối với mỗi cặp, chúng tôi liên tục áp dụng một phép biến đổi xác định: xác định số lớn hơn trong hai số và trừ đi số nhỏ hơn từ số đó. Nếu cả hai số đều bằng nhau, chúng ta trừ đi số này với số kia, ngay lập tức sẽ tạo ra số 0. 

Mỗi thao tác giảm nghiêm ngặt ít nhất một trong các số và chúng tôi tiếp tục cho đến khi ít nhất một thành phần trở thành 0. Nhiệm vụ là tính toán, đối với mỗi cặp đầu vào, có bao nhiêu phép trừ như vậy xảy ra trước khi kết thúc. 

Các ràng buộc cho phép tối đa 1000 cặp, với mỗi giá trị lớn tới 10^9. Một giải pháp mô phỏng từng phép trừ một có thể chuyển thành công việc tuyến tính trên mỗi bước. Trong trường hợp xấu nhất, chẳng hạn như các cặp số Fibonacci liên tiếp, số lượng phép toán tỷ lệ thuận với độ lớn của chính các số đó, khiến cho việc mô phỏng trực tiếp hoàn toàn không khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi các số bằng nhau. Ví dụ: bắt đầu bằng (5, 5), quy trình thực hiện một thao tác và ngay lập tức đạt đến (0, 5). Bất kỳ lý do nào giả định tỷ lệ giảm nghiêm ngặt hoặc chỉ xem xét mức giảm không đồng đều phải giải thích rõ ràng về hành vi chấm dứt này, nếu không sẽ có nguy cơ xảy ra lỗi sai sót trong lần đếm bước cuối cùng. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp bắt chước quá trình một cách chính xác. Ở mỗi bước, chúng ta trừ giá trị nhỏ hơn khỏi giá trị lớn hơn và tăng bộ đếm. Điều này đúng vì nó tuân theo các quy tắc một cách trung thực, nhưng chi phí của nó phụ thuộc vào số bước trừ cần thiết. 

Vấn đề quan trọng là quá trình này có thể thực hiện rất nhiều bước. Trong trường hợp xấu nhất, khi một số chỉ lớn hơn số kia một chút, mỗi thao tác sẽ làm giảm giá trị lớn hơn đi một lượng rất nhỏ. Điều này dẫn đến một chuỗi các cập nhật tỷ lệ thuận với số lượng lớn hơn, vượt xa mức có thể chấp nhận được đối với các giá trị lên tới 10^9. 

Quan sát quan trọng là chúng ta không thay đổi số nhỏ hơn trong các phép trừ lặp đi lặp lại trong khi số lớn hơn vẫn lớn hơn đáng kể. Thay vào đó, chúng ta liên tục trừ cùng một giá trị cho đến khi số lớn hơn giảm xuống dưới giá trị đó. Đây chính xác là hành vi chia số nguyên. Nếu a lớn hơn b nhiều thì chúng ta thực hiện phép trừ sàn(a / b) trong một khối khái niệm, giảm a thành a % b. Đây là phép biến đổi tương tự được sử dụng trong thuật toán Euclide, ngoại trừ việc chúng tôi cũng tích lũy số lượng phép trừ mà chúng tôi đã nén. 

Chúng tôi liên tục thay thế giá trị lớn hơn bằng phần còn lại của nó so với giá trị nhỏ hơn và chúng tôi thêm thương số vào câu trả lời. Quá trình tiếp tục cho đến khi một giá trị trở thành 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(max(a, b)) mỗi cặp | O(1) | Quá chậm | 
| Nhảy Euclide (Tối ưu hóa phân chia) | O(log max(a, b)) mỗi cặp | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán

1. Xử lý cặp sao cho giá trị đầu tiên luôn là giá trị lớn hơn. Việc hoán đổi đảm bảo chúng ta luôn thực hiện các phép rút gọn theo một hướng nhất quán, giúp đơn giản hóa việc suy luận về số lần chúng ta trừ. 
2. Nếu giá trị nhỏ hơn bằng 0, hãy dừng ngay lập tức. Không thể thực hiện thêm thao tác nào nữa, vì vậy số lượng tích lũy đã là cuối cùng. 
3. Tính xem giá trị nhỏ hơn khớp với giá trị lớn hơn bao nhiêu lần bằng phép chia số nguyên. Thương số này biểu thị chính xác số lượng phép trừ mà quy trình đơn giản sẽ thực hiện trước khi giá trị lớn hơn giảm xuống dưới giá trị nhỏ hơn. 
4. Thêm thương số này vào tổng số hoạt động đang thực hiện. Điều này nén nhiều phép trừ đơn lẻ lặp đi lặp lại thành một bước số học mà không thay đổi trạng thái cuối cùng. 
5. Thay thế giá trị lớn hơn bằng phần dư của nó theo modulo giá trị nhỏ hơn. Điều này phản ánh trạng thái sau khi thực hiện tất cả các phép trừ đó cùng một lúc. 
6. Hoán đổi vai trò một lần nữa nếu cần thiết để giá trị lớn hơn luôn ở đầu tiên, sau đó lặp lại quy trình. 

Việc rút gọn lặp lại này phản ánh thuật toán Euclide, ngoại trừ việc thay vì chỉ theo dõi tiến trình gcd, chúng tôi cũng đang tích lũy tổng số bước thương được thực hiện ở mỗi giai đoạn rút gọn. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào khi a lớn hơn b, quá trình trừ sẽ liên tục biến đổi (a, b) thành (a - b, b), sau đó (a - 2b, b), v.v. cho đến khi a trở nên nhỏ hơn b. Số bước như vậy chính xác là tầng (a / b) và giá trị kết quả là mod b. Bởi vì mọi phép trừ đều bảo toàn tính bất biến mà số nhỏ hơn không thay đổi trong giai đoạn này, nên việc nén toàn bộ chuỗi thành một phép chia không bỏ qua hoặc hợp nhất các trạng thái cấu trúc riêng biệt một cách không chính xác. Do đó, quá trình này tương đương với việc tính tổng tất cả các thương số gặp phải trong quá trình rút gọn Euclide. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_pair(a, b):
    ops = 0
    while b:
        if a < b:
            a, b = b, a
        ops += a // b
        a %= b
    return ops

def main():
    n = int(input())
    out = []
    for _ in range(n):
        a, b = map(int, input().split())
        out.append(str(solve_pair(a, b)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp được cấu trúc xung quanh việc thực thi nhiều lần rằng chúng tôi luôn trừ giá trị nhỏ hơn cho giá trị lớn hơn. Bất biến vòng lặp là`b`là phần tử nhỏ hơn trước mỗi bước chia. biểu hiện`a // b`đếm chính xác có bao nhiêu phép trừ ngây thơ sẽ xảy ra trước khi`a`giảm xuống dưới`b`, Và`a %= b`cập nhật trạng thái cho phần còn lại sau tất cả các phép trừ đó. 

Điều kiện chấm dứt`while b:`đảm bảo quá trình dừng khi một số trở thành số 0, khớp với quy tắc dừng ban đầu. Bước hoán đổi là cần thiết, vì sau khi thực hiện modulo, vai trò của các số có thể đảo ngược. 

## Ví dụ đã hoạt động 

### Ví dụ 1: (24, 17) 

Chúng tôi theo dõi các bước Euclide nén. 

| một | b | một // b | hoạt động đã thêm | một % b | 
| --- | --- | --- | --- | --- | 
| 24 | 17 | 1 | 1 | 7 | 
| 17 | 7 | 2 | 2 | 3 | 
| 7 | 3 | 2 | 2 | 1 | 
| 3 | 1 | 3 | 3 | 0 | 

Câu trả lời cuối cùng là 1 + 2 + 2 + 3 = 8. 

Dấu vết này cho thấy mỗi hàng tương ứng như thế nào với một khối đầy đủ các phép trừ lặp lại trong quy trình ban đầu. Mỗi bước còn lại phản ánh trạng thái sau khi sử dụng hết tất cả các phép trừ có thể có với cặp hiện tại. 

### Ví dụ 2: (10, 4) 

| một | b | một // b | hoạt động đã thêm | một % b | 
| --- | --- | --- | --- | --- | 
| 10 | 4 | 2 | 2 | 2 | 
| 4 | 2 | 2 | 2 | 0 | 

Tổng số hoạt động = 4. 

Điều này chứng tỏ rằng ngay cả những đầu vào nhỏ cũng có thể chứa nhiều lớp phép trừ lặp đi lặp lại và thuật toán nén chúng một cách rõ ràng thành các bước chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log max(a, b)) mỗi cặp | Mỗi bước modulo giảm ít nhất một số xuống phần còn lại nhỏ hơn nghiêm ngặt, phản ánh độ sâu thuật toán của Euclid | 
| Không gian | O(1) | Chỉ có một số lượng biến không đổi được duy trì trên mỗi cặp | 

Hành vi logarit xuất phát từ thực tế là thuật toán Euclide giảm độ lớn của các số một cách nhanh chóng và số bước còn lại được giới hạn bởi số chữ số trong đầu vào. Với tối đa 1000 cặp, điều này thoải mái phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve_pair(a, b):
        ops = 0
        while b:
            if a < b:
                a, b = b, a
            ops += a // b
            a %= b
        return ops

    n, *rest = map(int, inp.split())
    n = n
    lines = inp.strip().splitlines()[1:]
    out = []
    for line in lines:
        a, b = map(int, line.split())
        out.append(str(solve_pair(a, b)))
    return "\n".join(out)

# provided sample
assert run("1\n24 17\n") == "8"

# equal numbers
assert run("1\n5 5\n") == "1"

# one divides another
assert run("1\n10 2\n") == "5"

# chain-like case
assert run("1\n7 3\n") == "3"

# large numbers
assert run("1\n1000000000 999999999\n") == str((1000000000 // 999999999) + (999999999 // 1))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 5 | 1 | Giá trị bằng nhau chấm dứt ngay lập tức | 
| 10 2 | 5 | Trường hợp chia hết chính xác | 
| 7 3 | 3 | Giảm Euclide nhiều bước | 
| 1e9 999999999 | tính toán | Trường hợp ứng suất biên lớn | 

## Vỏ cạnh 

Khi cả hai số bằng nhau, thuật toán thực hiện đúng một thao tác trước khi kết thúc. Đối với đầu vào (5, 5), việc hoán đổi không liên quan và phép chia đầu tiên cho 5 // 5 = 1, sau đó phần còn lại trở thành 0. Vòng lặp dừng ngay lập tức, khớp với quy tắc đẳng thức dẫn đến một bước trừ duy nhất. 

Khi một số lớn hơn nhiều so với số kia, chẳng hạn như (100, 1), thuật toán sẽ thực hiện một bước chia duy nhất với thương 100 và số dư 0. Điều này phù hợp với thực tế là quy trình đơn giản sẽ trừ đi 1 nhiều lần cho đến khi số lớn hơn biến mất. 

Khi các số là các số Fibonacci liên tiếp, chẳng hạn như (21, 13), mỗi bước modulo sẽ giảm cặp này thành cặp Fibonacci trước đó, đảm bảo quá trình này có nhiều lớp Euclide. Thuật toán tích lũy chính xác từng thương số, phản ánh chuỗi trừ dài mà lẽ ra sẽ rõ ràng trong mô phỏng lực lượng vũ phu.
