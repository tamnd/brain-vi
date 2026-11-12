---
title: "CF 1656B - Phép trừ"
description: "Chúng ta bắt đầu với một dãy số nguyên. Trong một thao tác, chúng ta chọn một phần tử, xóa nó khỏi mảng và trừ giá trị của nó khỏi mọi phần tử còn lại. Sau chính xác $n-1$ thao tác, chỉ còn lại một số."
date: "2026-06-10T03:32:17+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1656
codeforces_index: "B"
codeforces_contest_name: "CodeTON Round 1 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1100
weight: 1656
solve_time_s: 109
verified: true
draft: false
---

[CF 1656B - Phép trừ](https://codeforces.com/problemset/problem/1656/B) 

**Đánh giá:** 1100 
**Tags:** cấu trúc dữ liệu, tham lam, toán học, hai con trỏ 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một dãy số nguyên. Trong một thao tác, chúng ta chọn một phần tử, xóa nó khỏi mảng và trừ giá trị của nó khỏi mọi phần tử còn lại. 

Sau chính xác$n-1$hoạt động, chỉ còn lại một số. Câu hỏi đặt ra là liệu có tồn tại một số thứ tự loại bỏ để giá trị còn lại cuối cùng bằng với mục tiêu nhất định hay không$k$. 

Thoạt nhìn, đây có vẻ giống như một vấn đề mô phỏng hoặc tìm kiếm trạng thái vì mọi thao tác đều thay đổi tất cả các giá trị còn lại. Số lượng các chuỗi hoạt động có thể có là rất lớn, đại khái là$n!$, vì vậy việc khám phá trực tiếp chúng là không thể. 

Các ràng buộc đưa ra gợi ý rõ ràng rằng giải pháp mong muốn đơn giản hơn nhiều. Tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Điều này cho phép các thuật toán như$O(n)$hoặc$O(n \log n)$, nhưng bất kỳ giá trị bậc hai nào trong tổng kích thước đầu vào sẽ quá đắt. 

Phần khó khăn là nhận ra thông tin nào từ mảng thực sự quan trọng. Một người đọc ngây thơ có thể tập trung vào các phép biến đổi lặp đi lặp lại của mảng, nhưng giá trị cuối cùng hóa ra chỉ phụ thuộc vào sự khác biệt giữa các phần tử ban đầu. 

Một trường hợp không rõ ràng là khi mục tiêu đã xuất hiện trong mảng. 

đầu vào:```
1
2 17
17 0
```Đầu ra:```
YES
```Chỉ với hai phần tử, bỏ 0 để lại 17 không thay đổi. Một giải pháp chỉ tìm kiếm sự khác biệt bằng$k$giữa các trạng thái biến đổi riêng biệt có thể bỏ lỡ trường hợp đơn giản này. 

Một trường hợp tinh tế khác là khi tồn tại các giá trị trùng lặp. 

đầu vào:```
1
4 5
4 2 2 7
```Đầu ra:```
YES
```Câu trả lời đến từ cặp đôi$(2,7)$, chênh lệch của chúng là 5. Các bản sao không liên quan. Việc triển khai bất cẩn loại bỏ các bản sao quá sớm và sau đó đưa ra lý do không chính xác về các thao tác có thể tạo ra kết quả sai. 

Số âm cũng có thể. 

đầu vào:```
1
3 7
-5 2 9
```Đầu ra:```
YES
```Từ$2 - (-5) = 7$, câu trả lời là tích cực mặc dù một số giá trị là âm. Bất kỳ giải pháp nào giả định tất cả các số đều không âm sẽ thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi trình tự loại bỏ có thể. Đối với mỗi bước, chúng tôi chọn một trong các phần tử còn lại, cập nhật tất cả các phần tử khác và tiếp tục đệ quy. 

Cách tiếp cận này đúng vì nó khám phá rõ ràng mọi trình tự pháp lý. Vấn đề là số lượng khả năng. Với$n$có những phần tử$n!$lệnh loại bỏ có thể. Ngay cả đối với$n=15$, điều này đã vượt xa giới hạn thực tế. 

Để tìm ra giải pháp tốt hơn, chúng ta hãy xem xét điều gì xảy ra với giá trị còn lại cuối cùng. 

Giả sử hai phần tử$a$Và$b$vẫn ở một thời điểm nào đó. Nếu chúng ta loại bỏ$a$, giá trị cuối cùng trở thành$b-a$. Nếu chúng ta loại bỏ$b$, giá trị cuối cùng trở thành$a-b$. 

Quan sát quan trọng là mọi chuỗi thao tác cuối cùng đều tạo ra sự khác biệt giữa hai phần tử mảng ban đầu. Thực tế, nếu chúng ta muốn giá trị cuối cùng là$k$, phải tồn tại hai phần tử ban đầu có hiệu chính xác bằng$k$. 

Tại sao? Hãy xem xét hoạt động cuối cùng. Ngay trước nó, vẫn còn hai con số. Bản thân hai số đó có thể được truy nguyên về các phần tử ban đầu thông qua các phép trừ trước đó. Quan sát biên tập cho vấn đề này là việc đạt được$k$tương đương với việc tìm hai số ban đầu$x$Và$y$như vậy$y-x=k$. 

Điều này loại bỏ hoàn toàn sự cần thiết phải mô phỏng các hoạt động. Vấn đề trở thành: 

Mảng có chứa một cặp giá trị có hiệu chính xác bằng$k$? 

Sau khi được định dạng lại theo cách này, bộ băm sẽ giải quyết được vấn đề ngay lập tức. Với mọi giá trị$x$, kiểm tra xem$x+k$cũng tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và chèn tất cả giá trị vào tập băm. 
2. Đối với mọi phần tử$x$trong mảng, kiểm tra xem$x+k$tồn tại trong tập hợp. 
3. Nếu giá trị đó tồn tại, xuất ra`YES`. 

Cặp đôi$(x, x+k)$có sự khác biệt chính xác$k$, đó chính xác là điều kiện cần thiết. 
4. Nếu không có phần tử nào thỏa mãn điều kiện sau khi quét toàn bộ mảng, xuất ra`NO`. 

### Tại sao nó hoạt động 

Đặc tính quan trọng là quá trình vận hành có thể tạo ra$k$nếu và chỉ khi tồn tại hai phần tử mảng ban đầu có hiệu bằng nhau$k$. 

Hướng thuận là bất kỳ chuỗi thành công nào cuối cùng cũng tương ứng với sự khác biệt giữa hai giá trị ban đầu. Chiều ngược lại là nếu hai phần tử$x$Và$x+k$tồn tại, chúng ta có thể sắp xếp các phép toán sao cho giá trị cuối cùng trở thành$(x+k)-x=k$. 

Thuật toán kiểm tra chính xác điều kiện này. Nếu một cặp như vậy tồn tại, nó sẽ trả về`YES`; nếu không nó sẽ trả về`NO`. Vì điều kiện vừa cần vừa đủ nên thuật toán đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        s = set(a)

        found = False
        for x in a:
            if x + k in s:
                found = True
                break

        ans.append("YES" if found else "NO")

    sys.stdout.write("\n".join(ans))

solve()
```Việc thực hiện theo thuật toán trực tiếp. 

Một tập băm lưu trữ tất cả các giá trị mảng, đưa ra kết quả mong đợi$O(1)$kiểm tra thành viên. Đối với mỗi phần tử$x$, chúng tôi kiểm tra xem$x+k$đang có mặt. Trận đấu thành công đầu tiên là đủ để trả lời`YES`. 

Sử dụng một bộ là quan trọng. Một vòng lặp lồng nhau sẽ yêu cầu$O(n^2)$so sánh, điều đó là không cần thiết. Tổng kích thước đầu vào đạt$2 \cdot 10^5$, vì vậy xử lý tuyến tính được ưa thích hơn. 

Không có vấn đề tràn trong Python vì số nguyên có độ chính xác tùy ý. Dù sao thì các giá trị vẫn nằm trong phạm vi số nguyên bình thường. 

Điều kiện phải được kiểm tra như`x + k in s`, không`abs(x - y) == k`. Vấn đề đòi hỏi sự khác biệt chính xác$k$và đặc tính biên tập dựa trên cặp có thứ tự$y - x = k$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
4 2 2 7
```| Bước | x | x + k | Có mặt trong bộ? | 
| --- | --- | --- | --- | 
| 1 | 4 | 9 | Không | 
| 2 | 2 | 7 | Có | 

Thuật toán dừng ngay lập tức và in`YES`. 

Điều này chứng tỏ sự quan sát trung tâm. Cặp đôi$(2,7)$khác nhau 5, do đó mục tiêu có thể đạt được bất kể trình tự hoạt động chi tiết. 

### Ví dụ 2 

đầu vào:```
5 4
1 9 1 3 4
```| Bước | x | x + k | Có mặt trong bộ? | 
| --- | --- | --- | --- | 
| 1 | 1 | 5 | Không | 
| 2 | 9 | 13 | Không | 
| 3 | 1 | 5 | Không | 
| 4 | 3 | 7 | Không | 
| 5 | 4 | 8 | Không | 

Không có cặp hợp lệ nào tồn tại nên câu trả lời là`NO`. 

Ví dụ này cho thấy rằng các bản sao không tự động trợ giúp. Điều quan trọng là sự tồn tại của một cặp có hiệu chính xác là$k$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Một đường chuyền để xây dựng bộ và một đường chuyền để kiểm tra các cặp | 
| Không gian |$O(n)$| Lưu trữ cho bộ băm | 

Vì tổng của tất cả$n$giá trị trên các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$, tổng thời gian chạy là tuyến tính ở kích thước đầu vào. Điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        ans = []

        for _ in range(t):
            n, k = map(int, input().split())
            a = list(map(int, input().split()))

            s = set(a)
            ok = any(x + k in s for x in a)

            ans.append("YES" if ok else "NO")

        return "\n".join(ans)

    return solve()

# provided sample
assert run(
"""4
4 5
4 2 2 7
5 4
1 9 1 3 4
2 17
17 0
2 17
18 18
"""
) == """YES
NO
YES
NO"""

# minimum size, positive answer
assert run(
"""1
2 1
5 6
"""
) == "YES"

# minimum size, negative answer
assert run(
"""1
2 1
5 7
"""
) == "NO"

# all equal values
assert run(
"""1
5 3
8 8 8 8 8
"""
) == "NO"

# negative values present
assert run(
"""1
3 7
-5 2 9
"""
) == "YES"

# large boundary-style pattern
n = 200000
arr = " ".join(map(str, range(n)))
inp = f"1\n{n} 199999\n{arr}\n"
assert run(inp) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 5 6`| CÓ | Trường hợp dương tính hợp lệ nhỏ nhất | 
|`2 1 / 5 7`| KHÔNG | Trường hợp phủ định hợp lệ nhỏ nhất | 
| Tất cả các giá trị bằng nhau | KHÔNG | Xử lý trùng lặp | 
|`-5 2 9`,`k=7`| CÓ | Số âm | 
| Mảng lớn liên tiếp | CÓ | Hiệu suất ở kích thước tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 17
17 0
```Bộ này là`{17, 0}`. Khi`x = 0`, thuật toán kiểm tra`0 + 17 = 17`, tồn tại. Câu trả lời trở thành`YES`. Điều này xử lý kích thước mảng nhỏ nhất có thể một cách chính xác. 

Coi như:```
1
4 5
4 2 2 7
```Bộ này là`{2, 4, 7}`. Khi`x = 2`, thuật toán tìm`7`trong tập hợp và trả về`YES`. Giá trị trùng lặp không ảnh hưởng đến tính chính xác vì điều kiện chỉ phụ thuộc vào sự tồn tại. 

Coi như:```
1
3 7
-5 2 9
```Bộ này là`{-5, 2, 9}`. Vì`x = -5`, thuật toán kiểm tra`-5 + 7 = 2`, tồn tại. Câu trả lời là`YES`. Các giá trị âm không yêu cầu xử lý đặc biệt vì việc tra cứu tập hợp hàm băm hoạt động giống hệt nhau đối với số nguyên âm và số nguyên dương. 

Coi như:```
1
5 3
8 8 8 8 8
```Bộ chỉ chứa`{8}`. Mọi kiểm tra đều hỏi liệu`11`tồn tại. Không, vì vậy thuật toán đưa ra`NO`. Điều này xác nhận rằng chỉ các bản sao không thể tạo ra sự khác biệt bằng 3.
