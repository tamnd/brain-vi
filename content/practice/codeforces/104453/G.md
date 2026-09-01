---
title: "CF 104453G - \u0421\u0431\u043e\u0440 \u0443\u0440\u043e\u0436\u0430\u044f"
description: "Chúng tôi được cung cấp một số loại cây trồng. Đối với mỗi loại cây trồng, chúng tôi biết số kg đã được thu hoạch và một hộp bảo quản có thể chứa được bao nhiêu cây trồng cụ thể đó. Hạn chế chính là các hộp dành riêng cho loại cây trồng, nghĩa là bạn không thể trộn các loại cây trồng khác nhau trong cùng một hộp."
date: "2026-06-30T14:35:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104453
codeforces_index: "G"
codeforces_contest_name: "ICPC Central Russia Regional Qualyfing Round, 2021"
rating: 0
weight: 104453
solve_time_s: 76
verified: true
draft: false
---

[CF 104453G - \u0421\u0431\u043e\u0440 \u0443\u0440\u043e\u0436\u0430\u044f](https://codeforces.com/problemset/problem/104453/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số loại cây trồng. Đối với mỗi loại cây trồng, chúng tôi biết số kg đã được thu hoạch và một hộp bảo quản có thể chứa được bao nhiêu cây trồng cụ thể đó. Hạn chế chính là các hộp dành riêng cho loại cây trồng, nghĩa là bạn không thể trộn các loại cây trồng khác nhau trong cùng một hộp. 

Đối với mỗi loại cây trồng, chúng ta phải xác định cần bao nhiêu hộp giống hệt nhau để chứa toàn bộ trọng lượng thu hoạch của cây trồng đó. Vì hộp là những đơn vị không thể chia cắt nên mọi trọng lượng còn sót lại không lấp đầy hộp vẫn cần có hộp bổ sung. Sau khi tính toán giá trị này cho mỗi vụ mùa, chúng tôi tính tổng số hộp trên tất cả các vụ mùa. 

Kích thước đầu vào có thể lên tới 100.000 cây trồng. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng việc điền từng mục vào hộp hoặc lặp lại trên kilogam. Bất kỳ giải pháp nào cũng phải xử lý từng phần cắt trong thời gian không đổi, tạo ra độ phức tạp O(n) tuyến tính tổng thể. 

Một trường hợp phức tạp xuất hiện khi tổng số lượng thu hoạch chia hết cho dung lượng hộp. Ví dụ: nếu 20 kg được đựng trong hộp có kích thước 4 kg thì câu trả lời chính xác là 5 hộp. Nhưng nếu 21 kg được đựng trong hộp cỡ 4 kg thì đáp án là 6 hộp. Phép chia số nguyên ngây thơ không có xử lý trần sẽ tính sai 5 trong trường hợp thứ hai. 

Một trường hợp góc khác là khi n bằng 0. Trong trường hợp này, không có cây trồng, do đó không cần hộp và kết quả đầu ra đúng là 0. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề này là xử lý từng loại cây trồng một cách độc lập và mô phỏng việc đóng gói sản phẩm thu hoạch vào hộp. Đối với một vụ mùa, chúng tôi có thể liên tục trừ đi dung tích hộp cho trọng lượng còn lại và đếm số lần chúng tôi thực hiện việc này cho đến khi không còn gì. Điều này mô hình chính xác quy trình thực nhưng quá chậm trong trường hợp xấu nhất. Nếu một cây trồng có 100.000 kg và kích thước hộp là 1 kg, thì mô phỏng này thực hiện 100.000 thao tác chỉ cho một cây trồng và trên 100.000 cây trồng, điều này trở thành 10^10 thao tác, điều này hoàn toàn không khả thi. 

Quan sát chính là mỗi loại cây trồng đều độc lập và tuân theo cấu trúc số học đơn giản. Về cơ bản, chúng tôi đang tính toán cần bao nhiêu nhóm kích thước b_i để bao gồm các mục a_i. Đây chính xác là vấn đề phân chia trần nhà. Thay vì mô phỏng việc nhóm, chúng ta có thể tính toán trực tiếp số lượng hộp đầy đủ bằng cách sử dụng số học số nguyên. 

Mỗi vụ có số hộp là: 

trần(a_i / b_i) 

Điều này có thể được tính toán một cách hiệu quả như sau: 

(a_i + b_i - 1) // b_i 

Phép biến đổi này hoạt động vì việc thêm b_i - 1 đảm bảo rằng mọi số dư khác 0 sẽ đẩy phép chia lên một ô đầy đủ, trong khi bội số chính xác vẫn không thay đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng | O(tổng a_i / b_i) | O(1) | Quá chậm | 
| Phân chia trần theo vụ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số loại cây trồng n. Nếu n bằng 0 thì câu trả lời ngay lập tức là 0 vì không có gì để lưu trữ. 
2. Khởi tạo biến tích lũy Total_boxes về 0. Điều này sẽ lưu trữ tổng số hộp cần thiết trên tất cả các loại cây trồng. 
3. Với mỗi vụ mùa i, đọc cặp (a_i, b_i), thể hiện trọng lượng thu hoạch và dung tích hộp. 
4. Tính số hộp cần thiết cho vụ mùa này bằng phép chia trần: (a_i + b_i - 1) // b_i. Điều này đảm bảo rằng ngay cả những hộp cuối cùng được lấp đầy một phần cũng được tính. 
5. Thêm giá trị này vào Total_boxes. Mỗi loại cây trồng đóng góp độc lập nên tổng kết có giá trị mà không có sự tương tác giữa các loại cây trồng. 
6. Sau khi xử lý tất cả các phần cắt, xuất ra các hộp tổng. 

### Tại sao nó hoạt động

Mỗi phần cắt giảm xuống vấn đề đóng gói một chiều trong đó chúng tôi phân chia một lượng cố định a_i thành các nhóm có kích thước b_i. Bất kỳ bao bì hợp lệ nào cũng phải sử dụng ít nhất hộp đầy đủ tầng (a_i / b_i), và nếu còn sót lại thì không thể tránh khỏi việc bổ sung thêm một hộp vì chúng tôi không thể chia hộp hoặc trộn cây trồng. Công thức (a_i + b_i - 1) // b_i nắm bắt chính xác giới hạn dưới này và không giải pháp nào có thể sử dụng ít hộp hơn, khiến nó vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    total = 0

    for _ in range(n):
        a, b = map(int, input().split())
        total += (a + b - 1) // b

    print(total)

if __name__ == "__main__":
    solve()
```Giải pháp đọc mỗi phần cắt một lần và ngay lập tức chuyển đổi nó thành số lượng hộp cần thiết. Biểu thức (a + b - 1) // b là thủ thuật số nguyên tiêu chuẩn để chia trần và tránh hoàn toàn số học dấu phẩy động, giúp việc tính toán vừa nhanh vừa an toàn. 

Tổng tích lũy được duy trì dưới dạng một số nguyên duy nhất và vì mỗi phép toán là O(1), nên độ phức tạp tổng thể vẫn là tuyến tính. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
10 2
20 4
40 3
```Chúng tôi xử lý từng loại cây trồng một cách độc lập. 

| Cắt | một | b | Tính toán | Hộp | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 2 | (10+1)//2 = 11//2 | 5 | 
| 2 | 20 | 4 | (20+3)//4 = 23//4 | 5 | 
| 3 | 40 | 3 | (40+2)//3 = 42//3 | 14 | 

Tổng số hộp = 5 + 5 + 14 = 24. 

Điều này khẳng định rằng mỗi vụ mùa là độc lập và tổng số chỉ đơn giản là tổng của các mức chia trần. 

### Mẫu 2 

đầu vào:```
6
7 10
1 8
6 3
3 6
1 6
1 1
```| Cắt | một | b | Tính toán | Hộp | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 10 | (7+9)//10 = 16//10 | 1 | 
| 2 | 1 | 8 | (1+7)//8 = 8//8 | 1 | 
| 3 | 6 | 3 | (6+2)//3 = 8//3 | 2 | 
| 4 | 3 | 6 | (3+5)//6 = 8//6 | 1 | 
| 5 | 1 | 6 | (1+5)//6 = 6//6 | 1 | 
| 6 | 1 | 1 | (1+0)//1 = 1//1 | 1 | 

Tổng số hộp = 7. 

Ví dụ này nhấn mạnh cả hai thái cực: dung lượng rất nhỏ và phép chia chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần cắt được xử lý trong thời gian không đổi bằng một phép toán số học | 
| Không gian | O(1) | Chỉ một tổng hiện có được lưu trữ bất kể kích thước đầu vào | 

Thuật toán dễ dàng xử lý n lên tới 100.000 vì nó chỉ thực hiện một vài phép tính số nguyên trên mỗi dòng đầu vào, nằm trong giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""3
10 2
20 4
40 3
""") == "24"

assert run("""6
7 10
1 8
6 3
3 6
1 6
1 1
""") == "7"

# minimum input
assert run("0\n") == "0"

# exact division case
assert run("""2
10 5
20 4
""") == str((10//5) + ((20+3)//4))

# large remainder case
assert run("1\n100000 99999\n") == "2"

# all ones
assert run("""4
1 1
1 1
1 1
1 1
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=0 | 0 | trường hợp trống | 
| 10 5, 20 4 | 2 + 5 | phép chia chính xác và chia phần dư | 
| 100000 99999 | 2 | công suất gần giới hạn lớn | 
| tất cả 1 1 | 4 | trường hợp tối thiểu thống nhất | 

## Vỏ cạnh 

Với n = 0, vòng lặp không bao giờ thực hiện và tổng vẫn bằng 0, do đó kết quả đầu ra là chính xác mà không cần xử lý đặc biệt. 

Đối với phần cắt trong đó a_i chia hết chính xác cho b_i, chẳng hạn như 20 và 4, công thức sẽ cho (20 + 3) // 4 = 23 // 4 = 5, khớp với cách đóng gói chính xác dự kiến. Không có hộp bổ sung nào được thêm vào vì phần còn lại bằng 0 và phép cộng b_i - 1 không vượt qua bội số của b_i. 

Đối với cây trồng như 7 và 10, trong đó sản lượng thu hoạch nhỏ hơn sức chứa của hộp, (7 + 9) // 10 = 16 // 10 = 1, phản ánh chính xác rằng ngay cả khi hộp được lấp đầy một phần vẫn cần thiết.
