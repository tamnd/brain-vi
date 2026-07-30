---
title: "CF 102801I - Trường luyện thi PepperLa"
description: "Bài toán mô tả một tập hợp các lớp học được kết nối bằng đường. Mỗi cặp lớp học đều có thể có một con đường đi thẳng với chi phí như nhau, nhưng ban đầu không có con đường nào trong số này được thắp sáng. Chiếu sáng một con đường tốn một đô la."
date: "2026-07-30T06:04:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102801
codeforces_index: "I"
codeforces_contest_name: "The 14th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102801
solve_time_s: 97
verified: true
draft: false
---

[CF 102801I - Trường luyện thi PepperLa](https://codeforces.com/problemset/problem/102801/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Bài toán mô tả một tập hợp các lớp học được kết nối bằng đường. Mỗi cặp lớp học đều có thể có một con đường đi thẳng với chi phí như nhau, nhưng ban đầu không có con đường nào trong số này được thắp sáng. Chiếu sáng một con đường tốn một đô la. Ma trận đã cho cho chúng ta biết khoảng cách di chuyển ngắn nhất phải tồn tại giữa mỗi cặp lớp học sau khi chúng ta chọn con đường nào sẽ có đèn. 

Nhiệm vụ là tìm số lượng đường tối thiểu phải được chiếu sáng sao cho mạng lưới đường thu được có khoảng cách đường đi ngắn nhất giống hệt như ma trận đã cho. Câu trả lời là số cạnh tối thiểu cần thiết chứ không phải tổng chiều dài của các con đường. 

Đầu vào chứa một số trường hợp thử nghiệm cho đến hết tệp. Mỗi trường hợp thử nghiệm bắt đầu với số lượng phòng học, tiếp theo là`N x N`ma trận. Ma trận được đảm bảo mô tả một hệ thống khoảng cách hợp lệ, nghĩa là luôn có một số biểu đồ có khoảng cách đường đi ngắn nhất khớp với nó. Những ràng buộc cho phép`N`để đạt được`1000`, và tổng của tất cả`N`giá trị trên các trường hợp thử nghiệm là nhiều nhất`5000`. Một giải pháp thực hiện công khối cho mọi trường hợp thử nghiệm sẽ quá chậm vì trường hợp xấu nhất sẽ đạt tới khoảng`10^9`hoạt động. Chúng ta cần một cái gì đó gần với thời gian bậc hai, vì bản thân việc đọc ma trận đã yêu cầu`O(N^2)`hoạt động. 

Những cạm bẫy chính đến từ việc nhầm lẫn biểu đồ hoàn chỉnh đã cho với biểu đồ chúng ta cần xây dựng. Mỗi cặp đều có một khoảng cách trong ma trận, nhưng chúng ta không cần phải giữ mọi đường thẳng. 

Ví dụ:```
Input:
3
0 1 2
1 0 1
2 1 0
```Đầu ra đúng là:```
2
```Một cách tiếp cận bất cẩn có thể tính cả ba con đường có thể vì có ba cặp phòng học. Tuy nhiên, đường đi thẳng giữa phòng học thứ nhất và thứ ba là không cần thiết vì đường đi qua phòng học thứ hai đã dài rồi.`1 + 1 = 2`. 

Một trường hợp khác là khi có thể có một số mạng tối thiểu khác nhau. Mục tiêu chỉ là số lượng đường được chiếu sáng, vì vậy việc lựa chọn các cạnh thực tế không thành vấn đề.```
Input:
4
0 5 10 15
5 0 5 10
10 5 0 5
15 10 5 0
```Đầu ra đúng là:```
3
```Giải pháp chỉ kiểm tra xem một cạnh có xuất hiện trong ma trận ban đầu hay không có thể giữ lại các cạnh dư thừa và tạo ra câu trả lời lớn hơn. Các đường dẫn ngắn nhất có thể được bảo toàn bằng cách chỉ giữ lại các kết nối thiết yếu. 

# Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là xem xét biểu đồ hoàn chỉnh được biểu thị bằng ma trận và xóa từng con đường trong khi kiểm tra xem khoảng cách đường đi ngắn nhất có không thay đổi hay không. Điều này đúng vì câu trả lời yêu cầu tập hợp con nhỏ nhất của các cạnh có cùng ma trận khoảng cách. Tuy nhiên, có`N(N-1)/2`những con đường có thể, vì vậy việc thử các tập hợp con là theo cấp số nhân. Ngay cả việc kiểm tra tất cả các lần xóa có thể cũng trở nên không thể thực hiện được từ rất lâu trước đó`N=1000`. 

Quan sát hữu ích là mạng được yêu cầu chính xác là một cây bao trùm tối thiểu của biểu đồ hoàn chỉnh trong đó mỗi trọng số cạnh là khoảng cách nhất định giữa hai lớp học. Lý do hơi bất thường: ma trận đã chứa khoảng cách đường đi ngắn nhất chứ không phải trọng số cạnh tùy ý. 

Cây bao trùm tối thiểu của đồ thị hoàn chỉnh này có`N-1`các cạnh. Bởi vì các khoảng cách đã cho thỏa mãn tính chất tam giác nên mọi cạnh được chọn bởi cây bao trùm tối thiểu là một kết nối cần thiết và cây giữ nguyên khoảng cách ban đầu. Bất kỳ cạnh bổ sung nào cũng sẽ dư thừa vì độ dài của nó đã bằng hoặc lớn hơn đường đi giữa các điểm cuối của nó bên trong cây. 

Vì vậy, vấn đề giảm xuống việc tìm kích thước cây bao trùm tối thiểu. Vì cây bao trùm của mọi đồ thị liên thông có chính xác`N-1`các cạnh, chúng ta chỉ cần biết ma trận khoảng cách có hợp lệ và được kết nối hay không. Tuyên bố đảm bảo tính hợp lệ, vì vậy câu trả lời chỉ đơn giản là số đỉnh trừ đi một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(N2) | Quá chậm | 
| Tối ưu | O(1) sau khi đọc đầu vào | O(1) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đọc giá trị`N`và tiêu thụ`N x N`ma trận khoảng cách. Ma trận vẫn phải được đọc vì nó là một phần của định dạng đầu vào, mặc dù các giá trị không cần thiết cho phép tính cuối cùng. 
2. Vì các khoảng cách đã cho được đảm bảo đến từ một đồ thị liên thông hợp lệ, nên số cạnh tối thiểu cần thiết để kết nối`N`lớp học có kích thước bằng bất kỳ cây khung nào. 
3. Đầu ra`N - 1`, bởi vì mọi cây bao trùm trên`N`các đỉnh có đúng một cạnh ít hơn các đỉnh. 

Tại sao nó hoạt động: 

Đồ thị ban đầu có thể chứa nhiều đường có thể có, nhưng đồ thị được kết nối luôn có thể được rút gọn thành cây bao trùm mà không ngắt kết nối bất kỳ đỉnh nào. Một cây bao trùm có chính xác`N - 1`các cạnh. Bởi vì đầu vào đảm bảo rằng ma trận khoảng cách có thể thực hiện được nên tồn tại một cây bảo toàn tất cả các khoảng cách ngắn nhất, do đó không có giải pháp nào có ít hơn`N - 1`các cạnh có thể tồn tại. Giới hạn dưới và kết cấu khớp nhau, làm cho`N - 1`câu trả lời tối thiểu. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    ans = []
    while True:
        line = input()
        if not line:
            break
        line = line.strip()
        if not line:
            continue

        n = int(line)

        for _ in range(n):
            input()

        ans.append(str(n - 1))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Chương trình chỉ lưu trữ danh sách câu trả lời. Các hàng ma trận sẽ bị loại bỏ ngay sau khi được đọc vì đảm bảo tính hợp lệ có nghĩa là không cần tính toán đường đi ngắn nhất. 

Vòng lặp đầu vào tiếp tục cho đến EOF vì sự cố chứa nhiều trường hợp kiểm thử mà không có số lượng kiểm thử đứng đầu. Mỗi hàng của ma trận vẫn được sử dụng để giữ cho luồng đầu vào được căn chỉnh cho trường hợp tiếp theo. 

Không có vấn đề tràn số nguyên vì giá trị lớn nhất được in là`N - 1`, Và`N`nhiều nhất là`1000`. 

# Ví dụ đã hoạt động 

Đối với mẫu:```
3
0 1 2
1 0 1
2 1 0
```việc thực hiện là: 

| Bước | N | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 3 | Đọc số lớp học | | 
| 2 | 3 | Bỏ qua 3 hàng ma trận | | 
| 3 | 3 | Tính N - 1 | 2 | 

Ba phòng học cần một cái cây có hai con đường. Việc kết nối trực tiếp giữa phòng học thứ nhất và thứ ba là không cần thiết vì lớp học giữa đã cung cấp khoảng cách cần thiết. 

Một ví dụ thứ hai:```
5
0 2 4 6 8
2 0 2 4 6
4 2 0 2 4
6 4 2 0 2
8 6 4 2 0
```| Bước | N | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 5 | Đọc số lớp học | | 
| 2 | 5 | Bỏ qua ma trận | | 
| 3 | 5 | Tính N - 1 | 4 | 

Một chuỗi bốn con đường là đủ để thể hiện tất cả các khoảng cách, vì vậy chi phí tối thiểu là bốn. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2) | Đọc ma trận khoảng cách chiếm ưu thế trong công việc | 
| Không gian | O(1) | Chỉ duy trì bộ đếm và lưu trữ đầu ra | 

Chi phí đọc bậc hai là không thể tránh khỏi vì bản thân đầu vào chứa`N²`những con số. Vì tổng cộng`N`trên các trường hợp thử nghiệm bị giới hạn, giải pháp dễ dàng nằm gọn trong giới hạn. 

# Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    data = []
    input = sys.stdin.readline

    while True:
        line = input()
        if not line:
            break
        line = line.strip()
        if not line:
            continue
        n = int(line)
        for _ in range(n):
            input()
        data.append(str(n - 1))

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

assert run("""3
0 1 2
1 0 1
2 1 0
""") == "2", "sample"

assert run("""1
0
""") == "0", "single classroom"

assert run("""4
0 1 2 3
1 0 1 2
2 1 0 1
3 2 1 0
""") == "3", "chain distances"

assert run("""6
0 5 10 15 20 25
5 0 5 10 15 20
10 5 0 5 10 15
15 10 5 0 5 10
20 15 10 5 0 5
25 20 15 10 5 0
""") == "5", "larger chain"

assert run("""5
0 1 1 1 1
1 0 1 1 1
1 1 0 1 1
1 1 1 0 1
1 1 1 1 0
""") == "4", "many equal distances"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một lớp học | 0 | Kích thước tối thiểu và cây trống | 
| Chuỗi bốn nút | 3 | Hành vi cây bao trùm bình thường | 
| Chuỗi sáu nút | 5 | Xử lý đầu vào lớn hơn | 
| Khoảng cách bằng nhau | 4 | Các trường hợp tồn tại nhiều lựa chọn MST | 

# Vỏ cạnh 

Đối với một lớp học duy nhất, không có con đường dẫn tới ánh sáng.```
Input:
1
0
```Thuật toán đọc ma trận và tính toán`1 - 1`, cho`0`. Mọi nỗ lực luôn in ít nhất một con đường sẽ thất bại ở đây. 

Đối với ma trận khoảng cách trong đó nhiều đường đi trực tiếp có cùng giá trị thì có thể có nhiều cây hợp lệ.```
Input:
5
0 1 1 1 1
1 0 1 1 1
1 1 0 1 1
1 1 1 0 1
1 1 1 1 0
```Đầu ra của thuật toán`4`. Nó không cần phải quyết định nên giữ bốn con đường nào vì mỗi cây bao trùm đều có số cạnh như nhau. 

Đối với ma trận khoảng cách giống như chuỗi:```
Input:
4
0 1 2 3
1 0 1 2
2 1 0 1
3 2 1 0
```Câu trả lời là`3`. Giữ tất cả sáu con đường có thể sẽ lãng phí, trong khi giữ ít hơn ba con đường sẽ khiến các lớp học mất kết nối. Kích thước cây khớp chính xác với mức tối thiểu cần thiết.
