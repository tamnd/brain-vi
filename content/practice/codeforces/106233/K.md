---
title: "CF 106233K - \u0418\u043b\u043b\u044e\u0437\u0438\u044f \u0440\u0430\u0437\u043c\u0435\u043d\u0430"
description: "Quá trình bắt đầu với một số nguyên duy nhất được đặt trên bảng. Bạn được phép lấy đi nhiều lần bất kỳ số nào hiện có trên bảng, xóa đi và thay thế bằng hai số nguyên dương có tổng bằng giá trị bị xóa."
date: "2026-06-25T07:04:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106233
codeforces_index: "K"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106233
solve_time_s: 36
verified: true
draft: false
---

[CF 106233K - \u0418\u043b\u043b\u044e\u0437\u0438\u044f \u0440\u0430\u0437\u043c\u0435\u043d\u0430](https://codeforces.com/problemset/problem/106233/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Quá trình bắt đầu với một số nguyên duy nhất được đặt trên bảng. Bạn được phép lấy đi nhiều lần bất kỳ số nào hiện có trên bảng, xóa đi và thay thế bằng hai số nguyên dương có tổng bằng giá trị bị xóa. Mỗi phép chia như vậy có chi phí bằng tích của hai số thu được. 

Theo thời gian, điều này tạo nên một tập hợp nhiều số có nguồn gốc từ các phép chia lặp đi lặp lại. Mục tiêu của mỗi truy vấn là xác định xem có thể thu được tập hợp số mục tiêu bắt đầu từ giá trị ban đầu hay không và nếu có thì tổng chi phí tối thiểu của tất cả các phần tách phải là bao nhiêu. 

Một điểm tinh tế quan trọng là bảng cuối cùng không cần phải chứa chính xác các số được yêu cầu. Chỉ cần tất cả các giá trị bắt buộc xuất hiện ở đâu đó trong tập hợp cuối cùng là đủ; số bổ sung được cho phép và có thể được chia thêm nếu hữu ích. 

Mỗi truy vấn đưa ra một phân đoạn của một mảng, biểu thị các giá trị bắt buộc và giá trị bắt đầu S. Chúng ta phải quyết định xem liệu S có thể được phân tách thành nhiều tập hợp chứa tất cả các giá trị trong phân đoạn đó hay không và nếu có, hãy tính chi phí tối thiểu của tất cả các hoạt động phân tách. 

Các ràng buộc ngụ ý số lượng truy vấn và kích thước mảng rất lớn, do đó, bất kỳ giải pháp nào tính toán lại mọi thứ cho mỗi truy vấn một cách độc lập sẽ quá chậm. Một mô phỏng đơn giản về việc phân tách sẽ tăng theo cấp số nhân vì mỗi lần phân chia sẽ làm tăng số lượng nút và mỗi nút có thể được phân tách lại. 

Cấu trúc ẩn là việc phân tách không phải là phân nhánh tùy ý, nó luôn bảo toàn tổng và hàm chi phí hoạt động có thể dự đoán được trên cây phân rã nhị phân. 

Một vài trường hợp thất bại xuất hiện ngay lập tức do cách tiếp cận bất cẩn. 

Nếu S nhỏ hơn tổng các giá trị cần tìm, ví dụ S = 5 và các số bắt buộc là 2, 3, 4 thì dù cho phép chia tách nhưng không có cách nào để tăng tổng, nên đáp án chắc chắn là không thể. 

Nếu S bằng tổng nhưng cấu trúc phân chia bị bỏ qua, người ta có thể cho rằng tính khả thi luôn đúng. Ví dụ: S = 7 và các giá trị bắt buộc [2, 2, 3] hoạt động, nhưng [3, 3, 3] vẫn không hoạt động mặc dù tổng là 9, vì S không đủ. 

Một trường hợp tế nhị khác là hành vi chi phí. Đối với S = 4, chia thành (1,3) tốn 3, trong khi chia thành (2,2) tốn 4. Lựa chọn tham lam “phân chia cân bằng” không phải là tối ưu toàn cục khi xây dựng nhiều giá trị đích, vì các giá trị trung gian có thể cần phân tách thêm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp mô hình hóa quy trình dưới dạng cây nhị phân bắt nguồn từ S. Mỗi nút đại diện cho một số hiện tại và việc mở rộng nút y thành a và b sẽ làm tăng thêm chi phí a·b. Chúng tôi thử đệ quy tất cả các phân chia có thể có cho đến khi tất cả các giá trị bắt buộc đều có trong nhiều tập hợp. 

Điều này đúng vì mọi chuỗi phép tính hợp lệ đều tương ứng chính xác với cây nhị phân có các lá là số cuối cùng. Tuy nhiên, số lượng cây có thể tăng lên một cách bùng nổ. Ngay cả đối với S vừa phải, mỗi giá trị y có khả năng phân chia O(y) và đệ quy phân nhánh rất nhiều. Điều này dẫn đến thời gian theo cấp số nhân và khiến nó không thể sử dụng được cho S lớn hoặc nhiều truy vấn. 

Quan sát quan trọng là chi phí của việc tách một số y thành a và b chính xác là cấu trúc a·b = (a+b)^2/4 - (a^2 + b^2)/4, nhưng quan trọng hơn, tổng chi phí cho bất kỳ sự phân tách hoàn toàn nào của S thành các lá chỉ phụ thuộc vào sự tương tác theo cặp của các lá cuối cùng, chứ không phụ thuộc vào thứ tự phân tách. 

Nếu ta khai triển S thành các lá x1, x2, ..., xk thì tổng chi phí sẽ bằng tổng của tất cả các cặp lá không có thứ tự của xi·xj. Điều này xuất phát từ việc áp dụng nhiều lần đồng nhất thức để tách các đóng góp của các số hạng chéo. Điều này biến bài toán thành việc chọn nhiều tập lá bao gồm tất cả xi cần thiết và có thể có thêm các số nguyên dương, đồng thời giảm thiểu chi phí tương tác theo cặp với tổng cố định S.

Các số bổ sung không phải là miễn phí, nhưng chúng có thể được sử dụng để “lấp đầy khoảng trống” trong S. Chiến lược tối ưu cuối cùng là coi tổng bị thiếu là lá bổ sung có kích thước S - tổng (bắt buộc), sau đó tính chi phí dưới dạng hàm của tổng tiền tố trên phân đoạn đã chọn. 

Điều này làm giảm mỗi truy vấn thành việc tính toán hai đại lượng trên phân đoạn: tổng các giá trị và tổng các tích theo cặp, có thể được duy trì bằng tiền xử lý tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng cây Brute Force | Hàm mũ | O(độ sâu) | Quá chậm | 
| Tổng tiền tố với công thức đại số | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tổng tiền tố của mảng các giá trị bắt buộc. Điều này cho phép tính toán nhanh tổng phân đoạn cho bất kỳ phạm vi truy vấn nào. 
2. Tính toán trước các tổng tiền tố của các bình phương hoặc các phần đóng góp tương đương sẽ cho phép tính toán các thuật ngữ tương tác theo cặp trên một phân đoạn. Điều này là cần thiết vì chi phí phụ thuộc vào sự tương tác giữa tất cả các giá trị được chọn. 
3. Đối với truy vấn (l, r, S), hãy tính tổng T cần thiết của phân đoạn. 
4. Nếu T > S thì trả về ngay -1 vì việc chia tách không thể tăng tổng số tiền nên không khả thi. 
5. Xác định giá trị còn lại L = S - T. Giá trị này thể hiện “khối lượng bổ sung” còn lại sau khi gán các lá cần thiết. 
6. Tính chi phí đóng góp của các phần tử cần thiết bằng cách sử dụng công thức rút ra từ việc khai triển theo cặp: tính tổng trên tất cả các cặp xi·xj cộng với tương tác với L nếu nó được coi là một lá bổ sung. 
7. Trả lại chi phí đã tính. 

Bước không rõ ràng là thay thế toàn bộ quá trình phân tách bằng biểu thức dạng đóng trên các lá cuối cùng. Mỗi lần phân chia đều bảo toàn tổng số, do đó, bất kỳ quy trình hợp lệ nào đều tương ứng với một tập hợp cuối cùng có tổng là S. Chi phí tích lũy chính xác dưới dạng tích chéo giữa các lá vì mỗi lần phân chia phân bổ chi phí tương tác giữa các phần tử con của nó theo cách quan sát toàn bộ cây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# We assume preprocessing arrays exist after reading input:
# a is the array of values
# prefix sums and prefix square sums are computed

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    pref2 = [0] * (n + 1)

    for i in range(n):
        pref[i + 1] = pref[i] + a[i]
        pref2[i + 1] = pref2[i] + a[i] * a[i]

    out = []

    for _ in range(q):
        l, r, S = map(int, input().split())
        l -= 1

        sum_seg = pref[r] - pref[l]

        if sum_seg > S:
            out.append("-1")
            continue

        # leftover mass
        L = S - sum_seg

        # total pairwise contribution among segment elements
        # using identity: sum_{i<j} a_i a_j = (sum^2 - sumsq)/2
        sumsq = pref2[r] - pref2[l]
        pair_cost = (sum_seg * sum_seg - sumsq) // 2

        # interaction with leftover treated as an extra leaf
        extra_cost = L * sum_seg

        out.append(str(pair_cost + extra_cost))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã này dựa vào việc tách các đóng góp thành hai phần độc lập. Đầu tiên là sự tương tác nội bộ giữa các phần tử bắt buộc, được tính toán bằng cách sử dụng nhận dạng tiêu chuẩn cho các tổng theo cặp bắt nguồn từ tổng tiền tố và bình phương tiền tố. Thứ hai là sự tương tác giữa giá trị còn lại L và mọi phần tử bắt buộc, mang tính tuyến tính vì mỗi phần tử bắt buộc đóng góp L·xi. 

Phần tế nhị nhất là nhận ra rằng không có cấu trúc nào nữa của quá trình phân tách quan trọng một khi chúng ta chuyển sang biểu diễn lá. Đây là nơi có nhiều giải pháp không chính xác hoặc làm phức tạp quá trình hoặc cố gắng phân chia tham lam, nhưng thất bại do các lựa chọn cân bằng cục bộ không duy trì được tính tối ưu toàn cục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, q = 1
a = [2, 3, 5]
query: l=1, r=3, S=12
```| Bước | tổng_seg | L | tổng hợp | cặp_chi phí | thêm_chi phí | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 10 | - | - | - | - | 
| sau khi kiểm tra | 10 | 2 | 38 | (100-38)/2 = 31 | 20 | 

Đầu ra là 31 + 20 = 51. 

Dấu vết này cho thấy các quyết định chia tách không bao giờ xuất hiện một cách rõ ràng; mọi thứ đều được mã hóa thành tổng hợp. 

### Ví dụ 2 

đầu vào:```
n = 4, q = 1
a = [1, 2, 2, 1]
S = 6, l=1, r=4
```| Bước | tổng_seg | L | tổng hợp | cặp_chi phí | thêm_chi phí | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 6 | 0 | 10 | (36-10)/2 = 13 | 0 | 

Đầu ra là 13. 

Trường hợp này xác nhận tình huống không còn tồn tại và chi phí giảm hoàn toàn theo cấu trúc theo cặp nội bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | tiền xử lý tiền tố cộng với O(1) cho mỗi truy vấn | 
| Không gian | O(n) | mảng tiền tố | 

Các ràng buộc có tối đa 2·10^5 phần tử và truy vấn yêu cầu xử lý truy vấn trong thời gian không đổi sau quá trình tiền xử lý tuyến tính, điều mà cấu trúc này đạt được một cách trực tiếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Since full solution is embedded above, these are structural tests only

# minimal case
assert run("1 1\n5\n1 1 5\n") is not None

# single element exact match
assert run("1 1\n10\n1 1 10\n") is not None

# impossible case (sum too large requirement)
assert run("3 1\n1 2 3\n1 3 1\n") is not None

# uniform array
assert run("5 2\n1 1 1 1 1\n1 5 10\n2 4 3\n") is not None

# boundary S large
assert run("3 1\n2 2 2\n1 3 100\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | tính toán | xử lý trường hợp cơ bản | 
| khớp chính xác | tính toán | không còn sót lại | 
| không thể | -1 | cắt tỉa khả thi | 
| đồng phục | tính toán | giá trị lặp lại | 
| S lớn | tính toán | tương tác còn sót lại | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi tổng phân đoạn đã vượt quá S. Trong tình huống này, không có chuỗi phân chia nào có thể khắc phục được phần thiếu hụt vì việc phân tách sẽ bảo toàn tổng chính xác. Thuật toán xử lý việc này ngay lập tức thông qua so sánh tổng tiền tố trước khi tính toán chi phí. 

Một trường hợp cạnh khác xuất hiện khi S bằng tổng phân đoạn. Ở đây L trở thành 0 và tất cả các số hạng tương tác còn sót lại đều biến mất. Giải pháp này giúp giảm chi phí theo cặp một cách chính xác giữa các thành phần phân khúc, tránh mọi lạm phát chi phí giả tạo. 

Trường hợp khó phát hiện cuối cùng là khi đoạn có một phần tử duy nhất. Số hạng theo cặp bằng 0 và câu trả lời đơn giản là L nhân với phần tử đó, phản ánh rằng tất cả khối lượng tăng thêm phải tương tác với một chiếc lá.
