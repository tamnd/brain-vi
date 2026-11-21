---
title: "CF 234G - Thực hành"
description: "Chúng ta có n cầu thủ bóng đá, được đánh số từ 1 đến n. Mỗi buổi tập bao gồm việc chia tất cả người chơi thành hai đội không trống."
date: "2026-06-04T10:00:19+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "divide-and-conquer", "implementation"]
categories: ["algorithms"]
codeforces_contest: 234
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 145 (Div. 2, ACM-ICPC Rules)"
rating: 1600
weight: 234
solve_time_s: 98
verified: true
draft: false
---

[CF 234G - Thực hành](https://codeforces.com/problemset/problem/234/G) 

**Đánh giá:** 1600 
**Tas:** thuật toán xây dựng, chia để trị, thực hiện 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`cầu thủ bóng đá, được đánh số từ`1`ĐẾN`n`. 

Mỗi buổi tập bao gồm việc chia tất cả người chơi thành hai đội không trống. Sau một số lần luyện tập, chúng tôi muốn mỗi cặp người chơi phải được tách ra ít nhất một lần, nghĩa là có một số lần luyện tập trong đó hai người chơi được xếp vào các đội đối diện nhau. 

Nhiệm vụ không chỉ là xây dựng một lịch trình như vậy mà còn phải sử dụng số lượng thực hành tối thiểu có thể. 

Đầu vào chỉ chứa một số nguyên`n`. Đầu ra trước tiên phải in số lượng bài tập tối thiểu, sau đó mô tả từng bài tập bằng cách liệt kê một trong hai đội. Mọi cách xây dựng tối ưu đều được chấp nhận. 

Ràng buộc`n ≤ 1000`đủ nhỏ để chúng ta có thể xuất ra tất cả các đội một cách rõ ràng. Thách thức không phải là độ phức tạp tính toán mà là tìm ra số lượng thực hành tối thiểu và xây dựng chúng. 

Một cách hữu ích để suy nghĩ về vấn đề này là gán cho mỗi người chơi một mã nhị phân. Mỗi thực hành tương ứng với một vị trí bit. Trong quá trình luyện tập, người chơi có chút`0`đi đến một đội và những người chơi có chút`1`đi sang đội khác. Hai người chơi bị tách ra bất cứ khi nào mã của họ khác nhau một chút. 

Vì có tối đa 1000 người chơi nên biểu diễn nhị phân lên tới 10 bit là đủ vì`2^10 = 1024`. 

Có một vài trường hợp khó nhận thấy. 

Vì`n = 2`, chỉ tồn tại một cặp Một bài tập duy nhất đặt người chơi 1 đấu với người chơi 2 là đủ. 

đầu vào:```
2
```Một đầu ra hợp lệ là:```
1
1 1
```Việc thực hiện bất cẩn luôn dẫn đến kết quả`⌈log2 n⌉`thực hành mà không xem xét các giá trị nhỏ có thể vô tình tạo ra một nhóm trống trong một số thực hành. 

Một trường hợp cạnh khác xảy ra khi`n`không phải là sức mạnh của hai. 

đầu vào:```
5
```Chúng ta cần ba thực hành bởi vì`2^2 = 4 < 5 ≤ 8 = 2^3`. Một số vị trí bit có thể có sự phân chia rất không đồng đều, nhưng mọi buổi tập vẫn phải có cả hai đội không trống. Việc triển khai in mù tất cả các vị trí bit mà không kiểm tra sẽ không chính xác nếu vị trí bit chỉ chứa số 0 trong số các trình phát hiện có. 

Điều tinh tế cuối cùng là việc đánh số người chơi bắt đầu từ 1. Sử dụng số`1..n`trực tiếp vì mã nhị phân có thể tăng số bit cần thiết lên một. Việc xây dựng sạch sẽ sử dụng mã`0..n-1`. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là nghĩ trực tiếp về các cặp người chơi. có`n(n-1)/2`cặp. Chúng ta có thể liên tục thiết kế các phương pháp thực hành và kiểm tra xem cặp nào đã được tách ra. Việc tìm kiếm như vậy nhanh chóng trở thành tổ hợp vì mỗi lần luyện tập là sự phân chia người chơi thành hai nhóm không trống. Ngay cả đối với mức độ vừa phải`n`, số lượng phân vùng có thể là rất lớn. 

Cấu trúc thực sự xuất hiện khi chúng ta hỏi thông tin mà hoạt động đó cung cấp. Một thực tế chỉ cho chúng ta biết mỗi người chơi nằm ở phía nào của vách ngăn. Nếu chúng ta biểu diễn`m`luyện tập, mỗi người chơi sẽ nhận được một`m`-bit chữ ký mô tả sự phân công nhóm của nó trong tất cả các hoạt động. 

Hai người chơi bị tách ra ít nhất một lần khi chữ ký của họ khác nhau. Nếu hai người chơi có chữ ký giống hệt nhau thì họ luôn xuất hiện ở cùng một phía và không bao giờ bị tách rời. 

Điều này biến vấn đề thành một vấn đề mã hóa. Chúng tôi cần`n`chữ ký riêng biệt. Với`m`bit, có nhiều nhất`2^m`chữ ký khác nhau, vì vậy:$$2^m \ge n$$Kể từ đây:$$m \ge \lceil \log_2 n \rceil$$Điều này đưa ra giới hạn thấp hơn cho câu trả lời. 

Quan sát tương tự ngay lập tức đưa ra một cách xây dựng. Chỉ định người chơi`i`biểu diễn nhị phân của`i-1`. Vì các giá trị`0,1,\dots,n-1`là khác biệt, tất cả các chữ ký đều khác biệt. Đối với mỗi vị trí bit, hãy tạo một phương pháp phân tách người chơi theo bit đó. 

Mỗi cặp người chơi có cách biểu diễn nhị phân khác nhau, vì vậy chúng khác nhau đôi chút. Trong quá trình thực hành tương ứng, họ được xếp vào các đội đối diện nhau, đáp ứng yêu cầu. 

Giới hạn dưới và kết cấu khớp nhau, chứng tỏ tính tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số nguyên nhỏ nhất`m`như vậy`2^m ≥ n`. 
2. Đánh số người chơi từ`0`ĐẾN`n-1`nội bộ. Các giá trị này đóng vai trò là mã nhị phân duy nhất. 
3. Đối với từng vị trí bit`b`từ`0`ĐẾN`m-1`, tạo một bài thực hành. 
4. Trong thực tế`b`, đặt mọi người chơi có mã có bit`b`bằng`1`vào đội một. 
5. Tất cả người chơi còn lại tự động thuộc về đội thứ hai. 
6. Xuất các cầu thủ ở đội một cho mỗi buổi tập. 

Tại sao chỉ xuất ra những người chơi có bit là an toàn`1`? Từ`2^m ≥ n`, vị trí bit cao nhất được sử dụng là cần thiết. Đối với mỗi vị trí bit`b < m`, tồn tại ít nhất một mã có bit`b = 1`và ít nhất một mã có bit`b = 0`giữa`0..n-1`. Như vậy cả hai đội đều không trống. 

### Tại sao nó hoạt động 

Mỗi người chơi nhận được biểu diễn nhị phân của một số nguyên duy nhất từ`0`ĐẾN`n-1`. 

Hãy xem xét bất kỳ hai người chơi khác nhau. Các số nguyên được gán của chúng khác nhau nên cách biểu diễn nhị phân của chúng khác nhau ở ít nhất một vị trí bit. Trong quá trình luyện tập tương ứng với bit đó, một người chơi thuộc đội thứ nhất và người còn lại thuộc đội thứ hai. Như vậy mỗi cặp được tách ra ít nhất một lần. 

Bây giờ hãy xem xét số lượng thực hành tối thiểu. Sau đó`m`thực tiễn, mỗi người chơi chỉ có thể được mô tả bằng một`m`-bit chữ ký. Có nhiều nhất`2^m`chữ ký riêng biệt. Vì tất cả`n`người chơi phải có chữ ký khác nhau, chúng tôi cần`2^m ≥ n`. Do đó ít nhất`⌈log₂ n⌉`thực tiễn là cần thiết. 

Công trình của chúng tôi sử dụng chính xác`⌈log₂ n⌉`thực tiễn nên nó là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    m = 0
    while (1 << m) < n:
        m += 1

    print(m)

    for b in range(m):
        team = []
        for i in range(n):
            if (i >> b) & 1:
                team.append(str(i + 1))

        print(len(team), *team)

solve()
```Vòng lặp đầu tiên tính toán số bit tối thiểu cần thiết để biểu diễn`n`mã riêng biệt. 

Người chơi có số`i + 1`nhận mã nội bộ`i`. sử dụng`0..n-1`là quan trọng bởi vì chính xác`n`cần có các mã riêng biệt và điều này tránh lãng phí một giá trị mã. 

Đối với mỗi vị trí bit, giải pháp tập hợp tất cả người chơi có mã chứa`1`trong chút đó. Những người chơi đó tạo thành đội đầu tiên. Những người khác ngầm thuộc về đội thứ hai. 

Một điểm tinh tế là định dạng đầu ra chỉ yêu cầu một đội được liệt kê. Những người chơi còn lại tự động thành lập đội khác. 

Một điểm tinh tế khác là chứng minh rằng đội được liệt kê không bao giờ trống. Từ`m`là giá trị nhỏ nhất với`2^m ≥ n`, mọi vị trí bit bên dưới`m`xuất hiện dưới dạng`1`có ít nhất một số trong số`0..n-1`. Vì vậy, mỗi bài tập được in đều có ít nhất một người chơi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```Chúng ta cần cái nhỏ nhất`m`với`2^m ≥ 2`, Vì thế`m = 1`. 

| Vị trí bit | Mã | Người chơi có bit = 1 | Dòng đầu ra | 
| --- | --- | --- | --- | 
| 0 | 0,1 | {2} |`1 2`| 

Đầu ra:```
1
1 2
```Người chơi 1 và người chơi 2 ở hai đội đối diện nhau trong buổi tập duy nhất nên cặp duy nhất sẽ bị tách ra. 

### Ví dụ 2 

đầu vào:```
5
```Chúng tôi cần`m = 3`bởi vì`2^2 < 5 ≤ 2^3`. 

Mã nội bộ là: 

| Người chơi | Mã | 
| --- | --- | 
| 1 | 000 | 
| 2 | 001 | 
| 3 | 010 | 
| 4 | 011 | 
| 5 | 100 | 

Thực hành xây dựng: 

| Vị trí bit | Người chơi có bit = 1 | 
| --- | --- | 
| 0 | 2, 4 | 
| 1 | 3, 4 | 
| 2 | 5 | 

Đầu ra có thể:```
3
2 2 4
2 3 4
1 5
```Hãy xem xét người chơi 3 và 5. Mã của họ là`010`Và`100`. Chúng khác nhau ở bit 1 và 2, vì vậy chúng được tách biệt trong nhiều lần thực hành. Đối số tương tự áp dụng cho mọi cặp vì tất cả các mã đều khác biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Chúng tôi kiểm tra từng người chơi ở mọi vị trí bit | 
| Không gian | O(n) | Một danh sách đội được lưu trữ cùng một lúc | 

Từ`n ≤ 1000`, chúng ta có nhiều nhất`⌈log₂ 1000⌉ = 10`thực hành. Thuật toán thực hiện khoảng`1000 × 10 = 10000`kiểm tra, đó là tầm thường trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())

    m = 0
    while (1 << m) < n:
        m += 1

    out = [str(m)]

    for b in range(m):
        team = []
        for i in range(n):
            if (i >> b) & 1:
                team.append(str(i + 1))
        out.append(f"{len(team)}" + ("" if not team else " " + " ".join(team)))

    return "\n".join(out)

# provided sample
assert run("2\n") == "1\n1 2"

# minimum size
assert run("2\n").splitlines()[0] == "1"

# first non-power of two
assert run("3\n").splitlines()[0] == "2"

# power of two
assert run("8\n").splitlines()[0] == "3"

# maximum size
assert run("1000\n").splitlines()[0] == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`| một thực hành | Trường hợp hợp lệ nhỏ nhất | 
|`3`| hai thực hành | Chuyển đổi phi sức mạnh của hai | 
|`8`| ba thực hành | Sức mạnh chính xác của hai | 
|`1000`| mười thực hành | Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
2
```Thuật toán tính toán`m = 1`. Người chơi 1 nhận được mã`0`, người chơi 2 nhận được mã`1`. Bài tập đơn đặt người chơi 2 vào đội được liệt kê và người chơi 1 vào đội khác. Cặp duy nhất được tách ra ngay lập tức. 

Coi như:```
5
```Thuật toán tính toán`m = 3`. Cách thực hành thứ ba chỉ chứa người chơi 5 vì chỉ có mã`100`có tập bit cao nhất. Điều này vẫn hợp lệ vì cả hai đội vẫn không trống. Việc triển khai bất cẩn có thể từ chối không chính xác các phân vùng mất cân bằng cao như vậy. 

Coi như:```
8
```Đây`m = 3`chính xác. Mỗi người chơi nhận được một mã 3 bit riêng biệt từ`000`ĐẾN`111`. Việc xây dựng đạt được giới hạn dưới một cách chính xác, cho thấy không cần thực hành lần thứ tư. Thuộc tính tách cặp theo trực tiếp từ tính duy nhất của biểu diễn nhị phân.
