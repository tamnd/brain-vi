---
title: "CF 1583A - Bài Hát Windblume"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có một mảng các số nguyên dương riêng biệt và nhiệm vụ là chọn càng nhiều phần tử càng tốt sao cho tổng của các phần tử được chọn không phải là số nguyên tố."
date: "2026-06-14T23:10:15+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1583
codeforces_index: "A"
codeforces_contest_name: "Technocup 2022 - Elimination Round 1"
rating: 800
weight: 1583
solve_time_s: 293
verified: false
draft: false
---

[CF 1583A - Windblume Ode](https://codeforces.com/problemset/problem/1583/A) 

**Đánh giá:** 800 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 4m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có một mảng các số nguyên dương riêng biệt và nhiệm vụ là chọn càng nhiều phần tử càng tốt sao cho tổng của các phần tử được chọn không phải là số nguyên tố. Trong số tất cả các lựa chọn hợp lệ, chúng ta phải xuất ra bất kỳ tập hợp con nào có kích thước tối đa có thể. 

Đầu ra không phải là tổng mà là chỉ số của các phần tử được chọn. Vì vậy, mục tiêu thực sự là quyết định xem liệu chúng ta có thể bao gồm tất cả các phần tử hay không và nếu không, việc loại bỏ nhỏ nhất nào sẽ khắc phục được tính nguyên tố của tổng. 

Hạn chế chính là mỗi mảng có tối đa 100 phần tử và mỗi giá trị nhiều nhất là 200. Điều này khiến không thể ép buộc tất cả các tập hợp con vì số lượng tập hợp con tăng theo cấp số nhân là 2ⁿ. Một bảng liệt kê đầy đủ sẽ yêu cầu tối đa khoảng 2¹⁰⁰ tập hợp con cho mỗi trường hợp thử nghiệm, vượt xa mọi giới hạn khả thi ngay cả trong Python được tối ưu hóa. 

Quan sát cấu trúc quan trọng là chúng ta không tối ưu hóa hàm tùy ý trên các tập hợp con. Điều duy nhất quan trọng là tổng tổng của các phần tử được chọn và tổng đó là số nguyên tố hay hợp số. 

Một cách tiếp cận ngây thơ cố gắng loại bỏ các phần tử một cách tham lam hoặc kiểm tra các tập hợp con theo thứ tự tùy ý có thể thất bại trong các trường hợp khó phát hiện. Ví dụ: loại bỏ phần tử nhỏ nhất trước tiên không nhất thiết phải sửa được tổng nguyên tố và loại bỏ phần tử lớn nhất có thể làm thu nhỏ tập hợp con một cách không cần thiết ngay cả khi việc loại bỏ khác sẽ giữ nguyên kích thước. 

Một trường hợp thất bại khác xuất phát từ việc giả định rằng toàn bộ mảng luôn hợp lệ. Nếu tổng là số nguyên tố thì tập hợp đầy đủ không hợp lệ, mặc dù việc loại bỏ chỉ một phần tử được chọn cẩn thận có thể khắc phục được. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi tạo ra mọi tập hợp con, tính tổng của nó, kiểm tra xem nó có phải là tập hợp con hay không và theo dõi kích thước tập hợp con lớn nhất thỏa mãn điều kiện. Điều này đúng vì nó đánh giá trực tiếp mọi khả năng. Tuy nhiên, nó yêu cầu đánh giá 2ⁿ tập hợp con và tính tổng cho mỗi tập hợp con sẽ thêm một yếu tố khác, khiến nó trở nên hiệu quả O(n·2ⁿ). Với n = 100, điều này là không thể. 

Quan sát quan trọng là chúng ta không cần phải tìm kiếm trong không gian tập hợp con. Chúng tôi chỉ quan tâm đến tổng của các phần tử được chọn. Vì tất cả các số đều dương nên việc thêm các phần tử luôn làm tăng tổng và tập hợp con lớn nhất chỉ đơn giản là mảng đầy đủ trừ khi tổng của nó là số nguyên tố. 

Nếu tổng số đã là tổng hợp thì câu trả lời là tất cả các phần tử. Trường hợp có vấn đề duy nhất là khi tổng số tiền là số nguyên tố. Trong trường hợp đó, việc loại bỏ bất kỳ phần tử đơn lẻ nào sẽ làm thay đổi tổng và chúng ta chỉ cần kiểm tra xem việc loại bỏ một phần tử có thể tạo thành tổng hợp hay không. Vì tất cả các số đều dương nên việc loại bỏ nhiều hơn một phần tử sẽ chỉ làm giảm kích thước tập hợp con hơn nữa, vì vậy giải pháp tối ưu phải là mảng đầy đủ hoặc mảng trừ đi một phần tử. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra tính nguyên tố của tổng đầy đủ và nếu nó là số nguyên tố thì tìm bất kỳ phần tử đơn lẻ nào mà việc loại bỏ nó sẽ tạo thành tổng còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2ⁿ) | O(n) | Quá chậm | 
| Tối ưu | O(n·√S) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Gọi S là tổng của tất cả các phần tử. 

1. Tính S bằng cách tính tổng tất cả các phần tử của mảng. Điều này đưa ra tổng tương ứng với việc chọn toàn bộ mảng. 
2. Kiểm tra xem S có phải là số nguyên tố hay không bằng cách thử các ước số lên tới √S. Nếu S là hợp số thì chúng ta đã hoàn tất và có thể chọn tất cả các chỉ số. 
3. Nếu S là số nguyên tố thì chúng ta cần loại bỏ đúng một phần tử. Hãy thử từng chỉ số i và tính S - a[i]. Nếu giá trị này là tổng hợp, chúng ta có thể chọn tất cả các phần tử ngoại trừ i và stop. 
4. Xuất tập hợp con kết quả. 

Lý do chúng tôi chỉ xem xét việc loại bỏ một phần tử là vì việc loại bỏ nhiều phần tử sẽ làm giảm đáng kể kích thước tập hợp con và chúng tôi đang tối đa hóa số lượng phần tử. 

### Tại sao nó hoạt động

Tính đúng đắn dựa trên một lập luận cực trị đơn giản. Bất kỳ câu trả lời hợp lệ nào cũng là tập hợp con của mảng đầy đủ. Nếu tổng đầy đủ là hợp số thì nó chiếm ưu thế hơn tất cả các tập con nhỏ hơn về kích thước, do đó nó phải tối ưu. Nếu tổng đầy đủ là số nguyên tố thì mọi giải pháp hợp lệ đều phải tránh tổng chính xác đó. Cách gần nhất để duy trì kích thước tối đa là loại bỏ số phần tử nhỏ nhất có thể và loại bỏ một phần tử là đủ vì việc thay đổi tổng bằng bất kỳ số dương nào sẽ phá vỡ giá trị chính xác là số nguyên tố. Vì chúng tôi kiểm tra tất cả các lần xóa đơn lẻ nên chúng tôi đảm bảo tìm được tập hợp con tối đa hợp lệ bất cứ khi nào có tập hợp con đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(x):
    if x < 2:
        return False
    if x % 2 == 0:
        return x == 2
    i = 3
    while i * i <= x:
        if x % i == 0:
            return False
        i += 2
    return True

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    total = sum(a)

    if not is_prime(total):
        print(n)
        print(*[i + 1 for i in range(n)])
        continue

    # try removing one element
    found = False
    for i in range(n):
        if not is_prime(total - a[i]):
            print(n - 1)
            print(*[j + 1 for j in range(n) if j != i])
            found = True
            break

    # guaranteed to exist per problem statement
    if not found:
        # fallback (should never happen)
        print(1)
        print(1)
```Việc triển khai trước tiên sẽ tính tổng tổng và kiểm tra tính nguyên tố bằng phương pháp chia thử √n tiêu chuẩn. Nếu tổng không phải là số nguyên tố, nó sẽ xuất trực tiếp tất cả các chỉ số. Mặt khác, nó lặp lại từng phần tử và kiểm tra xem việc loại bỏ nó có tạo ra tổng hợp hay không. Ứng viên thành công đầu tiên được sử dụng. 

Nhánh dự phòng chỉ được đưa vào để đảm bảo an toàn, mặc dù vấn đề đảm bảo sự tồn tại của một tập hợp con hợp lệ trong mọi trường hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
8 1 2
```| Bước | Tổng số tiền | Kiểm tra chính | Hành động | 
| --- | --- | --- | --- | 
| 1 | 11 | nguyên tố | cần loại bỏ | 
| 2 | 11 - 8 = 3 | nguyên tố | không hợp lệ | 
| 3 | 11 - 1 = 10 | tổng hợp | chọn chỉ số {2,3} | 

Điều này cho thấy rằng ngay cả khi tập hợp đầy đủ không hợp lệ, việc loại bỏ một phần tử được chọn cẩn thận sẽ khắc phục tình trạng ngay lập tức. 

### Ví dụ 2 

đầu vào:```
6 9 4 2
```| Bước | Tổng số tiền | Kiểm tra chính | Hành động | 
| --- | --- | --- | --- | 
| 1 | 21 | tổng hợp | lấy tất cả | 

Ở đây mảng đầy đủ đã thỏa mãn điều kiện nên không cần loại bỏ. 

Những ví dụ này xác nhận rằng thuật toán không bao giờ thực hiện việc loại bỏ không cần thiết và luôn duy trì kích thước tập hợp con tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n√S) mỗi lần kiểm tra | kiểm tra tính nguyên tố trên tổng và tối đa n lần xóa | 
| Không gian | O(1) | chỉ lưu trữ đầu vào và một vài biến | 

Các ràng buộc giữ n nhỏ và tổng được giới hạn tối đa là 200·100 = 20000, do đó việc kiểm tra tính nguyên tố diễn ra cực kỳ nhanh trong thực tế và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()  # placeholder, assumes integrated solution runner

# provided samples (formatting adjusted for single-run testing context)
# custom tests would require integrating solution into callable function

# minimal size
inp1 = """1
3
1 2 3
"""

# all equal pattern avoided by distinct constraint, but small structured case
inp2 = """1
3
2 3 5
"""

# maximum n
inp3 = "1\n100\n" + " ".join(map(str, range(1, 101))) + "\n"

# check composite sum directly
inp4 = """1
4
2 2 2 2
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tăng nhỏ | toàn bộ hoặc loại bỏ | độ đúng cơ sở | 
| số tiền gần chuẩn | trường hợp loại bỏ | sửa chữa phần tử đơn | 
| trường hợp tối đa | đầy đủ hoặc gần đầy | hiệu suất | 
| cấu trúc thống nhất | tổng hợp ổn định | tính đúng đắn dưới sự đối xứng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tổng chính xác là số nguyên tố. Trong tình huống đó, việc lấy tất cả các phần tử không thành công mặc dù việc loại bỏ một phần tử sẽ khắc phục được nó. Ví dụ: một mảng như`[8, 1, 2]`tổng bằng 11, là số nguyên tố. Thuật toán phát hiện chính xác điều này và cố gắng loại bỏ từng phần tử một cho đến khi tổng trở thành hợp số. 

Một trường hợp khác là khi loại bỏ bất kỳ phần tử đơn lẻ nào vẫn để lại tổng nguyên tố. Bài toán đảm bảo rằng tình huống này không xảy ra với các dữ liệu đầu vào hợp lệ, do đó việc tìm kiếm trên các lần xóa đơn lẻ là đủ và luôn thành công. 

Trường hợp cạnh cuối cùng là các mảng nhỏ có kích thước 3. Ngay cả ở đây, logic vẫn đúng vì tổng đầy đủ là tổng hợp hoặc ít nhất một lần loại bỏ tạo ra tổng tổng hợp, đảm bảo tồn tại câu trả lời hợp lệ mà không cần xem xét các tập hợp con trống hoặc nhiều loại bỏ.
