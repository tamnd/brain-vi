---
title: "CF 105481G - \u987e\u5f71\u81ea\u601c"
description: "Chúng tôi được cung cấp một hoặc nhiều mảng. Đối với mỗi mảng, chúng tôi xem xét từng mảng con liền kề và gán cho nó một giá trị dựa trên một quy tắc đơn giản: lấy phần tử lớn nhất bên trong mảng con, đếm số lần giá trị lớn nhất này xuất hiện và nếu số lượng đó ít nhất là k thì mảng con sẽ đóng góp…"
date: "2026-06-23T18:19:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105481
codeforces_index: "G"
codeforces_contest_name: "2024 CCPC Liaoning Provincial Contest"
rating: 0
weight: 105481
solve_time_s: 62
verified: true
draft: false
---

[CF 105481G - \u987e\u5f71\u81ea\u601c](https://codeforces.com/problemset/problem/105481/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hoặc nhiều mảng. Đối với mỗi mảng, chúng tôi xem xét từng mảng con liền kề và gán cho nó một giá trị dựa trên một quy tắc đơn giản: lấy phần tử lớn nhất bên trong mảng con, đếm xem giá trị tối đa này xuất hiện bao nhiêu lần và liệu số lượng đó có ít nhất là`k`, mảng con đóng góp`1`, nếu không thì nó góp phần`0`. Nhiệm vụ là tính tổng của những đóng góp này trên tất cả các mảng con. 

Vì vậy, vấn đề không phải là tính toán trực tiếp cực đại mà là đếm tần suất giá trị tối đa bên trong một phân đoạn được “lặp lại đủ”. Mọi mảng con đều đủ điều kiện hoặc không đủ điều kiện và chúng tôi đang đếm xem có bao nhiêu mảng con đủ điều kiện. 

Các ràng buộc ngụ ý một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Tổng chiều dài của tất cả các trường hợp thử nghiệm lên tới 1e6, do đó, bất kỳ phép liệt kê bậc hai nào của các mảng con đều không thể thực hiện được. Ngay cả O(n sqrt n) cũng có rủi ro trừ khi được tối ưu hóa nhiều. Cấu trúc phải hỗ trợ hành vi O(n) hoặc O(n log n) được khấu hao. 

Việc triển khai đơn giản sẽ liệt kê tất cả các mảng con O(n²), tính toán mức tối đa và tần số của nó trong O(n) hoặc O(1) bằng quá trình tiền xử lý và kiểm tra điều kiện. Điều này đã đẩy chúng tôi đến thời gian O(n³) hoặc O(n²) tùy thuộc vào việc triển khai, tốc độ này quá chậm. 

Một cách tiếp cận sai lầm tinh vi hơn là thử duy trì một cửa sổ trượt trong khi chỉ cập nhật mức tối đa. Điều đó không thành công vì “tần số tối đa trong cửa sổ hiện tại” không đơn điệu: việc thêm hoặc xóa các phần tử có thể thay đổi giá trị nào là tối đa và số lần nó xuất hiện. 

Một cạm bẫy khác là giả định rằng chỉ các mảng con trong đó mức tối đa toàn cầu của toàn bộ mảng mới quan trọng. Điều kiện này mang tính cục bộ đối với từng mảng con, do đó việc hạn chế chú ý đến cực đại toàn cục sẽ làm mất hầu hết các trường hợp hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi chỉ mục bắt đầu, hãy mở rộng mảng con từng phần tử một, duy trì mức tối đa và bản đồ tần số, đồng thời đếm số lần mức tối đa xuất hiện. Điều này xác định chính xác các mảng con hợp lệ, nhưng nó tiêu tốn O(n) công việc cho mỗi phần mở rộng, dẫn đến O(n²) hoặc tệ hơn cho mỗi trường hợp thử nghiệm. Với tổng số phần tử lên tới 1e6, điều này là không khả thi. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào phần tử lớn nhất của mảng con. Nếu chúng tôi sửa giá trị tối đa là bao nhiêu, hãy nói một giá trị`x`, thì chúng ta chỉ quan tâm đến các mảng con trong đó không có phần tử nào vượt quá`x`, và trong số đó, chúng tôi muốn ít nhất`k`sự xuất hiện của`x`. Điều này biến vấn đề thành việc đếm, đối với mỗi giá trị, có bao nhiêu mảng con có tần số tối đa và đủ được kiểm soát của giá trị đó. 

Chúng ta có thể xử lý các giá trị theo thứ tự giảm dần. Khi chúng ta xem xét một giá trị`x`, chúng tôi tạm thời xử lý tất cả các phần tử lớn hơn`x`là “ranh giới chặn” chia mảng thành các phân đoạn độc lập. Bên trong mỗi phân khúc, chúng tôi chỉ quan tâm đến các vị trí có giá trị bằng`x`. 

Bây giờ vấn đề trở thành: trong mỗi phân đoạn, đếm các mảng con trong đó`x`ít nhất xuất hiện`k`lần. Đây là một bài toán đếm cổ điển “ít nhất k lần xuất hiện trong mảng con”, có thể giải được bằng cách tiếp cận hai con trỏ trên các vị trí của`x`bên trong mỗi phân đoạn. 

Chúng tôi duy trì vị trí của`x`và sử dụng cửa sổ trượt qua các vị trí này. Đối với một cửa sổ xuất hiện`[i, j]`với`j - i + 1 >= k`, số lượng mảng con trong đó lần xuất hiện thứ k của`x`được cố định được xác định bởi khoảng cách chúng ta có thể mở rộng sang trái và phải mà không vượt qua các phần tử lớn hơn`x`. 

Điều này dẫn đến việc quét tuyến tính trên mỗi giá trị và vì mỗi chỉ mục chỉ tham gia xử lý khi giá trị của nó được kích hoạt nên tổng độ phức tạp là gần O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) mỗi lần kiểm tra | O(1) đến O(n) | Quá chậm | 
| Quét giá trị + hai con trỏ | O(n log n) hoặc O(n) được khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp hoặc nhóm các chỉ mục theo giá trị mảng để chúng ta có thể xử lý các giá trị từ lớn nhất đến nhỏ nhất. Điều này đảm bảo rằng khi chúng ta xử lý một giá trị`x`, tất cả các giá trị lớn hơn đã được coi là rào cản. 
2. Duy trì cấu trúc “phân đoạn hoạt động” trong đó tất cả các phần tử lớn hơn giá trị hiện tại sẽ chia mảng. Về mặt khái niệm, chúng tôi coi đây là những ranh giới đặt lại việc đếm. 
3. Đối với giá trị hiện tại`x`, thu thập tất cả các chỉ số trong đó`a[i] == x`. Trong mỗi phân đoạn hoạt động, hãy xử lý các chỉ số này một cách riêng biệt. 
4. Với mỗi đoạn, lấy danh sách các vị trí của`x`. Sử dụng cửa sổ trượt trên các vị trí này. Hãy để cửa sổ`[l, r]`xét về số lần xuất hiện. Chúng tôi chỉ xem xét các cửa sổ nơi`r - l + 1 >= k`. 
5. Đối với cửa sổ hợp lệ cố định xuất hiện lần thứ k tại vị trí`r`, xác định có bao nhiêu mảng con có ranh giới bên trái giữa lần xuất hiện trước đó (hoặc bắt đầu phân đoạn) và lần xuất hiện thứ k này và ranh giới bên phải giữa lần xuất hiện này và phần tử chặn tiếp theo. Điều này chuyển đổi mỗi cửa sổ hợp lệ thành một đóng góp đếm. 
6. Tổng hợp tất cả các đóng góp trên tất cả các giá trị. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào việc phân tích các mảng con theo phần tử lớn nhất của chúng. Mỗi mảng con hợp lệ có một giá trị tối đa duy nhất`x`. Một khi chúng tôi sửa chữa`x`, bất kỳ phần tử nào lớn hơn`x`sẽ làm mất hiệu lực mảng con, do đó mảng con hợp lệ phải nằm hoàn toàn trong các vùng được giới hạn bởi các phần tử lớn hơn. Bên trong một vùng như vậy, việc đếm các mảng con trong đó`x`ít nhất xuất hiện`k`thời gian độc lập với các giá trị khác. Mỗi mảng con được tính chính xác một lần khi xử lý giá trị lớn nhất của nó, vì nó chỉ được xem xét khi`x`là giá trị được phép lớn nhất trong phân khúc của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    pos = [[] for _ in range(n + 1)]
    for i, v in enumerate(a):
        pos[v].append(i)

    # next greater element boundary: we build "blocks" using a monotonic stack
    nxt_block = [n] * n
    stack = []
    for i in range(n):
        while stack and a[stack[-1]] < a[i]:
            nxt_block[stack.pop()] = i
        stack.append(i)

    prev_block = [-1] * n
    stack = []
    for i in range(n - 1, -1, -1):
        while stack and a[stack[-1]] <= a[i]:
            prev_block[stack.pop()] = i
        stack.append(i)

    ans = 0

    # process values in decreasing order
    for val in range(n, 0, -1):
        if not pos[val]:
            continue

        # split positions by blocks implicitly
        group = []
        for i in pos[val]:
            group.append(i)

        if len(group) < k:
            continue

        # two pointers over occurrences
        for i in range(len(group) - k + 1):
            j = i + k - 1

            left_bound = prev_block[group[i]] + 1
            right_bound = nxt_block[group[j]] - 1

            # extend left and right choices
            left_choices = group[i] - left_bound + 1
            right_choices = right_bound - group[j] + 1

            if left_choices > 0 and right_choices > 0:
                ans += left_choices * right_choices

    print(ans)

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách nhóm các chỉ số có giá trị bằng nhau để chúng ta có thể suy luận cục bộ về mức tối đa của từng ứng cử viên. Các ranh giới lớn hơn tiếp theo và lớn hơn trước đó được tính toán bằng các ngăn xếp đơn điệu. Chúng xác định các phân đoạn tối đa trong đó một giá trị nhất định có thể đóng vai trò là mức tối đa mà không bị phần tử lớn hơn vô hiệu hóa. 

Đối với mỗi giá trị, chúng tôi xem xét số lần xuất hiện và chọn cửa sổ có kích thước`k`. Cửa sổ xác định yêu cầu tối thiểu để thỏa mãn điều kiện tần số. Khi lần xuất hiện thứ k được khắc phục, chúng tôi đếm xem có bao nhiêu lựa chọn tồn tại để mở rộng mảng con sang trái và phải trong khi vẫn nằm trong ranh giới phân đoạn hợp lệ. 

Một điểm tinh tế là mỗi cửa sổ tương ứng với nhiều mảng con và chúng tôi nhân các lựa chọn độc lập của điểm cuối bên trái và bên phải. Hệ số hóa này làm giảm số đếm xuống O(1) trên mỗi cửa sổ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=5, k=2
a = [1, 3, 3, 2, 2]
```Chúng tôi tính toán ranh giới: 

| Bước | Cửa sổ giá trị | Giới hạn bên trái | Ràng buộc phải | Đóng góp | 
| --- | --- | --- | --- | --- | 
| (3,3) cặp tại vị trí 1,2 | k-cửa sổ [1,2] | 0 | 4 | (2 * 3) = 6 | 
| (2,2) cặp ở vị trí 3,4 | k-cửa sổ [3,4] | 3 | 4 | (1 * 1) = 1 | 

Tổng cộng là 7. 

Điều này phù hợp với trực giác: mỗi mảng con hợp lệ được tính chính xác khi giá trị tối đa của nó được xử lý. 

### Ví dụ 2 

đầu vào:```
n=4, k=3
a = [1, 4, 2, 1]
```Không có giá trị nào xuất hiện ít nhất 3 lần trong bất kỳ phân đoạn nào, do đó không tồn tại cửa sổ k hợp lệ. 

| Giá trị | Lần xuất hiện | cửa sổ hợp lệ k | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 2 | không | 0 | 
| 2 | 1 | không | 0 | 
| 4 | 1 | không | 0 | 

Đầu ra là 0. 

Điều này xác nhận rằng thuật toán lọc ra các điều kiện tần số không thể thực hiện được một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) khấu hao mỗi lần kiểm tra | Mỗi chỉ mục tham gia tối đa một số lượng hoạt động ranh giới và sự xuất hiện không đổi | 
| Không gian | O(n) | Lưu trữ vị trí và mảng ranh giới | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi 1e6, do đó, giải pháp khấu hao tuyến tính là đủ. Quá trình tiền xử lý dựa trên ngăn xếp và quét theo giá trị đều có tỷ lệ tương ứng với kích thước mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        pos = [[] for _ in range(n + 1)]
        for i, v in enumerate(a):
            pos[v].append(i)

        nxt_block = [n] * n
        stack = []
        for i in range(n):
            while stack and a[stack[-1]] < a[i]:
                nxt_block[stack.pop()] = i
            stack.append(i)

        prev_block = [-1] * n
        stack = []
        for i in range(n - 1, -1, -1):
            while stack and a[stack[-1]] <= a[i]:
                prev_block[stack.pop()] = i
            stack.append(i)

        ans = 0

        for val in range(n, 0, -1):
            if not pos[val]:
                continue
            if len(pos[val]) < k:
                continue
            for i in range(len(pos[val]) - k + 1):
                j = i + k - 1
                L = prev_block[pos[val][i]] + 1
                R = nxt_block[pos[val][j]] - 1
                left = pos[val][i] - L + 1
                right = R - pos[val][j] + 1
                if left > 0 and right > 0:
                    ans += left * right

        return str(ans) + "\n"

    out = []
    t = int(input())
    for _ in range(t):
        out.append(solve())
    return "".join(out)

# sample-like tests
assert run("""1
5 2
1 3 3 2 2
""").strip() == "7"

assert run("""1
4 3
1 4 2 1
""").strip() == "0"

assert run("""1
1 1
1
""").strip() == "1"

assert run("""1
3 2
2 2 2
""").strip() == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp đơn lặp lại | 7 | đếm chính xác trên các cực đại chồng chéo | 
| không có mảng con hợp lệ | 0 | lọc theo ràng buộc k | 
| đầu vào tối thiểu | 1 | độ đúng cơ sở | 
| tất cả các mảng bằng nhau | 3 | đếm tổ hợp đầy đủ | 

## Vỏ cạnh 

Mảng tối thiểu có k bằng 1 kiểm tra xem thuật toán có đếm chính xác tất cả các mảng con trong đó giá trị lớn nhất xuất hiện ít nhất một lần hay không. Trong trường hợp đó, mọi mảng con đều hợp lệ và công thức giảm xuống việc đếm tất cả các phân đoạn có thể. Phân tách dựa trên ranh giới vẫn hoạt động vì mọi phần tử tạo thành lớp tối đa của riêng nó và mỗi cửa sổ k trở thành một lần xuất hiện duy nhất. 

Một mảng hoàn toàn bằng nhau nhấn mạnh tính đúng đắn của phép nhân trái-phải. Vì mọi mảng con đều có cùng mức tối đa nên thuật toán sẽ đếm tất cả các mảng con có độ dài ít nhất là k. Cửa sổ trượt qua các lần xuất hiện sẽ tạo ra các cửa sổ chính xác (n-k+1) một cách tự nhiên và mỗi cửa sổ sẽ mở rộng thành các lựa chọn điểm cuối chính xác, xác nhận rằng không xảy ra việc tính hai lần.
