---
title: "CF 1097B - Petr và Khóa kết hợp"
description: "Chúng ta được cung cấp một chiếc khóa hình tròn hoạt động giống như một mặt số 360 độ. Bắt đầu từ số 0, chúng ta thực hiện một chuỗi các phép quay, mỗi phép quay có độ lớn cố định, nhưng chúng ta có thể tự do chọn hướng của nó: theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ."
date: "2026-06-13T05:55:32+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp"]
categories: ["algorithms"]
codeforces_contest: 1097
codeforces_index: "B"
codeforces_contest_name: "Hello 2019"
rating: 1200
weight: 1097
solve_time_s: 237
verified: true
draft: false
---

[CF 1097B - Petr và Khóa kết hợp](https://codeforces.com/problemset/problem/1097/B) 

**Đánh giá:** 1200 
**Tags:** bitmasks, vũ phu, dp 
**Thời gian giải:** 3 phút 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chiếc khóa hình tròn hoạt động giống như một mặt số 360 độ. Bắt đầu từ số 0, chúng ta thực hiện một chuỗi các phép quay, mỗi phép quay có độ lớn cố định, nhưng chúng ta có thể tự do chọn hướng của nó: theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. Theo chiều kim đồng hồ có thể được coi là thêm góc, trong khi ngược chiều kim đồng hồ sẽ trừ nó. 

Sau khi áp dụng tất cả các phép quay theo thứ tự, con trỏ kết thúc ở một góc cuối cùng modulo 360. Nhiệm vụ là xác định xem có tồn tại một lựa chọn hướng cho tất cả các phép quay sao cho vị trí cuối cùng trở về chính xác bằng 0 hay không. 

Kích thước đầu vào rất nhỏ, tối đa 15 góc quay và mỗi góc quay được giới hạn 180 độ. Điều này ngay lập tức gợi ý rằng việc tìm kiếm theo cấp số nhân trên tất cả các lựa chọn hướng là khả thi vì tổng số phép gán dấu nhiều nhất là 2^15, tức là khoảng 32 nghìn cấu hình. Ngay cả với việc đánh giá đơn giản cho mỗi cấu hình, điều này vẫn nằm trong giới hạn. 

Một điểm tinh tế là vấn đề hoàn toàn là về tổng ròng modulo 360. Không có ràng buộc trung gian, do đó thứ tự là cố định và chỉ có dấu hiệu quan trọng. 

Các trường hợp cạnh chủ yếu có kích thước n nhỏ. Nếu n = 1, chỉ tồn tại một vòng quay và cách duy nhất để kết thúc bằng 0 là nếu vòng quay đó là 0 hoặc 360, nhưng vì các giá trị ít nhất là 1 nên câu trả lời luôn là KHÔNG. Một sự hiểu lầm ngây thơ có thể cố gắng giảm modulo 360 ngay lập tức cho mỗi bước, nhưng điều đó không ảnh hưởng đến tính chính xác vì chúng ta chỉ quan tâm đến tổng cuối cùng. 

Một tình huống khó khăn khác là khi tổng vượt quá 360 nhiều lần. Ví dụ: các phép quay như 180, 180, 180 vẫn có thể hoạt động vì các hướng có thể bị hủy để tạo thành bội số của 360. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để nghĩ về vấn đề này là thử mọi cách có thể để gán hướng cho mỗi vòng quay. Đối với mỗi góc, chúng tôi cộng hoặc trừ nó, sau đó kiểm tra xem tổng kết quả có chia hết cho 360 hay không. Cách tiếp cận bạo lực này sẽ thử tất cả các phép gán dấu 2^n. Điều này đúng vì nó liệt kê rõ ràng mọi cấu hình hợp lệ. 

Chi phí của phương pháp này tăng theo cấp số nhân. Với n = 15, chúng tôi đánh giá tối đa 32768 cấu hình và với mỗi cấu hình, chúng tôi tính tổng theo O(n), đưa ra khoảng 5×10^5 thao tác. Điều này đã ổn trong Python nhưng chúng ta có thể đơn giản hóa hơn nữa. 

Quan sát quan trọng là chúng ta không cần bất kỳ sự tối ưu hóa nào ngoài sức mạnh vũ phu, bởi vì ràng buộc đủ nhỏ. Vấn đề về cơ bản là một biến thể tổng tập hợp con trong đó mỗi phần tử có thể dương hoặc âm. Điều này trực tiếp ánh xạ tới phép liệt kê bitmask. 

Vì vậy, giải pháp tối ưu chỉ đơn giản là lặp lại tất cả các mặt nạ từ 0 đến (1 << n) - 1, diễn giải từng bit dưới dạng lựa chọn dấu, tính tổng kết quả và kiểm tra xem nó có bằng bội số của 360 hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu (bitmask) | O(n · 2^n) | O(1) | Đã chấp nhận | 
| Tối ưu (giống nhau) | O(n · 2^n) | O(1) | Đã chấp nhận | 

Trong bài toán này, vũ lực đã là tối ưu do có những hạn chế. 

## Hướng dẫn thuật toán

1. Đọc danh sách các góc. Mỗi góc đại diện cho một sự đóng góp cố định mà hướng của nó chưa được quyết định. 
2. Lặp lại tất cả các tập hợp con có thể có của phép gán dấu bằng cách sử dụng mặt nạ bit từ 0 đến (2^n - 1). Mỗi bit cho biết chúng ta xoay theo chiều kim đồng hồ hay ngược chiều kim đồng hồ cho bước đó. Điều này mã hóa tất cả các đường dẫn quyết định hợp lệ có thể có. 
3. Đối với mỗi mặt nạ bit, hãy khởi tạo tổng chạy về 0. Tổng này thể hiện độ dịch chuyển góc thực sau khi áp dụng các hướng đã chọn. 
4. Với mỗi chỉ mục i trong danh sách xoay, hãy kiểm tra bit thứ i của mặt nạ. Nếu nó được đặt, hãy thêm a_i vào tổng, nếu không thì trừ a_i. Điều này mô hình hóa hai hướng có thể có cho mỗi vòng quay. 
5. Sau khi xử lý tất cả các phép quay cho một mặt nạ nhất định, hãy tính tổng modulo 360. Nếu nó bằng 0 thì tồn tại một cấu hình hợp lệ và chúng ta có thể dừng ngay lập tức. 
6. Nếu không có mặt nạ nào tạo ra kết quả modulo bằng 0, hãy kết luận rằng không có chuỗi hướng nào đưa con trỏ về vị trí bắt đầu. 

### Tại sao nó hoạt động 

Mỗi chuỗi hướng dẫn hợp lệ tương ứng duy nhất với một mặt nạ bit. Việc chuyển đổi từ lựa chọn hướng sang +a_i hoặc -a_i sẽ bảo toàn góc cuối cùng chính xác. Bởi vì chúng tôi liệt kê tất cả 2^n khả năng nên chúng tôi bao gồm mọi kết quả thực tế có thể xảy ra. Điều kiện modulo thể hiện tính chất hình tròn của khóa, đảm bảo rằng mọi bội số của 360 độ đều tương đương với 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [int(input()) for _ in range(n)]
    
    for mask in range(1 << n):
        total = 0
        for i in range(n):
            if mask & (1 << i):
                total += a[i]
            else:
                total -= a[i]
        if total % 360 == 0:
            print("YES")
            return
    
    print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo phép liệt kê bitmask được mô tả trước đó. Mỗi mặt nạ thể hiện sự phân công đầy đủ các hướng theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. Vòng lặp bên trong tính toán chuyển vị kết quả. Kiểm tra modulo bao quanh mặt số tròn. 

Một sai lầm phổ biến là quên rằng phép trừ cũng có giá trị như phép cộng; cả hai phải được xem xét một cách đối xứng. Một cạm bẫy tiềm ẩn khác là cố gắng giảm các giá trị trung gian modulo 360 quá sớm, điều này an toàn về mặt toán học nhưng không cần thiết và có thể che khuất lý luận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
10
20
30
```Chúng tôi kiểm tra các bài tập ký hiệu khác nhau. 

| Mặt nạ | Biểu hiện | Tổng hợp | 
| --- | --- | --- | 
| 000 | -10 -20 -30 | -60 | 
| 001 | -10 -20 +30 | 0 | 
| 010 | -10 +20 -30 | -20 | 
| 011 | -10 +20 +30 | 40 | 
| 100 | +10 -20 -30 | -40 | 
| 101 | +10 -20 +30 | 20 | 
| 110 | +10 +20 -30 | 0 | 
| 111 | +10 +20 +30 | 60 | 

Chúng tôi quan sát thấy các mặt nạ hợp lệ như 001 và 110 tạo ra tổng chia hết cho 360 (thực tế là bằng 0). Điều này khẳng định tính khả thi. 

### Ví dụ 2 

đầu vào:```
1
1
```| Mặt nạ | Biểu hiện | Tổng hợp | 
| --- | --- | --- | 
| 0 | -1 | -1 | 
| 1 | +1 | 1 | 

Không có cấu hình nào mang lại bội số của 360, vì vậy câu trả lời là KHÔNG. 

Điều này cho thấy trường hợp cạnh trong đó một phép quay khác 0 không thể được cân bằng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 2^n) | Mỗi mặt nạ trong số 2^n đánh giá n phép quay | 
| Không gian | O(1) | Chỉ sử dụng bộ lưu trữ và bộ đếm đầu vào | 

Với n ≤ 15, số phép toán tối đa là khoảng 15 × 32768, tốc độ này khá nhanh trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("3\n10\n20\n30\n") == "YES"

# single element impossible
assert run("1\n1\n") == "NO"

# already multiple of 360 via cancellation
assert run("2\n180\n180\n") == "YES"

# mixed case
assert run("3\n90\n90\n180\n") == "YES"

# all same small values
assert run("4\n10\n10\n10\n10\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | KHÔNG | không thể thực hiện được một bước | 
| 2 180 180 | CÓ | hủy tới 360 | 
| 4 số 10 giống hệt nhau | CÓ | cân bằng tổ hợp | 

## Vỏ cạnh 

Với n = 1, thuật toán kiểm tra cả +a và -a. Vì a nằm trong khoảng từ 1 đến 180, không bằng bội số của 360, nên không có mặt nạ nào thỏa mãn điều kiện, tạo ra NO một cách chính xác. 

Đối với trường hợp tất cả các góc đều là 180, chẳng hạn như 180 180, bảng liệt kê bao gồm (+180, -180) và (-180, +180), cả hai đều có tổng bằng 0, trả về chính xác CÓ. 

Đối với các đầu vào hỗn hợp lớn hơn như 90, 90, 90, 90, tồn tại nhiều mặt nạ có tổng bằng 360 hoặc 0 và tìm kiếm toàn diện đảm bảo tìm thấy ít nhất một mặt nạ nếu hợp lệ.
