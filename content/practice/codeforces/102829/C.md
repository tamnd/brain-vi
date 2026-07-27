---
title: "CF 102829C - Kevin's Meme Reacts"
description: "Kevin bắt đầu chuỗi phản ứng meme bằng một phản ứng trên bài đăng của mình. Mỗi đêm số lượng phản ứng hiện tại sẽ tự động tăng gấp đôi. Vào buổi sáng, Kevin có thể thêm một phản ứng theo cách thủ công và phản ứng mới đó sẽ được tính ngay lập tức."
date: "2026-07-26T15:28:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102829
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 11-06-20 Div. 1 (Tryout)"
rating: 0
weight: 102829
solve_time_s: 32
verified: true
draft: false
---

[CF 102829C - Phản ứng về Meme của Kevin](https://codeforces.com/problemset/problem/102829/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Kevin bắt đầu chuỗi phản ứng meme bằng một phản ứng trên bài đăng của mình. Mỗi đêm số lượng phản ứng hiện tại sẽ tự động tăng gấp đôi. Vào buổi sáng, Kevin có thể thêm một phản ứng theo cách thủ công và phản ứng mới đó sẽ được tính ngay lập tức. Mục tiêu là đạt được chính xác`n`tổng số phản ứng đồng thời giảm thiểu số lượng phản ứng thủ công mà Kevin thực hiện, tính phản ứng đầu tiên bắt đầu chuỗi. Vấn đề yêu cầu số lượng bổ sung thủ công tối thiểu đó. 

Cách quan trọng để xem quá trình là xem xét giá trị đóng góp của mỗi phản ứng thủ công. Nếu Kevin thêm một phản ứng và nó tồn tại qua`k`đêm nhân đôi, nó đóng góp chính xác`2^k`phản ứng với tổng số cuối cùng. Phản ứng ban đầu chỉ đơn giản là một đóng góp như vậy với`k = 0`. 

Đầu vào chứa một số nguyên duy nhất`n`, đại diện cho số lượng phản ứng cuối cùng mong muốn. Đầu ra là số lượng phản ứng riêng lẻ tối thiểu mà Kevin phải thêm theo cách thủ công để có được tổng số chính xác đó. 

Ràng buộc`n <= 10^9`có nghĩa là chúng ta không thể mô phỏng mọi chuỗi sáng và đêm có thể xảy ra. Việc tìm kiếm tất cả các cách để đặt phản ứng sẽ tăng theo cấp số nhân vì mọi phản ứng thủ công có thể xảy ra vào một thời điểm khác nhau. Chúng ta cần một giải pháp chỉ thực hiện một số lượng nhỏ các phép toán, lý tưởng nhất là logarit theo`n`, bởi vì`10^9`chỉ có khoảng 30 chữ số nhị phân. 

Các trường hợp đặc biệt chính xuất phát từ các giá trị có vẻ như yêu cầu nhiều bước nhưng thực tế thì không. Ví dụ: khi đầu vào là:```
8
```đầu ra đúng là:```
1
```Một sự mô phỏng bất cẩn có thể tạo thêm các phản ứng bổ sung, nhưng một phản ứng ban đầu sẽ nhân đôi ba lần để trở thành`8`. 

Một trường hợp khó khăn khác là khi câu trả lời không phải là số ngày cần thiết. Ví dụ:```
5
```Đầu ra đúng là:```
2
```Dây chuyền có thể đạt tới`4`sử dụng một phản ứng và sau đó Kevin thêm một phản ứng nữa để có được`5`. Đếm số lần nhân đôi sẽ đưa ra số lượng sai vì bài toán yêu cầu phản ứng thủ công chứ không phải số ngày trôi qua. 

Trường hợp ranh giới cuối cùng là:```
1
```Đầu ra đúng là:```
1
```Phản ứng đầu tiên là bắt buộc vì đây là điểm khởi đầu của quá trình. Việc triển khai coi phản ứng đầu tiên là miễn phí sẽ trả về 0 không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử các lịch trình phản ứng thủ công khác nhau. Chúng ta có thể tưởng tượng việc quyết định mỗi sáng Kevin sẽ phản ứng hay chờ đợi, sau đó mô phỏng việc nhân đôi hàng đêm cho đến khi đạt được mục tiêu. Điều này đúng với các giá trị nhỏ vì cuối cùng mọi lịch trình có thể đều có thể được kiểm tra, nhưng số lượng lịch trình tăng quá nhanh. Đối với mục tiêu gần`10^9`, việc khám phá tất cả các lựa chọn sẽ đòi hỏi nhiều hơn thời gian sẵn có. 

Quan sát hữu ích đến từ việc bỏ qua dòng thời gian và chỉ tập trung vào sự đóng góp của từng phản ứng thủ công. Mỗi phản ứng thủ công sẽ trở thành lũy thừa của hai trong lần đếm cuối cùng. Nếu Kevin phản ứng ngay từ đầu thì phản ứng đó góp phần`1`,`2`,`4`,`8`, vân vân tùy thuộc vào số đêm trôi qua. Phản ứng sau đóng góp một lũy thừa nhỏ hơn bằng hai. 

Điều này có nghĩa là số cuối cùng`n`phải được xây dựng như là tổng sức mạnh của hai. Số lũy thừa tối thiểu của hai cần thiết để tạo thành một số chính xác là số`1`các bit trong biểu diễn nhị phân của nó. Mỗi bit được đặt đại diện cho một lũy thừa của hai phải được đóng góp bởi một phản ứng thủ công riêng biệt. 

Ví dụ,`13`ở dạng nhị phân là`1101`, có nghĩa là:`13 = 8 + 4 + 1`Ba lũy thừa của hai tương ứng với ba phản ứng thủ công. Không có sự sắp xếp nào có thể sử dụng ít hơn vì mỗi phản ứng thủ công chỉ đóng góp một thành phần lũy thừa hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số thời điểm phản ứng có thể xảy ra | O(1) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng phản ứng mục tiêu`n`. 
2. Đếm xem có bao nhiêu bit bằng nhau`1`trong biểu diễn nhị phân của`n`. Mỗi bit như vậy đại diện cho một lũy thừa của hai phải đến từ một phản ứng thủ công. 
3. Kết quả này được tính là số phản ứng tối thiểu Kevin phải thực hiện. 

Tại sao nó hoạt động: mỗi phản ứng Kevin thêm theo cách thủ công sẽ đóng góp một lũy thừa hai vào tổng số cuối cùng vì hoạt động duy nhất trong tương lai là nhân đôi. Biểu diễn nhị phân của`n`là sự phân rã độc đáo của nó thành lũy thừa của hai. Vì mỗi bit thiết lập yêu cầu một đóng góp và quá trình phân tách không thể rút ngắn, nên việc đếm các bit thiết lập sẽ đưa ra số lượng phản ứng thủ công tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    print(n.bit_count())

if __name__ == "__main__":
    solve()
```Giải pháp đọc số mục tiêu và sử dụng tính năng tích hợp sẵn của Python`bit_count()`hoạt động đếm số bit thiết lập trong biểu diễn nhị phân của nó. Điều này phù hợp trực tiếp với quan sát toán học từ thuật toán. 

Không có vòng lặp mô phỏng nên không có phép tính toán thời gian hoặc chuyển tiếp ranh giới nào bị sai. Chi tiết quan trọng duy nhất đó là`n = 1`phải sản xuất`1`, được xử lý một cách tự nhiên vì biểu diễn nhị phân chứa một bit được đặt. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
Input:
5
```Biểu diễn nhị phân là`101`. 

| n | Dạng nhị phân | Đặt số bit được tính | Trả lời | 
| --- | --- | --- | --- | 
| 5 | 101 | 2 | 2 | 

Hai bit tập hợp đại diện cho sự đóng góp`4`Và`1`. Kevin có thể để phản ứng đầu tiên tăng gấp đôi hai lần để đạt được`4`, sau đó thêm một phản ứng theo cách thủ công. 

Đối với ví dụ thứ hai:```
Input:
8
```Biểu diễn nhị phân là`1000`. 

| n | Dạng nhị phân | Đặt số bit được tính | Trả lời | 
| --- | --- | --- | --- | 
| 8 | 1000 | 1 | 1 | 

Chỉ có một lũy thừa của hai trong sự phân rã. Kevin chỉ cần phản ứng ban đầu và có thể đợi ba lần nhân đôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Số lượng bit được xử lý tỷ lệ thuận với độ dài nhị phân của`n`. | 
| Không gian | O(1) | Chỉ có giá trị đầu vào và câu trả lời được lưu trữ. | 

Giá trị lớn nhất có thể chỉ có khoảng 30 chữ số nhị phân nên thuật toán dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
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
assert run("5\n") == "2\n", "sample 1"
assert run("8\n") == "1\n", "sample 2"

# custom cases
assert run("1\n") == "1\n", "minimum value"
assert run("15\n") == "4\n", "all bits set"
assert run("1024\n") == "1\n", "single high bit"
assert run("1000000000\n") == "13\n", "large boundary value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Phản ứng ban đầu được tính. | 
|`15`|`4`| Các số có nhiều bit được đặt yêu cầu nhiều phản ứng. | 
|`1024`|`1`| Sức mạnh thuần túy của cả hai chỉ cần một phản ứng. | 
|`1000000000`|`13`| Các giá trị lớn được xử lý mà không cần mô phỏng. | 

## Vỏ cạnh 

cho`n = 8`, thuật toán chuyển số sang dạng nhị phân`1000`và đếm một bit được đặt. Kết quả là`1`, phù hợp với thực tế là chỉ riêng phản ứng ban đầu cũng có thể phát triển thông qua việc nhân đôi để đạt được mục tiêu. 

Vì`n = 5`, dạng nhị phân là`101`, do đó có hai bit được đặt. Thuật toán xác định những đóng góp cần thiết như`4`Và`1`, đưa ra câu trả lời đúng`2`. Phương pháp chỉ dựa trên việc đếm các bước nhân đôi sẽ thất bại ở đây vì việc điều chỉnh cuối cùng yêu cầu phản ứng thủ công bổ sung. 

Vì`n = 1`, dạng nhị phân là`1`, vậy câu trả lời là`1`. Thuật toán giữ nguyên số lượng phản ứng ban đầu, tránh sai lầm phổ biến là coi phản ứng ban đầu là tự do.
