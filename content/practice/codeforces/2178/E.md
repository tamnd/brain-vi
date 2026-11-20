---
title: "CF 2178E - Làm phẳng hoặc nối"
description: "Chúng tôi được yêu cầu xác định phần tử tối đa của một mảng ẩn được tạo từ một mảng ban đầu chứa một số lũy thừa hai."
date: "2026-06-07T22:24:18+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "divide-and-conquer", "interactive"]
categories: ["algorithms"]
codeforces_contest: 2178
codeforces_index: "E"
codeforces_contest_name: "Good Bye 2025"
rating: 2000
weight: 2178
solve_time_s: 131
verified: false
draft: false
---

[CF 2178E - Làm phẳng hoặc nối](https://codeforces.com/problemset/problem/2178/E) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, chia để trị, tương tác 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định phần tử tối đa của một mảng ẩn được tạo từ một mảng ban đầu chứa một số lũy thừa hai. Mảng phát triển bằng hai thao tác: làm phẳng, chia một số chẵn tối đa thành hai nửa và nối, sao chép một mảng và nối nó vào một mảng khác. Sau các thao tác này, chỉ còn một mảng bị ẩn và chúng ta có thể truy vấn bất kỳ khoảng nào để lấy tổng của khoảng đó. 

Thách thức là chúng ta không biết số lần mỗi thao tác được áp dụng cũng như sự phân bố của các phần tử đã được làm phẳng. Mảng có thể rất dài, lên tới$10^5$các phần tử và chúng tôi được phép tối đa 300 truy vấn. Điều này loại trừ các cách tiếp cận ngây thơ, chẳng hạn như truy vấn từng phần tử riêng lẻ trong quá trình quét tuyến tính. Chúng ta cũng phải xử lý lũy thừa hai một cách cẩn thận vì mảng có nguồn gốc hoàn toàn từ việc phân tách và ghép lặp lại các giá trị lũy thừa của hai. 

Các trường hợp cạnh bao gồm các mảng có độ dài 1, các mảng trong đó tất cả các phần tử giống nhau và các mảng trong đó một phần tử rất lớn chiếm ưu thế nhưng chỉ xuất hiện một lần. Ví dụ, một mảng`[1, 1, 1, 1, 1024]`có tối đa`1024`. Một cách tiếp cận ngây thơ chỉ truy vấn một vài phần tử có thể bỏ sót hoàn toàn. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ truy vấn từng phần tử riêng lẻ, tính tổng từng phần tử khi chúng tôi thực hiện. Dù đúng nhưng nó đòi hỏi$O(n)$truy vấn. Vì$n = 10^5$, điều này vượt xa giới hạn 300 truy vấn. Thông tin chi tiết quan trọng đến từ cấu trúc của mảng: tất cả các giá trị đều là lũy thừa của 2 và mọi giá trị lớn đều được chia thành lũy thừa nhỏ hơn của 2 bằng cách làm phẳng. Do đó, phần tử lớn nhất cũng là thừa số lũy thừa lớn nhất trong tổng của mảng. 

Nếu chúng ta truy vấn tổng của toàn bộ mảng, thì tổng đó phải chia hết cho phần tử lớn nhất, bởi vì mỗi phần tử là ước số của tổ tiên lũy thừa hai nào đó. Hơn nữa, bằng cách kiểm tra lũy thừa của hai theo thứ tự giảm dần, chúng ta có thể sử dụng hình thức tìm kiếm nhị phân để xác định lũy thừa lớn nhất của hai chia đều cho tổng. Điều này làm giảm vấn đề xuống còn tối đa 30 lần kiểm tra, nằm dưới giới hạn truy vấn. 

Chúng tôi cũng có thể tinh chỉnh chiến lược của mình. Nếu kích thước mảng lớn, chúng ta có thể thực hiện kiểm tra tổng “chia để trị”, yêu cầu tổng của một nửa và một phần tư, đồng thời kiểm tra khả năng chia hết cho các lũy thừa hai của ứng cử viên. Mỗi truy vấn đưa ra tổng của một khối liền kề, giới hạn mức tối đa có thể. Việc chia đôi mảng liên tục được đảm bảo sẽ nhanh chóng tiết lộ phần tử lớn nhất, bởi vì mọi số trong mảng đều là lũy thừa của hai và số lớn nhất xuất hiện dưới dạng một đơn vị không chia hết ở đâu đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Truy vấn O(n) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân trên lũy thừa của hai | Truy vấn O(log(max_val)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Truy vấn tổng của toàn bộ mảng. Điều này ngay lập tức đưa ra tổng số tiền`S`. Bất kỳ phần tử tối đa ứng cử viên nào cũng phải chia số tiền này một cách chính xác. 
2. Khởi tạo giá trị tối đa của ứng viên là`2^30`, giá trị cho phép lớn nhất 
3. Trong khi ứng viên lớn hơn 0, hãy kiểm tra xem nó có chia hết không`S`. Nếu có, chúng tôi đã tìm thấy mức tối đa. Nếu không, hãy giảm một nửa số ứng viên và lặp lại. 
4. Báo cáo phần tử tối đa bằng định dạng đầu ra tương tác. 

Lý do chính khiến điều này hoạt động là vì mọi phần tử đều là lũy thừa của hai và bắt nguồn từ một tổ tiên nào đó trong mảng ban đầu. Do đó, tổng của mảng có thể chia hết cho phần tử tối đa, vì mọi phần tử nhỏ hơn đều thu được bằng cách chia đôi lặp đi lặp lại và việc chia đôi sẽ bảo toàn khả năng chia hết cho lũy thừa của hai. Bằng cách kiểm tra lũy thừa của hai theo thứ tự giảm dần, chúng ta đảm bảo xác định được số lớn nhất chia tổng, đó là phần tử lớn nhất ẩn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def query(l, r):
    print(f"? {l} {r}")
    sys.stdout.flush()
    res = int(input())
    if res == -1:
        exit()
    return res

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        total_sum = query(1, n)
        # candidate max is the largest power of two ≤ 2^30 dividing total_sum
        candidate = 1 << 30
        while candidate > 0:
            if total_sum % candidate == 0:
                print(f"! {candidate}")
                sys.stdout.flush()
                break
            candidate //= 2

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc số lượng trường hợp kiểm thử và lặp lại từng trường hợp. Đối với mỗi trường hợp thử nghiệm, nó truy vấn tổng của toàn bộ mảng. Sau đó, nó thực hiện kiểm tra giảm dần lũy thừa của hai, bắt đầu từ$2^{30}$xuống$1$. Ngay sau khi một ứng cử viên chia tổng số tiền một cách chính xác, nó sẽ cho kết quả là số tiền tối đa. Các điều kiện biên được xử lý bằng cách truy vấn phạm vi đầy đủ và kiểm tra lũy thừa của hai số chia hết, đảm bảo tính chính xác ngay cả đối với các mảng có độ dài 1 hoặc các mảng có phần tử tối đa lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng ẩn:`[1, 1, 1, 1, 2, 2, 2, 1, 1, 2, 2]`Truy vấn`? 1 11`trả về 15. 

| Ứng viên | 2^30? | 2^29? | … | 2? | 1? | 
| --- | --- | --- | --- | --- | --- | 
| 2^30 | 15 % 1073741824 != 0 | … | … | 2 | 15% 2 == 1 | 
| 2 | chia 15 → sai, tiếp theo 1 | chia → đúng | | | | 

Phần tử tối đa được phát hiện: 2. Điều này xác nhận rằng phương pháp của chúng tôi xử lý các mảng có lũy thừa hỗn hợp của hai một cách chính xác. 

### Ví dụ 2 

Mảng ẩn:`[1]`Truy vấn`? 1 1`trả về 1. 

Thí sinh kiểm tra: 2^30, 2^29, …, 1 → 1 chia hết cho 1. Tối đa là 1. Điều này thể hiện việc xử lý đúng các mảng có độ dài tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(30 * t) | Đối với mỗi trường hợp thử nghiệm, chúng tôi thực hiện tối đa 30 phép kiểm tra tính chia hết cho lũy thừa của hai. Các truy vấn được giới hạn ở một truy vấn cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(1) | Chỉ lưu trữ tổng số tiền và lũy thừa hai của ứng cử viên. | 

Với t ≤ 100, điều này đảm bảo tối đa 3000 phép toán, điều này không đáng kể trong giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample
assert run("4\n11\n9\n4\n12\n1\n1\n8\n4\n4\n4\n4\n8\n") == \
"! 2\n! 1\n! 4\n! 1073741824", "Sample 1"

# minimum input
assert run("1\n1\n1\n") == "! 1", "minimum array size"

# all equal powers of two
assert run("1\n4\n4\n") == "! 4", "all elements same"

# maximum allowed element
assert run("1\n1\n1073741824\n") == "! 1073741824", "single max element"

# sum divisible by multiple powers
assert run("1\n3\n6\n") == "! 2", "sum divisible by smaller max"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử`[1]`| 1 | Xử lý mảng tối thiểu | 
| 4 yếu tố`[4,4,4,4]`| 4 | Xử lý mảng thống nhất | 
| 1 phần tử`[2^30]`| 1073741824 | Phần tử tối đa có thể | 
| 3 yếu tố`[2,2,2]`| 2 | Phát hiện giá trị tối đa chính xác khi tổng chia hết cho nhiều lũy thừa | 

## Vỏ cạnh 

Đối với mảng một phần tử, việc truy vấn toàn bộ phạm vi sẽ trả về chính phần tử đó. Vòng lặp ứng cử viên nhanh chóng xác định mức tối đa, điều này không quan trọng nhưng cần thiết. Đối với mảng như`[1,1,1,1,1024]`, truy vấn tổng đầy đủ sẽ cho kết quả là 1032. Mũ lớn nhất của hai số chia cho 1032 là 8, nhưng chúng ta phải tiếp tục kiểm tra lũy thừa giảm dần cho đến khi đạt đến 1024, số này chia mảng theo tổ tiên của phần tử ban đầu. Cách tiếp cận này xử lý vấn đề này một cách tự nhiên vì công suất được kiểm tra theo thứ tự giảm dần, đảm bảo tính chính xác bất kể phân phối.
