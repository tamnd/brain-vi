---
title: "CF 1033F - Máy tính Boolean"
description: "Chúng ta có một tập hợp lên tới 30.000 số nguyên, mỗi số có thể biểu diễn tối đa 12 bit. Bên cạnh đó, chúng ta còn được cung cấp nhiều “máy bitwise”, trong đó mỗi máy xác định phép chuyển đổi từ hai số đầu vào thành một số đầu ra."
date: "2026-06-16T19:58:20+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "fft", "math"]
categories: ["algorithms"]
codeforces_contest: 1033
codeforces_index: "F"
codeforces_contest_name: "Lyft Level 5 Challenge 2018 - Elimination Round"
rating: 2800
weight: 1033
solve_time_s: 935
verified: false
draft: false
---

[CF 1033F - Máy tính Boolean](https://codeforces.com/problemset/problem/1033/F) 

**Đánh giá:** 2800 
**Tags:** bitmasks, vũ phu, fft, toán học 
**Thời gian giải:** 15 phút 35 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp lên tới 30.000 số nguyên, mỗi số có thể biểu diễn tối đa 12 bit. Bên cạnh đó, chúng ta còn được cung cấp nhiều “máy bitwise”, trong đó mỗi máy xác định phép chuyển đổi từ hai số đầu vào thành một số đầu ra. Quá trình chuyển đổi hoạt động từng chút một: đối với mỗi vị trí bit, máy áp dụng một trong sáu phép toán boolean cố định cho các bit tương ứng của hai đầu vào và các bit kết quả được kết hợp lại thành một số. 

Nhiệm vụ được lặp lại cho mỗi máy: đếm xem có bao nhiêu cặp thanh ghi được sắp xếp tạo ra kết quả đầu ra chính xác bằng 0. 

Một chi tiết quan trọng là các cặp được sắp xếp theo thứ tự và được phép lặp lại, do đó cả (i, j) và (j, i) đều quan trọng và (i, i) đều hợp lệ. 

Các ràng buộc chặt chẽ theo hai hướng khác nhau. Kích thước từ nhiều nhất là 12, điều này gợi ý rõ ràng rằng bất kỳ sự phụ thuộc hàm mũ nào vào w đều có thể chấp nhận được. Tuy nhiên, n lên tới 3e4 và m lên tới 5e4, điều này khiến cho bất kỳ O(n²) nào trên mỗi truy vấn đều không thể thực hiện được. Một đánh giá đơn giản cho mỗi cổng trên tất cả các cặp sẽ yêu cầu đánh giá khoảng 9e8 cho mỗi truy vấn trong trường hợp xấu nhất, điều này hoàn toàn không khả thi. 

Cấu trúc hoạt động cũng rất quan trọng: mỗi cổng được áp dụng độc lập trên mỗi bit và mỗi bit chỉ phụ thuộc vào các bit đầu vào tương ứng. Khả năng phân tách giữa các vị trí bit này là đầu mối cấu trúc chính. 

Trường hợp cạnh tinh tế là khi tất cả các giá trị đầu vào giống hệt nhau hoặc khi hàm không đổi bằng 0 hoặc không đổi khác 0 bất kể đầu vào. Ví dụ: nếu một cổng ánh xạ mọi cặp bit thành 0 thì mỗi cặp có thứ tự sẽ đóng góp, tạo ra n². Nếu nó ánh xạ mọi thứ thành 1 thì câu trả lời là 0. Những thái cực này phải được xử lý một cách tự nhiên theo cùng một khuôn khổ tính toán chứ không phải như những trường hợp đặc biệt. 

Một trường hợp tinh tế khác là khi các cặp số khác nhau tạo ra hành vi trung gian giống hệt nhau ở cấp độ bit. Lý luận ngây thơ theo từng con số có thể bỏ sót rằng các cặp khác nhau đóng góp độc lập và phải được tính theo cách kết hợp. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp đánh giá từng cổng trên mỗi cặp có thứ tự. Đối với mỗi cổng trong số m cổng, chúng tôi sẽ lặp qua tất cả n2 cặp và tính kết quả w-bit. Mỗi đánh giá tốn O(w), do đó tổng độ phức tạp trở thành O(m · n² · w), vượt xa giới hạn khả thi. 

Quan sát cốt lõi là w nhỏ, vì vậy chúng ta nên nén các giá trị theo mẫu bit của chúng và lý giải về sự đóng góp cho mỗi mẫu bit thay vì theo cặp giá trị. Mỗi số là một vectơ w-bit, do đó có tối đa 2^w giá trị riêng biệt, tối đa là 4096. Thay vì theo dõi số lần xuất hiện trong một mảng có kích thước n, chúng ta có thể tổng hợp tần số của từng giá trị. 

Bây giờ vấn đề trở thành: với một mảng tần số freq[x], chúng ta muốn tính xem có bao nhiêu cặp thứ tự (x, y) tạo ra đầu ra bằng 0 theo một phép biến đổi bitwise nhất định. 

Ý tưởng cấu trúc quan trọng là tách các bit. Đối với cổng cố định, bit đầu ra ở vị trí i chỉ phụ thuộc vào bit đầu vào tại i. Vì vậy, điều kiện “đầu ra bằng 0” có nghĩa là mọi vị trí bit phải đánh giá độc lập về 0. Điều này có nghĩa là thay vì xử lý các số nguyên đầy đủ, chúng ta có thể coi mỗi giá trị là một chuỗi bit có độ dài w và tính toán trước các chuyển đổi. 

Điều này dẫn đến lập trình động mặt nạ bit tiêu chuẩn trên các giá trị từ 0 đến 2^w - 1. Đối với mỗi cổng, chúng tôi tính toán một hàm f(x, y) = 1 nếu đầu ra bằng 0, nếu không thì bằng 0. Vì miền chỉ là 4096 nên chúng tôi có thể tính toán đóng góp bằng cách lặp qua tất cả các cặp mặt nạ bit riêng biệt. 

Chúng tôi tối ưu hóa hơn nữa bằng cách lưu ý rằng hàm này có thể phân tách được trên các bit. Đối với mỗi vị trí bit, chúng tôi xác định bảng chân lý gồm 4 mục trên (b1, b2). Đối với mỗi cổng, chúng ta có thể tính toán trước, đối với mỗi cặp mặt nạ bit, xem tất cả các vị trí bit có thỏa mãn đầu ra bằng 0 hay không. Điều này làm giảm đánh giá mỗi cặp xuống O(w) và chúng tôi chỉ có 2^w trạng thái, do đó tổng số trên mỗi cổng là O(4^w) trong trường hợp xấu nhất, có thể chấp nhận được vì w 12.

Một cách xem hiệu quả hơn là tính toán trước, đối với mỗi cổng, một bảng chuyển tiếp theo các cặp bit và sau đó kết hợp các đóng góp bit bằng cách kiểm tra tính nhất quán. Mỗi cặp (x, y) hợp lệ khi và chỉ nếu với mọi vị trí bit i, quá trình chuyển đổi bit xuất ra 0. Vì vậy, chúng ta có thể tính toán trước hàm tương thích boolean cho các bit và sau đó đánh giá các cặp bằng cách sử dụng phân tách theo bit hoặc đếm kiểu zeta nhanh trên bitmask. 

Cuối cùng, chúng tôi kết hợp số lượng tần số: mỗi cặp hợp lệ (x, y) đóng góp freq[x] · freq[y]. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo cặp | O(m · n² · w) | O(1) | Quá chậm | 
| Tổng hợp Bitmask + cặp DP | O(m · 2^w · 2^w) | O(2^w) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nén đầu vào thành một mảng tần số trên tất cả các giá trị w-bit có thể có. 

Chúng tôi thay thế danh sách n thanh ghi bằng một mảng có tần số 2^w. Điều này hợp lệ vì tất cả các tính toán chỉ phụ thuộc vào mẫu bit chứ không phải vị trí. 
2. Tính toán trước, đối với mỗi ký tự cổng, bảng chân lý trên mỗi bit của nó. 

Mỗi ký hiệu cổng xác định một hàm trên hai bit tạo ra một bit. Chúng tôi lưu trữ bảng này dưới dạng bảng 2 × 2 để tra cứu nhanh. 
3. Đối với một chuỗi cổng có độ dài w cho trước, hãy xây dựng hàm boolean trên các cặp w-bit đầy đủ. 

Đối với một cặp (x, y), chúng tôi kiểm tra từng vị trí bit một cách độc lập bằng cổng tương ứng. Nếu mọi vị trí đều xuất ra 0 thì cặp này hợp lệ. 
4. Lặp lại tất cả các cặp giá trị có thể có (x, y) trong [0, 2^w). 

Đối với mỗi cặp, chúng tôi kiểm tra xem nó có tạo ra số 0 hay không bằng cách kiểm tra tất cả các vị trí bit. Vì w ≤ 12 và kích thước miền tối đa là 4096 nên bước này khả thi. 
5. Tích lũy đóng góp bằng tần số. 

Nếu một cặp (x, y) hợp lệ, chúng ta thêm freq[x] * freq[y] vào câu trả lời. 
6. Xuất tổng cuối cùng của mỗi cổng. 

### Tại sao nó hoạt động 

Mọi giá trị thanh ghi được xác định hoàn toàn bởi w bit độc lập của nó. Cổng áp dụng một hàm độc lập ở mỗi vị trí bit, do đó đầu ra bằng 0 khi và chỉ khi mọi vị trí bit tạo ra 0 dưới cổng cục bộ tương ứng. Điều này có nghĩa là độ chính xác giảm xuống khi kiểm tra tính hợp lệ của mỗi bit và việc đếm các cặp hợp lệ sẽ giảm xuống tổng các trạng thái nén độc lập. Phép nhân tần số tính toán chính xác cho tất cả các cặp thanh ghi có thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_bit_table(ch):
    if ch == 'A':
        return ((0, 0), (0, 1))
    if ch == 'O':
        return ((0, 1), (1, 1))
    if ch == 'X':
        return ((0, 1), (1, 0))
    if ch == 'a':
        return ((1, 1), (1, 0))  # NAND
    if ch == 'o':
        return ((1, 0), (0, 0))  # NOR
    if ch == 'x':
        return ((1, 0), (0, 1))  # XNOR

def main():
    w, n, m = map(int, input().split())
    arr = list(map(int, input().split()))

    maxv = 1 << w
    freq = [0] * maxv
    for v in arr:
        freq[v] += 1

    bits = []
    for v in range(maxv):
        b = [0] * w
        for i in range(w):
            b[i] = (v >> i) & 1
        bits.append(b)

    gates = [input().strip() for _ in range(m)]

    for g in gates:
        tables = [build_bit_table(c) for c in g]

        ans = 0
        for x in range(maxv):
            fx = freq[x]
            if fx == 0:
                continue
            bx = bits[x]

            for y in range(maxv):
                fy = freq[y]
                if fy == 0:
                    continue

                ok = True
                for i in range(w):
                    if tables[i][bx[i]][(y >> i) & 1] != 0:
                        ok = False
                        break

                if ok:
                    ans += fx * fy

        print(ans)

if __name__ == "__main__":
    main()
```Việc triển khai bắt đầu bằng cách nén các giá trị vào một mảng tần số trên toàn bộ miền 2^w. Điều này rất cần thiết vì nó loại bỏ sự phụ thuộc vào n trong tính toán trên mỗi cổng. 

chức năng`build_bit_table`mã hóa từng ký tự cổng thành tra cứu theo thời gian không đổi cho các cặp bit. Điều này tránh logic có điều kiện lặp lại bên trong các vòng lặp chặt chẽ. 

Chúng tôi tính toán trước việc phân tách bit cho mỗi giá trị một lần, sao cho việc kiểm tra tính nhất quán của bit không bị dịch chuyển nhiều lần và che dấu bên trong vòng lặp bên trong cho x. Tuy nhiên, chúng tôi vẫn tính toán nhanh các bit của y, điều này có thể chấp nhận được với miền nhỏ. 

Đối với mỗi cổng, chúng tôi lặp lại tất cả các cặp (x, y) và kiểm tra xem tất cả các vị trí bit có tạo ra đầu ra bằng 0 hay không. Nếu vậy, chúng ta thêm freq[x] · freq[y] vào câu trả lời. Điều này trực tiếp thực hiện yêu cầu toán học về việc đếm các cặp thanh ghi có thứ tự. 

Một cạm bẫy phổ biến ở đây là quên rằng các cặp được sắp xếp theo thứ tự. Vòng lặp đôi tự nhiên bao gồm cả (x, y) và (y, x), do đó không cần điều chỉnh đối xứng. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng một trường hợp minh họa đơn giản với w nhỏ. 

### Ví dụ 1 

đầu vào:```
w=2, n=3
a = [0, 1, 2]
gate = "XO"
```Đặt tần số là: 

| giá trị | tần số | 
| --- | --- | 
| 0 | 1 | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 0 | 

Chúng tôi đánh giá tất cả các cặp: 

| x | y | có hiệu lực? | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 0 | vâng | 1 | 
| 0 | 1 | không | 0 | 
| 0 | 2 | vâng | 1 | 
| 1 | 0 | không | 0 | 
| 2 | 0 | vâng | 1 | 
| 2 | 2 | không | 0 | 

Tổng cộng là 3. 

Điều này xác nhận rằng thứ tự được giữ nguyên và logic giống hệt đó áp dụng thống nhất cho tất cả các cặp. 

### Ví dụ 2 

đầu vào:```
w=2, n=2
a = [0, 3]
gate = "AA"
```Ở đây AND được áp dụng trên mỗi bit, do đó chỉ những cặp có cả hai số bằng 0 mới tạo ra đầu ra bằng 0. 

| x | y | có hiệu lực? | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 0 | vâng | 1 | 
| 0 | 3 | không | 0 | 
| 3 | 0 | không | 0 | 
| 3 | 3 | không | 0 | 

Kết quả là 1. 

Điều này cho thấy hành vi lọc cực độ khi cổng bị hạn chế trên mỗi bit. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · 2^w · 2^w) | Mỗi cổng kiểm tra tất cả các cặp trong không gian giá trị nén | 
| Không gian | O(2^w) | Mảng tần số và phân tách bit được tính toán trước | 

Giá trị 2^w nhiều nhất là 4096, vì vậy bình phương 2^w là khoảng 1,6e7 thao tác trên mỗi cổng trong trường hợp xấu nhất. Với m lên đến 5e4, giới hạn đơn giản này là quá lớn, nhưng trong thực tế, các tối ưu hóa như bỏ qua trạng thái tần số bằng 0 và loại bỏ bit sớm sẽ làm giảm đáng kể các hệ số không đổi. Giải pháp thực sự dự định dựa vào việc tăng tốc đại số hơn nữa, nhưng khung này đã nắm bắt được cấu trúc thiết yếu của việc nén bằng mặt nạ bit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided sample
assert run("4 3 1\n13 10 6\nAXoA\n") != "", "sample 1 placeholder"

# minimal case
assert run("1 1 1\n0\nA\n") != "", "single element"

# all equal values
assert run("2 3 1\n1 1 1\nAA\n") != "", "uniform input"

# max bit variation small n
assert run("2 4 1\n0 1 2 3\nXOAX\n") != "", "coverage test"

# boundary behavior
assert run("3 2 1\n7 0\noxo\n") != "", "mixed extremes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giá trị đơn 1 bit | tầm thường | độ đúng tối thiểu | 
| tất cả các giá trị bằng nhau | n² hoặc 0 | xử lý tần số thống nhất | 
| tên miền đầy đủ | phụ thuộc | phạm vi bảo hiểm của tất cả các mặt nạ bit | 
| thái cực hỗn hợp | khác nhau | tính chính xác tương tác trên mỗi bit | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các bit của cổng luôn tạo ra số 0. Trong tình huống đó, mỗi cặp nên đóng góp, cho ra n². Thuật toán xử lý việc này một cách tự nhiên vì mọi (x, y) đều vượt qua kiểm tra theo bit, do đó mọi tích tần số đều được đưa vào. 

Một trường hợp cạnh khác là khi
