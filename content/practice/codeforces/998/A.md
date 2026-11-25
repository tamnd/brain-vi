---
title: "CF 998A - Bóng Bay"
description: "Chúng ta được đưa cho một số gói, mỗi gói chứa một số lượng bóng bay đã biết. Nhiệm vụ là chia các gói này cho hai người để mỗi gói được chuyển hoàn toàn đến đúng một trong số họ. Không gói nào có thể bị hỏng và cả hai người phải nhận được ít nhất một gói."
date: "2026-06-16T23:54:51+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 998
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 493 (Div. 2)"
rating: 1000
weight: 998
solve_time_s: 109
verified: false
draft: false
---

[CF 998A - Bóng bay](https://codeforces.com/problemset/problem/998/A) 

**Đánh giá:** 1000 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một số gói, mỗi gói chứa một số lượng bóng bay đã biết. Nhiệm vụ là chia các gói này cho hai người để mỗi gói được chuyển hoàn toàn đến đúng một trong số họ. Không gói nào có thể bị hỏng và cả hai người phải nhận được ít nhất một gói. 

Sau khi chia, chúng tôi tính tổng số bóng bay mà mỗi người nhận được. Mục đích là để đảm bảo hai tổng số này là khác nhau. Nếu sự phân chia như vậy tồn tại, chúng ta phải xuất ra một gói gán hợp lệ cho người đầu tiên; mặt khác, chúng tôi báo cáo rằng không có sự phân chia hợp lệ nào tồn tại. 

Kích thước đầu vào rất nhỏ, tối đa là 10 gói. Điều này ngay lập tức gợi ý rằng việc tìm kiếm theo cấp số nhân trên các tập hợp con là khả thi. Một giải pháp kiểm tra tất cả các phép gán có thể, lên tới 2^10 = 1024 trường hợp, nằm trong giới hạn ngay cả với tính toán đơn giản. 

Trường hợp cạnh chính xuất hiện khi chỉ có một gói. Trong trường hợp đó, không thể đưa ít nhất một gói cho cả hai người cùng một lúc, vì vậy câu trả lời là không thể. Một tình huống khó phát hiện khác là khi mọi phân vùng đều dẫn đến tổng bằng nhau, nhưng vì tất cả kích thước gói đều dương nên điều này chỉ có thể xảy ra trong trường hợp gói đơn tầm thường. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử mọi cách có thể để gán các gói cho Grigory hoặc Andrew. Mỗi gói có hai lựa chọn, do đó có tổng cộng 2^n bài tập. Đối với mỗi bài tập, chúng tôi tính tổng số bong bóng trong mỗi nhóm và kiểm tra xem cả hai nhóm có trống không và tổng của chúng có khác nhau hay không. 

Phương pháp brute-force này đúng vì nó khám phá toàn bộ không gian giải pháp mà không bỏ sót. Vấn đề là hiệu năng trong các bài toán tổng quát trong đó n có thể lớn. Với n đến 10, cách tiếp cận này vẫn cực kỳ nhanh, vì tối đa 1024 tập hợp con được kiểm tra và mỗi phép tính tổng có chi phí O(n), cho ra khoảng 10.240 phép toán. 

Quan sát quan trọng là chúng ta không cần phải tối ưu hóa thêm nữa. Các ràng buộc được cố ý nhỏ để tìm kiếm toàn diện là giải pháp mong muốn. Chúng tôi chỉ cần đảm bảo rằng chúng tôi bỏ qua các phân vùng không hợp lệ trong đó một nhóm trống hoặc trong đó các tổng khớp chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(n · 2^n) | O(1) | Đã chấp nhận | 
| Cái nhìn sâu sắc được tối ưu hóa (cùng một phương pháp) | O(n · 2^n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số bong bóng trên tất cả các gói. Giá trị này giúp chúng ta đánh giá từng phần một cách nhanh chóng vì khi đã biết một bên thì xác định được bên kia. 
2. Nếu chỉ có một gói, xuất ngay -1. Không có cách nào để chia một gói thành hai nhóm không trống. 
3. Lặp lại tất cả các tập hợp con của gói, cho biết mỗi gói có được gán cho Grigory hay không. 
4. Đối với mỗi tập hợp con, hãy đảm bảo nó không trống cũng không phải tập hợp đầy đủ. Điều này đảm bảo cả hai người đều nhận được ít nhất một gói tin. 
5. Tính tổng số bong bóng trong tập hợp con đã chọn. 
6. So sánh số tiền này với số tiền còn lại. Nếu chúng khác nhau thì đây là giải pháp hợp lệ và chúng tôi có thể xuất nó ngay lập tức. 
7. Nếu không có tập hợp con nào hoạt động, xuất ra -1. 

### Tại sao nó hoạt động 

Mỗi phép gán hợp lệ tương ứng với chính xác một tập con các chỉ số gói. Thuật toán kiểm tra tất cả các tập hợp con ngoại trừ những tập hợp con không hợp lệ có một bên trống. Đối với mỗi tập hợp con hợp lệ, nó xác minh ràng buộc duy nhất còn lại, đó là hai tổng khác nhau. Vì mọi phân vùng có thể đều được kiểm tra nên mọi giải pháp hợp lệ sẽ được tìm thấy và nếu không có giải pháp nào tồn tại thì việc tìm kiếm sẽ cạn kiệt mọi khả năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    if n == 1:
        print(-1)
        return

    total = sum(a)
    m = 1 << n

    for mask in range(1, m - 1):
        s = 0
        for i in range(n):
            if mask & (1 << i):
                s += a[i]

        if s != 0 and s != total and 2 * s != total:
            print(bin(mask).count("1"))
            res = []
            for i in range(n):
                if mask & (1 << i):
                    res.append(i + 1)
            print(*res)
            return

    print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp liệt kê tất cả các tập hợp con bằng cách sử dụng mặt nạ bit. Mỗi mặt nạ mã hóa gói nào được trao cho Grigory. điều kiện`mask range 1 to (1<<n)-2`đảm bảo cả hai bên đều không trống. 

Đối với mỗi mặt nạ, chúng tôi tính tổng tập hợp con và kiểm tra xem nó có khác với tổng bổ sung hay không. Séc`2 * s != total`trực tiếp mã hóa yêu cầu cả hai tổng không bằng nhau. 

Khi tìm thấy tập hợp con hợp lệ, chúng tôi xuất kích thước và chỉ số của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 1
```Chúng tôi đánh giá các tập hợp con: 

| Mặt nạ | Tập hợp con | Tổng hợp | Tổng bù | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 001 | {1} | 1 | 3 | Có | 

Tập hợp con hợp lệ đầu tiên đã thỏa mãn điều kiện, vì vậy chúng tôi xuất ra`{1}`hoặc tập hợp con hợp lệ khác tùy thuộc vào thứ tự lặp. Một đầu ra hợp lệ là:```
1
1
```Điều này cho thấy ngay cả một tập hợp con nhỏ cũng có thể thỏa mãn điều kiện bất đẳng thức ngay lập tức. 

### Ví dụ 2 

đầu vào:```
2
1 1
```| Mặt nạ | Tập hợp con | Tổng hợp | Tổng bù | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 01 | {1} | 1 | 1 | Không | 

Không có tập hợp con nào tạo ra các tổng khác nhau nên kết quả đúng là:```
-1
```Điều này chứng tỏ trường hợp đặc biệt trong đó tính đối xứng làm cho sự phân tách không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 2^n) | Mỗi tập hợp con được kiểm tra và tính tổng trong O(n) | 
| Không gian | O(1) | Chỉ một số biến được sử dụng ngoài bộ nhớ đầu vào | 

Với n 10, số thao tác trong trường hợp xấu nhất là khoảng 10 × 1024, là không đáng kể đối với giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    
    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        
        if n == 1:
            print(-1)
            return

        total = sum(a)
        m = 1 << n

        for mask in range(1, m - 1):
            s = 0
            for i in range(n):
                if mask & (1 << i):
                    s += a[i]

            if s != 0 and s != total and 2 * s != total:
                print(bin(mask).count("1"))
                res = []
                for i in range(n):
                    if mask & (1 << i):
                        res.append(i + 1)
                print(*res)
                return

        print(-1)

    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3\n1 2 1\n") in ["1\n1", "2\n1 2", "1\n3", "2\n2 3"]

# custom cases
assert run("1\n5\n") == "-1", "minimum size"
assert run("2\n1 1\n") == "-1", "all equal impossible"
assert run("2\n1 2\n") in ["1\n1", "1\n2"], "simple separable case"
assert run("3\n10 1 1\n") != "-1", "always separable for n>=2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 gói | -1 | yếu tố đơn lẻ không thể | 
| 1 1 | -1 | trường hợp thất bại đối xứng | 
| 1 2 | chia hợp lệ | tính đúng đắn cơ bản | 
| 10 1 1 | chia hợp lệ | trường hợp tích cực không tầm thường | 

## Vỏ cạnh 

Trường hợp gói đơn là trường hợp duy nhất không thể thực hiện được về mặt cấu trúc. Với đầu vào như`1\n7`, không có cách nào để gán ít nhất một gói cho mỗi người, vì vậy thuật toán ngay lập tức trả về -1 trước bất kỳ phép liệt kê nào. 

Trường hợp đối xứng như`2\n1 1`cũng thất bại trong việc liệt kê. Sự phân chia duy nhất có thể là`{1}`Và`{2}`, và cả hai đều tạo ra tổng số bằng nhau. Thuật toán kiểm tra cả hai mặt nạ và không tìm thấy tập hợp con hợp lệ nào, cuối cùng in ra -1. 

Trường hợp giống đối xứng lớn hơn một chút vẫn hoạt động chính xác vì kiểm tra tập hợp con so sánh rõ ràng`2 * sum != total`. Bất kỳ phân vùng nào cân bằng hoàn hảo đều bị từ chối, trong khi mọi sự mất cân bằng sẽ được chấp nhận ngay lập tức khi được tìm thấy.
