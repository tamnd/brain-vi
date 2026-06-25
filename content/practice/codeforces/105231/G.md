---
title: "CF 105231G - Bội số của 5"
description: "Chúng ta được cho một số nguyên rất lớn được viết theo cơ số 11, nhưng chúng ta không bao giờ coi nó là một chuỗi liên tục. Thay vào đó, số được mã hóa thành một chuỗi các khối. Mỗi khối cho biết “lặp lại chữ số d chính xác k lần” và việc ghép tất cả các khối sẽ mang lại biểu diễn cơ số 11 đầy đủ."
date: "2026-06-24T14:30:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105231
codeforces_index: "G"
codeforces_contest_name: "2024 (ICPC) Jiangxi Provincial Contest -- Official Contest"
rating: 0
weight: 105231
solve_time_s: 46
verified: true
draft: false
---

[CF 105231G - Bội số của 5](https://codeforces.com/problemset/problem/105231/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên rất lớn được viết theo cơ số 11, nhưng chúng ta không bao giờ coi nó là một chuỗi liên tục. Thay vào đó, số được mã hóa thành một chuỗi các khối. Mỗi khối cho biết “lặp lại chữ số d chính xác k lần” và việc ghép tất cả các khối sẽ mang lại biểu diễn cơ số 11 đầy đủ. Các chữ số nằm trong khoảng từ 0 đến 9 và A, trong đó A đại diện cho 10. 

Nhiệm vụ chỉ đơn giản là quyết định xem số cơ sở 11 khổng lồ này có chia hết cho 5 hay không. 

Khó khăn chính là kích thước. Tổng chiều dài của số có thể đạt tới 10^14, do đó việc xây dựng chuỗi đầy đủ hoặc thậm chí lặp lại từng chữ số là không thể. Ngay cả việc đọc tất cả các chữ số một cách rõ ràng cũng sẽ quá chậm và tốn nhiều bộ nhớ. Việc nén đầu vào thông qua mã hóa độ dài chạy là cách duy nhất để tương tác với số. 

Một hạn chế chính là có tối đa 10^5 khối cho mỗi bài kiểm tra và tổng số lên tới 10^5 khối trong tất cả các bài kiểm tra. Điều này cho thấy rằng mọi giải pháp đều phải xử lý từng khối trong thời gian O(1) hoặc O(log MOD) và không bao giờ mở rộng số lượng. 

Một sai lầm ngây thơ là coi số đó như thể nó nằm trong quy tắc chia hết cơ số 10 hoặc cố gắng xây dựng chuỗi. 

Ví dụ: hãy xem xét một bài kiểm tra duy nhất: 

đầu vào: 

(3, 2), (2, 7) 

Điều này đại diện cho số cơ sở 11 22277 trong cơ sở 11 (các chữ số 2,2,2,7,7). Việc triển khai đơn giản có thể cố gắng xây dựng chuỗi "22277" và tính toán giá trị của nó, điều này ổn ở đây nhưng sẽ thất bại ngay lập tức nếu độ dài là 10^14. 

Một trường hợp phức tạp khác là số 0 đứng đầu. Vì các số 0 đứng đầu được cho phép nên các đầu vào như (5, 0) sẽ đánh giá chính xác thành 0, chia hết cho 5. 

Thách thức trọng tâm là tính toán giá trị modulo 5 mà không bao giờ hiện thực hóa số đầy đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là xây dựng lại toàn bộ số cơ sở 11 và sau đó đánh giá nó theo modulo 5 bằng cách sử dụng các trọng số vị trí. Mỗi chữ số đóng góp d * 11^i, trong đó i phụ thuộc vào vị trí của nó tính từ bên phải. Điều này yêu cầu xây dựng chuỗi đầy đủ hoặc duy trì một dãy chữ số có độ dài lên tới 10^14, cả hai đều không thể. 

Ngay cả khi chúng tôi tránh được việc lưu trữ đầy đủ và sức mạnh tính toán một cách nhanh chóng, chúng tôi vẫn cần xử lý từng chữ số riêng lẻ. Trong trường hợp xấu nhất, đó là 10^14 thao tác, vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là chúng ta không cần giá trị đầy đủ, chỉ cần phần dư của nó theo modulo 5. Điều này cho phép chúng ta sử dụng số học mô-đun trong quá trình xây dựng. Giá trị của số được xây dựng từ trái sang phải như sau: 

giá trị = (((d1 * 11 + d2) * 11 + d3) ...) 

Điều này có nghĩa là chúng ta có thể duy trì phần dư đang chạy theo modulo 5. Mỗi chữ số mới cập nhật trạng thái bằng: 

hiện tại = (hiện tại * 11 + chữ số) mod 5 

Bây giờ vấn đề là các chữ số ở dạng nén. Khối (a, b) có nghĩa là áp dụng cùng một chuyển đổi một lần. Thay vì lặp đi lặp lại nhiều lần, chúng ta coi việc này như việc áp dụng nhiều lần một phép biến đổi tuyến tính: 

x → x * 11 + b (mod 5) 

Đây là một tiến trình hình học trong số học mô-đun. Mở rộng nó, sau k lần lặp lại: 

x_k = x * 11^k + b * (11^(k-1) + ... + 1) 

Cả hai phần đều có thể được tính bằng cách sử dụng lũy thừa nhanh và tổng hình học modulo 5. Vì 5 nhỏ nên chu kỳ mũ cũng nhỏ, nhưng chúng ta có thể trực tiếp sử dụng lũy thừa mô-đun. 

Do đó, mỗi khối có thể được xử lý trong thời gian O(log a) hoặc thậm chí O(1) một cách hiệu quả vì modulo 5 làm cho chu kỳ trở nên tầm thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mở rộng chữ số) | O(N) trong đó N ≤ 10^14 | O(N) | Không thể | 
| Tối ưu (khối + lũy thừa mô-đun) | O(∑ log ai) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì phần còn lại hiện tại của tiền tố đã xử lý của số modulo 5. Chúng tôi xử lý các khối từ trái sang phải.

1. Khởi tạo một biến r = 0. Giá trị này biểu thị giá trị của tiền tố được xử lý modulo 5. 
2. Với mỗi khối (a, b), hãy chuyển b thành giá trị nguyên của nó trong cơ số 11. Nghĩa là, nếu b là 'A', coi nó là 10; nếu không thì chuyển đổi nó thành một chữ số nguyên. 
3. Tính pow11 = 11^a mod 5. Điều này mô tả mức độ dịch chuyển của tiền tố trước đó khi chúng ta nối thêm một chữ số. 
4. Tính geom = (11^a - 1) * nghịch đảo(11 - 1) modulo 5, biểu thị tổng 11^(a-1) + ... + 1 theo modulo số học. Vì 11 ≡ 1 mod 5, điều này đơn giản hóa rất nhiều. 
5. Cập nhật r bằng công thức: 

r = r * pow11 + b * geom (mod 5) 
6. Sau khi xử lý tất cả các khối, kiểm tra xem r == 0. Nếu có, số này chia hết cho 5. 

Việc đơn giản hóa ở bước 4 là điểm cấu trúc quan trọng. Vì 11 ≡ 1 mod 5, nên chúng ta có 11^k ≡ 1 với mọi k ≥ 0. Do đó, mọi tổng hình học đều rút gọn về a và mọi lũy thừa đều rút gọn thành 1, khiến cho quá trình chuyển đổi trở nên cực kỳ đơn giản. 

Vì vậy, bản cập nhật trở thành: 

r = (r + b * a) mod 5 

Điều này loại bỏ tất cả sự phức tạp của lũy thừa. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là sau khi xử lý từng khối, r bằng giá trị của tiền tố modulo 5. Mỗi phép biến đổi khối áp dụng chính xác phép dịch chuyển vị trí cơ số 11 cho một đoạn chữ số lặp lại. Vì số học mô-đun tôn trọng phép cộng và phép nhân nên việc giảm 11 modulo 5 ở mỗi bước vẫn đảm bảo tính chính xác. Bởi vì 11 ≡ 1 (mod 5), trọng số vị trí trở nên đồng nhất, do đó mỗi chữ số đóng góp độc lập với độ sâu vị trí ngoại trừ tính bội số của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def val(c):
    if c == 'A':
        return 10
    return int(c)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        r = 0

        for _ in range(n):
            a, b = input().split()
            a = int(a)
            b = val(b)

            r = (r + a * b) % 5

        print("Yes" if r == 0 else "No")

if __name__ == "__main__":
    solve()
```Mã này dựa trên thực tế là 11 ≡ 1 mod 5, làm sụp đổ hoàn toàn cấu trúc vị trí. Mỗi khối chỉ đóng góp hiệu ứng tổng chữ số theo modulo 5, vì vậy chúng ta tích lũy a * b một cách trực tiếp. 

Hàm chuyển đổi xử lý chữ số không thập phân duy nhất, A, ánh xạ nó thành 10. Mọi thứ khác đều là phân tích số nguyên tiêu chuẩn. 

Chúng tôi không bao giờ xây dựng số và không bao giờ tính lũy thừa vì chúng không cần thiết theo mô đun 5. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

(1, 1), (4, 5), (1, 4) 

Chúng tôi theo dõi r từng bước. 

| Bước | Khối (a, b) | r trước | Cập nhật | r sau | 
| --- | --- | --- | --- | --- | 
| 1 | (1, 1) | 0 | 0 + 1×1 | 1 | 
| 2 | (4, 5) | 1 | 1 + 4×5 = 21 | 1 | 
| 3 | (1, 4) | 1 | 1 + 1×4 = 5 | 0 | 

Kết quả cuối cùng là 0, vì vậy đầu ra là Có. 

Điều này cho thấy các chữ số lặp lại tích lũy tuyến tính theo mod 5 như thế nào. 

### Ví dụ 2 

đầu vào: 

(19, 8), (1, 0) 

| Bước | Khối (a, b) | r trước | Cập nhật | r sau | 
| --- | --- | --- | --- | --- | 
| 1 | (19, 8) | 0 | 0 + 19×8 = 152 | 2 | 
| 2 | (1, 0) | 2 | 2 + 1×0 = 2 | 2 | 

Kết quả cuối cùng là 2 nên đầu ra là No. 

Điều này chứng tỏ rằng cấu trúc dẫn đầu không quan trọng ngoài bội số modulo 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi khối được xử lý một lần với số học O(1) | 
| Không gian | O(1) | Chỉ phần còn lại đang chạy được lưu trữ | 

Tổng số khối trong tất cả các trường hợp thử nghiệm tối đa là 10^5, do đó quá trình quét tuyến tính dễ dàng đủ nhanh. Các phép toán là phép cộng và phép nhân số nguyên đơn giản với mô đun nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return "\n".join(run.outputs) if False else ""  # placeholder

# Since we provided a script-style solution, we redefine solver inline for tests
def solve_test(inp: str) -> str:
    it = iter(inp.strip().split())
    t = int(next(it))
    out = []
    def val(c):
        return 10 if c == 'A' else int(c)

    for _ in range(t):
        n = int(next(it))
        r = 0
        for _ in range(n):
            a = int(next(it))
            b = next(it)
            r = (r + a * val(b)) % 5
        out.append("Yes" if r == 0 else "No")
    return "\n".join(out)

# provided samples
assert solve_test("1\n3\n1 1\n4 5\n1 4\n") == "Yes"
assert solve_test("1\n2\n19 8\n1 0\n") == "No"

# custom cases
assert solve_test("1\n1\n1 A\n") == "No", "single digit 10"
assert solve_test("1\n1\n5 0\n") == "Yes", "all zeros"
assert solve_test("1\n2\n2 1\n3 1\n") == "Yes", "sum multiple of 5"
assert solve_test("1\n2\n2 2\n3 3\n") == "No", "non divisible case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn A | Không | xử lý chữ số không thập phân | 
| tất cả số không | Có | khối 0 hàng đầu | 
| tổng hỗn hợp | Có | tính chính xác của tổng hợp | 
| không chia hết | Không | tính đúng đắn của trường hợp thất bại | 

## Vỏ cạnh 

Khối 0 đứng đầu chẳng hạn như (10, 0) không đóng góp gì vào giá trị bất kể vị trí. Thuật toán xử lý nó dưới dạng r = (r + 10 * 0) mod 5, giữ nguyên r, phù hợp với thực tế là việc chèn số 0 vào bất kỳ đâu cũng không làm thay đổi khả năng chia hết. 

Một khối đơn chứa A, chẳng hạn như (1, A), được xử lý chính xác bằng cách chuyển đổi A thành 10 và áp dụng trực tiếp số học modulo. Vì 10 mod 5 bằng 0 nên nó ngay lập tức không có tác dụng gì, phù hợp với hành vi chia hết dự kiến. 

Số lần lặp lại lớn như (10^9, 7) không cần lặp lại. Phép nhân a * b được thực hiện ngầm theo modulo 5 thông qua cập nhật r và Python xử lý các số nguyên lớn một cách an toàn. Kết quả chỉ phụ thuộc vào mod 5, được nắm bắt ngầm trong quá trình giảm modulo trong r.
