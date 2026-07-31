---
title: "CF 102569H - Tranh Cây"
description: "Cây đại diện cho một mạng lưới các đỉnh được kết nối. Một thao tác chọn hai đỉnh và tô màu mọi đỉnh và cạnh trên đường đi duy nhất giữa chúng. Mục tiêu là tìm ra số lượng tuyến đường được chọn nhỏ nhất có thể bao phủ mọi phần của cây."
date: "2026-07-31T07:54:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102569
codeforces_index: "H"
codeforces_contest_name: "2020, XIII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102569
solve_time_s: 91
verified: true
draft: false
---

[CF 102569H - Tranh cây](https://codeforces.com/problemset/problem/102569/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cây đại diện cho một mạng lưới các đỉnh được kết nối. Một thao tác chọn hai đỉnh và tô màu mọi đỉnh và cạnh trên đường đi duy nhất giữa chúng. Mục tiêu là tìm ra số lượng tuyến đường được chọn nhỏ nhất có thể bao phủ mọi phần của cây. 

Đầu vào mô tả các kết nối của cây. Vì một cái cây có`n`đỉnh luôn có`n - 1`các cạnh, toàn bộ cấu trúc được xác định bởi các kết nối này. Đầu ra là số lượng lựa chọn đường dẫn tối thiểu cần thiết để làm cho toàn bộ cây được sơn. 

Ràng buộc`n <= 200000`thay đổi cách chúng ta tiếp cận vấn đề. Một giải pháp khám phá nhiều đường đi có thể hoặc thực hiện duyệt cây nhiều lần cho mọi lựa chọn sẽ không phù hợp với giới hạn 2 giây. Chúng ta cần một thuật toán gần với thời gian tuyến tính vì bản thân đầu vào chứa tới khoảng hai trăm nghìn cạnh. Bất cứ điều gì xung quanh`O(n^2)`hoặc tệ hơn là quá chậm. 

Một sai lầm phổ biến là chỉ tập trung vào lá cây. Những chiếc lá đóng vai trò quan trọng vì chúng là điểm cuối tự nhiên của đường vẽ, nhưng các đỉnh bên trong có bậc lẻ cũng ảnh hưởng đến câu trả lời. Ví dụ, trong một cái cây mà tâm có ba nhánh, việc chỉ đếm số lá sẽ đưa ra trực giác sai lầm vì bản thân tâm phải đóng vai trò là điểm cuối trong cách sắp xếp đường dẫn nào đó. 

Hãy xem xét cây này:```
4
1 2
3 2
4 2
```Câu trả lời đúng là`2`. Một cách tiếp cận bất cẩn chỉ đếm ba lá có thể cho rằng một đường đi là đủ vì một đường đi có thể nối hai lá và đi qua tâm, nhưng một thao tác không thể bao phủ cả ba nhánh. Trung tâm có bằng cấp`3`, do đó, một trong những điểm cuối cần thiết cũng được tạo ở đó. 

Một trường hợp ranh giới khác là một chuỗi đơn giản:```
5
1 2
2 3
3 4
4 5
```Câu trả lời là`1`. Một giải pháp luôn trả về số lá chia cho hai sẽ có tác dụng ở đây, nhưng một giải pháp giả định rằng mỗi điểm phân nhánh cần một thao tác riêng biệt có thể trả về một giá trị lớn hơn không chính xác. Hai điểm cuối của chuỗi là đỉnh bậc lẻ duy nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là tạo ra mọi đường đi có thể có trong cây, sau đó tìm kiếm tập hợp nhỏ nhất trong số những đường đi này mà hợp của chúng chứa mọi cạnh. có`O(n^2)`các cặp điểm cuối có thể có và việc kiểm tra từng tập hợp con của chúng là theo cấp số nhân. Ngay cả khi đã tối ưu hóa, điều này nhanh chóng trở nên không thể đối với`n = 200000`. 

Lực lượng vũ phu hoạt động vì mọi câu trả lời hợp lệ thực sự là một tập hợp các đường dẫn, nhưng nó không thành công vì số lượng bộ sưu tập có thể lớn hơn nhiều so với kích thước đầu vào. 

Quan sát quan trọng xuất phát từ việc xem xét độ đỉnh. Mỗi khi chúng ta sử dụng một thao tác đường dẫn, hai điểm cuối của đường dẫn đó là những nơi duy nhất mà đường dẫn được sử dụng đóng góp một số lẻ các cạnh đường dẫn tới. Tất cả các đỉnh bên trong của đường đi đều nhận được hai cạnh đường đi, do đó chúng không ảnh hưởng đến tính chẵn lẻ. 

Nếu chúng ta chọn một số đường đi bao phủ toàn bộ cây thì các đỉnh cần làm điểm cuối của các đường đi này chính xác là các đỉnh có bậc lẻ. Một đường dẫn duy nhất có thể cố định hai đỉnh bậc lẻ vì nó có hai điểm cuối. Điều này đưa ra giới hạn dưới: nếu cây có`x`ít nhất là các đỉnh bậc lẻ`x / 2`đường dẫn được yêu cầu. 

Giới hạn dưới này cũng có thể đạt được. Chúng ta có thể ghép các đỉnh bậc lẻ và biến mỗi cặp thành điểm cuối của một đường đi. Các đường dẫn có thể chồng lên nhau ở các đỉnh, điều này được cho phép vì việc vẽ một thứ gì đó nhiều lần không có tác động tiêu cực. Mọi cạnh của cây đều có thể được đưa vào quá trình ghép nối này, do đó không cần thêm đường dẫn. 

Câu trả lời đơn giản là một nửa số đỉnh có bậc lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số đường đi có thể | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các cạnh của cây và lưu trữ bậc của mỗi đỉnh. Mỗi cạnh đều tăng bậc của cả hai điểm cuối của nó vì bậc cho chúng ta biết có bao nhiêu nhánh rời khỏi một đỉnh. 
2. Đếm xem có bao nhiêu đỉnh có bậc lẻ. Đây là những đỉnh không thể được xử lý hoàn toàn bằng cách là các điểm bên trong của đường dẫn, vì vậy chúng phải xuất hiện dưới dạng điểm cuối. 
3. Chia số này cho hai và xuất kết quả. Mỗi thao tác đóng góp chính xác hai điểm cuối và đối số ghép nối cho thấy số lượng thao tác này luôn đủ. 

Tại sao nó hoạt động: 

Bất biến quan trọng là tính chẵn lẻ của độ đỉnh. Thao tác đường dẫn sẽ thay đổi phối cảnh của cây thành một tập hợp các cạnh đường dẫn đã sử dụng. Bên trong một đường dẫn được vẽ, mọi đỉnh không phải là điểm cuối đều nhận được hai cạnh được sử dụng, giữ tính chẵn lẻ của nó. Chỉ các điểm cuối đường dẫn mới đóng góp tính chẵn lẻ. Vì cây ban đầu có chính xác các đỉnh bậc lẻ cần được biểu diễn dưới dạng điểm cuối, nên bất kỳ giải pháp nào cũng cần ít nhất một nửa số đường đi so với các đỉnh lẻ. Việc ghép các đỉnh lẻ này thành các đường đi sẽ đạt đến giới hạn đó, do đó mức tối thiểu chính xác là số đỉnh bậc lẻ chia cho hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    degree = [0] * (n + 1)

    for _ in range(n - 1):
        u, v = map(int, input().split())
        degree[u] += 1
        degree[v] += 1

    odd = 0
    for i in range(1, n + 1):
        if degree[i] % 2 == 1:
            odd += 1

    print(odd // 2)

if __name__ == "__main__":
    solve()
```Mảng`degree`chỉ lưu trữ thông tin cần thiết từ cây. Chúng ta không cần danh sách kề vì câu trả lời cuối cùng chỉ phụ thuộc vào mức độ chẵn lẻ của mỗi đỉnh. 

Trong khi đọc từng cạnh, cả hai điểm cuối đều nhận được một cạnh phụ, do đó cả hai giá trị độ đều tăng lên. Sau khi tất cả các cạnh được xử lý, vòng lặp sẽ đếm các đỉnh có bậc lẻ. 

Một cây luôn có số đỉnh bậc lẻ là số chẵn, do đó việc chia số nguyên cho 2 sẽ cho số lượng chính xác các phép toán cần thực hiện. Số nguyên Python xử lý các giá trị độ tối đa mà không có bất kỳ mối lo ngại nào về tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 2
1 3
1 4
1 5
```Cây là một ngôi sao. Trung tâm có bằng cấp`4`, và cả bốn lá đều có bậc`1`. 

| Bước | Bằng cấp được xem xét | Số lẻ | Trả lời | 
| --- | --- | --- | --- | 
| Sau khi đọc các cạnh | 1:4, 2:1, 3:1, 4:1, 5:1 | 4 | 2 | 
| Tính toán cuối cùng | Bốn đỉnh lẻ cần ghép | 4 | 4/2 = 2 | 

Bốn lá là đỉnh bậc lẻ duy nhất. Hai đường dẫn có thể ghép chúng: một đường dẫn bao phủ hai lá đi qua tâm và một đường dẫn khác bao phủ hai lá còn lại. 

### Mẫu 2 

đầu vào:```
5
1 2
2 3
3 4
4 5
```Đây là một chuỗi thẳng. 

| Bước | Bằng cấp được xem xét | Số lẻ | Trả lời | 
| --- | --- | --- | --- | 
| Sau khi đọc các cạnh | 1:1, 2:2, 3:2, 4:2, 5:1 | 2 | 1 | 
| Tính toán cuối cùng | Chỉ có hai đầu là lẻ | 2 | 2/2 = 1 | 

Toàn bộ chuỗi đã là một đường dẫn, vì vậy chỉ cần một thao tác là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được đọc một lần và mỗi độ đỉnh được kiểm tra một lần | 
| Không gian | O(n) | Mảng độ lưu trữ một giá trị trên mỗi đỉnh | 

Giải pháp chỉ thực hiện một vài lần truyền tuyến tính qua đầu vào. Với`200000`đỉnh, điều này nằm trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    def solve():
        import sys
        input = sys.stdin.readline

        n = int(input())
        degree = [0] * (n + 1)

        for _ in range(n - 1):
            u, v = map(int, input().split())
            degree[u] += 1
            degree[v] += 1

        print(sum(d % 2 for d in degree) // 2)

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""5
1 2
1 3
1 4
1 5
""") == "2\n", "sample 1"

assert run("""5
1 2
2 3
3 4
4 5
""") == "1\n", "sample 2"

assert run("""4
1 2
3 2
4 2
""") == "2\n", "sample 3"

assert run("""2
1 2
""") == "1\n", "minimum size tree"

assert run("""6
1 2
1 3
1 4
1 5
1 6
""") == "3\n", "large star"

assert run("""7
1 2
2 3
3 4
4 5
3 6
3 7
""") == "2\n", "branching chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`đỉnh nối với nhau bằng một cạnh |`1`| Kích thước cây tối thiểu có thể | 
| Ngôi sao năm lá |`3`| Xử lý đúng nhiều lá độ lẻ | 
| Xích có nhánh phụ |`2`| Các đỉnh bậc lẻ bên trong được tính chính xác | 

## Vỏ cạnh 

Đối với ngôi sao ba nút:```
3
1 2
1 3
```bằng cấp là`1, 2, 1`. Có hai đỉnh bậc lẻ nên thuật toán đưa ra`1`. Một đường đi từ đỉnh`2`đến đỉnh`3`sơn toàn bộ cây. 

Đối với ngôi sao bốn nút:```
4
1 2
1 3
1 4
```bằng cấp là`3, 1, 1, 1`. Cả bốn đỉnh đều lẻ. Thuật toán đếm bốn đỉnh lẻ và trả về`2`. Một thao tác có thể nối hai lá qua tâm, nhưng thao tác thứ hai là bắt buộc đối với nhánh còn lại. 

Đối với chuỗi năm nút:```
5
1 2
2 3
3 4
4 5
```chỉ các đỉnh`1`Và`5`có mức độ lẻ. Thuật toán trả về`1`, bởi vì việc chọn hai điểm cuối này sẽ vẽ nên toàn bộ chuỗi. Điều này xác nhận rằng các đường dẫn dài không yêu cầu chia thành các hoạt động nhỏ hơn. 

Đối với cây có đỉnh phân nhánh là số lẻ:```
4
1 2
3 2
4 2
```đỉnh`2`có bằng cấp`3`, và lá cũng có độ`1`. Số lẻ là`4`, vậy câu trả lời là`2`. Thuật toán đưa tâm vào tính chẵn lẻ, tránh sai sót khi chỉ đếm lá.
