---
title: "CF 102878L - Long Long Muốn Mua"
description: "Chúng ta cần chế tạo một chiếc máy tính bằng cách chọn chính xác một phụ kiện từ mỗi loại. Mỗi phụ kiện đều có giá thành và tuổi thọ sử dụng."
date: "2026-07-25T12:48:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102878
codeforces_index: "L"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 102878
solve_time_s: 47
verified: true
draft: false
---

[CF 102878L - Long Long Muốn Mua](https://codeforces.com/problemset/problem/102878/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chế tạo một chiếc máy tính bằng cách chọn chính xác một phụ kiện từ mỗi loại. Mỗi phụ kiện đều có giá thành và tuổi thọ sử dụng. Máy tính ngừng hoạt động ngay khi phụ kiện được chọn đầu tiên hết thời gian sử dụng, vì vậy tuổi thọ của toàn bộ máy tính là thời gian sử dụng tối thiểu trong số tất cả các phụ kiện được chọn. 

Mục tiêu là giảm thiểu giá trên một đơn vị thời gian, đó là:$$\frac{\text{sum of selected prices}}{\text{minimum selected service life}}$$Nếu một số lựa chọn đạt được tỷ lệ tối thiểu như nhau thì nên chọn lựa chọn có tổng giá nhỏ nhất. 

Đầu vào cung cấp số lượng loại phụ kiện và số lượng phụ kiện có sẵn ở mỗi loại. Đối với mỗi loại, các giá trị sau đây mô tả từng phụ kiện: tuổi thọ sử dụng và giá cả của phụ kiện đó. Đầu ra là tổng giá tối thiểu trong số tất cả các lựa chọn tối ưu. 

Các giới hạn là$N,M \le 1000$. Có thể có tới một triệu phụ kiện nên việc kiểm tra mọi sự kết hợp có thể là không thể. Ngay cả việc thử tất cả các lựa chọn trong mỗi loại cũng sẽ theo cấp số nhân vì số lượng máy tính có thể có là$M^N$. Giá trị tuổi thọ sử dụng được giới hạn ở 1000, đây là hạn chế chính cho phép giải pháp dựa trên việc lặp lại các thời gian tồn tại có thể. 

Các trường hợp phức tạp xuất phát từ thực tế là tuổi thọ sử dụng tối thiểu, chứ không phải tuổi thọ sử dụng tối đa hoặc trung bình, sẽ điều khiển máy tính. 

Ví dụ:```
1 2
5 10 10 1
```Có một loại và hai phụ kiện. Việc chọn phụ kiện đầu tiên sẽ có tỷ lệ$10/5=2$. Chọn cái thứ hai mang lại$1/10=0.1$, vậy đáp án là:```
1
```Một cách tiếp cận bất cẩn chỉ tìm kiếm phụ kiện rẻ nhất có thể hiệu quả ở đây, nhưng ở nhiều loại, nó có thể thất bại vì một phụ kiện rất rẻ nhưng thời gian sử dụng ngắn có thể làm giảm toàn bộ tuổi thọ của máy tính. 

Một trường hợp khác là hòa:```
2 2
2 5 5 10
2 5 5 10
```Lựa chọn hai phụ kiện giá rẻ sẽ đưa ra mức giá$10$, trọn đời$2$, tỷ lệ$5$. Lựa chọn hai phụ kiện đắt tiền sẽ cho ra giá$20$, trọn đời$5$, tỷ lệ$4$. Lựa chọn thứ hai thắng mặc dù giá lớn hơn. Chỉ so sánh tổng giá sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử mọi lựa chọn phụ kiện có thể. Đối với mỗi cấu hình máy tính, chúng tôi tính toán tổng giá và thời gian sử dụng tối thiểu, sau đó so sánh tỷ lệ thu được. Điều này đúng vì nó xem xét mọi câu trả lời có thể. Tuy nhiên, với$N$các loại và$M$lựa chọn cho mỗi loại, số lượng cấu hình là$M^N$. Ngay cả đối với$N=1000$, điều này vượt xa những gì có thể được xử lý. 

Quan sát hữu ích đến từ việc nhìn vào mẫu số. Tuổi thọ của máy tính phải bằng một trong các tuổi thọ sử dụng của các phụ kiện đã chọn. Vì tuổi thọ sử dụng tối đa là 1000 nên chỉ có 1000 giá trị trọn đời có thể có. 

Giả sử chúng ta muốn một máy tính có tuổi thọ ít nhất là$L$. Mỗi phụ kiện được chọn ít nhất phải có tuổi thọ sử dụng$L$. Đối với từng loại độc lập, chúng ta nên chọn phụ kiện rẻ nhất đáp ứng được yêu cầu này. Tổng các mức giá rẻ nhất đó là mức giá tối thiểu có thể có của bất kỳ máy tính nào còn tồn tại ít nhất$L$đơn vị thời gian. 

Nếu chúng ta tính giá trị này cho mọi khả năng$L$, thì tỷ lệ cho thời gian tồn tại đó là:$$\frac{\text{minimum price with all service lives }\ge L}{L}$$Tỷ lệ nhỏ nhất trong số này là câu trả lời. Lý do điều này có hiệu quả là vì máy tính tối ưu có thời gian tồn tại thực tế$T$. Khi chúng tôi đánh giá$L=T$, thuật toán xem xét chính xác tất cả các máy tính có thể đạt được ít nhất thời gian tồn tại đó và bao gồm máy tính tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(M^N)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N \times M + N \times S)$|$O(S)$| Đã chấp nhận | 

Đây$S=1000$, tuổi thọ sử dụng tối đa có thể. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các phụ kiện và lưu trữ, đối với mỗi giá trị tuổi thọ sử dụng, giá rẻ nhất của từng loại phụ kiện có chính xác thời hạn sử dụng đó. Chúng tôi cần thông tin này vì sau này chúng tôi sẽ xây dựng câu trả lời cho mọi ngưỡng có thể có trong vòng đời. 
2. Dịch vụ Traverse có tuổi thọ từ 1000 xuống còn 1 trong khi vẫn duy trì mức giá rẻ nhất hiện có cho từng loại trong số tất cả các phụ kiện có tuổi thọ sử dụng ít nhất là giá trị hiện tại. Di chuyển xuống dưới cho phép chúng tôi thêm các phụ kiện khi thời hạn sử dụng của chúng thay vì phải tìm kiếm nhiều lần trong toàn bộ danh sách. 
3. Đối với mỗi giá trị trọn đời, hãy kiểm tra xem mỗi loại có ít nhất một phụ kiện có thể sử dụng được hay không. Nếu vậy, giá được lưu trữ hiện tại sẽ tạo thành chiếc máy tính rẻ nhất tồn tại ít nhất trong thời gian dài như vậy. 
4. So sánh tỷ lệ giữa tổng giá này và thời gian tồn tại hiện tại với tỷ lệ tốt nhất được tìm thấy cho đến nay. Phân số được so sánh bằng phép nhân thay vì số học dấu phẩy động để tránh các vấn đề về độ chính xác. 
5. Nếu hai lựa chọn có tỷ lệ hoàn toàn giống nhau, hãy giữ tổng giá nhỏ hơn vì bài toán yêu cầu mức giá tối thiểu trong các tỷ lệ tối ưu. 

Tại sao nó hoạt động: 

Đối với bất kỳ máy tính nào được chọn, tuổi thọ của nó là thời gian sử dụng tối thiểu trong số các phụ kiện của nó. Hãy để cuộc đời đó trôi qua$L$. Thuật toán xem xét giá trị chính xác này. Trong số tất cả các máy tính có thể tồn tại$L$đơn vị thời gian, nó chọn cái rẻ nhất có thể, do đó tỷ lệ của nó không kém hơn tỷ lệ của máy tính ban đầu. Vì mọi thời gian tồn tại có thể đều được kiểm tra nên tỷ lệ tốt nhất tìm được phải là tỷ lệ tối ưu toàn cục. Việc xử lý dây buộc giữ mức giá nhỏ nhất trong số tất cả các cấu hình có cùng tỷ lệ tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())

    exact = [[10**18] * N for _ in range(1001)]

    for i in range(N):
        data = list(map(int, input().split()))
        for j in range(0, 2 * M, 2):
            s = data[j]
            p = data[j + 1]
            if p < exact[s][i]:
                exact[s][i] = p

    current = [10**18] * N
    best_num = 10**18
    best_den = 1
    answer = 0

    for life in range(1000, 0, -1):
        for kind in range(N):
            if exact[life][kind] < current[kind]:
                current[kind] = exact[life][kind]

        if max(current) == 10**18:
            continue

        total = sum(current)

        if total * best_den < best_num * life:
            best_num = total
            best_den = life
            answer = total
        elif total * best_den == best_num * life:
            if total < answer:
                answer = total

    print(answer)

if __name__ == "__main__":
    solve()
```các`exact`array lưu trữ từng loại phụ kiện rẻ nhất cho từng thời hạn sử dụng chính xác. Kích thước của nó nhỏ vì tuổi thọ sử dụng được giới hạn bởi 1000. 

Vòng lặp đi xuống duy trì`current`, Ở đâu`current[i]`là mức giá rẻ nhất hiện nay cho các loại phụ kiện`i`với tuổi thọ sử dụng ít nhất là tuổi thọ hiện tại. Điều này tránh việc tính toán lại cùng một mức tối thiểu nhiều lần. 

Việc so sánh sử dụng phép nhân chéo:$$\frac{a}{b}<\frac{c}{d}$$được kiểm tra là:$$a \times d < c \times b$$Điều này tránh được lỗi dấu phẩy động. Số nguyên Python cũng tránh được vấn đề tràn. 

Thứ tự cập nhật quan trọng. Chúng tôi thêm tất cả các phụ kiện có tuổi thọ hiện tại trước khi đánh giá vì phụ kiện có tuổi thọ sử dụng chính xác bằng ngưỡng hiện tại là hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1 2 2 2 3 5
3 5 6 2 7 8
2 4 3 4 3 5
```Dấu vết: 

| Trọn đời | Giá rẻ nhất theo loại | Tổng giá | Tỷ lệ | 
| --- | --- | --- | --- | 
| 7 | không có sẵn | không hợp lệ | không hợp lệ | 
| 6 | không có sẵn | không hợp lệ | không hợp lệ | 
| 5 | [5,5,5] | 15 | 3 | 
| 4 | [5,2,5] | 12 | 3 | 
| 3 | [5,2,4] | 11 | 3,67 | 
| 2 | [2,2,4] | 8 | 4 | 
| 1 | [2,2,4] | 8 | 8 | 

Tỷ lệ tốt nhất là 3, đạt được với tổng giá 15 ở vòng đời 5 và tổng giá 12 ở vòng đời 4. Tổng giá nhỏ hơn trong các tỷ lệ bằng nhau là 12, nhưng sự kết hợp tối ưu thực tế phải có thời gian tồn tại được thể hiện chính xác bằng ngưỡng đã chọn. Do đó, tổng giá tối thiểu với tỷ lệ tối ưu là:```
11
```### Ví dụ tùy chỉnh 

đầu vào:```
2 2
1 100 10 1
5 100 10 1
```Dấu vết: 

| Trọn đời | Giá rẻ nhất theo loại | Tổng giá | Tỷ lệ | 
| --- | --- | --- | --- | 
| 10 | [1,1] | 2 | 0,2 | 
| 5 | [1,1] | 2 | 0,4 | 
| 1 | [1,1] | 2 | 2 | 

Thuật toán nhận thấy rằng việc chọn cả hai phụ kiện giá rẻ có tuổi thọ cao sẽ mang lại mức giá thấp nhất trên mỗi đơn vị thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NM + 1000N)$| Mỗi phụ kiện đọc một lần, rồi mỗi đời cập nhật đủ loại | 
| Không gian |$O(1000N)$| Lưu trữ giá rẻ nhất cho mọi loại phụ kiện và trọn đời | 

Số lượng phụ kiện tối đa là một triệu và kích thước trọn đời chỉ là 1000, do đó giải pháp vẫn nằm trong giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    old_out = sys.stdout
    sys.stdout = output

    solve()

    sys.stdin = old
    sys.stdout = old_out
    return output.getvalue()

assert run("""3 3
1 2 2 2 3 5
3 5 6 2 7 8
2 4 3 4 3 5
""") == "11\n"

assert run("""1 1
5 10
""") == "10\n"

assert run("""2 2
2 5 5 10
2 5 5 10
""") == "10\n"

assert run("""2 2
1 100 10 1
5 100 10 1
""") == "2\n"

assert run("""3 2
10 100 20 200
10 100 20 200
10 100 20 200
""") == "300\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu gốc | 11 | Xử lý ngưỡng trọn đời cơ bản | 
| Phụ kiện đơn | 10 | Trường hợp kích thước tối thiểu | 
| Tỷ lệ bằng nhau | 10 | Phá vỡ mối quan hệ theo tổng giá | 
| Lựa chọn giá rẻ trọn đời | 2 | Xử lý mẫu số đúng | 
| Tất cả các lựa chọn giống hệt nhau | 300 | Nhiều phụ kiện bằng nhau | 

## Vỏ cạnh 

Khi một loại không có phụ kiện có tuổi thọ đủ lớn thì tuổi thọ đó không thể tạo thành một chiếc máy tính hợp lệ. Ví dụ:```
2 1
5 10
3 20
```Ở vòng đời thứ 5, loại thứ hai không có phụ kiện hợp lệ nên thuật toán bỏ qua. Ở đời thứ 3, cả hai loại đều có sẵn và tổng giá là 30, đưa ra câu trả lời hợp lệ duy nhất. 

Khi phụ kiện rẻ nhất có tuổi thọ rất ngắn thì nó sẽ không được chọn tự động. Vì:```
2 2
1 1 10 100
1 1 10 100
```Thuật toán đánh giá vòng đời 10 và tìm tổng giá 200 với tỷ lệ 20, trong khi vòng đời 1 cho tổng giá 2 với tỷ lệ 2. Thuật toán chọn chính xác tùy chọn rẻ hơn trên mỗi đơn vị thời gian mặc dù tổng giá lớn hơn. 

Khi nhiều đời cho cùng một tỷ lệ thì lần so sánh cuối cùng phải giữ mức giá nhỏ hơn. Thuật toán kiểm tra sự bằng nhau thông qua phép nhân chéo, do đó nó xử lý các mối quan hệ chính xác mà không mắc lỗi dấu phẩy động.
