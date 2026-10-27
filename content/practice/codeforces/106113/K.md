---
title: "CF 106113K - Maraton de Peliculas"
description: "Sự cố mô tả một bộ sưu tập phim. Mỗi bộ phim đều có năm phát hành, tên đạo diễn và xếp hạng. Chúng ta cần xây dựng một cuộc chạy marathon phim dài nhất có thể bằng cách chọn một số phim và sắp xếp chúng theo thứ tự thời gian. Một cuộc chạy marathon hợp lệ có ba hạn chế."
date: "2026-06-25T11:39:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106113
codeforces_index: "K"
codeforces_contest_name: "Coding Cup TecNM 2025"
rating: 0
weight: 106113
solve_time_s: 30
verified: true
draft: false
---

[CF 106113K - Maraton de Peliculas](https://codeforces.com/problemset/problem/106113/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Sự cố mô tả một bộ sưu tập phim. Mỗi bộ phim đều có năm phát hành, tên đạo diễn và xếp hạng. Chúng ta cần xây dựng một cuộc chạy marathon phim dài nhất có thể bằng cách chọn một số phim và sắp xếp chúng theo thứ tự thời gian. 

Một cuộc chạy marathon hợp lệ có ba hạn chế. Số năm phát hành không được giảm, rating phải cải thiện nghiêm ngặt từ phim này sang phim khác, hai phim liên tiếp không được có cùng một đạo diễn. Nhiệm vụ là tìm số lượng phim tối đa có thể xuất hiện theo trình tự như vậy. 

Đầu vào cung cấp số lượng phim theo sau là dữ liệu của mỗi phim. Đầu ra là một số nguyên biểu thị độ dài marathon hợp lệ lớn nhất. 

Số lượng phim nhiều nhất là 3000. Điều này ngay lập tức loại trừ các thuật toán thử mọi tập hợp con, vì số lượng tập hợp con có thể tăng theo cấp số nhân. Một giải pháp quy hoạch động với số lần chuyển đổi bậc hai là thực tế bởi vì$3000^2 = 9 \times 10^6$, đủ nhỏ cho một ngôn ngữ được biên dịch và vẫn được chấp nhận trong Python với các thao tác đơn giản. 

Phần khó khăn không phải là kích thước của đầu vào mà là việc xử lý chính xác các quy tắc đặt hàng. Một sai lầm phổ biến là chỉ sắp xếp theo năm và cho rằng mọi bộ phim trước đó đều có thể là phần trước. Điều kiện xếp hạng rất nghiêm ngặt nên một bộ phim có xếp hạng bằng hoặc thấp hơn không thể kéo dài một cuộc chạy marathon. 

Hãy xem xét những trường hợp sau:```
3
2000 A 50
2001 A 60
2002 B 70
```Đầu ra đúng là:```
2
```Một giải pháp bất cẩn có thể chọn cả ba vì số năm tăng lên, nhưng hai bộ phim đầu tiên có cùng một đạo diễn và xuất hiện liên tiếp là điều bị cấm. 

Một ví dụ khác:```
3
2000 A 50
2001 B 50
2002 C 60
```Đầu ra đúng là:```
2
```Hai phim đầu tiên không thể liên tiếp nhau vì rating của chúng bằng nhau. Giải pháp sử dụng xếp hạng không giảm thay vì tăng xếp hạng nghiêm ngặt sẽ trả về sai 3. 

Trường hợp ranh giới cuối cùng là khi không có phim nào có thể theo sau phim khác:```
2
2000 A 90
2001 B 80
```Đầu ra đúng là:```
1
```Mỗi bộ phim đều đã là một cuộc chạy đua marathon có độ dài một, vì vậy câu trả lời không bao giờ có thể trở thành con số 0. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi thứ tự phim có thể có và kiểm tra xem nó có thỏa mãn các điều kiện hay không. Điều này đúng vì mọi cuộc chạy marathon có thể xảy ra cuối cùng đều sẽ được xem xét. Tuy nhiên, chỉ riêng số tập con có thể có là$2^N$, và với$N=3000$điều này là không thể. 

Một ý tưởng mạnh mẽ hơn là sắp xếp phim theo năm và sử dụng chuyển đổi lập trình động. Ngay cả khi đó, việc tìm kiếm đệ quy ngây thơ trên tất cả các lựa chọn vẫn khám phá quá nhiều nhánh vì nhiều lựa chọn khác nhau dẫn đến cùng một trạng thái còn lại. 

Nhận xét quan trọng là sau khi sắp xếp phim theo thời gian, thông tin duy nhất cần thiết về một phần marathon là bộ phim cuối cùng được chọn. Nếu chúng ta biết bộ phim cuối cùng, chúng ta biết năm hiện tại, xếp hạng và đạo diễn, đó chính xác là những giá trị cần thiết để quyết định xem có thể thêm một bộ phim khác hay không. 

Cho phép`dp[i]`đại diện cho cuộc chạy marathon hợp lệ dài nhất kết thúc bằng phim`i`. Đối với mỗi bộ phim trước đó`j`, chúng tôi kiểm tra xem`j`có thể ngay trước đó`i`. Nếu năm của`j`không lớn hơn năm`i`, đánh giá của`j`nhỏ hơn đánh giá của`i`, và các giám đốc khác nhau, thì`dp[i]`có thể được cập nhật từ`dp[j]`. 

Bước sắp xếp là bước làm cho quá trình chuyển đổi trở nên đơn giản. Chúng tôi sắp xếp theo năm và sau đó xếp hạng. Sau khi sắp xếp, mọi người tiền nhiệm có thể xuất hiện trước người kế nhiệm của nó trong danh sách, bởi vì người kế nhiệm phải có năm ít nhất lớn bằng và xếp hạng lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^N)$|$O(N)$| Quá chậm | 
| Lập trình động sau khi sắp xếp |$O(N^2)$|$O(N)$| Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đọc tất cả các phim và lưu trữ từng phim dưới dạng một bộ chứa năm, đạo diễn và xếp hạng. Sắp xếp phim theo năm đầu tiên và xếp hạng thứ hai. 

Thứ tự được sắp xếp sẽ tạo ra dòng thời gian trong đó phim hợp lệ trước đó chỉ có thể xuất hiện trước phim hiện tại. Thứ tự xếp hạng trong cùng một năm cũng ngăn cản việc chuyển đổi không thể thực hiện được do xếp hạng bằng hoặc nhỏ hơn. 

1. Tạo một mảng`dp`Ở đâu`dp[i]`bắt đầu bằng số 1 vì mỗi bộ phim đều có thể tự tạo thành một cuộc chạy marathon. 

Việc khởi tạo này xử lý các trường hợp không thể đặt phim nào khác trước phim. 

1. Đối với mỗi bộ phim`i`, kiểm tra mọi bộ phim trước đó`j`. 

Bộ phim`j`có thể kéo dài một cuộc chạy marathon kết thúc ở`j`nếu năm của nó không sau`i`, đánh giá của nó nhỏ hơn`i`đánh giá của và các giám đốc là khác nhau. 

1. Khi có một chuyển đổi hợp lệ, hãy cập nhật:$$dp[i] = \max(dp[i], dp[j] + 1)$$Bộ phim mới được thêm vào sau cuộc đua marathon hay nhất đã kết thúc lúc`j`. 

1. Đáp án là giá trị lớn nhất trong`dp`. 

Cuộc đua marathon dài nhất có thể kết thúc ở bất kỳ bộ phim nào, vì vậy chúng ta không thể chỉ kiểm tra phần tử cuối cùng sau khi sắp xếp. 

## Tại sao nó hoạt động 

Bất biến của mảng lập trình động là`dp[i]`luôn lưu trữ cuộc chạy marathon hay nhất có thể mà bộ phim cuối cùng chính xác là phim`i`. 

Khi xem xét phim`i`, mọi phim trước đó có thể xuất hiện trong số các chỉ số trước đó. Nếu một bộ phim trước đó có thể kết nối hợp pháp với`i`, quá trình chuyển đổi sẽ xem xét kết thúc cuộc đua marathon tối ưu ở đó và kéo dài nó thêm một. Vì mọi tiền đề cuối cùng có thể đều được kiểm tra nên không có cuộc chạy marathon hợp lệ nào kết thúc vào lúc`i`bị bỏ lỡ. Lấy mức tối đa trên tất cả các kết thúc sẽ mang lại mức tối ưu toàn cầu. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    movies = []

    for _ in range(n):
        y, director, p = input().split()
        movies.append((int(y), int(p), director))

    movies.sort()

    dp = [1] * n
    ans = 1

    for i in range(n):
        year_i, score_i, director_i = movies[i]

        for j in range(i):
            year_j, score_j, director_j = movies[j]

            if year_j <= year_i and score_j < score_i and director_j != director_i:
                dp[i] = max(dp[i], dp[j] + 1)

        ans = max(ans, dp[i])

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu vào được chuyển đổi thành các bộ dữ liệu chứa các giá trị số trong năm và điểm số để việc so sánh được thực hiện trực tiếp. Giám đốc vẫn là một chuỗi vì chỉ cần kiểm tra sự bình đẳng. 

Sắp xếp công dụng`(year, score, director)`đặt hàng. Đạo diễn chỉ được đưa vào với vai trò là người quyết định cuối cùng và không ảnh hưởng đến tính chính xác vì các quá trình chuyển đổi vẫn kiểm tra rõ ràng cả ba điều kiện chạy marathon. 

Các vòng lặp lồng nhau thực hiện chuyển đổi lập trình động. điều kiện`score_j < score_i`là nghiêm ngặt vì xếp hạng phải được cải thiện. Điều kiện về đạo diễn ngăn cản các bộ phim liên tiếp của cùng một đạo diễn. 

Không cần xử lý số nguyên đặc biệt vì câu trả lời nhiều nhất là 3000. 

# Ví dụ đã hoạt động 

Đối với mẫu:```
6
2001 Nolan10 60
2004 Miyazaki 65
2002 Coppola 55
1999 Tarantino 50
2003 Cuaron 70
2005 Jeunet 80
```Sau khi sắp xếp theo năm: 

| Phim | Năm | Điểm | Giám đốc | dp | 
| --- | --- | --- | --- | --- | 
| Tarantino | 1999 | 50 | Tarantino | 1 | 
| Nolan10 | 2001 | 60 | Nolan10 | 2 | 
| Coppola | 2002 | 55 | Coppola | 1 | 
| Cuaron | 2003 | 70 | Cuaron | 3 | 
| Miyazaki | 2004 | 65 | Miyazaki | 3 | 
| Jeunet | 2005 | 80 | Jeunet | 4 | 

Chuỗi tốt nhất là:```
Tarantino -> Nolan10 -> Cuaron -> Jeunet
```với chiều dài 4. 

Một ví dụ khác:```
3
2000 A 50
2001 A 60
2002 B 70
```| Phim | Năm | Điểm | Giám đốc | dp | 
| --- | --- | --- | --- | --- | 
| A | 2000 | 50 | A | 1 | 
| A | 2001 | 60 | A | 2 | 
| B | 2002 | 70 | B | 3 | 

Bảng trên cho thấy sự chuyển đổi nếu chúng ta bỏ qua hạn chế của đạo diễn. Quá trình chuyển đổi thực tế từ phim A đầu tiên sang phim A thứ hai không hợp lệ, vì vậy các giá trị thực là: 

| Phim | Phim trước hợp lệ | dp | 
| --- | --- | --- | 
| Một 2000 50 | không | 1 | 
| A 2001 60 | không | 1 | 
| B 2002 70 | hoặc Một bộ phim | 2 | 

Câu trả lời là 2, chứng minh tại sao điều kiện đạo diễn phải được kiểm tra trong quá trình chuyển đổi. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| Mỗi phim sẽ kiểm tra tất cả các phim trước đó sau khi sắp xếp | 
| Không gian |$O(N)$| Lưu trữ phim và các giá trị lập trình động | 

Với$N \leq 3000$, số lần chuyển đổi bậc hai là khoảng chín triệu lần kiểm tra, vừa vặn thoải mái trong giới hạn dự định. 

# Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    movies = []

    for _ in range(n):
        y, director, p = input().split()
        movies.append((int(y), int(p), director))

    movies.sort()

    dp = [1] * n
    ans = 1

    for i in range(n):
        yi, pi, di = movies[i]
        for j in range(i):
            yj, pj, dj = movies[j]
            if yj <= yi and pj < pi and dj != di:
                dp[i] = max(dp[i], dp[j] + 1)
        ans = max(ans, dp[i])

    return str(ans)

assert solve("""6
2001 Nolan10 60
2004 Miyazaki 65
2002 Coppola 55
1999 Tarantino 50
2003 Cuaron 70
2005 Jeunet 80
""") == "4"

assert solve("""3
2000 A 50
2001 A 60
2002 B 70
""") == "2"

assert solve("""1
2000 A 100
""") == "1"

assert solve("""5
2000 A 10
2000 B 20
2000 C 30
2001 D 40
2002 E 50
""") == "5"

assert solve("""4
2000 A 90
2001 B 80
2002 C 70
2003 D 60
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu gốc | 4 | Chuỗi bình thường qua nhiều năm | 
| Cùng một giám đốc trong các ứng cử viên liên tiếp | 2 | Hạn chế giám đốc | 
| Một bộ phim | 1 | Kích thước đầu vào tối thiểu | 
| Tăng xếp hạng cùng năm | 5 | Đặt hàng cùng năm | 
| Giảm xếp hạng | 1 | Cải thiện xếp hạng nghiêm ngặt | 

# Vỏ cạnh 

Đối với trường hợp cùng giám đốc:```
3
2000 A 50
2001 A 60
2002 B 70
```Thuật toán sắp xếp phim theo thứ tự thời gian. Khi xử lý phim thứ hai, nó thấy rằng phim đầu tiên có mối quan hệ năm và xếp hạng hợp lệ nhưng cùng một đạo diễn nên quá trình chuyển đổi bị từ chối. Bộ phim thứ ba có thể kéo dài thời gian chạy marathon của một bộ phim, đưa ra câu trả lời là 2. 

Để xếp hạng bằng nhau:```
3
2000 A 50
2001 B 50
2002 C 60
```Hai phim đầu không kết nối được vì điểm không tăng. Phim cuối cùng có thể theo sau một trong hai vì 50 nhỏ hơn 60. Thuật toán trả về 2. 

Đối với trường hợp mọi quá trình chuyển đổi đều thất bại:```
2
2000 A 90
2001 B 80
```Phim thứ 2 ra muộn hơn nhưng rating thấp hơn nên không thể kéo dài phim đầu tiên. Cả hai`dp`giá trị vẫn là 1 và câu trả lời là 1.
