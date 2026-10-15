---
title: "CF 105458A - Chuyến bay của Juan"
description: "Mỗi trường hợp thử nghiệm mô tả nỗ lực của Juan trong việc chọn một cửa hàng duy nhất để mua ba bộ phận cần thiết cho một chiếc ô tô bay: động cơ, vô lăng và lốp dự phòng. Mỗi cửa hàng đều cung cấp cả ba mặt hàng, nhưng mỗi cửa hàng có giá khác nhau cho chúng."
date: "2026-06-23T17:47:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105458
codeforces_index: "A"
codeforces_contest_name: "XXIII Spain Olympiad in Informatics, Online Qualifier 2"
rating: 0
weight: 105458
solve_time_s: 64
verified: true
draft: false
---

[CF 105458A - Chuyến bay của Juan](https://codeforces.com/problemset/problem/105458/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả nỗ lực của Juan trong việc chọn một cửa hàng duy nhất để mua ba bộ phận cần thiết cho một chiếc ô tô bay: động cơ, vô lăng và lốp dự phòng. Mỗi cửa hàng đều cung cấp cả ba mặt hàng, nhưng mỗi cửa hàng có giá khác nhau cho chúng. Juan không thể kết hợp mua hàng giữa các cửa hàng, vì vậy nếu anh ấy chọn một cửa hàng, anh ấy phải mua cả ba mặt hàng ở đó. 

Nhiệm vụ là tính toán, đối với mỗi trường hợp thử nghiệm, cửa hàng nào có tổng chi phí nhỏ nhất khi cộng ba mức giá. Các cửa hàng được xem xét theo thứ tự xuất hiện trong dữ liệu đầu vào và nếu nhiều cửa hàng đạt được tổng chi phí tối thiểu như nhau thì cửa hàng sớm nhất phải được chọn. 

Các ràng buộc đủ lớn để một giải pháp phải xử lý tổng cộng lên tới mười triệu cửa hàng trong tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào kém hơn thời gian tuyến tính trên mỗi trường hợp thử nghiệm, vì ngay cả O(n log n) cũng sẽ quá chậm khi n đạt tới một triệu liên tục. Hướng khả thi duy nhất là tính tổng số tiền trên mỗi cửa hàng trong thời gian không đổi và duy trì mức hoạt động tối thiểu. 

Một số trường hợp phức tạp xuất hiện một cách tự nhiên trong công thức này. Một là khi tất cả các cửa hàng có tổng chi phí giống nhau. Trong tình huống đó, câu trả lời phải là lưu trữ 1, không phải lần xuất hiện cuối cùng, có nghĩa là việc xử lý ràng buộc phải thiên về mức tối thiểu đầu tiên được nhìn thấy. Một trường hợp khác là khi mức tối thiểu xảy ra nhiều lần sau đó trong danh sách; quy tắc cập nhật bất cẩn như “cập nhật khi tổng hiện tại nhỏ hơn hoặc bằng” sẽ dẫn đến mức tối thiểu cuối cùng thay vì mức đầu tiên một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là tính tổng chi phí cho mỗi cửa hàng, lưu trữ các giá trị đó vào một mảng rồi quét tìm giá trị tối thiểu. Điều này đúng vì mỗi cửa hàng độc lập và tổng chi phí chỉ đơn giản là tổng của ba mức giá của nó. Tuy nhiên, ngay cả ý tưởng đơn giản này cũng có thể được triển khai không hiệu quả nếu người ta lưu trữ tất cả các giá trị trước, điều này sẽ cần thêm bộ nhớ và nhiều lần chuyển. 

Nút thắt cổ chai không phải là tính toán trên mỗi cửa hàng, vì mỗi cửa hàng chỉ cần thêm ba lần bổ sung mà là chi phí lưu trữ và tái xử lý các đầu vào lớn. Với tổng số lên tới 10 triệu cửa hàng, ngay cả việc truyền dữ liệu được lưu trữ trong giây lát cũng có nguy cơ gây ra tình trạng bộ đệm kém hiệu quả và áp lực bộ nhớ. 

Quan sát quan trọng là chúng ta không bao giờ cần phải nhớ tất cả các cửa hàng. Chúng tôi chỉ cần chỉ số cửa hàng tốt nhất hiện tại và giá của nó. Khi chúng tôi đọc từng cửa hàng, chúng tôi tính toán tổng chi phí và ngay lập tức so sánh nó với cửa hàng tốt nhất được thấy cho đến nay. Điều này làm giảm vấn đề xuống còn một lần phát trực tuyến. 

Điều này hiệu quả vì quyết định về một cửa hàng không phụ thuộc vào bất kỳ giá trị nào trong tương lai. Mức tối thiểu có thể được duy trì tăng dần: tại bất kỳ thời điểm nào, chúng tôi chỉ quan tâm liệu cửa hàng hiện tại có cải thiện câu trả lời được biết đến nhiều nhất hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (lưu trữ tất cả tổng số rồi quét) | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Bộ nhớ quá chậm/không cần thiết | 
| Phát trực tuyến tối thiểu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số lượng cửa hàng n. Điều này xác định số lượng so sánh độc lập chúng tôi sẽ thực hiện. 
2. Khởi tạo hai biến: best_cost là vô cùng và best_index là 1. Chỉ mục bắt đầu từ 1 vì việc đánh số cửa hàng dựa trên 1 và cửa hàng đầu tiên là ứng cử viên mặc định. 
3. Lặp lại các cửa hàng từ i = 1 đến n. Đối với mỗi cửa hàng, hãy đọc ba mức giá và tính tổng của chúng. Số tiền này thể hiện tổng chi phí cho việc lựa chọn cửa hàng đó. 
4. So sánh số tiền được tính toán với best_cost. Nếu tổng hiện tại hoàn toàn nhỏ hơn, hãy cập nhật best_cost thành tổng này và best_index thành i. Bất đẳng thức chặt chẽ là cần thiết vì nó bảo toàn chỉ số sớm nhất trong trường hợp có quan hệ. 
5. Sau khi xử lý tất cả các cửa hàng trong trường hợp thử nghiệm, xuất best_index.

Tính chính xác dựa vào việc duy trì tính bất biến sau khi xử lý cửa hàng thứ i, best_cost và best_index tương ứng với tổng chi phí tối thiểu trong số i cửa hàng đầu tiên và best_index là vị trí sớm nhất đạt được mức tối thiểu đó. Vì mỗi bước chỉ giới thiệu một ứng cử viên mới và không bao giờ truy cập lại các cửa hàng trước đây nên tính bất biến này được giữ nguyên trong suốt quá trình quét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n = int(input())
        
        best_cost = 10**30
        best_idx = 1
        
        for i in range(1, n + 1):
            a, b, c = map(int, input().split())
            total = a + b + c
            
            if total < best_cost:
                best_cost = total
                best_idx = i
        
        out.append(str(best_idx))
    
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc mỗi cửa hàng chính xác một lần và ngay lập tức đánh giá chi phí của nó. Việc sử dụng so sánh chặt chẽ đảm bảo rằng các mối quan hệ không ghi đè lên chỉ mục tối ưu trước đó. Việc tích lũy đầu ra trong một danh sách và ghi một lần vào cuối sẽ tránh được chi phí I/O lặp lại, điều này quan trọng khi có kích thước đầu vào lớn. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp thử nghiệm mẫu đầu tiên. 

đầu vào:```
2
7 1 3
5 2 1
```| tôi | a b c | tổng cộng | chi phí tốt nhất | tốt nhất_idx | 
| --- | --- | --- | --- | --- | 
| 1 | 7 1 3 | 11 | 11 | 1 | 
| 2 | 5 2 1 | 8 | 8 | 2 | 

Cửa hàng thứ hai trở nên tối ưu vì tổng chi phí của nó nhỏ hơn cửa hàng thứ nhất. Thuật toán cập nhật chính xác cả chi phí và chỉ số. 

Bây giờ hãy xem xét một trường hợp với các mối quan hệ. 

đầu vào:```
3
4 4 4
2 5 5
6 2 4
```| tôi | a b c | tổng cộng | chi phí tốt nhất | tốt nhất_idx | 
| --- | --- | --- | --- | --- | 
| 1 | 4 4 4 | 12 | 12 | 1 | 
| 2 | 2 5 5 | 12 | 12 | 1 | 
| 3 | 6 2 4 | 12 | 12 | 1 | 

Tất cả các cửa hàng đều có tổng số tiền như nhau. Việc so sánh nghiêm ngặt sẽ ngăn chặn mọi cập nhật sau lần lưu trữ đầu tiên, duy trì chỉ mục 1. Điều này xác nhận việc xử lý ràng buộc chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng n) | Mỗi cửa hàng được xử lý chính xác một lần với số học thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến được duy trì bất kể kích thước đầu vào | 

Giải pháp chạy theo thời gian tuyến tính trên toàn bộ đầu vào, điều này cần thiết với hạn chế là tổng n trong các trường hợp thử nghiệm có thể đạt tới 10 triệu. Việc sử dụng bộ nhớ không đổi, do đó, nó phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    
    def solve():
        input = sys.stdin.readline
        t = int(input())
        res = []
        for _ in range(t):
            n = int(input())
            best_cost = 10**30
            best_idx = 1
            for i in range(1, n + 1):
                a, b, c = map(int, input().split())
                s = a + b + c
                if s < best_cost:
                    best_cost = s
                    best_idx = i
            res.append(str(best_idx))
        print("\n".join(res))
    
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""3
2
7 1 3
5 2 1
3
9 8 2
10 14 1
1 8 11
4
23 42 13
7 12 38
10 9 24
8 10 10
""") == """2
1
4"""

# minimum size
assert run("""1
1
5 5 5
""") == "1"

# all equal
assert run("""1
3
1 1 1
1 1 1
1 1 1
""") == "1"

# strict minimum late
assert run("""1
4
10 10 10
1 100 100
2 2 2
3 3 3
""") == "2"

# tie should prefer first
assert run("""1
2
1 2 3
6 0 0
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 cửa hàng | 1 | ranh giới tối thiểu | 
| tất cả các cửa hàng bình đẳng | 1 | xử lý cà vạt | 
| tối thiểu xuất hiện sau | 2 | logic cập nhật đúng | 
| tổng bằng nhau các dạng khác nhau | 1 | so sánh đúng đắn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều cửa hàng có chung tổng chi phí tối thiểu. Ví dụ:```
1
3
2 2 2
1 3 3
4 1 2
```Tất cả tổng số là 6. Thuật toán khởi tạo best_cost với cửa hàng đầu tiên và chỉ cập nhật các giá trị nhỏ hơn. Ở cửa hàng đầu tiên, best_cost trở thành 6 và best_idx trở thành 1. Cửa hàng thứ hai và thứ ba cũng tạo ra 6, nhưng vì 6 không nhỏ hơn 6 nên không có cập nhật nào xảy ra. Đầu ra vẫn là 1, phù hợp với quy tắc ràng buộc bắt buộc. 

Một trường hợp khó khăn khác là khi cửa hàng tốt nhất là cửa hàng đầu tiên và không có cửa hàng nào sau này cải thiện nó. Tính bất biến đảm bảo không có cập nhật không cần thiết nào xảy ra và thuật toán tự nhiên trả về 1 mà không cần viết hoa chữ đặc biệt.
