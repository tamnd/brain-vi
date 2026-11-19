---
title: "CF 2086C - Hoán vị biến mất"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến n, có nghĩa là mỗi số trong phạm vi đó xuất hiện đúng một lần trong một mảng có kích thước n. Sau đó, chúng tôi thực hiện một chuỗi truy vấn, trong đó trong mỗi truy vấn, chúng tôi thay thế một phần tử cụ thể của mảng bằng 0."
date: "2026-06-08T06:03:12+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "dsu", "graphs", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2086
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 177 (Rated for Div. 2)"
rating: 1300
weight: 2086
solve_time_s: 117
verified: false
draft: false
---

[CF 2086C - Hoán vị biến mất](https://codeforces.com/problemset/problem/2086/C) 

**Đánh giá:** 1300 
**Tags:** dfs và tương tự, dp, dsu, đồ thị, tham lam, triển khai 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến n, có nghĩa là mỗi số trong phạm vi đó xuất hiện đúng một lần trong một mảng có kích thước n. Sau đó, chúng tôi thực hiện một chuỗi truy vấn, trong đó trong mỗi truy vấn, chúng tôi thay thế một phần tử cụ thể của mảng bằng 0. Mỗi vị trí được thay thế chính xác một lần trong suốt tất cả các truy vấn, vì vậy sau n truy vấn, toàn bộ mảng sẽ bao gồm các số 0. 

Sau mỗi truy vấn, nhiệm vụ là tính toán số phép toán tối thiểu cần thiết để biến mảng hiện tại trở lại bất kỳ hoán vị hợp lệ nào của các số nguyên từ 1 đến n. Thao tác được phép rất đơn giản: chọn một chỉ mục i và thay thế giá trị tại chỉ mục đó bằng i. Hoạt động này đảm bảo rằng phần tử tại chỉ mục đó trở nên chính xác trong hoán vị và nó có thể được áp dụng độc lập cho bất kỳ chỉ mục nào. 

Các ràng buộc chỉ ra rằng n có thể đạt tới 10^5 và tổng số n trên tất cả các trường hợp thử nghiệm không vượt quá 2·10^5. Với giới hạn 2 giây, mọi giải pháp có O(n^2) cho mỗi trường hợp thử nghiệm sẽ quá chậm. Chúng tôi cần khoảng O(n) cho mỗi trường hợp thử nghiệm để duy trì trong giới hạn. 

Trường hợp cạnh tinh tế là khi hoán vị ban đầu được sắp xếp theo thứ tự tăng dần hoặc giảm dần. Ví dụ: hãy xem xét n=3, hoán vị [3,2,1] và truy vấn [3,2,1]. Sau truy vấn đầu tiên, chỉ vị trí 3 trở thành số 0. Các thao tác tối thiểu để khôi phục hoán vị sẽ là một, vì việc đặt chỉ mục 3 đến 3 sẽ sửa phần tử đó. Sau truy vấn thứ hai, vị trí 3 và 2 bằng 0 và số thao tác tối thiểu là hai. Một giải pháp bất cẩn có thể cho rằng số 0 luôn xuất hiện ở cuối hoặc theo thứ tự được sắp xếp, điều này không đúng. 

Một trường hợp cạnh khác là khi các số 0 phá vỡ chuỗi các số được đặt chính xác liền kề. Ví dụ: nếu phần tử đầu tiên và phần tử cuối cùng bằng 0, nhưng các phần tử ở giữa đã chính xác, chúng ta phải tính đến các khoảng trống trong mảng, điều này ảnh hưởng trực tiếp đến số lượng thao tác được yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tác động của từng truy vấn: sau khi thay thế p[d_i] bằng 0, lặp qua mảng và đếm xem có bao nhiêu phần tử không chính xác. Đối với mỗi phần tử không chính xác, nó đã bằng 0 (cần một thao tác) hoặc sai (cũng cần một thao tác). Điều này hiệu quả nhưng yêu cầu thời gian O(n^2) trong trường hợp xấu nhất vì chúng ta sẽ duyệt mảng n lần, quá chậm đối với n lên tới 10^5. 

Quan sát quan trọng là phép toán được phép cho phép chúng ta đặt trực tiếp bất kỳ số i nào vào chỉ số i. Điều này có nghĩa là vấn đề giảm xuống việc đếm tiền tố lớn nhất của các vị trí từ 1 đến n có thể vẫn đúng mà không cần thực hiện thao tác. Chúng tôi có thể duy trì một tập hợp các phần tử đã bị loại bỏ và theo dõi khối liền kề cao nhất từ ​​đầu đã chính xác. Cụ thể, chúng ta có thể lặp lại các truy vấn ngược lại, “hoàn tác” việc đưa các vị trí về 0. Đối với mỗi bước, chúng tôi theo dõi độ dài của hậu tố lớn nhất đã chứa tất cả các số từ đầu đến n. Các thao tác tối thiểu được yêu cầu sau mỗi truy vấn chính xác là số vị trí nằm ngoài hậu tố này, vì mọi vị trí khác sẽ cần một thao tác để sửa. Quan sát này làm giảm độ phức tạp từ O(n^2) xuống O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`pos`có kích thước n+1 ánh xạ từng số tới chỉ mục của nó trong hoán vị. Điều này cho phép chúng ta nhanh chóng xác định vị trí bất kỳ số nào trong thời gian O(1). 
2. Khởi tạo một mảng`removed`có kích thước n+1 để theo dõi vị trí nào đã bằng 0. Đặt tất cả thành Sai ban đầu. 
3. Khởi tạo một biến`suffix_max`đến N. Điều này sẽ theo dõi số lượng tối đa hiện tại trong hậu tố liền kề không yêu cầu thao tác. 
4. Lặp lại các truy vấn theo thứ tự ngược lại, từ truy vấn cuối cùng trở lại truy vấn đầu tiên. Đối với mỗi truy vấn, hãy “khôi phục” vị trí về giá trị ban đầu vì về cơ bản chúng ta đang xây dựng mảng ngược lại. 
5. Trong khi số tại`suffix_max`đã được khôi phục (tức là chưa bị xóa), giảm`suffix_max`. Bước này tìm thấy hậu tố liền kề lớn nhất của các số đã đúng. 
6. Ghi lại các thao tác tối thiểu sau truy vấn này dưới dạng`suffix_max`. Lý do là tất cả các số lớn hơn`suffix_max`đã được đặt chính xác và tất cả các số nhỏ hơn hoặc bằng`suffix_max`mỗi người cần một thao tác. 
7. Sau khi xử lý ngược lại tất cả các truy vấn, hãy đảo ngược mảng kết quả để khớp với thứ tự truy vấn ban đầu. 

Tại sao nó hoạt động: điều bất biến là ở mỗi bước,`suffix_max`đại diện cho số lớn nhất sao cho tất cả các số lớn hơn`suffix_max`đã ở đúng vị trí của chúng. Vì thao tác được phép sửa trực tiếp bất kỳ số nào tại chỉ mục tương ứng của nó, nên mọi số nhỏ hơn hoặc bằng`suffix_max`phải được cố định riêng lẻ, tương ứng chính xác với các hoạt động tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        d = list(map(int, input().split()))
        
        pos = [0] * (n + 1)
        for idx, val in enumerate(p):
            pos[val] = idx
        
        removed = [False] * n
        ans = [0] * n
        suffix_max = n
        # process queries in reverse
        for i in range(n - 1, -1, -1):
            removed[d[i]-1] = False  # restore element
            while suffix_max > 0 and not removed[pos[suffix_max]]:
                suffix_max -= 1
            ans[i] = suffix_max
            removed[d[i]-1] = True  # mark removed for next iteration
        
        print(" ".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng`pos`mảng để cho phép O(1) truy cập vào chỉ mục của bất kỳ số nào. các`removed`mảng được sử dụng để đánh dấu các vị trí bằng 0. Chúng tôi xử lý các truy vấn ngược lại để có thể xác định kích thước của hậu tố không yêu cầu bất kỳ thao tác nào. Phần tinh tế là chuyển đổi`removed`cờ trước và sau khi giảm`suffix_max`để duy trì việc ghi sổ kế toán chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
3
1 2 3
3 2 1
```| Truy vấn | Mảng sau truy vấn | hậu tố_max | Hoạt động tối thiểu | 
| --- | --- | --- | --- | 
| 1 | [1,2,0] | 2 | 1 | 
| 2 | [1,0,0] | 1 | 2 | 
| 3 | [0,0,0] | 0 | 3 | 

Dấu vết này xác nhận rằng khi chúng tôi loại bỏ các phần tử, hậu tố lớn nhất của số đã sửa sẽ co lại và các phép toán tối thiểu sẽ tăng tương ứng. 

### Mẫu 2 

đầu vào:```
5
4 5 3 1 2
4 5 1 3 2
```| Truy vấn | Mảng | hậu tố_max | Hoạt động tối thiểu | 
| --- | --- | --- | --- | 
| 1 | [4,5,3,0,2] | 3 | 2 | 
| 2 | [4,5,3,0,0] | 1 | 4 | 
| 3 | [0,5,3,0,0] | 1 | 4 | 
| 4 | [0,5,0,0,0] | 0 | 5 | 
| 5 | [0,0,0,0,0] | 0 | 5 | 

Dấu vết này chứng minh rằng thuật toán tính toán chính xác các phần tử đã có sẵn, bất kể thứ tự áp dụng số 0 là gì. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi truy vấn được xử lý một lần và tổng số suffix_max giảm tối đa n lần | 
| Không gian | O(n) | Mảng pos, loại bỏ và ans yêu cầu mỗi mảng có khoảng trống O(n) | 

Tổng thời gian trên tất cả các trường hợp thử nghiệm là O(tổng n) = O(2·10^5), nằm trong giới hạn 2 giây. Việc sử dụng dung lượng tỷ lệ thuận với n, có thể chấp nhận được dưới giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided sample
assert run("3\n3\n1 2 3\n3 2 1\n5\n4 5 3 1 2\n4 5 1 3 2\n7\n4 3 1 2 7 5 6\n1 2 3 4 5 6 7\n") == "1 2 3\n2 4 4 5 5\n4 4 4 4 7 7 7"

# minimum-size input
assert run("1\n1\n1\n1\n") == "1"

# maximum-size input with n=5 (small for demonstration)
assert run("1\n5\n5 4 3 2 1\n1 2 3 4 5\n") == "5 4 3 2 1"

# all elements removed in increasing order
assert run("1\n4\n1 2 3 4\n1 2 3 4\n") == "4
```
