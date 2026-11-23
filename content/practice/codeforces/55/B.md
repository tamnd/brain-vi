---
title: "CF 55B - Số nhỏ nhất"
description: "Chúng ta bắt đầu với bốn số nguyên trên bảng. Chúng ta cũng biết trình tự chính xác của ba phép toán phải được áp dụng, trong đó mỗi phép toán là phép cộng hoặc phép nhân."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 55
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 51"
rating: 1600
weight: 55
solve_time_s: 107
verified: true
draft: false
---

[CF 55B - Số nhỏ nhất](https://codeforces.com/problemset/problem/55/B) 

**Đánh giá:** 1600 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với bốn số nguyên trên bảng. Chúng ta cũng biết trình tự chính xác của ba phép toán phải được áp dụng, trong đó mỗi phép toán là phép cộng hoặc phép nhân. Ở mỗi bước, chúng ta có thể chọn bất kỳ hai số hiện có nào, áp dụng thao tác cần thiết cho chúng và đặt kết quả trở lại bảng. Sau ba thao tác chỉ còn lại một số. 

Nhiệm vụ là tìm giá trị cuối cùng tối thiểu có thể. 

Điều khó khăn là thứ tự thực hiện các thao tác là cố định nhưng việc lựa chọn hai số tham gia vào mỗi thao tác là hoàn toàn linh hoạt. Các cặp khác nhau có thể tạo ra kết quả rất khác nhau. 

Những hạn chế là cực kỳ nhỏ. Chỉ có bốn con số và có đúng ba phép tính. Ngay cả khi chúng tôi thử mọi cách ghép nối có thể có ở mỗi bước thì tổng số trạng thái vẫn rất nhỏ. Tìm kiếm vũ phu là hoàn toàn thực tế trong thời gian giới hạn. 

Các số tối đa là 1000, vì vậy các sản phẩm trung gian vừa vặn thoải mái bên trong các số nguyên 64 bit. Giá trị lớn nhất có thể xuất hiện khi tất cả các số đều là 1000 và mọi phép toán đều là phép nhân:$1000^4 = 10^{12}$Điều này nằm trong phạm vi số nguyên của Python. 

Có một số trường hợp đặc biệt có thể âm thầm phá vỡ việc triển khai không chính xác. 

Nếu tồn tại nhiều giá trị bằng nhau, chúng ta vẫn phải coi chúng là các vị trí riêng biệt. Coi như:```
1 1 2 2
+ * +
```Lựa chọn đầu tiên`1`và thứ hai`1`khác với việc chọn một`1`và một`2`. Việc triển khai bất cẩn bằng cách sử dụng các bộ sẽ làm mất trạng thái hợp lệ. 

Thứ tự hoạt động không thể được sắp xếp lại. Ví dụ:```
1 2 3 4
+ * *
```Chúng ta phải áp dụng`+`Đầu tiên. Chúng ta không được phép nhân trước khi cộng, ngay cả khi điều đó sẽ cho kết quả nhỏ hơn. 

Ở mỗi bước chúng ta có thể chọn bất kỳ cặp nào, không chỉ các phần tử liền kề. Ví dụ:```
1 10 1 10
* + +
```Bước đầu tiên tốt nhất là nhân hai`1`s để giữ kết quả nhỏ. Việc hạn chế tìm kiếm ở các phần tử lân cận sẽ bỏ lỡ câu trả lời tối ưu. 

Một điểm tinh tế khác là phép cộng và phép nhân không thể thay thế cho nhau khi có số 0. Coi như:```
0 5 6 7
* + +
```Nước đi tối ưu là nhân lên`0`với`7`ngay lập tức sản xuất`0`, điều này giữ cho tổng số nhỏ về sau. Các chiến lược tham lam chỉ dựa trên những giá trị nhỏ nhất hiện tại có thể thất bại. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Ở mỗi bước, chúng tôi thử mọi cặp có thể có trong số các số hiện có. Đối với mỗi cặp, chúng tôi áp dụng thao tác cần thiết, xây dựng trạng thái tiếp theo và tiếp tục đệ quy. 

Ban đầu có 4 số nên có:$\binom{4}{2}=6$những lựa chọn có thể. 

Sau một thao tác còn lại 3 số, cho:$\binom{3}{2}=3$sự lựa chọn. 

Cuối cùng còn lại 2 số nên chỉ còn 1 lựa chọn. 

Tổng số đường dẫn thực hiện có thể là:$6 \times 3 \times 1 = 18$Đó là nhỏ bé. Ngay cả việc tìm kiếm theo chiều sâu đệ quy khám phá mọi khả năng cũng sẽ diễn ra ngay lập tức. 

Việc triển khai bạo lực ngây thơ có thể tạo ra tất cả các dấu ngoặc đơn và hoán vị có thể có một cách riêng biệt, nhưng điều đó nhanh chóng trở thành các trạng thái lộn xộn và trùng lặp. Quan sát rõ ràng hơn là bản thân trò chơi đã xác định quá trình chuyển đổi trạng thái một cách tự nhiên. Ở mỗi bước, chúng ta chỉ cần tập hợp số hiện tại và chỉ mục của thao tác tiếp theo. 

Điều này dẫn đến một tìm kiếm đệ quy nhỏ gọn: 

1. Chọn hai chỉ số. 
2. Áp dụng thao tác hiện tại. 
3. Lặp lại danh sách nhỏ hơn. 
4. Theo dõi giá trị cuối cùng tối thiểu. 

Vì cây tìm kiếm chỉ có 18 lá nên không cần cắt tỉa hay lập trình động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với bảng liệt kê rõ ràng | O(4! × dấu ngoặc đơn) | O(1) | Đã chấp nhận | 
| Tìm kiếm trạng thái đệ quy | O(18) | Độ sâu đệ quy O(3) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên ban đầu và ba phép tính. 
2. Định nghĩa hàm đệ quy`dfs(arr, step)`Ở đâu`arr`là danh sách hiện tại của các số còn lại và`step`cho chúng ta biết thao tác nào phải được áp dụng tiếp theo. 
3. Nếu`arr`chỉ chứa một số, trả lại vì mọi thao tác đã hoàn tất. 
4. Nếu không, hãy thử từng cặp chỉ số`(i, j)`với`i < j`. 
5. Áp dụng thao tác`ops[step]`ĐẾN`arr[i]`Và`arr[j]`. 

Nếu hoạt động là`+`, tính tổng của chúng. 

Nếu hoạt động là`*`, tính tích của chúng. 
6. Xây dựng mảng tiếp theo bằng cách loại bỏ cặp đã chọn và chèn kết quả mới. 

Các số còn lại giữ nguyên giá trị. Thứ tự không quan trọng về mặt logic, nhưng việc giữ một cấu trúc nhất quán sẽ đơn giản hóa việc thực hiện. 
7. Tính toán đệ quy kết quả nhỏ nhất có thể có từ trạng thái tiếp theo này. 
8. Lấy mức tối thiểu trong tất cả các lựa chọn cặp. 
9. In kết quả cuối cùng do tìm kiếm đệ quy trả về. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán sẽ khám phá mọi động thái hợp pháp mà tuyên bố vấn đề cho phép. Bất kỳ chuỗi thao tác hợp lệ nào cũng tương ứng với chính xác một đường dẫn trong cây đệ quy. Vì phép đệ quy kiểm tra tất cả các đường dẫn như vậy và trả về giá trị cuối cùng nhỏ nhất trong số đó nên nó không thể bỏ sót câu trả lời tối ưu. 

Độ sâu đệ quy chính xác là ba vì mỗi cấp độ thực hiện một thao tác. Cuối cùng chỉ còn lại một con số, đó là kết quả cuối cùng được tạo ra bởi chuỗi lựa chọn cụ thể đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

nums = list(map(int, input().split()))
ops = input().split()

def dfs(arr, step):
    if len(arr) == 1:
        return arr[0]

    best = float('inf')

    n = len(arr)

    for i in range(n):
        for j in range(i + 1, n):

            if ops[step] == '+':
                val = arr[i] + arr[j]
            else:
                val = arr[i] * arr[j]

            nxt = []

            for k in range(n):
                if k != i and k != j:
                    nxt.append(arr[k])

            nxt.append(val)

            best = min(best, dfs(nxt, step + 1))

    return best

print(dfs(nums, 0))
```Hàm đệ quy phản ánh trực tiếp quá trình của vấn đề. Mảng hiện tại đại diện cho các số vẫn được viết trên bảng, trong khi`step`cho biết thao tác nào phải được sử dụng tiếp theo. 

Trường hợp cơ sở xuất hiện khi chỉ còn lại một số. Tại thời điểm đó, toàn bộ chuỗi thao tác đã được áp dụng nên giá trị còn lại là kết quả của nhánh đó. 

Các vòng lặp lồng nhau liệt kê từng cặp không có thứ tự. sử dụng`i < j`tránh làm việc trùng lặp vì chọn`(a, b)`giống hệt với việc chọn`(b, a)`cho cả phép cộng và phép nhân. 

Trạng thái tiếp theo được xây dựng cẩn thận. Chúng tôi sao chép mọi số ngoại trừ hai số đã chọn, sau đó nối thêm giá trị mới được tính toán. Điều này bảo tồn số trùng lặp một cách chính xác. Việc sử dụng tập hợp hoặc loại bỏ theo giá trị sẽ nguy hiểm vì các số bằng nhau có thể xuất hiện nhiều lần. 

Số nguyên Python tự động xử lý các giá trị lớn, vì vậy lỗi tràn không phải là vấn đề đáng lo ngại ở đây. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, loại 64 bit là đủ vì giá trị tối đa có thể là$10^{12}$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 1 1
+ + *
```Dấu vết của đường đi tối ưu: 

| Bước | Số hiện tại | Hoạt động | Cặp đôi được chọn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | [1, 1, 1, 1] | + | 1, 1 | [1, 1, 2] | 
| 1 | [1, 1, 2] | + | 1, 1 | [2, 2] | 
| 2 | [2, 2] | * | 2, 2 | [4] | 

Đường dẫn này cho 4, nhưng nó không tối ưu. 

Một con đường khác: 

| Bước | Số hiện tại | Hoạt động | Cặp đôi được chọn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | [1, 1, 1, 1] | + | 1, 1 | [1, 1, 2] | 
| 1 | [1, 1, 2] | + | 1, 2 | [1, 3] | 
| 2 | [1, 3] | * | 1, 3 | [3] | 

Giá trị tối thiểu có thể đạt được là 3. 

Ví dụ này cho thấy tại sao việc thử tất cả các cặp lại quan trọng. Mặc dù các phép toán là cố định nhưng các nhóm trung gian khác nhau sẽ làm thay đổi đáng kể phép nhân cuối cùng. 

### Ví dụ 2 

đầu vào:```
0 5 6 7
* + +
```Dấu vết của đường đi tối ưu: 

| Bước | Số hiện tại | Hoạt động | Cặp đôi được chọn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | [0, 5, 6, 7] | * | 0, 7 | [5, 6, 0] | 
| 1 | [5, 6, 0] | + | 0, 5 | [6, 5] | 
| 2 | [6, 5] | + | 6, 5 | [11] | 

Câu trả lời cuối cùng: 11. 

Thay vào đó, nếu chúng ta nhân`6 * 7`đầu tiên, chúng tôi nhận được: 

| Bước | Số hiện tại | Hoạt động | Cặp đôi được chọn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | [0, 5, 6, 7] | * | 6, 7 | [0, 5, 42] | 
| 1 | [0, 5, 42] | + | 0, 5 | [42, 5] | 
| 2 | [42, 5] | + | 42, 5 | [47] | 

Điều này chứng tỏ tại sao các quyết định tham lam của địa phương lại không đáng tin cậy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(18) | Có 6 lựa chọn ở bước đầu tiên, 3 lựa chọn ở bước thứ hai và 1 ở bước thứ ba | 
| Không gian | O(3) | Độ sâu đệ quy không bao giờ vượt quá 3 | 

Không gian tìm kiếm rất nhỏ nên chương trình về cơ bản chạy ngay lập tức. Việc sử dụng bộ nhớ cũng không đáng kể vì chỉ có một vài mảng ngắn tồn tại đồng thời. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    nums = list(map(int, input().split()))
    ops = input().split()

    def dfs(arr, step):
        if len(arr) == 1:
            return arr[0]

        best = float('inf')
        n = len(arr)

        for i in range(n):
            for j in range(i + 1, n):

                if ops[step] == '+':
                    val = arr[i] + arr[j]
                else:
                    val = arr[i] * arr[j]

                nxt = []

                for k in range(n):
                    if k != i and k != j:
                        nxt.append(arr[k])

                nxt.append(val)

                best = min(best, dfs(nxt, step + 1))

        return best

    print(dfs(nums, 0))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run("1 1 1 1\n+ + *\n") == "3", "sample 1"

# all zeros
assert run("0 0 0 0\n* * *\n") == "0", "all zeros"

# large values
assert run("1000 1000 1000 1000\n* * *\n") == "1000000000000", "large multiplication"

# duplicate values
assert run("1 1 2 2\n+ * +\n") == "5", "duplicate handling"

# operation order matters
assert run("1 2 3 4\n+ * *\n") == "14", "fixed operation order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0 0 / * * *`|`0`| Xử lý đúng số 0 | 
|`1000 1000 1000 1000 / * * *`|`1000000000000`| Giá trị trung gian lớn | 
|`1 1 2 2 / + * +`|`5`| Số trùng lặp được xử lý chính xác | 
|`1 2 3 4 / + * *`|`14`| Hoạt động phải theo thứ tự nhất định | 

## Vỏ cạnh 

Hãy xem xét trường hợp giá trị trùng lặp:```
1 1 2 2
+ * +
```Thuật toán xử lý các số theo chỉ số thay vì giá trị. Trong quá trình đệ quy nó có thể chọn hai`1`s, hoặc một`1`và một`2`, tạo ra các trạng thái tương lai khác nhau. Vì mỗi cặp chỉ mục được khám phá độc lập nên không có cấu hình hợp lệ nào bị mất. 

Bây giờ hãy kiểm tra ràng buộc thứ tự cố định:```
1 2 3 4
+ * *
```Mức đệ quy đầu tiên luôn được áp dụng`+`bởi vì`step = 0`. Thuật toán không bao giờ xem xét phép nhân đầu tiên. Một con đường tối ưu là:```
1 + 2 = 3
3 * 3 = 9
9 * 4 = 36
```Nhưng đường dẫn tối thiểu là:```
1 + 4 = 5
2 * 3 = 6
5 * 6 = 30
```Đệ quy đảm bảo tất cả các cặp hợp lệ theo thứ tự yêu cầu đều được kiểm tra. 

Cuối cùng, hãy xem xét trường hợp số 0:```
0 5 6 7
* + +
```Ở cấp độ đầu tiên, thuật toán khám phá tất cả sáu lựa chọn nhân. Một nhánh nhân lên`0 * 7`, sản xuất ngay`0`. Các phép cộng sau này giữ cho tổng số nhỏ, dẫn đến câu trả lời tối ưu 11. Vì mọi cặp có thể đều được kiểm tra nên không thể bỏ qua việc sử dụng số 0 có lợi.
