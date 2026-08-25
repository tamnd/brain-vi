---
title: "CF 104313I - \u041c\u0435\u0442\u0440\u043e"
description: "Chúng ta có một tập hợp các đường thẳng trên mặt phẳng, mỗi đường được xác định bởi một phương trình có dạng $y = kx + b$. Chúng ta không được yêu cầu phân tích giao điểm giữa các cặp đường tùy ý hoặc tìm điểm giao nhau hình học."
date: "2026-07-01T19:47:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104313
codeforces_index: "I"
codeforces_contest_name: "II \u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 104313
solve_time_s: 56
verified: true
draft: false
---

[CF 104313I - \u041c\u0435\u0442\u0440\u043e](https://codeforces.com/problemset/problem/104313/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta một tập hợp các đường thẳng trên mặt phẳng, mỗi đường được xác định bởi một phương trình có dạng$y = kx + b$. Chúng ta không được yêu cầu phân tích giao điểm giữa các cặp đường tùy ý hoặc tìm điểm giao nhau hình học. Thay vào đó, chúng ta chỉ quan tâm đến vị trí các đường này giao nhau với trục tung$x = 0$, tức là trục y. 

Mỗi dòng đóng góp chính xác một điểm trên trục y: giá trị của nó tại$x = 0$, đơn giản là$b$. Tuy nhiên, vấn đề không chỉ nằm ở việc liệt kê những giá trị này. Nhiều đường dây có thể chia sẻ cùng một điểm chặn$b$, nghĩa là chúng đi qua cùng một điểm trên trục y. Nhiệm vụ là chọn tọa độ nguyên$y$trên trục y sao cho số đường thẳng tối đa đi qua điểm$(0, y)$. Nếu một số giá trị y đạt được tần số tối đa này, chúng ta phải trả về y nhỏ nhất như vậy. 

Vì vậy, toàn bộ vấn đề quy về việc tìm giá trị thường xuyên nhất trong số tất cả các giá trị đã cho$b$các hệ số, với một bộ ngắt kết nối thiên về giá trị nhỏ nhất. 

Kích thước đầu vào đạt tới$10^5$, điều này ngay lập tức loại trừ bất kỳ$O(n^2)$so sánh. Chúng ta phải quy bài toán thành bài toán tổng hợp tần số, có thể giải được trong thời gian tuyến tính hoặc gần tuyến tính bằng cách sử dụng phép băm hoặc sắp xếp. 

Một trường hợp cạnh ngây thơ nhưng mang tính hướng dẫn xuất hiện khi nhiều dòng có chung điểm chặn: 

đầu vào:```
4
2 5
3 5
-1 5
10 5
```Tất cả các đường thẳng cắt trục y tại$y = 5$, vì vậy câu trả lời đúng là 5. Bất kỳ cách tiếp cận nào cố gắng xem xét giao điểm giữa các đường một cách nhầm lẫn thay vì chỉ đánh giá tại$x = 0$sẽ làm nhiệm vụ trở nên phức tạp hơn và có thể thất bại trong điều kiện hạn chế về thời gian. 

Một trường hợp tinh tế khác liên quan đến việc phá vỡ ràng buộc: 

đầu vào:```
3
1 1
2 2
3 2
```Ở đây, y = 2 xuất hiện hai lần trong khi y = 1 xuất hiện một lần, vì vậy câu trả lời là 2. Ví dụ: nếu tần số bằng nhau:```
3
1 1
2 2
3 1
```cả 1 và 2 đều xuất hiện hai lần nên chúng ta phải xuất ra 1 vì nó nhỏ hơn. 

## Phương pháp tiếp cận 

Cách giải thích mạnh mẽ sẽ là so sánh từng cặp đường thẳng và bằng cách nào đó suy ra điểm trục y nào được “chia sẻ nhiều nhất”. Nhưng vì mỗi đường đóng góp độc lập vào chính xác một điểm chặn trục y nên việc so sánh theo cặp là không cần thiết. Một cách tiếp cận đơn giản có thể cố gắng tính toán số lượng bằng cách kiểm tra từng dòng với tất cả các dòng khác, dẫn đến$O(n^2)$thời gian. Với$n = 10^5$, điều này trở thành$10^{10}$hoạt động, điều đó là không thể thực hiện được. 

Quan sát quan trọng là mỗi dòng được đặc trưng đầy đủ cho bài toán này bằng một số duy nhất: giao điểm y của nó$b$. Độ dốc$k$không bao giờ ảnh hưởng đến nơi nó gặp trục y. Do đó, vấn đề giảm xuống việc đếm tần số của các số nguyên và chọn tần số thường xuyên nhất với một tie-break nhỏ nhất về mặt từ điển. 

Điều này biến vấn đề thành một cấu trúc biểu đồ tiêu chuẩn lên đến$10^5$số nguyên, có thể được xử lý hiệu quả bằng cách sử dụng từ điển hoặc sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| So sánh cặp Brute Force |$O(n^2)$|$O(1)$| Quá chậm | 
| Đếm tần số (Bản đồ băm / Sắp xếp) |$O(n)$trung bình /$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách giảm mỗi dòng thành một giá trị duy nhất và sau đó giải quyết vấn đề tối đa hóa tần số. 

1. Đọc tất cả các dòng và chỉ trích xuất phần chặn$b$từ mỗi phương trình. Đây là giá trị duy nhất xác định giao điểm trục y, vì việc thiết lập$x = 0$loại bỏ thuật ngữ độ dốc. 
2. Duy trì bản đồ tần số từ các giá trị nguyên của$b$bao nhiêu lần chúng xuất hiện. Mỗi lần xuất hiện tương ứng với một đường đi qua cùng một điểm trên trục y. 
3. Lặp lại tất cả các mục trong bản đồ tần suất và theo dõi ứng viên tốt nhất. Ứng cử viên tốt nhất được xác định trước tiên theo tần số tối đa và trong trường hợp có mối quan hệ nhỏ hơn$b$. 
4. Xuất kết quả đã chọn$b$. 

Phần tinh tế nhất là quy tắc ràng buộc. Nó phải được thực thi một cách rõ ràng: bất cứ khi nào chúng tôi thấy tần số giống với tần số tốt nhất hiện tại, chúng tôi chỉ cập nhật câu trả lời nếu giá trị mới nhỏ hơn. 

### Tại sao nó hoạt động 

Mỗi đường đóng góp chính xác một điểm trên trục y và không có hai đường phân biệt nào có thể đóng góp bất cứ điều gì ngoài điểm giao nhau của chúng. Do đó, việc đếm số lượng đường đi qua tọa độ y cho trước hoàn toàn tương đương với việc đếm số lần giao điểm đó xuất hiện trong đầu vào. Thuật toán đếm toàn diện tất cả các đóng góp và chọn giá trị tần số tối đa với mức giới hạn xác định, do đó nó không thể bỏ lỡ ứng cử viên tốt hơn hoặc chọn một ứng cử viên dưới mức tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    freq = {}

    for _ in range(n):
        k, b = map(int, input().split())
        freq[b] = freq.get(b, 0) + 1

    best_y = None
    best_cnt = 0

    for y, cnt in freq.items():
        if cnt > best_cnt or (cnt == best_cnt and (best_y is None or y < best_y)):
            best_cnt = cnt
            best_y = y

    print(best_y)

if __name__ == "__main__":
    solve()
```Giải pháp cô lập chặn$b$ngay lập tức và bỏ qua$k$hoàn toàn vì nó không ảnh hưởng đến các giao điểm với trục y. Từ điển tần số tổng hợp các lần xuất hiện theo thời gian tuyến tính. Lần quét cuối cùng qua từ điển đảm bảo lựa chọn chính xác trong cả điều kiện thứ tự chính và phụ. 

Phải cẩn thận khi khởi tạo:`best_y`bắt đầu như`None`để giá trị gặp đầu tiên luôn được chấp nhận và các phép so sánh xử lý chính xác quy tắc ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 1
0 0
-10 1
```Chúng tôi xử lý các lần chặn và xây dựng tần số. 

| Bước | b | freq[b] sau khi cập nhật | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 0 | 1 | 
| 3 | 1 | 2 | 

Bây giờ đánh giá ứng viên tốt nhất: 

| y | đếm | tốt nhất_y | tốt nhất_cnt | quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | khởi tạo | 
| 0 | 1 | 1 | 2 | bỏ qua | 

Đầu ra là 1. 

Điều này xác nhận rằng các lần chặn lặp lại sẽ tích lũy chính xác và chiếm ưu thế trong các giá trị đơn lẻ nhỏ hơn. 

### Ví dụ 2 

đầu vào:```
4
1 5
2 3
3 3
4 5
```Tần số: 

| Bước | b | freq[b] sau khi cập nhật | 
| --- | --- | --- | 
| 1 | 5 | 1 | 
| 2 | 3 | 1 | 
| 3 | 3 | 2 | 
| 4 | 5 | 2 | 

Bây giờ lựa chọn: 

| y | đếm | tốt nhất_y | tốt nhất_cnt | quyết định | 
| --- | --- | --- | --- | --- | 
| 5 | 2 | 5 | 2 | khởi tạo | 
| 3 | 2 | 3 | 2 | hòa, thắng nhỏ hơn | 

Đầu ra là 3. 

Điều này thể hiện hành vi ràng buộc chính xác khi nhiều giá trị y có cùng tần số tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi dòng được xử lý một lần và tổng hợp từ điển có thời gian khấu hao không đổi | 
| Không gian |$O(n)$| Bản đồ tần số có thể lưu trữ tất cả các điểm chặn riêng biệt | 

Với$n \le 10^5$, thời gian tuyến tính và mức sử dụng bộ nhớ phù hợp thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import builtins
    input_backup = builtins.input

    def fake_input():
        return sys.stdin.readline().rstrip("\n")

    builtins.input = fake_input

    try:
        solve()
        return output.getvalue().strip()
    finally:
        builtins.input = input_backup
        sys.stdout = sys.__stdout__

# sample-like cases
assert run("3\n3 1\n0 0\n-10 1\n") == "1"
assert run("3\n1 1\n2 2\n3 2\n") == "2"

# all equal
assert run("4\n1 7\n2 7\n3 7\n4 7\n") == "7"

# tie-breaking
assert run("4\n1 5\n2 3\n3 3\n4 5\n") == "3"

# single element
assert run("1\n10 42\n") == "42"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều giống nhau b | giá trị đơn | xử lý tần số thống nhất | 
| cà vạt hỗn hợp | y được chọn nhỏ nhất | bẻ dây đúng cách | 
| dòng đơn | đầu ra trực tiếp | trường hợp cơ sở đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các đường có chung điểm chặn. Ví dụ:```
3
1 10
2 10
3 10
```Bản đồ tần số trở thành`{10: 3}`và thuật toán chọn ngay 10 là ứng cử viên tối đa và duy nhất. Không có logic ràng buộc nào được kích hoạt, xác nhận rằng đường dẫn khởi tạo là chính xác. 

Một trường hợp khác là xen kẽ các lần đánh chặn:```
6
1 1
2 2
3 1
4 2
5 3
6 3
```Tần số đều bằng nhau. Thuật toán quét các mục theo thứ tự từ điển tùy ý, nhưng việc bẻ khóa đảm bảo giá trị nhỏ nhất được chọn. Ngay cả khi thứ tự lặp lại không thể đoán trước được thì việc so sánh vẫn thực thi việc lựa chọn xác định 1. 

Trường hợp cuối cùng là đầu vào tối thiểu:```
1
100 -5
```Bản đồ chỉ chứa`-5`, vì vậy nó được chọn trực tiếp. Điều này xác nhận tính đúng đắn khi không có bước so sánh nào xảy ra một cách có ý nghĩa.
