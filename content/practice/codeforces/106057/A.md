---
title: "CF 106057A - Cây giảm dần"
description: "Chúng ta đang đếm một họ cây có gốc rất cụ thể trên các đỉnh được dán nhãn từ 1 đến n. Cây được bắt nguồn từ nút 1 và các nhãn hoạt động theo cách đơn điệu dọc theo bất kỳ đường dẫn từ gốc đến nút nào: bất cứ khi nào chúng ta di chuyển ra khỏi gốc, các nhãn phải tăng nghiêm ngặt."
date: "2026-06-22T18:41:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106057
codeforces_index: "A"
codeforces_contest_name: "CoU CSE Fest 2025 - Inter University Programming Contest (Divisional)"
rating: 0
weight: 106057
solve_time_s: 47
verified: true
draft: false
---

[CF 106057A - Cây cối suy giảm](https://codeforces.com/problemset/problem/106057/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang đếm một họ cây có gốc rất cụ thể trên các đỉnh được dán nhãn từ 1 đến n. Cây được bắt nguồn từ nút 1 và các nhãn hoạt động theo cách đơn điệu dọc theo bất kỳ đường dẫn từ gốc đến nút nào: bất cứ khi nào chúng ta di chuyển ra khỏi gốc, các nhãn phải tăng nghiêm ngặt. 

Đầu vào cung cấp nhiều giá trị của n trong các trường hợp thử nghiệm và với mỗi trường hợp, chúng ta cần tính toán có bao nhiêu cây có gốc hợp lệ tồn tại trong ràng buộc này. Mỗi cấu trúc hợp lệ là một cây trên n nút được gắn nhãn, nhưng không phải tất cả các cây được gắn nhãn đều được phép, chỉ những cây có nhãn không bao giờ giảm khi chúng ta di chuyển ra khỏi gốc. 

Ràng buộc cấu trúc chính là cục bộ nhưng hạn chế toàn cầu: mọi nút ngoại trừ nút gốc phải có nút cha có nhãn nhỏ hơn. Điều này ngay lập tức buộc phải có tính định hướng trong cách hình thành các cạnh và đó là điều thúc đẩy toàn bộ đối số đếm. 

Các ràng buộc đủ nhỏ để tăng trưởng giai thừa không phải là vấn đề, do đó, bất kỳ giải pháp nào liên quan đến O(n) hoặc thậm chí O(n log n) cho mỗi trường hợp thử nghiệm đều nhanh chóng. Điều này cũng báo hiệu rằng câu trả lời có thể có dạng đóng thay vì yêu cầu phép liệt kê tổ hợp hoặc DP trên cây. 

Một trường hợp thất bại khó phát hiện sẽ xuất hiện nếu người ta cố gắng coi trường hợp này là “đếm tất cả các cây có gốc” và sau đó áp dụng hiệu chỉnh. Ví dụ, với n = 3, tổng số cây có nhãn gốc đã là không cần thiết, nhưng nhiều cây trong số đó vi phạm điều kiện tăng dần. Nếu chúng ta bỏ qua ràng buộc thứ tự và sử dụng công thức đếm tổng quát của Cayley, chúng ta sẽ đếm quá nhiều. Ràng buộc về cấu trúc là toàn bộ vấn đề, không phải là một hạn chế nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các cây được gắn nhãn trên n nút, root chúng ở mức 1 và kiểm tra xem mọi đường dẫn từ gốc đến nút có tăng lên hay không. Ngay cả việc tạo ra tất cả các cây cũng không khả thi vì số lượng cây được gắn nhãn trên n nút là n^(n−2) và với n = 10 thì con số này đã lên tới hàng triệu, trong khi với n lớn hơn, nó bùng nổ vượt xa mọi giới hạn. 

Ngay cả khi chúng ta tránh việc liệt kê đầy đủ và thay vào đó cố gắng xây dựng cây tăng dần trong khi thực thi ràng buộc, chúng ta vẫn cần khám phá nhiều phép gán cha theo cấp số nhân và xác thực tính nhất quán toàn cục. 

Quan sát quan trọng là ràng buộc này hoàn toàn mang tính cục bộ khi được xem xét từ góc độ lựa chọn cha mẹ. Mọi nút i > 1 phải có nút cha có nhãn nhỏ hơn. Sau khi chúng tôi sửa lỗi cha cho mỗi nút, cấu trúc kết quả sẽ tự động là một cây gốc hợp lệ: không có chu kỳ vì các cạnh luôn đi từ nhãn lớn hơn đến nhãn nhỏ hơn và khả năng kết nối được đảm bảo vì việc lặp đi lặp lại của con trỏ cha cuối cùng sẽ đến nút 1. 

Điều này chuyển đổi vấn đề từ việc đếm số cây sang việc đếm các lựa chọn độc lập của cha mẹ dưới một hạn chế đơn giản. 

Đối với nút i, nút cha có thể là bất kỳ nút nào trong {1, 2, ..., i−1}. Các lựa chọn này độc lập giữa các nút, do đó tổng số công trình là tích trực tiếp trên tất cả i > 1. 

Tích đó trở thành (1) × (2) × ... × (n−1), chính xác là (n−1)!. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Cố định nút gốc là nút 1, vì nó được cho trước và phải là nút gốc của tất cả các đường dẫn. Điều này neo giữ cấu trúc và xác định hướng của tất cả các mối quan hệ cha mẹ. 
2. Với mỗi nút i từ 2 đến n, chọn nút cha của nó. Lựa chọn hợp lệ duy nhất là các nút có nhãn nhỏ hơn, bởi vì bất kỳ cạnh nào đến nhãn lớn hơn sẽ ngay lập tức vi phạm điều kiện tăng dọc theo đường đi từ gốc. 
3. Đếm xem có bao nhiêu lựa chọn hợp lệ cho nút i. Có chính xác i−1 cha mẹ có thể có. 
4. Nhân các lựa chọn độc lập này trên tất cả các nút. Vì lựa chọn gốc của mỗi nút không hạn chế lựa chọn của các nút khác ngoài điều kiện nhãn cục bộ nên tổng số là tích của tất cả (i−1). 
5. Tính toán tích số này lặp đi lặp lại dưới dạng giá trị giai thừa tích lũy lên tới n−1 cho mỗi trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Việc xây dựng xác định một cấu trúc có hướng trong đó mỗi nút trỏ đến chính xác một nút cha có nhãn nhỏ hơn. Điều này đảm bảo tính không tuần hoàn vì các nhãn giảm nghiêm ngặt khi đi theo các cạnh gốc, do đó các chu kỳ là không thể. Việc lặp đi lặp lại các con trỏ gốc cuối cùng phải đến được nút 1, điều này đảm bảo khả năng kết nối. 

Do đó, mỗi phép gán cha hợp lệ sẽ tạo ra chính xác một cây có gốc hợp lệ và mỗi cây hợp lệ tương ứng với chính xác một phép gán như vậy. Sự song hành giữa cây và sự lựa chọn của cha mẹ độc lập làm cho việc tính toán trở nên chính xác hơn là đánh giá quá cao. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ns = [int(input()) for _ in range(t)]
    
    max_n = max(ns)
    
    fact = [1] * (max_n + 1)
    for i in range(2, max_n + 1):
        fact[i] = fact[i - 1] * i
    
    out = []
    for n in ns:
        if n <= 1:
            out.append("1")
        else:
            out.append(str(fact[n - 1]))
    
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai tính toán trước các giai thừa lên đến n tối đa trong tất cả các trường hợp thử nghiệm để mỗi truy vấn có thể được trả lời trong O(1). Mảng Fact[i] lưu trữ i!, nên câu trả lời cho n là Fact[n−1]. 

Một sai lầm thường gặp ở đây là quên rằng tích bắt đầu từ i = 2, không phải i = 1. Điều đó làm thay đổi chỉ số giai thừa đúng một, do đó ánh xạ đúng là (n−1)!, không phải n!. 

Trường hợp đặc biệt n = 1 được xử lý riêng vì bảng giai thừa sẽ trả về Fact[0] = 1, vốn đã đúng nhưng nhánh rõ ràng làm cho logic rõ ràng hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 3 

Chúng tôi tính toán các bài tập cha mẹ hợp lệ. 

| Nút tôi | Lựa chọn của phụ huynh | Đóng góp | 
| --- | --- | --- | 
| 2 | {1} | 1 | 
| 3 | {1, 2} | 2 | 

Tổng cộng = 1 × 2 = 2 

Điều này xác nhận công thức (3−1)! = 2. 

Dấu vết cho thấy nút 3 có tính linh hoạt cao hơn nút 2 và cấu trúc nhân phát sinh một cách tự nhiên từ các lựa chọn độc lập. 

### Ví dụ 2 

đầu vào: 

n = 4 

| Nút tôi | Lựa chọn của phụ huynh | Đóng góp | 
| --- | --- | --- | 
| 2 | {1} | 1 | 
| 3 | {1, 2} | 2 | 
| 4 | {1, 2, 3} | 3 | 

Tổng cộng = 1 × 2 × 3 = 6 

Điều này chứng tỏ số lượng lựa chọn tăng tuyến tính với i như thế nào và cấu trúc sản phẩm khớp chính xác với mức tăng trưởng giai thừa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(max n) trên mỗi bộ kiểm tra | tính toán trước giai thừa chiếm ưu thế một lần | 
| Không gian | O(tối đa n) | lưu trữ các giá trị giai thừa | 

Các ràng buộc cho phép tính toán giai thừa đơn giản mà không cần các thủ thuật số học hoặc tối ưu hóa mô-đun. Ngay cả đối với n lớn nhất có thể trong các ràng buộc kiểu Codeforces điển hình cho vấn đề này, việc tính toán vẫn không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    t = int(input())
    ns = [int(input()) for _ in range(t)]
    
    max_n = max(ns)
    fact = [1] * (max_n + 1)
    for i in range(2, max_n + 1):
        fact[i] = fact[i - 1] * i
    
    res = []
    for n in ns:
        res.append(str(fact[n - 1] if n > 0 else 1))
    
    return "\n".join(res)

# sample-style checks
assert run("1\n1\n") == "1"
assert run("1\n2\n") == "1"
assert run("1\n3\n") == "2"

# custom cases
assert run("1\n4\n") == "6"
assert run("1\n5\n") == "24"
assert run("3\n1\n2\n3\n") == "1\n1\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 1 | 1 | trường hợp cơ sở | 
| n = 2 | 1 | cây không tầm thường nhỏ nhất | 
| nhiều truy vấn | 1 1 2 | tính nhất quán giữa các trường hợp thử nghiệm | 

## Vỏ cạnh 

Với n = 1, chỉ có một đỉnh duy nhất cũng là gốc nên có đúng một cây hợp lệ. Thuật toán sử dụng Fact[n−1] trở thành Fact[0], cho kết quả chính xác là 1. 

Với n = 2, nút 2 chỉ có thể chọn nút cha 1. Thuật toán tính toán Fact[1] = 1, khớp với cây duy nhất có thể. 

Đối với n lớn hơn, hãy xem xét n = 3. Thuật toán tính toán Fact[2] = 2. Điều này tương ứng chính xác với hai cấu hình gốc hợp lệ: nút 2 phải gắn với 1, trong khi nút 3 có thể gắn với 1 hoặc 2. Việc triển khai nắm bắt điều này một cách tự nhiên mà không cần viết hoa đặc biệt ngoài lập chỉ mục giai thừa.
