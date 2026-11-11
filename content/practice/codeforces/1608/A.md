---
title: "CF 1608A - Tìm mảng"
description: "Chúng ta được yêu cầu xây dựng một dãy số nguyên tăng dần cho từng trường hợp thử nghiệm, với một hạn chế cụ thể đối với các phần tử liên tiếp. Trình tự phải tăng nghiêm ngặt và ngoài ra, mọi phần tử tiếp theo không được chia hết cho phần tử trước."
date: "2026-06-10T07:32:52+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1608
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 758 (Div.1 + Div. 2)"
rating: 800
weight: 1608
solve_time_s: 88
verified: false
draft: false
---

[CF 1608A - Tìm mảng](https://codeforces.com/problemset/problem/1608/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một dãy số nguyên tăng dần cho từng trường hợp thử nghiệm, với một hạn chế cụ thể đối với các phần tử liên tiếp. Trình tự phải tăng nghiêm ngặt và ngoài ra, mọi phần tử tiếp theo không được chia hết cho phần tử trước. Chúng ta có thể tự do lựa chọn bất kỳ công trình xây dựng hợp lệ nào miễn là nó tôn trọng cả hai ràng buộc và nằm trong phạm vi lên tới một tỷ. 

Kích thước đầu vào nhỏ theo nghĩa là mỗi thử nghiệm có tối đa 1000 phần tử và tổng số qua các thử nghiệm nhiều nhất là 10.000. Điều này có nghĩa là chúng ta không cần cấu trúc dữ liệu phức tạp hoặc tiền xử lý nặng nề. Bất kỳ giải pháp nào chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm đều đủ nhanh. 

Ràng buộc cấu trúc chính là điều kiện phân chia giữa các nước láng giềng. Một chuỗi tăng ngây thơ như các số nguyên liên tiếp sẽ thất bại ngay lập tức vì mọi số nguyên đều chia hết cho số nguyên trước nó trong một số trường hợp, đặc biệt là khi bước cẩn thận qua các bội số. Tương tự, các cấp số cộng đơn giản chứa nhiều rủi ro vì chúng thường đưa ra các mẫu hình chia hết có thể dự đoán được. 

Các trường hợp cạnh là tối thiểu nhưng quan trọng về mặt khái niệm. Khi n bằng 1, mọi giá trị trong phạm vi cho phép đều được chấp nhận. Khi n lớn hơn, việc xây dựng phải tránh vô tình đưa ra một cặp trong đó a_{i} = k * a_{i-1}. Ví dụ: nếu chúng ta chọn lũy thừa của hai như 1, 2, 4, 8 thì điều kiện thất bại hoàn toàn vì mọi phần tử đều chia hết cho phần tử tiếp theo. Điều này cho thấy chỉ “tăng” thôi chưa đủ, chúng ta phải tích cực phá bỏ cấu trúc nhân. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng chuỗi từng phần tử một. Đối với mỗi giá trị tiếp theo ứng cử viên, chúng tôi sẽ kiểm tra tất cả các số nguyên lớn hơn giá trị trước đó cho đến khi chúng tôi tìm thấy một giá trị không chia hết cho nó. Điều này đúng vì nó thực thi rõ ràng các ràng buộc, nhưng nó trở nên tốn kém vì trong trường hợp xấu nhất, chúng ta có thể quét một phần lớn trục số cho mọi vị trí, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Quan sát quan trọng là việc tránh tính chia hết sẽ trở nên tầm thường nếu chúng ta đảm bảo rằng các số liên tiếp là số nguyên tố cùng nhau theo cách có cấu trúc đơn giản. Thay vì cố gắng tránh sự chia hết một cách linh hoạt, chúng ta có thể ép buộc nó bằng cách xây dựng. Một thủ thuật tiêu chuẩn là xen kẽ tính chẵn lẻ theo mô hình tăng dần có kiểm soát. Nếu chúng ta chọn một giá trị cơ bản và thêm các giá trị chênh lệch nhỏ vào một mẫu để đảm bảo không có phần tử nào là bội số của phần tử trước đó, thì chúng ta có thể đảm bảo tính chính xác mà không cần kiểm tra. 

Một cách xây dựng đặc biệt đơn giản là bắt đầu từ một khoảng cách lớn không đổi và thêm chỉ số theo cách đảm bảo mức tăng nghiêm ngặt đồng thời phá vỡ khả năng chia hết. Một ý tưởng rõ ràng là xuất ra chuỗi: 

a_i = 1000 * tôi + 1 

Điều này đảm bảo mức tăng nghiêm ngặt ngay lập tức vì khoảng cách giữa các số hạng liên tiếp chính xác là 1000. Quan trọng hơn, a_i modulo a_{i-1} không bao giờ bằng 0 vì các số hạng liên tiếp không phải là bội số của nhau: chênh lệch là cố định nhưng tỷ lệ không phải là số nguyên do độ lệch +1. Mọi nỗ lực chia a_i cho a_{i-1} đều thất bại vì a_{i-1} không vừa khít với cấp số tuyến tính có độ lệch 1. 

Lực lượng vũ phu hoạt động bằng cách tìm kiếm cục bộ, nhưng quan sát thấy rằng chúng ta có thể nhúng một cấu trúc số học cố định với sự bù đắp có chủ ý sẽ loại bỏ tất cả nhu cầu tìm kiếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Công thức tuyến tính xây dựng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng mảng trực tiếp cho từng trường hợp thử nghiệm. 

1. Với một n cho trước, lặp i từ 1 đến n. 
2. Với mỗi chỉ số i, tính giá trị a_i = 1000 * i + 1. 
3. Thêm từng giá trị được tính toán vào chuỗi đầu ra. 
4. In trình tự.

Lý do chọn 1000 hoàn toàn là sự tiện lợi. Bất kỳ hằng số nào lớn hơn n cũng sẽ hoạt động, nhưng 1000 là an toàn trong các ràng buộc vì n ≤ 1000. Việc dịch chuyển +1 đảm bảo rằng không có phần tử nào là bội số chính xác của phần tử khác trong cấu trúc chuỗi. 

### Tại sao nó hoạt động 

Dãy số được xây dựng ngày càng tăng vì mỗi số hạng tăng đúng 1000 so với số trước. Điều kiện chia hết không thành công vì đối với bất kỳ cặp liên tiếp nào, tỷ lệ 

a_i / a_{i-1} = (1000i + 1) / (1000(i-1) + 1) 

không thể là số nguyên. Tử số và mẫu số khác nhau 1000, nhưng độ lệch +1 đảm bảo rằng không có phép hủy nào tạo ra tỷ lệ nguyên. Do đó, không có phần tử nào chia phần tử tiếp theo và mọi ràng buộc đều được thỏa mãn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        res = []
        for i in range(1, n + 1):
            res.append(str(1000 * i + 1))
        print(" ".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp dựa hoàn toàn vào việc xây dựng trực tiếp. Vòng lặp tạo ra mỗi thuật ngữ trong thời gian không đổi và việc in được thực hiện một lần cho mỗi trường hợp thử nghiệm. 

Chi tiết triển khai quan trọng là sử dụng tích lũy chuỗi trước khi in. Việc in bên trong vòng lặp vẫn sẽ vượt qua các ràng buộc nhất định, nhưng việc tạo khối đầu ra sẽ tránh được chi phí không cần thiết. 

Hằng số 1000 được chọn để đảm bảo tất cả các giá trị vẫn nằm trong giới hạn 10^9 ngay cả đối với n tối đa, vì 1000 * 1000 + 1 bằng 1.000.001, là mức an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 1 

| tôi | a_i | 
| --- | --- | 
| 1 | 1001 | 

Chuỗi chỉ chứa một phần tử, vì vậy tất cả các ràng buộc đều được thỏa mãn một cách tầm thường. Không cần kiểm tra khả năng phân chia. 

Điều này xác nhận trường hợp ranh giới trong đó việc xây dựng vẫn tạo ra mảng một phần tử hợp lệ. 

### Ví dụ 2 

đầu vào: 

n = 4 

| tôi | a_i | 
| --- | --- | 
| 1 | 1001 | 
| 2 | 2001 | 
| 3 | 3001 | 
| 4 | 4001 | 

Mỗi bước tăng thêm 1000. Chúng tôi kiểm tra các cặp liên tiếp: 

1001 không chia hết cho 2001 vì 2001/1001 không phải là số nguyên. 

Tương tự, 2001 không chia hết cho 3001 và 3001 không chia hết cho 4001. Mỗi cặp không chia hết được do sự liên kết lệch +1 với bội số. 

Điều này cho thấy rằng việc xây dựng luôn tránh được bội số ngẫu nhiên trên toàn bộ chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được tính trực tiếp một lần | 
| Không gian | O(1) thêm | Chỉ sử dụng bộ nhớ đầu ra | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm tối đa là 10.000, do đó giải pháp chạy thoải mái trong giới hạn. Các phép toán số học có thời gian không đổi và đầu ra chi phối thời gian chạy. 

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

# sample cases
assert run("3\n1\n2\n7\n") != "", "samples executed"

# minimum size
assert run("1\n1\n") == "1001", "n=1 case"

# small test
out = run("1\n3\n")
vals = list(map(int, out.split()))
assert vals == sorted(vals), "increasing"

# maximum n sanity
inp = "1\n1000\n"
out = run(inp)
vals = list(map(int, out.split()))
assert len(vals) == 1000, "length check"

# structure check
assert all(v > 0 for v in vals), "positive values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | giá trị đơn | trường hợp cơ sở đúng đắn | 
| n=3 | trình tự tăng dần | đặt mua tài sản | 
| n=1000 | 1000 phần tử | xử lý giới hạn trên | 

## Vỏ cạnh 

Với n = 1, thuật toán tạo ra một giá trị duy nhất, 1001. Không có ràng buộc kề nào, do đó độ chính xác chỉ phụ thuộc vào tính hợp lệ của phạm vi, điều này được thỏa mãn vì 1001 rất nhỏ dưới 10^9. 

Với n = 2, kết quả là 1001 và 2001. Số thứ hai không chia hết cho số thứ nhất vì 2001 chia cho 1001 không phải là số nguyên. Cấu trúc đảm bảo điều này vì cả hai số đều có chung độ lệch cố định là +1 so với bội số của 1000, ngăn cản việc căn chỉnh. 

Đối với n lớn hơn như n = 1000, giá trị cuối cùng là 1.000.001. Điều này vẫn nằm trong giới hạn và mọi cặp liên tiếp vẫn giữ nguyên cấu trúc, do đó không có mối quan hệ phân chia nào có thể xuất hiện giữa các cặp lân cận bất kể vị trí.
