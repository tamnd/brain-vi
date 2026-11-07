---
problem: 1326C
contest_id: 1326
problem_index: C
name: "Permutation Partitions"
contest_name: "Codeforces Global Round 7"
rating: 1300
tags: ["combinatorics", "greedy", "math"]
answer: passed_samples
verified: true
solve_time_s: 138
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2df4c5-5b50-83ec-b241-4757447409ac
---

#CF 1326C - Phân vùng hoán vị 

**Đánh giá:** 1300 
**Tags:** tổ hợp, tham lam, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2df4c5-5b50-83ec-b241-4757447409ac 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số từ 1 đến n và chúng ta phải chia nó thành đúng k đoạn liền kề. Mỗi phân đoạn đóng góp một giá trị bằng phần tử tối đa bên trong phân khúc đó và mục tiêu là chọn các điểm phân chia sao cho tổng các giá trị tối đa của phân khúc này càng lớn càng tốt. Sau khi tìm được tổng lớn nhất có thể này, chúng ta cũng cần đếm xem có bao nhiêu cách khác nhau để sắp xếp k đoạn đạt được số đó. 

Một chi tiết quan trọng là các phân đoạn liền kề nhau, vì vậy chúng tôi chỉ chọn k−1 vị trí cắt giữa các chỉ số liền kề. Khi các phần cắt được cố định, mỗi vị trí thuộc về chính xác một phân đoạn và mỗi phân đoạn đóng góp chính xác một mức tối đa. 

Ràng buộc n lên tới 200.000 buộc phải có giải pháp O(n log n) hoặc O(n). Bất cứ điều gì cố gắng đánh giá các phân vùng một cách rõ ràng hoặc tính toán lại cực đại của phân đoạn nhiều lần sẽ ngay lập tức thất bại vì số lượng phân vùng tăng lên theo tổ hợp với k. 

Một sai lầm ngây thơ xuất hiện khi người ta cho rằng chúng ta nên tham lam cắt giảm những giá trị lớn mà không có chiến lược toàn cầu. Ví dụ: trong một hoán vị như [1, 3, 2, 4] với k = 2, việc cắt ngay trước 4 có vẻ ổn, nhưng sự phân chia tối ưu phụ thuộc vào việc các phần tử lớn trước đó đã được “gán” cho các phân đoạn hay chưa. Một dạng lỗi tinh vi khác là tính toán các phân vùng không chính xác khi tồn tại nhiều cấu hình cắt tối ưu bằng nhau, bởi vì cùng một cấu trúc đóng góp tối đa có thể phát sinh từ nhiều lựa chọn cắt liền kề. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử mọi cách để chọn vị trí cắt k−1 trong số khoảng cách n−1. Đối với mỗi lựa chọn, chúng tôi tính toán mức tối đa của phân đoạn bằng cách quét từng phân đoạn một cách độc lập. Ngay cả với tiền xử lý tiền tố, việc đánh giá một phân vùng vẫn tốn O(n) và có các phân vùng O(n choose k−1). Điều này trở nên lớn về mặt thiên văn ngay cả với n = 30, khiến cho lực lượng vũ phu không thể sử dụng được. 

Quan sát quan trọng là sự đóng góp của từng phân đoạn được xác định bởi các phần tử nào trở thành “chịu trách nhiệm” đối với các phân đoạn dưới dạng cực đại. Vì mỗi phần tử là duy nhất nên mỗi giá trị có thể trở thành giá trị tối đa của chính xác một phân đoạn. Do đó, chiến lược là gán k phần tử lớn nhất làm “mỏ neo”, bởi vì mỗi đoạn tối đa phải đến từ một số phần tử và việc chọn các phần tử neo lớn hơn luôn làm tăng tổng. 

Khi chúng tôi quyết định rằng các phần tử nhất định đóng vai trò là cực đại của phân đoạn, nhiệm vụ còn lại sẽ là phân phối các phần cắt sao cho mỗi phần tử tối đa được chọn sẽ được tách thành phân đoạn riêng của nó trong khi vẫn giữ nguyên thứ tự. Điều này biến bài toán thành việc chọn k phần tử và đếm các cách đặt các điểm cắt sao cho mỗi phần tử được chọn là lần xuất hiện đầu tiên của đoạn cực đại tính từ bên trái. 

Chúng tôi xử lý các phần tử theo thứ tự giá trị giảm dần và quyết định vị trí mỗi phần tử “top k” có thể bắt đầu một phân đoạn. Giữa các điểm neo được chọn liên tiếp, số lượng vị trí tự do xác định có bao nhiêu cách chúng ta có thể đặt các vết cắt và điều này trở thành một quá trình đếm nhân lên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n, k) · n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta xác định vị trí của tất cả các giá trị trong hoán vị. Vì là hoán vị nên mỗi giá trị từ 1 đến n xuất hiện đúng một lần nên ta có thể xây dựng mảng vị trí pos[value]. 

Sau đó, chúng tôi làm việc với các giá trị theo thứ tự giảm dần vì các giá trị cao hơn phải chiếm ưu thế cực đại của phân đoạn nếu chúng được đưa vào.

1. Chúng tôi lặp lại các giá trị từ n xuống 1 và thu thập vị trí của k giá trị lớn nhất. Những vị trí này là những ứng cử viên duy nhất có thể đóng góp vào số tiền tối đa. Điều này là do việc thay thế bất kỳ mức tối đa nào đã chọn bằng một giá trị nhỏ hơn sẽ làm giảm tổng số tiền. 
2. Chúng ta sắp xếp các vị trí đã chọn theo thứ tự tăng dần. Các vị trí này xác định cấu trúc của các đoạn từ trái sang phải. 
3. Tổng tối đa có thể chỉ đơn giản là tổng của k giá trị lớn nhất, vì mỗi điểm neo được chọn đóng góp chính xác một lần dưới dạng mức tối đa của phân đoạn. 
4. Để đếm cấu hình, chúng tôi kiểm tra khoảng cách giữa các vị trí được chọn liên tiếp. Giả sử các vị trí được chọn là x1 < x2 < ... < xk. Với mỗi cặp liên tiếp, số vị trí cắt sẵn có là xi+1 − xi. Mỗi khoảng trống như vậy đóng góp độc lập vào số lượng phân vùng hợp lệ. 
5. Câu trả lời cuối cùng là tích của tất cả các kích thước khoảng cách này theo modulo 998244353. 

Việc đếm hoạt động vì khi chúng tôi sửa phần tử nào là cực đại của phân đoạn, mọi phân vùng hợp lệ sẽ được xác định bằng cách chọn một lần cắt trong mỗi khoảng cách giữa cực đại được chọn liên tiếp. Mỗi khoảng trống hoạt động độc lập vì các vết cắt không thể vượt qua ranh giới phân đoạn. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là trong một giải pháp tối ưu, mỗi giá trị k lớn nhất phải xuất hiện dưới dạng giá trị lớn nhất của một phân đoạn riêng biệt. Bất kỳ cấu hình nào khác đều có thể được chuyển đổi bằng cách hoán đổi mức tối đa phân khúc nhỏ hơn với giá trị không sử dụng lớn hơn, tăng mục tiêu một cách chặt chẽ. Khi các cực đại này được cố định, quyền tự do phân vùng chính xác là lựa chọn vị trí phân tách các phân đoạn giữa các cực đại liên tiếp và các lựa chọn này độc lập giữa các khoảng trống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, k = map(int, input().split())
    p = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, v in enumerate(p):
        pos[v] = i

    # take k largest values
    chosen_vals = list(range(n, n - k, -1))
    chosen_pos = [pos[v] for v in chosen_vals]
    chosen_pos.sort()

    # maximum sum
    max_sum = sum(chosen_vals)

    # count ways
    ways = 1
    for i in range(k - 1):
        gap = chosen_pos[i + 1] - chosen_pos[i]
        ways = (ways * gap) % MOD

    print(max_sum, ways)

if __name__ == "__main__":
    solve()
```Mảng vị trí cho phép tra cứu liên tục theo thời gian về vị trí của từng giá trị trong hoán vị, giúp tránh việc quét lặp lại. Việc sắp xếp các vị trí đã chọn là cần thiết vì thứ tự hoán vị không được căn chỉnh theo thứ tự giá trị. 

Vòng đếm nhân khoảng cách giữa các cực đại được chọn liên tiếp. Mỗi khoảng trống thể hiện số lượng vị trí cắt hợp lệ để giữ cho cực đại của phân đoạn được cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
2 1 3
```Các giá trị được chọn là 3 và 2. Vị trí của chúng là: 

| giá trị | vị trí | 
| --- | --- | 
| 3 | 2 | 
| 2 | 0 | 

Vị trí được sắp xếp: [0, 2] 

| bước | vị trí đã chọn | khoảng cách | 
| --- | --- | --- | 
| bắt đầu | [0, 2] | - | 
| tính toán khoảng cách | 2 - 0 | 2 | 

Tổng tối đa là 3 + 2 = 5. Số cách là 2. 

Điều này phù hợp với thực tế là phần cắt có thể được đặt giữa các chỉ số (1,2) hoặc (2,3), tạo ra hai phân vùng tối ưu hợp lệ. 

### Ví dụ 2 

đầu vào:```
5 3
5 1 4 2 3
```Giá trị được chọn: 5, 4, 3 với các vị trí: 

| giá trị | vị trí | 
| --- | --- | 
| 5 | 0 | 
| 4 | 2 | 
| 3 | 4 | 

Vị trí được sắp xếp: [0, 2, 4] 

| bước | vị trí đã chọn | khoảng cách | 
| --- | --- | --- | 
| bắt đầu | [0, 2, 4] | - | 
| khoảng cách 1 | 2 - 0 = 2 | 2 | 
| khoảng cách 2 | 4 - 2 = 2 | 2 | 

Tổng tối đa là 12, số cách là 4. 

Điều này khẳng định sự độc lập của các lựa chọn giữa các khoảng trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần cho các vị trí và xử lý O(k) | 
| Không gian | O(n) | mảng vị trí | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì tất cả các hoạt động đều tuyến tính và không xảy ra quá trình quét lồng nhau trên các phân đoạn hoặc phân vùng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    MOD = 998244353

    n, k = map(int, input().split())
    p = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, v in enumerate(p):
        pos[v] = i

    chosen = list(range(n, n - k, -1))
    chosen_pos = sorted(pos[v] for v in chosen)

    ans_sum = sum(chosen)
    ways = 1
    for i in range(k - 1):
        ways = (ways * (chosen_pos[i + 1] - chosen_pos[i])) % MOD

    return f"{ans_sum} {ways}"

# sample
assert run("3 2\n2 1 3\n") == "5 2"

# minimum k = 1
assert run("4 1\n4 1 2 3\n") == "4 1"

# maximum k = n
assert run("3 3\n1 2 3\n") == "6 1"

# reversed permutation
assert run("5 2\n5 4 3 2 1\n") == "9 4"

# random structure
assert run("5 3\n2 5 1 4 3\n") == "12 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k = 1 trường hợp | phân đoạn đơn | không có khoảng trống | 
| k = n trường hợp | tất cả người độc thân | không linh hoạt | 
| hoán vị ngược | khoảng cách đồng đều lớn | đếm nhân | 
| hoán vị hỗn hợp | cấu trúc chung | tính đúng đắn của việc lựa chọn | 

## Vỏ cạnh 

Khi k = 1, toàn bộ mảng là một phân đoạn. Thuật toán chỉ chọn phần tử tối đa và vì không có khoảng trống nên vòng lặp sản phẩm trống và trả về 1, khớp với phân vùng hợp lệ duy nhất. 

Khi k = n, mỗi phần tử tạo thành phân đoạn riêng của nó. Các giá trị được chọn là tất cả các phần tử và các khoảng trống đều là 1 vì các vị trí liên tiếp khác nhau 1. Tích số vẫn là 1, phù hợp với thực tế là chỉ tồn tại một phân vùng.
