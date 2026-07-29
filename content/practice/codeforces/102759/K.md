---
title: "CF 102759K - Đồ thị may"
description: "Chúng ta có một tập hợp các điểm trên mặt phẳng biểu diễn các chấm trên một tấm vải. Trình tự may mô tả bước đi qua các dấu chấm này. Mỗi cặp chấm liên tiếp trong chuỗi sẽ tạo ra một cạnh, nhưng các cạnh xen kẽ giữa mặt trước và mặt sau của tấm vải."
date: "2026-07-29T00:17:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102759
codeforces_index: "K"
codeforces_contest_name: "XXI Open Cup, Grand Prix of Korea"
rating: 0
weight: 102759
solve_time_s: 65
verified: true
draft: false
---

[CF 102759K - Sơ đồ may](https://codeforces.com/problemset/problem/102759/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên mặt phẳng biểu diễn các chấm trên một tấm vải. Trình tự may mô tả bước đi qua các dấu chấm này. Mỗi cặp chấm liên tiếp trong chuỗi sẽ tạo ra một cạnh, nhưng các cạnh xen kẽ giữa mặt trước và mặt sau của tấm vải. Mục tiêu là làm cho cả hai bên chứa một biểu đồ được kết nối, không giao nhau bằng cách sử dụng chuỗi ngắn nhất có thể. 

Một chuỗi độ dài`k`tạo ra chính xác`k - 1`tổng số các cạnh. Mặt trước nhận các cạnh tại các vị trí`(1,2), (3,4), ...`, trong khi mặt sau nhận các cạnh xen kẽ còn lại. Mỗi bên phải kết nối tất cả`N`dấu chấm, vì vậy mỗi bên cần ít nhất`N - 1`các cạnh vì một biểu đồ được kết nối trên`N`đỉnh không thể có ít cạnh hơn cây. 

Điều này ngay lập tức đưa ra một giới hạn dưới. Hai bên cùng nhau cần ít nhất`2(N - 1)`các cạnh, vì vậy chuỗi cần ít nhất`2(N - 1) + 1 = 2N - 1`dấu chấm. Từ`N`nhiều nhất là`1000`, ngay cả thuật toán bậc hai cũng có thể được chấp nhận trong nhiều cài đặt, nhưng cấu trúc thực tế cho phép xây dựng tuyến tính đơn giản hơn nhiều. Chúng ta không cần các thuật toán hình học như bao lồi hay kiểm tra giao điểm. 

Các trường hợp chính xuất phát từ sự hiểu lầm "ngắn nhất" nghĩa là gì. Mục tiêu là số vị trí trong chuỗi chứ không phải tổng chiều dài của các đoạn được vẽ. 

Ví dụ: với hai dấu chấm:```
Input
2
10 10
20 20
```Độ dài đầu ra chính xác là`3`, với trình tự như sau:```
3
1 2 1
```Một giải pháp bất cẩn có thể cố gắng tạo ra hai cạnh khác nhau hoặc hai cây bao trùm khác nhau, nhưng cả hai bên đều được phép sử dụng cùng một cạnh hình học. Mặt trước và mặt sau là các lớp riêng biệt. 

Một trường hợp cạnh khác là khi tất cả các điểm được sắp xếp theo hình dạng phức tạp:```
Input
4
1 1
100 1
50 50
20 80
```Đầu ra đúng vẫn có độ dài`7`. Một cách triển khai đơn giản có thể cố gắng tìm một thứ tự đặc biệt không giao nhau cho tất cả các điểm, nhưng điều này là không cần thiết vì một ngôi sao có tâm tại bất kỳ điểm nào luôn không giao nhau. 

## Phương pháp tiếp cận 

Ý tưởng đầu tiên của nhiều người là xây dựng rõ ràng hai cây bao trùm phẳng khác nhau. Vì mỗi cây bao trùm đều có`N - 1`các cạnh, câu trả lời khi đó sẽ là chuỗi xen kẽ giữa hai cây đó. Điều này đúng nhưng nó tạo ra công việc không cần thiết. Việc tìm các cây phẳng thích hợp rồi sắp xếp các cạnh của chúng thành một dãy xen kẽ hợp lệ khó hơn nhiều so với yêu cầu thực tế của bài toán. 

Quan điểm bạo lực là chúng ta cần`2N - 2`tổng số cạnh và chúng ta có thể tìm kiếm trong số các cây có thể. Số lượng cây bao trùm có thể có trong một đồ thị hoàn chỉnh là rất lớn. Vì`N`điểm, biểu đồ hoàn chỉnh chứa`N(N-1)/2`các cạnh có thể có và số lượng cây có thể có là theo cấp số nhân, do đó cách tiếp cận này trở nên bất khả thi ngay lập tức. 

Quan sát quan trọng là cùng một cây có thể xuất hiện trên cả hai mặt của tấm vải. Hai bên độc lập. Nếu chúng ta chọn một dấu chấm làm tâm, nối mọi dấu chấm khác với dấu chấm đó và lặp lại các cạnh tương tự ở phía bên kia, thì cả hai mặt đều đã được kết nối và phẳng. 

Đồ thị hình sao luôn phẳng vì mọi cạnh đều có chung điểm cuối. Trình tự```
center, vertex1, center, vertex2, center, vertex3, ..., center
```tạo ra các cạnh sao xen kẽ ở hai bên. Mọi cạnh còn lại thuộc về mặt trước và các cạnh còn lại thuộc về mặt sau. Mỗi bên nhận được chính xác`N - 1`các cạnh. 

Giới hạn dưới chứng tỏ rằng một chuỗi có độ dài`2N - 1`là tốt nhất có thể, và việc xây dựng đạt đến giới hạn đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(N2) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chọn bất kỳ dấu chấm nào làm tâm của ngôi sao. Dấu chấm đầu tiên có tác dụng vì bài toán không đặt ra bất kỳ hạn chế nào về việc điểm nào sẽ trở thành tâm. 
2. Xuất ra tâm, sau đó là một dấu chấm khác, rồi lại ở giữa, lặp lại mẫu này cho đến khi mọi dấu chấm không ở giữa xuất hiện một lần. 

Điều này làm cho mỗi cặp chấm liên tiếp tạo thành một cạnh hình sao. Tính chất xen kẽ của trình tự may sẽ tự động phân bổ các cạnh này giữa hai bên. 
3. Đếm độ dài chuỗi được tạo ra và in nó. Chiều dài luôn là`2N - 1`, đó là độ dài tối thiểu có thể. 

Tại sao nó hoạt động: Mặt trước nhận được mọi cạnh khác của chuỗi. Các cạnh đó là tất cả các kết nối giữa tâm được chọn và một dấu chấm khác, vì vậy mặt trước là một ngôi sao chứa mọi dấu chấm. Mặt sau nhận các mép còn lại, là các mép hình sao giống nhau ở mặt kia của tấm vải. Một ngôi sao được kết nối và không có hai cạnh nào của nó có thể cắt nhau ngoại trừ ở tâm chung. Vì bất kỳ giải pháp hợp lệ nào cũng cần ít nhất`2N - 1`các vị trí trình tự, cách xây dựng này là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    for _ in range(n):
        input()

    ans = [1]
    for i in range(2, n + 1):
        ans.append(i)
        ans.append(1)

    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai bỏ qua tọa độ sau khi đọc chúng vì việc xây dựng không phụ thuộc vào sự sắp xếp hình học của các dấu chấm. Bất kỳ trung tâm được chọn nào cũng hoạt động. 

Trình tự bắt đầu bằng dấu chấm`1`. Đối với mỗi dấu chấm khác`i`, chúng tôi nối thêm`i`rồi quay trở lại trung tâm. Tâm được trả về là cần thiết vì cạnh tiếp theo phải tiếp tục xen kẽ giữa các cạnh trong khi vẫn thuộc cùng một ngôi sao. 

Trình tự cuối cùng chứa`1 + 2(N - 1)`các giá trị, chính xác là`2N - 1`. Không có vấn đề về ranh giới vì vòng lặp chỉ chạy trên các điểm không ở giữa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 1
2 4
3 2
4 5
5 3
```Một dấu vết có thể có của thuật toán: 

| Bước | Hành động hiện tại | Trình tự | 
| --- | --- | --- | 
| 1 | Chọn trung tâm 1 | 1 | 
| 2 | Thêm dấu chấm 2 và quay lại | 1 2 1 | 
| 3 | Thêm dấu chấm 3 và quay lại | 1 2 1 3 1 | 
| 4 | Thêm dấu chấm 4 và quay lại | 1 2 1 3 1 4 1 | 
| 5 | Thêm dấu chấm 5 và quay lại | 1 2 1 3 1 4 1 5 1 | 

Độ dài đầu ra là`9`, khớp với giá trị tối thiểu có thể`2N - 1`. 

### Ví dụ tùy chỉnh 

đầu vào:```
3
0 0
5 7
8 2
```Dấu vết: 

| Bước | Hành động hiện tại | Trình tự | 
| --- | --- | --- | 
| 1 | Chọn trung tâm 1 | 1 | 
| 2 | Thêm dấu chấm 2 và quay lại | 1 2 1 | 
| 3 | Thêm dấu chấm 3 và quay lại | 1 2 1 3 1 | 

Mặt trước nhận được các cạnh`(1,2)`Và`(1,3)`. Mặt sau nhận được hai cạnh giống nhau. Cả hai đều là những ngôi sao được kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi dấu chấm được đọc một lần và thêm một lần vào câu trả lời. | 
| Không gian | O(N) | Chuỗi câu trả lời lưu trữ`2N - 1`số nguyên. | 

Những ràng buộc cho phép`N = 1000`, nên nghiệm tuyến tính này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    n = int(input())
    for _ in range(n):
        input()

    ans = [1]
    for i in range(2, n + 1):
        ans.append(i)
        ans.append(1)

    out = str(len(ans)) + "\n" + " ".join(map(str, ans)) + "\n"

    sys.stdin = old_stdin
    return out

assert solve("""5
1 1
2 4
3 2
4 5
5 3
""") == """9
1 2 1 3 1 4 1 5 1
""", "sample 1"

assert solve("""2
10 10
20 20
""") == """3
1 2 1
""", "minimum size"

assert solve("""4
1 1
2 2
3 3
4 4
""") == """7
1 2 1 3 1 4 1
""", "all points on a line"

assert solve("""6
1000000000 1
2 999999999
500 500
700 800
900 100
300 200
""") == """11
1 2 1 3 1 4 1 5 1 6 1
""", "large coordinates"

assert solve("""3
5 5
5 10
10 5
""") == """5
1 2 1 3 1
""", "small planar case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai dấu chấm | Chiều dài`3`| Số đỉnh tối thiểu có thể có trong dãy | 
| Bốn dấu chấm thẳng hàng | Chiều dài`7`| Hình học không ảnh hưởng đến công trình | 
| Giá trị tọa độ lớn | Chiều dài`11`| Tọa độ không liên quan và không tràn | 
| Ba chấm | Chiều dài`5`| Luân phiên đúng với ngôi sao nhỏ nhất không tầm thường | 

## Vỏ cạnh 

Đối với hai dấu chấm, thuật toán tạo ra:```
1 2 1
```Cạnh đầu tiên đi về phía trước và cạnh thứ hai đi về phía sau. Cả hai bên đều chứa kết nối duy nhất có thể, vì vậy cả hai đều được kết nối. 

Đối với các điểm đã có sự sắp xếp hình học khó khăn, chẳng hạn như nhiều điểm tạo thành đa giác lồi hoặc đám mây ngẫu nhiên, thuật toán vẫn chọn một tâm và chỉ vẽ các đoạn từ tâm đó. Vì tất cả các phân đoạn đều có chung một điểm cuối nên không có cặp cạnh nào có thể giao nhau trong phần bên trong của chúng. 

Đối với kích thước đầu vào tối đa, thuật toán không thực hiện bất kỳ phép tính hình học nào. Nó chỉ lưu trữ và in`1999`số nguyên, vì chuỗi ngắn nhất có độ dài`2 * 1000 - 1`. Điều này giữ cho thời gian chạy tuyến tính và tránh sự phức tạp không cần thiết.
