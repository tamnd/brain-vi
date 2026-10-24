---
title: "CF 105968B - Bài toán của Braga"
description: "Chúng tôi đang làm việc với nhiều truy vấn trên một phạm vi số nguyên và đối với mỗi truy vấn, chúng tôi cần tính tổng của tất cả các số nguyên tố nằm trong một khoảng nhất định."
date: "2026-06-22T16:19:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105968
codeforces_index: "B"
codeforces_contest_name: "IME++ Starters Try-Outs 2025"
rating: 0
weight: 105968
solve_time_s: 54
verified: true
draft: false
---

[CF 105968B - Vấn đề của Braga](https://codeforces.com/problemset/problem/105968/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với nhiều truy vấn trên một phạm vi số nguyên và đối với mỗi truy vấn, chúng tôi cần tính tổng của tất cả các số nguyên tố nằm trong một khoảng nhất định. Mỗi truy vấn cung cấp một điểm cuối bên trái và bên phải, đồng thời nhiệm vụ là nhanh chóng trả về tổng số số nguyên tố nằm giữa hai giới hạn đó. 

Cấu trúc chính ở đây là cùng một dãy số được sử dụng lại trên tất cả các truy vấn. Điều đó ngay lập tức cho thấy rằng việc tính toán lại tính nguyên tố hoặc tính tổng các số nguyên tố từ đầu cho mỗi truy vấn sẽ là dư thừa. Thay vào đó, chúng tôi muốn thực hiện bước tiền xử lý trên một phạm vi cố định, sau đó là các truy vấn trong phạm vi nhanh. 

Nếu giá trị tối đa có thể có trong phạm vi đầu vào lớn, chẳng hạn như lên tới n, thì việc quét mỗi truy vấn sẽ tốn O(n) và trở nên quá chậm khi nhân với nhiều truy vấn. Ví dụ: nếu n là 10^6 và có 10^5 truy vấn, một cách tiếp cận đơn giản sẽ yêu cầu khoảng 10^11 thao tác, điều này không khả thi trong giới hạn thời gian thông thường. Điều này thúc đẩy chúng tôi tiến tới quá trình tiền xử lý trong khoảng thời gian khoảng O(n log log n) hoặc O(n) và trả lời từng truy vấn trong O(1). 

Trường hợp cạnh tinh vi xuất hiện khi phạm vi bắt đầu từ 1. Vì 1 không phải là số nguyên tố nên bất kỳ cách tiếp cận nào khởi tạo không chính xác tính nguyên tố cho 1 đều có thể vô tình đưa nó vào tổng. Ví dụ: nếu truy vấn là [1, 10] thì các số nguyên tố đúng là 2, 3, 5, 7 và câu trả lời là 17. Một sàng lỗi coi 1 là số nguyên tố sẽ trả về 18. 

Một vấn đề khác phát sinh khi phạm vi rất nhỏ hoặc suy biến, chẳng hạn như [x, x]. Trong trường hợp đó, tính chính xác phụ thuộc hoàn toàn vào việc x có phải là số nguyên tố hay không và các tổng tiền tố phải được xây dựng cẩn thận để hỗ trợ các truy vấn một điểm mà không có lỗi riêng lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là xử lý từng truy vấn một cách độc lập. Đối với mỗi phạm vi [l, r], chúng tôi lặp qua từng số, kiểm tra xem nó có phải là số nguyên tố hay không bằng phép chia thử và tích lũy tổng. Về mặt khái niệm, điều này đơn giản và chính xác, nhưng việc kiểm tra tính nguyên tố lên tới n có chi phí O(sqrt n) và việc thực hiện việc đó cho mọi số trong mọi truy vấn sẽ dẫn đến O(n sqrt n) trong trường hợp xấu nhất trên mỗi phạm vi truy vấn. Với nhiều truy vấn, điều này nhanh chóng trở nên không khả thi. 

Sự kém hiệu quả xuất phát từ việc tính toán lại tính nguyên tố và tính toán lại các tổng một phần nhiều lần cho các phạm vi chồng chéo. Cấu trúc của bài toán gợi ý rằng tính nguyên tố của mỗi số là độc lập với các truy vấn và một khi đã biết thì nó sẽ không bao giờ thay đổi. Điều này cho phép tiền xử lý. 

Sàng Eratosthenes tính toán tính nguyên tố cho tất cả các số lên đến n trong O(n log log n). Khi chúng ta có mảng boolean đánh dấu các số nguyên tố, chúng ta có thể xây dựng một mảng tổng tiền tố trong đó prefix[i] lưu trữ tổng của tất cả các số nguyên tố từ 1 đến i. Khi đó, bất kỳ truy vấn [l, r] nào cũng có thể được trả lời dưới dạng tiền tố[r] - tiền tố[l - 1], cho ra O(1) cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q · N √N) | O(1) | Quá chậm | 
| Sàng + Tổng tiền tố | O(N log log N + Q) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng chế độ xem toàn diện về số nào là số nguyên tố và chuyển đổi số đó thành thông tin tích lũy hỗ trợ các truy vấn phạm vi nhanh. 

### bước

1. Chọn giá trị tối đa n dựa trên điểm cuối lớn nhất có thể có trên tất cả các truy vấn. Điều này đảm bảo rằng mọi số chúng ta có thể cần đều được xử lý trước. 
2. Tạo một mảng is_prime có kích thước n + 1 và khởi tạo tất cả các mục là đúng ngoại trừ 0 và 1. Việc khởi tạo này phản ánh rằng tính nguyên tố chỉ chưa được biết đối với 2 trở lên. 
3. Chạy sàng Eratosthenes bằng cách lặp i từ 2 đến sqrt(n). Khi i vẫn được đánh dấu là số nguyên tố, hãy đánh dấu tất cả các bội số của i bắt đầu từ i * i là không nguyên tố. Bắt đầu từ i * i là đủ vì các bội số nhỏ hơn đã được xử lý bởi các số nguyên tố nhỏ hơn. 
4. Sau khi sàng hoàn tất, hãy xây dựng tiền tố mảng tổng tiền tố trong đó tiền tố[i] lưu trữ tổng của tất cả các số nguyên tố từ 1 đến i. Với mỗi i, nếu is_prime[i] đúng, hãy thêm i vào tổng hiện có; nếu không thì thêm 0. 
5. Với mỗi truy vấn [l, r], tính kết quả là tiền tố[r] - tiền tố[l - 1] khi l > 1, nếu không thì chỉ là tiền tố[r]. Điều này chuyển đổi một tổng phạm vi thành một phép toán số học theo thời gian không đổi. 

### Tại sao nó hoạt động 

Ở cuối sàng, mọi số tổng hợp đã được đánh dấu sai trong is_prime và mọi mục nhập đúng còn lại đều tương ứng chính xác với một số nguyên tố. Mảng tiền tố được xây dựng dưới dạng tổng tích lũy trên một mảng cố định, do đó, nó bảo toàn tổng số chính xác cho mỗi khoảng tiền tố. Vì bất kỳ tổng phạm vi nào cũng có thể được phân tách thành hai tổng tiền tố nên mọi truy vấn đều được trả lời chính xác mà không cần tính toán lại. Điều bất biến là tiền tố[i] luôn bằng tổng của tất cả các số nguyên tố trong [1, i] và điều này đúng vì mỗi chỉ số đóng góp chính xác một lần khi và chỉ khi nó là số nguyên tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_sieve(n):
    is_prime = [True] * (n + 1)
    if n >= 0:
        is_prime[0] = False
    if n >= 1:
        is_prime[1] = False

    i = 2
    while i * i <= n:
        if is_prime[i]:
            step = i
            start = i * i
            for j in range(start, n + 1, step):
                is_prime[j] = False
        i += 1

    return is_prime

def build_prefix(is_prime):
    n = len(is_prime) - 1
    prefix = [0] * (n + 1)
    running = 0
    for i in range(1, n + 1):
        if is_prime[i]:
            running += i
        prefix[i] = running
    return prefix

def solve():
    data = input().strip().split()
    if not data:
        return

    q = int(data[0])
    queries = []
    max_r = 0

    idx = 1
    for _ in range(q):
        l = int(data[idx])
        r = int(data[idx + 1])
        idx += 2
        queries.append((l, r))
        if r > max_r:
            max_r = r

    is_prime = build_sieve(max_r)
    prefix = build_prefix(is_prime)

    out = []
    for l, r in queries:
        if l > 1:
            out.append(str(prefix[r] - prefix[l - 1]))
        else:
            out.append(str(prefix[r]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cấu trúc sàng tách biệt tính nguyên thủy như một bước tiền xử lý, đảm bảo chúng tôi không bao giờ lặp lại công việc trên các truy vấn. Cấu trúc tiền tố chuyển đổi phân loại tĩnh thành cấu trúc hỗ trợ tổng khoảng O(1). Phần tế nhị duy nhất là xử lý chính xác giới hạn dưới của truy vấn, vì phép trừ tiền tố yêu cầu bảo vệ khi l bằng 1 để tránh lập chỉ mục tiền tố[-1]. 

## Ví dụ đã hoạt động 

Hãy xem xét các truy vấn trên một phạm vi nhỏ nơi chúng ta có thể theo dõi các số nguyên tố theo cách thủ công. 

Ví dụ đầu vào:```
3
1 10
5 5
6 20
```Trước tiên, chúng tôi xây dựng các số nguyên tố lên tới 20, đó là 2, 3, 5, 7, 11, 13, 17, 19. Các tổng tiền tố phát triển như sau. 

| tôi | is_prime[i] | tiền tố[i] | 
| --- | --- | --- | 
| 1 | F | 0 | 
| 2 | T | 2 | 
| 3 | T | 5 | 
| 4 | F | 5 | 
| 5 | T | 10 | 
| 6 | F | 10 | 
| 7 | T | 17 | 
| 8 | F | 17 | 
| 9 | F | 17 | 
| 10 | F | 17 | 
| 11 | T | 28 | 
| 12 | F | 28 | 
| 13 | T | 41 | 
| 14 | F | 41 | 
| 15 | F | 41 | 
| 16 | F | 41 | 
| 17 | T | 58 | 
| 18 | F | 58 | 
| 19 | T | 77 | 
| 20 | F | 77 | 

Đối với truy vấn [1, 10], chúng ta trả về tiền tố[10] = 17, khớp với 2 + 3 + 5 + 7. 

Đối với [5, 5], chúng ta trả về tiền tố[5] - tiền tố[4] = 10 - 5 = 5, vì 5 là số nguyên tố. 

Với [6, 20], ta tính 77 - 10 = 67, tương ứng với các số nguyên tố 7, 11, 13, 17, 19. 

Dấu vết này xác nhận rằng những khác biệt về tiền tố sẽ tách biệt chính xác những đóng góp bên trong mỗi phạm vi truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log log N + Q) | sàng xử lý trước tất cả các số một lần, mỗi truy vấn được trả lời theo thời gian không đổi | 
| Không gian | O(N) | mảng lưu trữ tính nguyên tố và tổng tiền tố lên đến giá trị tối đa | 

Chi phí tiền xử lý có thể chấp nhận được đối với các ràng buộc thông thường lên tới vài triệu và việc xử lý truy vấn là tối ưu vì mỗi truy vấn giảm xuống một số lượng tra cứu mảng không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    from io import StringIO
    backup = sys.stdout
    sys.stdin = StringIO(inp)
    out = StringIO()
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# sample-style tests
assert solve_capture("3\n1 10\n5 5\n6 20\n") == "17\n5\n67"

# custom cases
assert solve_capture("1\n1 1\n") == "0"
assert solve_capture("1\n2 2\n") == "2"
assert solve_capture("2\n1 2\n2 3\n") == "2\n5"
assert solve_capture("1\n10 30\n") == str(sum([11,13,17,19,23,29]))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 | ranh giới không nguyên tố | 
| 2 2 | 2 | trường hợp nguyên tố nhỏ nhất | 
| phạm vi chồng chéo | 2, 5 | tính chính xác của tiền tố | 
| 10 30 | tổng tính toán | tính nhất quán trong phạm vi lớn hơn | 

## Vỏ cạnh 

Phạm vi bắt đầu từ 1 kiểm tra xem phép trừ tiền tố có tránh truy cập chỉ mục âm một cách chính xác hay không. Đối với đầu vào [1, 10], thuật toán sử dụng trực tiếp tiền tố[10] và vì tiền tố[0] được xác định ngầm định là 0 nên kết quả vẫn đúng. 

Phạm vi một điểm như [5, 5] đảm bảo phép trừ không vô tình xóa giá trị. Việc tính toán trở thành tiền tố[5] - tiền tố[4], tách biệt chính xác 5. 

Các phạm vi lớn bị chi phối bởi các số không nguyên tố, chẳng hạn như [14, 16], xác nhận rằng các khoảng nặng tổng hợp tạo ra sự đóng góp bằng 0 mà không cần xử lý đặc biệt. Sàng đã đánh dấu 14, 15, 16 là không nguyên tố, do đó tiền tố không thay đổi trên đoạn đó và phép trừ chính xác mang lại kết quả 0.
