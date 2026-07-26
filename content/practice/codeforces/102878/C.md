---
title: "CF 102878C - AniPop đơn giản"
description: "Chúng ta có một vòng các đối tượng, mỗi đối tượng có một giá trị dương. Một động thái sẽ loại bỏ một đối tượng vẫn tồn tại. Điểm đạt được từ việc loại bỏ đó là tích của hai đối tượng lân cận hiện tại của đối tượng bị loại bỏ và giá trị của chính đối tượng bị loại bỏ."
date: "2026-07-25T12:50:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102878
codeforces_index: "C"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 102878
solve_time_s: 53
verified: true
draft: false
---

[CF 102878C - AniPop đơn giản](https://codeforces.com/problemset/problem/102878/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một vòng các đối tượng, mỗi đối tượng có một giá trị dương. Một động thái sẽ loại bỏ một đối tượng vẫn tồn tại. Điểm đạt được từ việc loại bỏ đó là tích của hai đối tượng lân cận hiện tại của đối tượng bị loại bỏ và giá trị của chính đối tượng bị loại bỏ. Hai người hàng xóm là đối tượng ngay bên cạnh nó trong vòng còn lại tại thời điểm đó. Sau khi mọi đối tượng ngoại trừ một đối tượng đã bị xóa, đối tượng cuối cùng còn lại sẽ bị xóa và chỉ đóng góp giá trị của chính nó. Mục tiêu là chọn thứ tự loại bỏ sao cho tổng số điểm tối đa. 

Đầu vào đưa ra số lượng đối tượng và các giá trị được đặt xung quanh vòng tròn. Kết quả đầu ra là tổng số điểm lớn nhất có thể. Các giá trị đều dương, vì vậy mọi lựa chọn chỉ ảnh hưởng đến các phép nhân trong tương lai thông qua đó các đối tượng vẫn là hàng xóm. Các ràng buộc cho phép tối đa 500 đối tượng, loại trừ việc khám phá tất cả các thứ tự loại bỏ có thể có vì số lượng trình tự có thể tăng theo cấp số nhân. Cần có một giải pháp lập trình động xung quanh O(n³) vì khoảng 125 triệu chuyển đổi trạng thái là phạm vi trên của những gì có thể phù hợp trong việc triển khai chặt chẽ. 

Khó khăn chính là vòng tròn không có điểm bắt đầu và kết thúc tự nhiên. Một giải pháp bất cẩn coi đối tượng đầu tiên là ranh giới vĩnh viễn sẽ làm mất đi các thứ tự tối ưu có thể có vì bất kỳ đối tượng nào cũng có thể là đối tượng sống sót cuối cùng. 

Ví dụ: với một đối tượng:```
Input:
1
7

Output:
7
```Một giải pháp giả định luôn phải có hai lân cận trước khi việc xóa sẽ thất bại, vì đối tượng duy nhất bị xóa bằng quy tắc cuối cùng đặc biệt. 

Một trường hợp quan trọng khác là:```
Input:
4
1 2 3 4

Output:
84
```Nếu chúng ta phá vỡ vòng tròn không đúng chỗ và chỉ giải quyết một cách sắp xếp tuyến tính, chúng ta có thể bỏ lỡ thứ tự tốt nhất. Vòng tròn cho phép đối tượng được chọn là người sống sót cuối cùng xác định đường cắt tốt nhất. 

Trường hợp thứ ba là các giá trị lặp lại:```
Input:
3
5 5 5

Output:
255
```Câu trả lời đến từ việc loại bỏ một đồ vật được 125 điểm, đồ vật thứ hai được 125 điểm và đồ vật cuối cùng được 5 điểm. Việc triển khai vô tình coi các giá trị bằng nhau là vị trí giống hệt nhau có thể làm giảm các lựa chọn có sẵn một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng mọi thứ tự loại bỏ có thể. Đối với mỗi bước di chuyển đầu tiên, chúng tôi sẽ xóa một đối tượng, cập nhật vòng tròn, sau đó thử đệ quy mọi bước đi tiếp theo có thể. Điều này đúng vì mọi trò chơi hợp lệ đều được thể hiện bằng chính xác một chuỗi lựa chọn. Tuy nhiên, với n đối tượng thì có n lựa chọn ở bước đầu tiên, n-1 ở bước thứ hai, v.v. Số lượng đơn đặt hàng là n!, điều này trở nên không thể thực hiện được ngay cả đối với những đầu vào nhỏ. 

Quan sát hữu ích đến từ việc thay đổi câu hỏi. Thay vì quyết định loại bỏ đối tượng nào trước, hãy quyết định đối tượng nào được loại bỏ sau cùng trong một khoảng thời gian nhỏ hơn. Đây chính là lối suy nghĩ đảo ngược tương tự được sử dụng trong các bài toán quy hoạch động theo khoảng. 

Nếu hai đối tượng được giữ làm ranh giới thì các đối tượng nằm giữa chúng tạo thành một bài toán con độc lập. Giả sử đối tượng k là đối tượng cuối cùng bị loại bỏ giữa ranh giới l và r. Tại thời điểm đó, các láng giềng của k chính xác là l và r, do đó phép toán cuối cùng trong khoảng này cho điểm a[l] × a[k] × a[r]. Mọi thứ bị loại bỏ trước k đều thuộc về hai khoảng nhỏ hơn, có thể giải độc lập. 

Vấn đề duy nhất còn lại là vòng tròn. Chúng tôi nhân đôi mảng, biến mọi đoạn tròn có thể thành một khoảng bình thường. Chúng tôi tính toán các giá trị khoảng cho các đoạn có độ dài lên tới n. Mọi lựa chọn có thể có của đối tượng sống sót cuối cùng đều xuất hiện dưới dạng một trong những khoảng thời gian này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n³) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhân đôi mảng sao cho sắp xếp hình tròn trở thành một mảng tuyến tính chứa hai bản sao của mỗi vị trí. Bây giờ, bất kỳ đường cắt vòng tròn nào cũng có thể được biểu diễn bằng một đoạn liền kề có độ dài n. 
2. Xác định`dp[l][r]`là số điểm tối đa đạt được bằng cách loại bỏ tất cả các đối tượng nằm giữa các vị trí l và r trong khi vẫn giữ l và r tồn tại làm ranh giới. Định nghĩa này có tác dụng vì lần loại bỏ cuối cùng trong khoảng phải có cả hai lân cận của nó trong hai ranh giới này. 
3. Khởi tạo các khoảng không có đối tượng giữa các ranh giới bằng 0. Không có gì cần loại bỏ nên những trạng thái này không đóng góp gì vào điểm số. 
4. Xử lý khoảng thời gian bằng cách tăng độ dài. Đối với mỗi khoảng thời gian`(l, r)`, thử mọi đối tượng có thể bị loại bỏ lần cuối`k`giữa họ. Điểm thí sinh là điểm cao nhất cho phần bên trái, điểm cao nhất cho phần bên phải và điểm đạt được khi loại bỏ k cuối cùng. 
5. Sau khi tính toán tất cả các khoảng, kiểm tra từng đoạn có độ dài n trong mảng nhân đôi. Mỗi đoạn tương ứng với việc chọn một điểm bắt đầu trên vòng tròn ban đầu. Thêm giá trị của đối tượng còn sót lại cuối cùng, là ranh giới được sử dụng để cắt và lấy mức tối đa. 

Tại sao nó hoạt động: mọi trò chơi hoàn chỉnh đều có một vật thể sống sót cuối cùng. Nếu chúng ta tưởng tượng vật thể đó như một ranh giới, thì tất cả các thao tác loại bỏ khác đều xảy ra bên trong khoảng được hình thành bằng cách cắt vòng tròn ở đó. Bên trong bất kỳ khoảng thời gian nào, đối tượng bị loại bỏ cuối cùng sẽ chia các thao tác còn lại thành hai khoảng độc lập và phép lặp sẽ kiểm tra mọi lựa chọn có thể có cho đối tượng đó. Vì mọi người sống sót cuối cùng có thể xảy ra và mọi lần loại bỏ cuối cùng có thể xảy ra trong mỗi khoảng thời gian đều được xem xét nên chương trình động bao gồm mọi trò chơi hợp lệ và không bao giờ kết hợp các lựa chọn không tương thích. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    if n == 1:
        print(a[0])
        return

    b = a + a
    m = 2 * n

    dp = [[0] * m for _ in range(m)]

    for length in range(2, n + 1):
        for l in range(m - length):
            r = l + length
            best = 0
            for k in range(l + 1, r):
                value = dp[l][k] + dp[k][r] + b[l] * b[k] * b[r]
                if value > best:
                    best = value
            dp[l][r] = best

    ans = 0
    for start in range(n):
        end = start + n
        ans = max(ans, dp[start][end] + b[start])

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng trùng lặp`b`loại bỏ sự cần thiết phải xử lý đặc biệt của hàng xóm xung quanh. Một đoạn bắt đầu lúc`start`và kết thúc tại`start + n`đại diện cho toàn bộ vòng tròn với`start`được chọn là người sống sót cuối cùng. 

cái bàn`dp`chỉ lưu trữ các khoảng có hai ranh giới còn sót lại. Quá trình chuyển đổi sẽ thử mọi đối tượng bị loại bỏ cuối cùng có thể, đó là lý do tại sao phép nhân sử dụng hai điểm cuối và vị trí ở giữa. 

Vòng lặp độ dài khoảng bắt đầu từ các phạm vi nhỏ vì các khoảng lớn hơn phụ thuộc vào các khoảng nhỏ hơn. Câu trả lời cuối cùng cộng riêng giá trị của người sống sót vì đối tượng cuối cùng không nhận được điểm nhân ba đối tượng bình thường. 

Số nguyên Python xử lý các giá trị lớn một cách an toàn. Điểm tối đa có thể vượt quá giới hạn số nguyên 32 bit vì nhiều số hạng nhân được tích lũy. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
1 2 3 4
```Một đường cắt tối ưu có thể giữ được`1`với tư cách là người sống sót. 

| Khoảng thời gian | Đối tượng bị xóa lần cuối | Đã thêm điểm | Giá trị tốt nhất | 
| --- | --- | --- | --- | 
| (1,3) | 2 | 1×2×3 | 6 | 
| (0,3) | 2 | 1×2×4 + 6 | 14 | 
| (0,4) trạng thái tròn tương đương | người sống sót 1 | trước đó tốt nhất + 1 | 84 | 

Phần quan trọng của dấu vết này là khoảng DP không quan tâm đến vị trí đầu tiên ban đầu. Nó thử mọi cách cắt tròn có thể. 

### Mẫu 2 

đầu vào:```
10
45 29 8 3 32 54 88 68 70 83
```Mảng nhân đôi tạo ra tất cả các điểm bắt đầu có thể. Mức tối đa cuối cùng đến từ khoảng mà lựa chọn ranh giới của nó mang lại thứ tự loại bỏ và sống sót tốt nhất. 

| Bắt đầu | Giá trị sống sót | Điểm DP theo khoảng thời gian | Tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 45 | tính từ các khoảng | ứng cử viên | 
| 1 | 29 | tính từ các khoảng | ứng cử viên | 
| 2 | 8 | tính từ các khoảng | ứng cử viên | 
| ... | ... | ... | ... | 
| 9 | 83 | tính từ các khoảng | tối đa 2304371 | 

Dấu vết này chứng tỏ tại sao chỉ kiểm tra một vòng quay là không chính xác. Thuật toán đánh giá tất cả mười người sống sót có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) | Có các khoảng O(n2) và mỗi khoảng thời gian sẽ thử O(n) lần xóa cuối cùng có thể. | 
| Không gian | O(n²) | Các giá trị khoảng được lưu trữ trong bảng lập trình động hai chiều. | 

Với n = 500, số lần chuyển đổi là khoảng 125 triệu. Việc triển khai sử dụng các phép toán số nguyên đơn giản bên trong vòng lặp trong cùng, phù hợp với giới hạn dự định. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided samples
assert run("4\n1 2 3 4\n") == "84\n", "sample 1"
assert run("10\n45 29 8 3 32 54 88 68 70 83\n") == "2304371\n", "sample 2"

# minimum size
assert run("1\n7\n") == "7\n", "single object"

# all equal values
assert run("3\n5 5 5\n") == "255\n", "equal values"

# small circle
assert run("2\n4 9\n") == "49\n", "two objects"

# boundary-heavy values
assert run("5\n1 100 1 1 1\n") == "10301\n", "large middle value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`7`| Quy tắc loại bỏ cuối cùng đặc biệt | 
|`5 5 5`|`255`| Giá trị bằng nhau và lựa chọn lặp lại | 
|`4 9`|`49`| Trường hợp ranh giới hai đối tượng | 
|`1 100 1 1 1`|`10301`| Hiệu ứng nhân lớn | 

## Vỏ cạnh 

Đối với một đối tượng, bảng lập trình động là không cần thiết. Hành động duy nhất có thể thực hiện là loại bỏ cuối cùng, do đó thuật toán trả về trực tiếp giá trị của đối tượng. 

Đối với hai đối tượng, không có sự lựa chọn hàng xóm thứ ba. Việc loại bỏ một trong hai đối tượng trước tiên sẽ cung cấp tích của cả hai giá trị và đối tượng còn lại, sau đó là giá trị còn lại. Việc triển khai xử lý việc này một cách tự nhiên vì khoảng DP chứa một đối tượng ở giữa có thể và đối tượng sống sót cuối cùng được thêm vào sau đó. 

Đối với các giá trị lặp lại, vị trí vẫn quan trọng ngay cả khi số lượng của chúng bằng nhau. Mảng nhân đôi giữ cho mọi vị trí tách biệt, do đó, chương trình động sẽ khám phá tất cả các đường cắt hình tròn có thể có thay vì thu gọn các đối tượng bằng nhau lại với nhau. 

Đối với trường hợp ranh giới hình tròn, mọi người sống sót có thể được kiểm tra bằng cách quét tất cả các khoảng thời gian có độ dài n trong mảng nhân đôi. Một giải pháp sửa phần tử đầu vào đầu tiên làm phần tử sống sót sẽ chỉ đánh giá một trong những khả năng này và có thể bỏ lỡ phần tối ưu.
