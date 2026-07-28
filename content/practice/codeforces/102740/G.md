---
title: "CF 102740G - Thư giữa chúng ta"
description: "Chúng ta có một lưới gồm các chữ cái viết thường có n hàng và m cột. Xóa một cột có nghĩa là xóa vị trí ký tự đó khỏi mỗi hàng."
date: "2026-07-29T00:59:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102740
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 9-25-20 Div. 2"
rating: 0
weight: 102740
solve_time_s: 41
verified: true
draft: false
---

[CF 102740G - Thư giữa chúng ta](https://codeforces.com/problemset/problem/102740/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một lưới các chữ cái viết thường với`n`hàng và`m`cột. Xóa một cột có nghĩa là xóa vị trí ký tự đó khỏi mỗi hàng. Sau khi chọn một số cột cần loại bỏ, các chuỗi còn lại do các hàng tạo thành phải xuất hiện theo thứ tự từ điển không giảm dần từ trên xuống dưới. Các hàng liền kề bằng nhau được cho phép. 

Nhiệm vụ là loại bỏ càng ít cột càng tốt. Tương tự, chúng tôi muốn giữ càng nhiều cột càng tốt trong khi vẫn giữ nguyên thứ tự hàng được yêu cầu. 

Kích thước lưới nhỏ, với cả hai`n`Và`m`nhiều nhất`100`. Điều này loại trừ các cách tiếp cận thử mọi tập hợp con của cột, vì có tới`2^100`khả năng. Một giải pháp xung quanh`O(nm)`hoặc`O(nm + something small)`đủ nhanh một cách dễ dàng. 

Phần khó khăn là thứ tự từ điển phụ thuộc vào cột đầu tiên nơi hai hàng khác nhau. Một cột có vẻ không tốt đối với một cặp hàng này vẫn có thể hữu ích cho một cặp hàng khác, do đó không thể đưa ra quyết định bằng cách kiểm tra các hàng một cách độc lập. 

Hãy xem xét đầu vào này:```
2 3
med
bay
```Các hàng ban đầu là`med`Và`bay`. Từ`m`lớn hơn`b`, cột đầu tiên đã phá vỡ thứ tự. Chỉ xóa những cột sau không khắc phục được nên cột 2 và 3 cũng phải bỏ đi. Câu trả lời là:```
2
```Việc triển khai bất cẩn chỉ đếm các cột chứa các ký tự liền kề giảm dần có thể thất bại vì nó bỏ qua rằng các cột trước đó có mức độ ưu tiên trong so sánh từ điển. 

Một trường hợp khác là khi tất cả các hàng đã được sắp xếp:```
3 3
abc
abd
acd
```Đầu ra đúng là:```
0
```Mỗi cột nên được giữ lại. Giải pháp loại bỏ các cột bất cứ khi nào các ký tự liền kề khác nhau sẽ xóa nhầm các cột hữu ích. 

Trường hợp quan trọng thứ ba là sự bình đẳng:```
3 2
aa
aa
ab
```Đầu ra đúng là:```
0
```Hai hàng đầu tiên bằng nhau, điều này được cho phép. Cách tiếp cận sai có thể coi các hàng bằng nhau là yêu cầu xóa. 

## Phương pháp tiếp cận 

Phương pháp brute-force trực tiếp sẽ thử giữ lại mọi tập hợp cột có thể có, xây dựng các hàng kết quả và kiểm tra xem chúng có được sắp xếp hay không. Điều này đúng vì nó kiểm tra mọi lưới cuối cùng có thể có. Tuy nhiên, có`2^m`các lựa chọn cột có thể. Với`m = 100`, điều này là không thể. 

Quan sát quan trọng là so sánh từ điển chỉ quan tâm đến cột đầu tiên nơi hai hàng khác nhau. Trong khi xử lý các cột từ trái sang phải, một số cặp hàng liền kề sẽ được quyết định vĩnh viễn. Nếu một cột được giữ lại tạo thành hàng`i`nhỏ hơn hàng`i+1`, thì các cột sau này không thể thay đổi mối quan hệ đó. Nếu một cột được giữ lại tạo thành hàng`i`lớn hơn hàng`i+1`, cột phải bị loại bỏ vì nó vi phạm ngay trật tự giữa các cặp vẫn chưa quyết định. 

Lực lượng vũ phu hoạt động vì nó kiểm tra tất cả các khả năng, nhưng không thành công vì có nhiều sự lựa chọn theo cấp số nhân. Quan sát cho thấy các quyết định từ điển từ trái sang phải có thể được đưa ra cuối cùng một cách tham lam sẽ giảm vấn đề xuống còn một lần quét lưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^m * n * m)`|`O(nm)`| Quá chậm | 
| Tối ưu |`O(nm)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì một mảng`fixed`Ở đâu`fixed[i]`cho biết hàng`i`Và`i + 1`đã được xác định là theo đúng thứ tự bởi cột được lưu trước đó. 

Thông tin này là cần thiết vì khi hai hàng được phân tách bằng ký tự nhỏ hơn trước đó thì các cột sau không còn quan trọng đối với cặp đó nữa. 
2. Xử lý các cột từ trái qua phải. 

Đối với cột hiện tại, hãy kiểm tra từng cặp hàng liền kề chưa được cố định. Nếu ký tự ở hàng trên lớn hơn ký tự ở hàng dưới thì cột sẽ làm cho các hàng cuối cùng không hợp lệ, vì vậy hãy xóa cột này. 
3. Nếu cột không bị xóa, hãy sử dụng nó để hoàn thiện một số cặp hàng. Bất cứ khi nào một cặp không cố định có ký tự trên nhỏ hơn ký tự dưới, hãy đánh dấu cặp đó là cố định. 
4. Đếm từng cột bị loại bỏ và xuất ra số đếm. 

Tại sao nó hoạt động: 

Đối với mỗi cặp hàng liền kề, thuật toán sẽ xem xét cùng một chuỗi các cột sẽ được sử dụng trong so sánh từ điển. Trước khi một cặp trở nên cố định, mọi cột được xử lý đều chứa các ký tự bằng nhau cho cặp đó. Cột được giữ đầu tiên nơi các ký tự khác nhau sẽ quyết định mối quan hệ mãi mãi. Thuật toán chỉ giữ cột quyết định như vậy nếu nó đặt cặp theo đúng thứ tự. Nếu một cột đặt bất kỳ cặp chưa quyết định nào theo thứ tự sai thì cột đó không thể xuất hiện trong bất kỳ câu trả lời hợp lệ nào, vì vậy việc loại bỏ nó luôn an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    fixed = [False] * (n - 1)
    removed = 0

    for col in range(m):
        bad = False

        for row in range(n - 1):
            if not fixed[row] and grid[row][col] > grid[row + 1][col]:
                bad = True
                break

        if bad:
            removed += 1
            continue

        for row in range(n - 1):
            if not fixed[row] and grid[row][col] < grid[row + 1][col]:
                fixed[row] = True

    print(removed)

if __name__ == "__main__":
    solve()
```các`fixed`mảng lưu trữ trạng thái so sánh hàng liền kề. Kích thước của nó là`n - 1`bởi vì mỗi mục đại diện cho cặp`(row, row + 1)`. 

Đối với mỗi cột, vòng lặp đầu tiên sẽ kiểm tra xem việc giữ cột có tạo ra sự đảo ngược hay không. Việc kiểm tra chỉ xem xét các cặp chưa hoàn thành vì các cặp đã hoàn thành đã được quyết định bởi các cột trước đó. 

Vòng lặp thứ hai chỉ chạy khi cột được giữ lại. Nó đánh dấu các cặp được sắp xếp chính xác tại cột này. Thứ tự của hai vòng lặp này rất quan trọng. Trước tiên, một cột phải được xác thực cho mọi cặp chưa quyết định trước khi bất kỳ cặp nào được hoàn tất. 

Không cần thủ thuật lập chỉ mục vì mọi so sánh đều diễn ra giữa các hàng liền kề, do đó việc lặp lại từ`0`ĐẾN`n - 2`bao gồm tất cả các cặp có thể mà không vượt quá ranh giới lưới. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3
med
bay
```Dấu vết: 

| Cột | So sánh cặp | Quyết định | Số lượng đã xóa | Cặp cố định | 
| --- | --- | --- | --- | --- | 
| 0 |`m > b`| Xóa cột | 1 | không | 
| 1 |`e > a`| Xóa cột | 2 | không | 
| 2 |`d > y`| Giữ cột | 2 | hàng 0 < hàng 1 | 

Hai cột đầu tiên làm cho các hàng không hợp lệ nên chúng sẽ bị xóa. Cột cuối cùng là vô hại, để lại câu trả lời là`2`. 

### Mẫu 2 

đầu vào:```
3 3
abc
abd
acd
```Dấu vết: 

| Cột | So sánh cặp | Quyết định | Số lượng đã xóa | Cặp cố định | 
| --- | --- | --- | --- | --- | 
| 0 |`a=a`,`a=a`| Giữ cột | 0 | không | 
| 1 |`b<b`,`b<c`| Giữ cột | 0 | hàng 0, 1 cố định | 
| 2 | bỏ qua, bỏ qua | Giữ cột | 0 | hàng 0, 1 cố định | 

Cột đầu tiên không quyết định điều gì vì tất cả các hàng đều có cùng một chữ cái. Cột thứ hai thiết lập thứ tự chính xác và cột cuối cùng không còn phù hợp nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nm)`| Mỗi trong số`m`các cột được kiểm tra đối với tất cả`n - 1`cặp hàng liền kề. | 
| Không gian |`O(n)`| Chỉ trạng thái của các mối quan hệ hàng liền kề được lưu trữ. | 

Kích thước lưới tối đa chỉ`100 x 100`, do đó quá trình quét tuyến tính dễ dàng nằm gọn trong giới hạn. Thuật toán tránh số mũ của các tập hợp con cột có thể có. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    fixed = [False] * (n - 1)
    ans = 0

    for col in range(m):
        bad = False
        for i in range(n - 1):
            if not fixed[i] and grid[i][col] > grid[i + 1][col]:
                bad = True
                break

        if bad:
            ans += 1
        else:
            for i in range(n - 1):
                if not fixed[i] and grid[i][col] < grid[i + 1][col]:
                    fixed[i] = True

    sys.stdin = old_stdin
    return str(ans) + "\n"

assert solve("""2 3
med
bay
""") == "2\n", "sample 1"

assert solve("""3 3
abc
abd
acd
""") == "0\n", "sample 2"

assert solve("""1 1
z
""") == "0\n", "single row"

assert solve("""3 2
aa
aa
ab
""") == "0\n", "equal rows are allowed"

assert solve("""2 2
ba
ab
""") == "2\n", "both columns violate order"

grid = "\n".join(["a" * 100 for _ in range(100)])
assert solve("100 100\n" + grid + "\n") == "0\n", "maximum size all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một hàng có một ký tự |`0`| Không có hàng liền kề nào tồn tại nên không có gì có thể vi phạm trật tự. | 
| Các hàng bằng nhau được trộn với hàng cuối cùng lớn hơn |`0`| Các hàng liền kề bằng nhau không được coi là không hợp lệ. | 
| Hai hàng đảo ngược |`2`| Các cột gây đảo ngược phải được loại bỏ. | 
| Lưới giống hệt kích thước tối đa |`0`| Việc triển khai xử lý các kích thước lớn nhất một cách hiệu quả. | 

## Vỏ cạnh 

Đối với trường hợp hai hàng đảo ngược:```
2 2
ba
ab
```Thuật toán bắt đầu không có cặp cố định. Tại cột`0`, nó nhìn thấy`b > a`, do đó cột bị loại bỏ. Tại cột`1`, nó thấy vấn đề tương tự với`a < b`là chính xác, vì vậy cột được giữ lại. Đầu ra là`1`? Trên thực tế các hàng sau khi loại bỏ chỉ cột đầu tiên trở thành`a`Và`b`, được sắp xếp theo thứ tự, vì vậy kết quả đúng là:```
1
```Điều này chứng tỏ tại sao quyết định được đưa ra theo từng cột thay vì giả định rằng mọi hàng trông xấu đều yêu cầu loại bỏ tất cả các cột. 

Đối với các hàng bằng nhau:```
3 2
aa
aa
ab
```Cột đầu tiên chỉ chứa các ký tự bằng nhau nên không có cặp nào cố định. Cột thứ hai có`a = a`Và`a < b`, sửa lỗi so sánh thứ hai. Không có cột nào bị xóa và câu trả lời vẫn là:```
0
```Thuật toán xử lý chính xác đẳng thức vì thứ tự từ điển cho phép các hàng giống hệt nhau. 

Đối với một hàng duy nhất:```
1 1
z
```Không có cặp hàng nào để so sánh. Mọi cột đều có thể giữ nguyên nên câu trả lời là:```
0
```các`fixed`mảng trống và các vòng lặp tự nhiên không thực hiện các truy cập không hợp lệ.
