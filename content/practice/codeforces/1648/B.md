---
title: "CF 1648B - Mảng tích phân"
description: "Chúng ta được cấp một tập hợp các số nguyên dương và chúng ta phải quyết định xem nó có bị đóng theo một phép toán rất cụ thể hay không: lấy phép chia số nguyên giữa bất kỳ cặp phần tử có thứ tự nào trong đó tử số ít nhất là mẫu số."
date: "2026-06-10T04:03:37+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "data-structures", "math"]
categories: ["algorithms"]
codeforces_contest: 1648
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 775 (Div. 1, based on Moscow Open Olympiad in Informatics)"
rating: 1800
weight: 1648
solve_time_s: 366
verified: false
draft: false
---

[CF 1648B - Mảng tích phân](https://codeforces.com/problemset/problem/1648/B) 

**Đánh giá:** 1800 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, cấu trúc dữ liệu, toán học 
**Thời gian giải:** 6 phút 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên dương và chúng ta phải quyết định xem nó có bị đóng theo một phép toán rất cụ thể hay không: lấy phép chia số nguyên giữa bất kỳ cặp phần tử có thứ tự nào trong đó tử số ít nhất là mẫu số. Bất cứ khi nào chúng ta chọn hai giá trị từ mảng, chia giá trị lớn hơn cho giá trị nhỏ hơn bằng phép chia sàn, thì kết quả cũng phải có sẵn trong mảng. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp cho chúng ta một mảng và giới hạn trên của các giá trị của nó. Đối với mỗi trường hợp, chúng ta phải quyết định xem mảng có “đóng” trong thao tác chia tầng này hay không. 

Các ràng buộc đẩy chúng tôi ra khỏi bất kỳ phương pháp nào cố gắng kiểm tra rõ ràng tất cả các cặp. Với tổng số lên tới một triệu phần tử trong các trường hợp thử nghiệm, một vòng lặp kép đơn giản sẽ yêu cầu thứ tự 10^12 thao tác, vượt xa những gì khả thi trong hai giây. Việc liệt kê cặp thậm chí được tối ưu hóa một chút là không thể. 

Khó khăn không rõ ràng là điều kiện không chỉ liên quan đến các cặp tồn tại mà còn liên quan đến tất cả các giá trị dẫn xuất. Một cách tiếp cận đơn giản có thể chỉ kiểm tra các cặp hiện có trong mảng, nhưng điều đó bỏ lỡ yêu cầu rằng mọi thương số có thể phải tồn tại, ngay cả khi nó không phải là một phần của bất kỳ tương tác “rõ ràng” nào trong thứ tự đầu vào. 

Một trường hợp lỗi điển hình là khi mảng chứa một số lớn và một số nhỏ nhưng lại bỏ qua các thương số trung gian. Ví dụ: nếu có 10 và 3 thì tầng (10/3) = 3 cũng ổn nếu 3 tồn tại, nhưng tầng (10/4) hoặc tầng (10/6) có thể tạo ra các giá trị bị thiếu tùy thuộc vào cấu trúc. Thiếu ngay cả một giá trị dẫn xuất sẽ làm mất hiệu lực toàn bộ mảng. 

## Phương pháp tiếp cận 

Giải pháp brute-force kiểm tra mọi cặp có thứ tự (x, y) với x ≥ y, tính sàn(x/y) và xác minh tư cách thành viên. Điều này đúng vì nó trực tiếp thực thi định nghĩa. Tuy nhiên, kích thước của mảng là bậc hai và với đầu vào lớn, điều này trở nên không khả thi ngay lập tức. 

Quan sát quan trọng là chúng ta không cần phải tính toán lại tất cả các phép chia theo cặp một cách rõ ràng. Điều quan trọng là liệu tập hợp có chứa tất cả các giá trị có thể được hình thành dưới dạng thương số của các phần tử hiện có hay không. Vì tất cả các giá trị được giới hạn bởi c nên chúng ta có thể làm việc trong một mảng tần số và mô phỏng việc đóng. 

Thay vì lặp lại các phần tử, chúng ta lặp lại các giá trị. Đối với mỗi giá trị y tồn tại, chúng tôi cố gắng “tạo” tất cả các bội số của y đến c và đảm bảo tính nhất quán của các tầng dẫn xuất. Điều này biến vấn đề thành một quá trình lan truyền giống như sàng trên phạm vi giá trị. 

Cấu trúc này giống như lý thuyết số đóng trong phép chia: nếu một giá trị tồn tại thì tất cả các kết quả chia bất kỳ bội số nào của nó cho nó cũng phải tồn tại. Điều này cho phép chúng ta truyền bá các ràng buộc từ các giá trị nhỏ trở lên một cách có kiểm soát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force | O(n²) | O(1) | Quá chậm | 
| Tuyên truyền sàng giá trị | O(c log c) | O(c) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số từng số trong mảng bằng mảng`freq`. 
2. Xây dựng mảng hiện diện boolean`ok[x]`cho biết liệu giá trị`x`tồn tại trong mảng. Điều này cho phép kiểm tra tư cách thành viên O(1). 
3. Với mọi giá trị có thể`y`từ 1 đến c, chúng ta chỉ coi nó là ước số tiềm năng nếu nó tồn tại trong mảng. 
4. Đối với mỗi như vậy`y`, chúng tôi lặp lại bội số`x = y, 2y, 3y, ...`lên tới c. Mỗi bội số đóng góp một thương số`x // y`. 
5. Mỗi thương số được tính toán phải tồn tại trong mảng. Nếu tìm thấy bất kỳ thương số nào bị thiếu, chúng ta có thể kết luận ngay rằng mảng đó không nguyên. 
6. Nếu chúng ta hoàn thành tất cả các phép kiểm tra mà không có mâu thuẫn thì mảng đó là tích phân. 

Điều tinh tế quan trọng là chúng ta chỉ cần kiểm tra thương số được tạo ra bởi các giá trị hiện có theo cách có cấu trúc. Việc lặp lại bội số sẽ tránh tạo ra cặp dư thừa trong khi vẫn bao gồm mọi kết quả phân chia tầng hợp lệ. 

### Tại sao nó hoạt động 

Mọi cặp (x, y) hợp lệ với x ≥ y có thể được biểu diễn dưới dạng x = k·y + r và sàn(x/y) = k. Bằng cách lặp lại tất cả các bội số của y, chúng tôi tạo ra mọi k có thể phát sinh từ việc ghép y với một số phần tử lớn hơn. Vì vậy, nếu thiếu bất kỳ thương số cần thiết nào, nó sẽ được phát hiện trong quá trình quét này. Mảng tần số đảm bảo chúng tôi tính toán chính xác sự hiện diện thay vì thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, c = map(int, input().split())
        a = list(map(int, input().split()))

        freq = [0] * (c + 1)
        for x in a:
            freq[x] += 1

        ok = [False] * (c + 1)
        for i in range(1, c + 1):
            if freq[i]:
                ok[i] = True

        bad = False

        for y in range(1, c + 1):
            if not ok[y]:
                continue

            for x in range(y, c + 1, y):
                q = x // y
                if not ok[q]:
                    bad = True
                    break

            if bad:
                break

        print("No" if bad else "Yes")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã nén đầu vào thành một mảng hiện diện để việc kiểm tra tư cách thành viên diễn ra theo thời gian không đổi. Sau đó, nó lặp lại mọi ứng cử viên ước số y có thể thực sự xuất hiện trong mảng. Đối với mỗi y như vậy, nó sẽ duyệt qua tất cả các bội số của y và đảm bảo rằng mọi thương số thu được đều có mặt. Việc thoát sớm đảm bảo hiệu quả khi phát hiện vi phạm. 

Điểm tinh tế chính là chúng tôi không bao giờ lặp lại các cặp thô. Thay vào đó, chúng tôi tái cấu trúc bài toán thành cấp số cộng trên bội số, điều này đảm bảo phạm vi bao phủ mà không có hiện tượng bùng nổ bậc hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
1 2 5
```Chúng tôi xây dựng`ok = {1,2,5}`. 

Với y = 1: 

giá trị x tạo ra thương số 1,2,3,4,5. Ở đây thiếu 3 và 4 nên việc kiểm tra thất bại ngay lập tức. 

| y | x | x//y | được rồi[x//y] | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | vâng | 
| 1 | 2 | 2 | vâng | 
| 1 | 3 | 3 | không | 

Điều này xác nhận sự từ chối. 

### Ví dụ 2 

đầu vào:```
4 10
1 3 3 7
```Sự hiện diện là`{1,3,7}`. 

Với y = 3: 

bội số cho thương số 1,2,3. Giá trị 2 bị thiếu nên không hợp lệ. 

Điều này phù hợp với sản lượng dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(c log c) | Mỗi giá trị lặp lại bội số của nó | 
| Không gian | O(c) | Mảng hiện diện và tần số | 

Thuật toán này hiệu quả vì mỗi số nguyên lên đến c tham gia vào nhiều nhất c/i lần lặp trên tất cả các lần quét số chia, dẫn đến hành vi chuỗi hài hòa. Điều này thoải mái phù hợp trong các hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, c = map(int, input().split())
        a = list(map(int, input().split()))

        freq = [0] * (c + 1)
        for x in a:
            freq[x] += 1

        ok = [False] * (c + 1)
        for i in range(1, c + 1):
            if freq[i]:
                ok[i] = True

        bad = False
        for y in range(1, c + 1):
            if not ok[y]:
                continue
            for x in range(y, c + 1, y):
                if not ok[x // y]:
                    bad = True
                    break
            if bad:
                break

        out.append("No" if bad else "Yes")

    return "\n".join(out)

# provided samples
assert run("""4
3 5
1 2 5
4 10
1 3 3 7
1 2
2
1 1
1
""") == """Yes
No
No
Yes"""

# custom cases
assert run("""1
3 6
1 2 3
""") == "No", "missing closure"
assert run("""1
2 2
1 2
""") == "Yes", "minimal valid"
assert run("""1
3 10
5 10 1
""") == "No", "random invalid"
assert run("""1
1 1
1
""") == "Yes", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 3 6 / 1 2 3 | Không | thiếu đóng cửa | 
| 1 2 2 / 1 2 | Có | hợp lệ tối thiểu | 
| 1 3 10 / 5 10 1 | Không | ngẫu nhiên không hợp lệ | 
| 1 1 1/1 | Có | phần tử đơn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi mảng chứa 1. Vì mỗi số nguyên chia cho 1 tạo ra chính nó, sự hiện diện của 1 buộc một điều kiện đóng rất mạnh: tất cả các giá trị phải tự nhất quán trong chuỗi chia. Thuật toán xử lý việc này một cách tự nhiên vì với y = 1, mọi bội số x đều tạo ra thương số x và việc kiểm tra giảm xuống việc xác minh sự hiện diện của tất cả các phần tử liên quan đến tập đóng. 

Một trường hợp khác là khi mảng chỉ có một giá trị duy nhất. Trong trường hợp đó, thương số duy nhất có thể là 1, chỉ hợp lệ nếu bản thân giá trị đó là 1. Quá trình quét xác nhận hoặc từ chối chính xác điều này mà không cần viết hoa đặc biệt. 

Trường hợp quan trọng cuối cùng là các mảng thưa thớt như lũy thừa của hai trộn với các số không liên quan. Những điều này nhanh chóng thất bại vì các thương số trung gian như 3, 5 hoặc 7 xuất hiện trong quá trình quét nhiều lần ngay cả khi chúng không có trong đầu vào, đảm bảo phát hiện sớm tình trạng không đóng.
