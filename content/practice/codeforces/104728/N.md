---
title: "CF 104728N - Sự cố A+B"
description: "Mỗi đầu vào bao gồm hai chữ cái viết hoa và mỗi chữ cái đại diện cho một số trong cơ số 26 trong đó A tương ứng với 0, B tương ứng với 1, v.v. cho đến Z là 25."
date: "2026-06-29T02:53:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104728
codeforces_index: "N"
codeforces_contest_name: "Huazhong University of Science of Technology Freshmen Cup 2023"
rating: 0
weight: 104728
solve_time_s: 59
verified: true
draft: false
---

[CF 104728N - Sự cố A+B](https://codeforces.com/problemset/problem/104728/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi đầu vào bao gồm hai chữ cái viết hoa và mỗi chữ cái đại diện cho một số trong cơ sở 26 trong đó`A`tương ứng với 0,`B`đến 1, v.v. cho đến`Z`là 25. Nhiệm vụ là diễn giải mỗi ký tự dưới dạng số cơ sở 26 có một chữ số, cộng hai giá trị rồi xuất lại kết quả dưới dạng số cơ sở 26 nhưng không có số 0 đứng đầu. 

Mặc dù chuỗi đầu vào luôn có độ dài bằng một, nhưng kết quả có thể yêu cầu nhiều hơn một ký tự vì việc thêm hai chữ số cơ sở 26 có thể tạo ra giá trị mang. Ví dụ,`Z`tương ứng với 25, vì vậy`Z + B`tương ứng với`25 + 1 = 26`, trong cơ số 26 được viết là`BA`bởi vì`26 = 1 × 26 + 0`. 

Các ràng buộc là cực kỳ nhỏ vì chỉ có một cặp ký tự. Điều này có nghĩa là bất kỳ giải pháp nào chạy trong thời gian không đổi là đủ. Ngay cả khi chúng ta xem xét các trường hợp thử nghiệm lặp lại theo giả thuyết, vấn đề vẫn giảm xuống mức số học đơn giản cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh chính là hành vi mang theo ở ranh giới. Khi tổng nhỏ hơn 26, đầu ra là một chữ cái. Khi tổng từ 26 trở lên, đầu ra sẽ có hai chữ cái. Một sai lầm ngây thơ là coi đầu ra là một ký tự đơn sau phép cộng, điều này sẽ không thành công đối với các đầu vào như`Z Z`hoặc`Z B`. 

Trường hợp lỗi ví dụ: đầu vào`Z Z`cho`25 + 25 = 50`, sẽ chuyển đổi sang cơ số 26 như`1 * 26 + 24 = BA`. Cách tiếp cận đơn giản chỉ có modulo-26 sẽ xuất ra không chính xác`Y`. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng chuyển đổi từng ký tự thành giá trị số của nó, thêm chúng và sau đó liên tục tạo chuỗi cơ sở 26 bằng cách chia lặp lại. Mặc dù điều này là quá mức cần thiết đối với phép cộng một chữ số, nhưng nó vẫn đúng: tính tổng số nguyên, sau đó liên tục lấy modulo 26 để trích xuất các chữ số. Trong cài đặt chung có chuỗi dài hơn, điều này sẽ lấy O(k) trong đó k là số chữ số trong kết quả. 

Tuy nhiên, vấn đề này chỉ liên quan đến hai chữ số đơn, vì vậy chúng ta không bao giờ cần lặp lại nhiều chữ số ngoại trừ nhiều nhất là hai ký tự đầu ra. Cấu trúc chỉ đơn giản là phép cộng hai chữ số cơ số 26 có thể mang theo. Quan sát quan trọng là bất kỳ tổng nào có hai chữ số trong`[0, 25]`nằm ở`[0, 50]`, phù hợp với tối đa hai chữ số cơ sở 26. Điều này làm giảm vấn đề tính thương và phần dư của phép chia cho 26 đúng một lần. 

Vì vậy, thay vì coi nó là chuyển đổi cơ số chung, chúng tôi tính trực tiếp chữ số cao là`sum // 26`và chữ số thấp như`sum % 26`, sau đó ánh xạ cả hai trở lại ký tự. Nếu chữ số cao bằng 0, chúng ta bỏ qua nó để tránh các số 0 đứng đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuyển đổi căn cứ vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Số học chữ số trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai ký tự và chuyển đổi mỗi ký tự thành giá trị số bằng cách trừ mã ASCII của`'A'`. Bản đồ này`A..Z`ĐẾN`0..25`. Bước này là cần thiết vì số học ở dạng số nguyên dễ hơn dạng ký tự. 
2. Cộng hai giá trị số nguyên để có được tổng thô trong phạm vi`[0, 50]`. Tổng này biểu thị giá trị trong cơ sở 26 trước khi chuyển đổi về ký tự. 
3. Tính chữ số cao bằng cách chia số nguyên cho 26. Điều này cho biết liệu có xảy ra lỗi hay không và chữ số đứng đầu của kết quả sẽ là bao nhiêu. 
4. Tính chữ số thấp sử dụng modulo 26. Kết quả này cho phần còn lại của biểu diễn cơ số 26. 
5. Nếu chữ số cao khác 0, hãy chuyển nó trở lại thành ký tự và xuất ra trước tiên. Điều này đảm bảo chữ số có nghĩa nhất sẽ xuất hiện đầu tiên trong chuỗi cuối cùng. 
6. Luôn chuyển đổi chữ số thấp thành ký tự và xuất ra. Đây là chữ số ít quan trọng nhất và luôn luôn hiện diện. 

### Tại sao nó hoạt động 

Thuật toán đang thực hiện chính xác việc chuyển đổi cơ số từ số thập phân sang cơ số 26 trên một số được đảm bảo tối đa là 50. Bất kỳ số nguyên nào cũng có thể được biểu diễn duy nhất dưới dạng`high × 26 + low`Ở đâu`0 ≤ low < 26`. Các phép toán chia và modulo tính toán biểu diễn này một cách trực tiếp. Vì chúng ta không bao giờ mất thông tin trong quá trình phân rã này nên việc xây dựng lại số từ hai chữ số này là chính xác. Việc bỏ qua chữ số 0 đứng đầu là hợp lệ vì các số 0 đứng đầu không được phép ở định dạng đầu ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s, t = input().split()
    
    a = ord(s) - ord('A')
    b = ord(t) - ord('A')
    
    total = a + b
    
    high = total // 26
    low = total % 26
    
    res = []
    
    if high != 0:
        res.append(chr(ord('A') + high))
    
    res.append(chr(ord('A') + low))
    
    print("".join(res))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã phân tích hai ký tự đầu vào và chuyển đổi chúng thành số nguyên trong phạm vi từ 0 đến 25. Sau đó, nó thực hiện phép cộng trong không gian số nguyên. Phép chia thương và phần dư thực hiện phân tách cơ số 26. Phần bổ sung có điều kiện cho chữ số cao đảm bảo rằng kết quả như`A`(khi tổng bằng 0) không có tiền tố không cần thiết`A`đại diện cho số 0 ở vị trí cao. 

Việc sử dụng`ord`Và`chr`đảm bảo ánh xạ trực tiếp giữa các ký tự chữ cái và chữ số, tránh mọi bảng tra cứu. Giải pháp là thời gian không đổi và bộ nhớ không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`A A`Chúng tôi chuyển đổi từng ký tự: 

| Bước | s | t | một | b | tổng cộng | cao | thấp | đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | A | A | 0 | 0 | 0 | - | - | - | 
| Thêm | - | - | - | - | 0 | 0 | 0 | A | 

Tổng bằng 0 nên cả hai chữ số đều bằng 0. Chữ số cao bị bỏ qua và chữ số thấp tương ứng với`A`. Đầu ra là`A`, xác nhận rằng số 0 được biểu diễn dưới dạng một ký tự đơn. 

### Ví dụ 2:`B C`| Bước | s | t | một | b | tổng cộng | cao | thấp | đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | B | C | 1 | 2 | 3 | - | - | - | 
| Thêm | - | - | - | - | 3 | 0 | 3 | D | 

Tổng là 3, nhỏ hơn 26 nên không có số mang. Kết quả là một chữ số, được ánh xạ tới`D`. Điều này xác nhận việc xử lý đúng các trường hợp không mang theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ chuyển đổi số học và ký tự theo thời gian không đổi | 
| Không gian | O(1) | Chỉ sử dụng một số biến cố định | 

Việc tính toán không phụ thuộc vào kích thước đầu vào và tất cả các phép toán đều là ánh xạ số học hoặc ký tự theo thời gian không đổi. Điều này dễ dàng phù hợp trong mọi giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    
    def solve():
        s, t = sys.stdin.readline().split()
        a = ord(s) - ord('A')
        b = ord(t) - ord('A')
        total = a + b
        high = total // 26
        low = total % 26
        res = []
        if high != 0:
            res.append(chr(ord('A') + high))
        res.append(chr(ord('A') + low))
        print("".join(res))
    
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("A A\n") == "A", "sample 1"
assert run("B C\n") == "D", "sample 2"
assert run("Z B\n") == "BA", "sample 3"

# custom cases
assert run("A B\n") == "B", "simple increment"
assert run("Z Z\n") == "BY", "max carry case"
assert run("Y Z\n") == "BX", "boundary carry"
assert run("A Z\n") == "Z", "no carry upper bound check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| A B | B | mức tăng không mang theo đơn giản | 
| Z Z | BỞI | lan truyền mang tối đa | 
| YZ | BX | tính đúng đắn mang theo gần ranh giới | 
| A Z | Z | ranh giới trên không mang theo | 

## Vỏ cạnh 

Trường hợp một cạnh là khi cả hai đầu vào đều`Z`. Ở đây các giá trị là 25 và 25, tổng bằng 50. Thuật toán tính toán`high = 50 // 26 = 1`Và`low = 50 % 26 = 24`, sản xuất`B`Và`Y`, vì vậy đầu ra là`BY`. Điều này xử lý chính xác tổng đầu vào lớn nhất có thể và chứng tỏ rằng số mang được mã hóa chính xác. 

Một trường hợp đặc biệt khác là khi một đầu vào`A`và cái còn lại là`Z`. Tổng là 25 nên`high = 0`Và`low = 25`, sản xuất`Z`. Thuật toán bỏ qua chữ số cao và chỉ xuất ra chữ số thấp, duy trì yêu cầu không có số 0 đứng đầu trong khi vẫn biểu thị giá trị chính xác. 

Trường hợp ranh giới cuối cùng là khi cả hai đầu vào đều`A`. Tổng bằng 0, cho`high = 0`Và`low = 0`, vì vậy đầu ra là một`A`. Điều này cho thấy việc biểu diễn số 0 được xử lý nhất quán mà không tạo ra chuỗi trống hoặc số 0 đứng đầu không hợp lệ.
