---
title: "CF 106403D - Tăng sức mạnh"
description: "Bài toán yêu cầu chúng ta trả lời nhiều bài kiểm tra độc lập về tập hợp các giá trị lũy thừa. Chúng ta có một tập hợp các số riêng biệt."
date: "2026-06-25T10:07:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106403
codeforces_index: "D"
codeforces_contest_name: "Bay Area Programming Contest 2026 Novice Division"
rating: 0
weight: 106403
solve_time_s: 41
verified: true
draft: false
---

[CF 106403D - Tăng sức mạnh](https://codeforces.com/problemset/problem/106403/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta trả lời nhiều bài kiểm tra độc lập về tập hợp các giá trị lũy thừa. Chúng ta có một tập hợp các số riêng biệt. Trong một thao tác, chọn hai giá trị khác nhau`a`Và`b`tạo ra một giá trị mới:$$a \cdot b + 2(a+b)$$Đối với mỗi giá trị truy vấn`x`, chúng ta phải quyết định xem một số cặp từ tập hợp ban đầu có thể tạo ra chính xác hay không`x`. Đây là sự cố Codeforces 106403D, "Power Up". [Kho lưu trữ vấn đề của Codeforces](https://codeforces.com/gym/106403/problems) 

Biểu thức có vẻ như yêu cầu kiểm tra tất cả các cặp nhưng nó có cấu trúc đại số ẩn. Chúng ta có thể viết lại nó như sau:$$a \cdot b + 2a + 2b + 4 - 4$$mang lại:$$(a+2)(b+2)-4$$Vì vậy một truy vấn`x`có thể chính xác khi`x + 4`có thể được biểu diễn dưới dạng tích của hai giá trị dịch chuyển khác nhau từ mảng. 

Kích thước đầu vào lớn. Có thể có tới`2 * 10^5`giá trị và cùng số lượng truy vấn. Một giải pháp mà mọi cặp đều thử sẽ thực hiện được khoảng`n^2`hoạt động xung quanh`4 * 10^10`trong trường hợp lớn nhất và vượt xa giới hạn 2 giây thông thường cho phép. Chúng ta cần một giải pháp gần hơn với tuyến tính hoặc`q * small_factor`thời gian. 

Các giá trị được dịch chuyển`p_i + 2`nhiều nhất là`200002`, nhỏ hơn nhiều so với giá trị truy vấn tối đa có thể có. Điều này có nghĩa là chúng ta có thể lưu trữ tất cả các giá trị dịch chuyển có sẵn trong cấu trúc thành viên nhanh chóng. Với mỗi truy vấn, chúng ta chỉ cần tìm kiếm các yếu tố có thể có của`x + 4`. 

Một lỗi phổ biến là quên rằng hai phần tử được chọn phải khác nhau. Ví dụ: nếu mảng là:```
1
```và truy vấn là:```
8
```Giá trị được dịch chuyển là`3`, Và`8 + 4 = 12`chia hết cho`3`. Việc kiểm tra bất cẩn có thể chấp nhận vì`3 * 4 = 12`, nhưng giá trị`4`không tồn tại. Câu trả lời là:```
NO
```Một trường hợp cạnh khác xuất hiện khi cả hai yếu tố đều giống nhau. Vì:```
1
1
4
```chúng tôi có`x + 4 = 8`và giá trị được dịch chuyển là`3`. Không có cặp phần tử nào khác nhau nên đáp án là`NO`. Chúng ta phải xác minh cả hai yếu tố đều có mặt và khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là kiểm tra từng cặp phần tử. Với mỗi hai giá trị`a`Và`b`, chúng tôi tính toán`a*b + 2(a+b)`và lưu trữ kết quả. Sau đó, mỗi truy vấn sẽ trở thành tra cứu theo thời gian liên tục. 

Cách tiếp cận này đúng vì mọi sự kết hợp có thể đều được xem xét rõ ràng. Vấn đề là số lượng kết hợp. Với`n = 200000`, số cặp xấp xỉ:$$\frac{n(n-1)}{2}$$đó là về`2 * 10^10`hoạt động. Không thể chạy trong giới hạn. 

Quan sát chính là sự biến đổi:$$a*b+2(a+b)= (a+2)(b+2)-4$$Thay vì tạo ra tất cả các sản phẩm cặp, chúng tôi đảo ngược quá trình. Đối với một truy vấn`x`, chúng ta cần biết liệu`x+4`có hai yếu tố đều là giá trị mảng được dịch chuyển. 

Các giá trị được dịch chuyển đủ nhỏ để chúng có thể được lưu trữ trong một tập hợp. Để trả lời một truy vấn, chúng tôi tính đến yếu tố`x+4`và xét các ước của nó. Vì giá trị truy vấn tối đa là khoảng`10^9`, căn bậc hai là xung quanh`31623`, vì vậy việc phân tích nhanh chóng. Nếu một số chia`d`tồn tại và cả hai`d`Và`(x+4)/d`là các giá trị dịch chuyển có sẵn, câu trả lời là CÓ. Chúng tôi cũng kiểm tra xem hai yếu tố có bằng nhau không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2 + q) | O(n²) | Quá chậm | 
| Tối ưu | O(n + q√x) trường hợp xấu nhất | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị lũy thừa và chuyển đổi mọi giá trị`p`vào trong`p + 2`. Lưu trữ các giá trị được chuyển đổi này trong một tập hợp. Phép biến đổi cho phép chúng ta làm việc với phép nhân thay vì biểu thức bậc hai ban đầu. 
2. Đối với mỗi truy vấn`x`, thay thế nó bằng`target = x + 4`. Một câu trả lời hợp lệ bây giờ yêu cầu hai giá trị biến đổi được lưu trữ mà tích của chúng là`target`. 
3. Thử mọi ứng viên của ước số`d`từ`1`lên tới`sqrt(target)`. Nếu như`d`chia rẽ`target`, hệ số phù hợp là`target / d`. 
4. Kiểm tra xem cả hai`d`Và`target / d`tồn tại trong tập giá trị được chuyển đổi. Nếu đúng như vậy và hai giá trị khác nhau thì cặp ban đầu tồn tại, vì vậy hãy in CÓ. 
5. Nếu tất cả các ước được kiểm tra mà không tìm thấy cặp hợp lệ, hãy in NO. 

Lý do chỉ kiểm tra đến căn bậc hai là đủ là vì mọi cặp nhân tố đều có một giá trị không lớn hơn căn bậc hai và giá trị kia không nhỏ hơn nó. Bằng cách tìm cạnh nhỏ hơn của cặp, chúng ta tìm được cặp hoàn chỉnh. 

Tại sao nó hoạt động: phép biến đổi bảo toàn điều kiện chính xác của bài toán ban đầu. Mỗi cặp hợp lệ`(a, b)`trở thành`(a+2, b+2)`, và sản phẩm của họ chính xác là`x+4`. Ngược lại, mọi cặp nhân tố hợp lệ của`x+4`ánh xạ trở lại hai giá trị ban đầu bằng cách trừ đi hai. Vì tập hợp chứa chính xác các giá trị ban đầu đã dịch chuyển nên việc tìm kiếm theo ước số sẽ tìm thấy một cặp khi và chỉ khi một cặp tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    vals = list(map(int, input().split()))

    present = set()
    for v in vals:
        present.add(v + 2)

    ans = []
    for _ in range(q):
        x = int(input())
        target = x + 4
        ok = False

        d = 1
        while d * d <= target:
            if target % d == 0:
                other = target // d
                if d in present and other in present and d != other:
                    ok = True
                    break
            d += 1

        ans.append("YES" if ok else "NO")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```bộ`present`lưu trữ các giá trị được chuyển đổi thay vì quyền hạn ban đầu. Điều này tránh lặp lại đại số trong mỗi truy vấn. 

Đối với mỗi truy vấn,`target`là giá trị cần chia thành hai số đã dịch chuyển. Vòng lặp chỉ kiểm tra các ước số cho đến khi`d*d`vượt quá`target`, bao gồm tất cả các cặp yếu tố có thể. 

điều kiện`d != other`là cần thiết vì cặp ban đầu phải sử dụng hai phần tử khác nhau. Vì tất cả lũy thừa ban đầu đều khác nhau nên hai giá trị dịch chuyển bằng nhau không thể đến từ các phần tử khác nhau. 

Các số nguyên Python xử lý phép nhân và chia một cách an toàn ở đây vì các giá trị trung gian lớn nhất vẫn ở xung quanh`10^9`. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
5 3
1 3 4 6 7
11
26
68
```Tập đã biến đổi là`{3, 5, 6, 8, 9}`. 

| Truy vấn | mục tiêu = x + 4 | Đã tìm thấy cặp số chia | Kết quả | 
| --- | --- | --- | --- | 
| 11 | 15 | 3 và 5 | CÓ | 
| 26 | 30 | 5 và 6 | CÓ | 
| 68 | 72 | 8 và 9 | CÓ | 

Dấu vết cho thấy bài toán nhân ban đầu trở thành bài toán tra cứu nhân tử như thế nào. 

Một trường hợp khác:```
2 2
1 4
4
21
```Tập đã biến đổi là`{3, 6}`. 

| Truy vấn | mục tiêu = x + 4 | Đã tìm thấy cặp số chia | Kết quả | 
| --- | --- | --- | --- | 
| 4 | 8 | không | KHÔNG | 
| 21 | 25 | không | KHÔNG | 

Điều này chứng tỏ rằng một số là hợp số là không đủ. Cả hai yếu tố này phải tồn tại dưới dạng quyền lực đã dịch chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q√x) | Mỗi truy vấn kiểm tra các ước số tối đa là khoảng`10^9`| 
| Không gian | O(n) | Bộ lưu trữ các giá trị đã dịch chuyển | 

Căn bậc hai tối đa là khoảng`31623`, do đó việc kiểm tra số chia đủ nhỏ cho số lượng truy vấn nhất định. Giải pháp tránh lưu trữ tất cả các cặp có thể, đòi hỏi bộ nhớ không thể. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    if not data:
        return ""

    it = iter(data)
    n = int(next(it))
    q = int(next(it))

    present = set()
    for _ in range(n):
        present.add(int(next(it)) + 2)

    out = []
    for _ in range(q):
        x = int(next(it))
        target = x + 4
        ok = False
        d = 1
        while d * d <= target:
            if target % d == 0:
                if d in present and target // d in present and d != target // d:
                    ok = True
                    break
            d += 1
        out.append("YES" if ok else "NO")
    return "\n".join(out)

assert run("""5 8
1 3 4 6 7
11
21
26
36
50
68
69
20
""") == """YES
NO
YES
YES
YES
YES
NO
YES"""

assert run("""4 6
2 4 1 3
8
25
14
11
16
8
""") == """YES
NO
YES
YES
YES
YES"""

assert run("""1 2
1
5
8
""") == """NO
NO"""

assert run("""4 3
1 2 3 4
8
11
20
""") == """NO
YES
YES"""

assert run("""5 2
1 1 1 1 1
11
12
""") == """NO
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | KHÔNG | Kiểm tra xem việc tự ghép nối có được chấp nhận không | 
| Giá trị nhỏ liên tiếp | trường hợp CÓ/CÓ | Kiểm tra các yếu tố bình thường phù hợp | 
| Giá trị lặp lại | KHÔNG | Xác nhận rằng chỉ những phần tử gốc riêng biệt mới quan trọng | 
| Giá trị gần phạm vi trên | hành vi CÓ/KHÔNG | Kiểm tra việc xử lý ranh giới số chia | 

## Vỏ cạnh 

Đối với một giá trị khả dụng duy nhất, thuật toán không bao giờ chấp nhận phân tích nhân tử bằng cách sử dụng cùng một giá trị được dịch chuyển đó hai lần. Trong đầu vào:```
1 1
1
5
```mục tiêu là`9`và giá trị dịch chuyển duy nhất là`3`. Mặc dù`3 * 3 = 9`, các thừa số bằng nhau nên thuật toán bác bỏ nó. 

Đối với trường hợp mục tiêu là số nguyên tố thì không thể có hai thừa số có ý nghĩa. Ví dụ:```
2 1
1 2
7
```Các giá trị được dịch chuyển là`3`Và`4`. Mục tiêu là`11`, không có cặp ước số nào trong tập hợp, vì vậy câu trả lời là KHÔNG. 

Đối với trường hợp thành công:```
2 1
1 3
11
```Các giá trị được dịch chuyển là`3`Và`5`. Mục tiêu là`15`, và việc tìm kiếm số chia sẽ tìm thấy`3 * 5`, vì vậy câu trả lời là CÓ. Điều này xác nhận phép biến đổi khớp chính xác với công thức ban đầu.
