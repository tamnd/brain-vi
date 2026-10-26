---
title: "CF 106096A - Xây dựng trên boong"
description: "Vấn đề là về việc xây dựng một bộ bài Clash Royale từ các lá bài được ghép nối. Chúng ta có hai mảng, trong đó chỉ mục i mô tả một cặp thẻ có giá trị a[i] và b[i]. Khi xây dựng bộ bài, chúng ta không thể chỉ lấy một lá bài trong một cặp."
date: "2026-06-25T11:59:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106096
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 10-1-25 Div. 2 (Beginner)"
rating: 0
weight: 106096
solve_time_s: 41
verified: true
draft: false
---

[CF 106096A - Xây dựng trên boong](https://codeforces.com/problemset/problem/106096/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề là về việc xây dựng một bộ bài Clash Royale từ các lá bài được ghép nối. Chúng tôi có hai mảng, trong đó chỉ mục`i`mô tả một cặp thẻ có chi phí`a[i]`Và`b[i]`. Khi xây dựng bộ bài, chúng ta không thể chỉ lấy một lá bài trong một cặp. Chọn một chỉ mục có nghĩa là cả hai thẻ từ chỉ mục đó đều được thêm vào. Phải chọn chính xác bốn chỉ số, vì mỗi cặp được chọn đóng góp hai quân bài và bộ bài cuối cùng phải chứa tám quân bài. Mục tiêu là tìm tổng chi phí thuốc tiên tối thiểu có thể có của tám thẻ đã chọn. 

Phần quan trọng của các ràng buộc là số lượng cặp có thể lên tới 100. Con số này đủ nhỏ cho cách tiếp cận bậc hai hoặc bậc ba, nhưng lại quá lớn để liệt kê mọi nhóm bốn chỉ số có thể có. Việc chọn trực tiếp bốn chỉ số có khoảng`n^4`khả năng, sẽ trở thành khoảng 100 triệu kết hợp khi`n = 100`và mỗi sự kết hợp vẫn yêu cầu kiểm tra chi phí của nó. Chúng ta cần sử dụng thực tế rằng bốn là một số nhỏ cố định và giảm không gian tìm kiếm. 

Các trường hợp đặc biệt xuất phát từ việc coi một cặp như hai quân bài riêng biệt hoặc quên rằng mọi chỉ số được chọn đều đóng góp cả hai quân bài. Ví dụ: nếu đầu vào là:```
4
1 2 3 4
4 3 2 1
```Chọn bốn chỉ số đầu tiên là bộ bài khả thi duy nhất và câu trả lời là:```
20
```Một giải pháp bất cẩn sắp xếp tất cả tám thẻ riêng lẻ và cố gắng chọn những thẻ nhỏ nhất sẽ chỉ chọn sai một thẻ từ một số cặp. 

Một trường hợp phức tạp khác là khi bốn chỉ số tốt nhất không phải là tổng của bốn cặp nhỏ nhất. Ví dụ:```
5
1 100 1 100 1
100 1 100 1 1
```Tổng cặp đều là`101, 101, 101, 101, 2`. Bộ bài tối thiểu phải bao gồm chỉ mục có tổng`2`và ba trong số các chỉ số khác, nhưng việc chỉ chọn theo giá trị thẻ riêng lẻ có thể dẫn đến lựa chọn không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi bộ bốn chỉ số có thể có. Đối với mỗi bộ, chúng tôi thêm hai thẻ từ mỗi chỉ số đã chọn và giữ tổng số nhỏ nhất. Điều này đúng vì mỗi bộ bài hợp lệ đều tương ứng với chính xác một nhóm gồm bốn chỉ số. Tuy nhiên, số lượng lựa chọn`C(n,4)`. Với`n = 100`, tức là khoảng 3,9 triệu nhóm. Đây không phải là thảm họa, nhưng việc triển khai trở nên nặng nề không cần thiết và các ràng buộc được thiết kế xung quanh việc nhận thấy một sự chuyển đổi đơn giản hơn. 

Quan sát quan trọng là chỉ số được chọn luôn đóng góp`a[i] + b[i]`đến tổng số. Thứ tự bên trong của hai thẻ không quan trọng vì cả hai đều phải được đưa vào. Vấn đề trở thành việc chọn chính xác bốn giá trị từ mảng tổng cặp. Khi chúng tôi tính toán các khoản tiền này, câu trả lời chỉ đơn giản là tổng của bốn giá trị nhỏ nhất. 

Lực lượng vũ phu hoạt động vì nó kiểm tra mọi bộ bài hợp lệ. Sự cải tiến đến từ việc nhận ra rằng tất cả thông tin về một chỉ số có thể được nén thành một con số, đóng góp của nó vào điểm số cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(1) | Quá chậm/không cần thiết | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số cặp bài và hai dãy chứa giá trị của lá bài. 
2. Tạo một danh sách mới trong đó mỗi phần tử`a[i] + b[i]`. Điều này có hiệu quả vì việc chọn một chỉ số luôn đặt cả hai lá bài từ chỉ mục đó vào bộ bài, vì vậy giá trị duy nhất quan trọng là tổng đóng góp. 
3. Sắp xếp danh sách tổng cặp. Việc sắp xếp cho phép chúng ta xem xét những đóng góp rẻ nhất có thể trước tiên. 
4. Thêm bốn giá trị đầu tiên từ danh sách đã sắp xếp. Bốn chỉ số này tạo ra bộ bài tối thiểu có thể có tám lá bài vì việc thay thế bất kỳ khoản đóng góp lớn hơn đã chọn nào bằng một khoản đóng góp nhỏ hơn chưa được sử dụng chỉ có thể làm giảm tổng số. 
5. In kết quả tổng chi phí tối thiểu. 

Tại sao nó hoạt động: mỗi bộ bài hợp lệ chính xác là sự lựa chọn của bốn chỉ số và mỗi chỉ số được chọn có giá cố định bằng tổng của hai lá bài của nó. Sau khi chuyển bài toán sang chọn 4 số thì lựa chọn tối ưu luôn là 4 số nhỏ nhất. Nếu tập hợp được chọn chứa giá trị lớn hơn trong khi giá trị không được chọn nhỏ hơn, việc hoán đổi chúng sẽ tạo ra một bộ bài hợp lệ rẻ hơn, điều này mâu thuẫn với tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    costs = []
    for i in range(n):
        costs.append(a[i] + b[i])

    costs.sort()

    print(sum(costs[:4]))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã đọc hai mảng. Các mảng được giữ riêng biệt vì định dạng đầu vào cung cấp chúng riêng biệt, nhưng thuật toán ngay lập tức kết hợp chúng thành các giá trị duy nhất quan trọng. 

Vòng lặp xây dựng danh sách đóng góp. Không cần phải lưu trữ các chỉ số đã chọn vì chỉ yêu cầu tổng chi phí tối thiểu chứ không phải bộ bài thực tế. 

Sau khi phân loại, lấy`costs[:4]`cũng giống như việc chọn bốn cặp rẻ nhất có thể. Ranh giới lát cắt được cố định vì bộ bài cuối cùng luôn yêu cầu chính xác bốn chỉ số. 

Số nguyên Python xử lý các khoản tiền có thể một cách an toàn. Vì giá mỗi thẻ tối đa là 1000 và chỉ có 8 thẻ được thêm vào nên việc tràn thẻ không phải là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6
4 3 9 2 9 2
1 3 8 3 1 5
```| Bước | Cặp tiền | Tổng được sắp xếp | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Tạo số tiền | 5, 6, 17, 5, 10, 7 | | 0 | 
| Sắp xếp | | 5, 5, 6, 7, 10, 17 | 0 | 
| Lấy bốn nhỏ nhất | | | 23 | 

Thuật toán chọn các cặp có đóng góp`5, 5, 6, 7`, mang lại tổng cộng`23`. Điều này chứng tỏ rằng cặp này là đơn vị thực sự được lựa chọn. 

### Mẫu 2 

đầu vào:```
4
1 2 4 8
1 2 4 8
```| Bước | Cặp tiền | Tổng được sắp xếp | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Tạo số tiền | 2, 4, 8, 16 | | 0 | 
| Sắp xếp | | 2, 4, 8, 16 | 0 | 
| Lấy bốn nhỏ nhất | | | 30 | 

Tất cả bốn chỉ số phải được sử dụng vì có chính xác bốn cặp có sẵn. Kết quả là tổng của cả 8 lá bài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Chúng tôi tính toán tất cả các tổng cặp và sắp xếp chúng | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách các cặp đóng góp | 

Kích thước đầu vào tối đa chỉ là 100 cặp, do đó việc sắp xếp có thể dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng ở mức tối thiểu vì danh sách được lưu trữ có một giá trị cho mỗi chỉ mục. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    costs = [a[i] + b[i] for i in range(n)]
    costs.sort()

    return str(sum(costs[:4])) + "\n"

assert solve("""6
4 3 9 2 9 2
1 3 8 3 1 5
""") == "23\n", "sample 1"

assert solve("""4
1 2 4 8
1 2 4 8
""") == "30\n", "sample 2"

assert solve("""4
1 2 3 4
4 3 2 1
""") == "20\n", "all pairs must be chosen"

assert solve("""5
1 100 1 100 1
100 1 100 1 1
""") == "305\n", "pair constraint"

assert solve("""100
""" + " ".join(["1000"] * 100) + "\n" + " ".join(["1000"] * 100) + "\n") == "8000\n", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bốn cặp có tổng số bằng nhau | 20 | Xử lý trường hợp phải chọn từng cặp | 
| Thẻ lớn và nhỏ hỗn hợp | 305 | Xác nhận rằng các cặp không thể tách rời | 
| 100 cặp giống hệt nhau | 8000 | Xác nhận kích thước đầu vào tối đa | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên, hãy xem xét:```
4
1 2 3 4
4 3 2 1
```Tổng cặp trở thành`[5, 5, 5, 5]`. Thuật toán sắp xếp chúng và lấy cả bốn giá trị, tạo ra`20`. Nó không cố gắng chọn các thẻ riêng lẻ`1, 2, 2, 3`bởi vì những thẻ đó đến từ các kết hợp không hợp lệ khác nhau. 

Đối với trường hợp cạnh thứ hai:```
5
1 100 1 100 1
100 1 100 1 1
```Tổng các cặp là`[101, 101, 101, 101, 2]`. Sắp xếp mang lại`[2, 101, 101, 101, 101]`. Bộ bài được chọn có giá`305`. Thuật toán thành công vì nó so sánh đóng góp của cả cặp thay vì từng thẻ riêng lẻ.
