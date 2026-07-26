---
title: "CF 102878A - Chênh lệch IQ"
description: "Nhiệm vụ là tìm số duy nhất trong danh sách có tính chẵn lẻ khác với tất cả các số còn lại. Đầu vào mô tả một chuỗi các số nguyên. Mọi giá trị ngoại trừ một giá trị đều là chẵn hoặc lẻ, trong khi giá trị còn lại có tính chẵn lẻ ngược lại."
date: "2026-07-25T12:49:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102878
codeforces_index: "A"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 102878
solve_time_s: 33
verified: true
draft: false
---

[CF 102878A - Chênh lệch IQ](https://codeforces.com/problemset/problem/102878/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là tìm số duy nhất trong danh sách có tính chẵn lẻ khác với tất cả các số còn lại. Đầu vào mô tả một chuỗi các số nguyên. Mọi giá trị ngoại trừ một giá trị đều là chẵn hoặc lẻ, trong khi giá trị còn lại có tính chẵn lẻ ngược lại. Đầu ra là vị trí dựa trên 1 của giá trị khác nhau đó trong chuỗi ban đầu. 

Kích thước của trình tự này nhỏ nhưng bài học dự kiến ​​là về việc nhận biết thông tin nào thực sự quan trọng. Bản thân các giá trị không liên quan ngoại trừ việc chúng có chia hết cho hai hay không. Quét tuyến tính là đủ vì mỗi phần tử chỉ cần được kiểm tra một lần. Ngay cả khi kích thước chuỗi được tăng lên khoảng$10^5$hoặc$10^6$, MỘT$O(n)$giải pháp vẫn sẽ phù hợp thoải mái trong giới hạn lập trình cạnh tranh thông thường, trong khi các phương pháp so sánh từng cặp sẽ trở nên bất khả thi vì chúng yêu cầu$O(n^2)$hoạt động. 

Một sai lầm phổ biến là cho rằng giá trị đầu tiên xác định tính chẵn lẻ của đa số. Ví dụ: nếu đầu vào là:```
3
1 2 4
```đầu ra đúng là:```
1
```Số đầu tiên là số lẻ và hai số còn lại là số chẵn. Phương thức khởi tạo câu trả lời chỉ dựa trên phần tử đầu tiên có thể thất bại nếu nó không đếm hoặc xác minh đúng đa số. 

Một trường hợp đặc biệt khác là khi giá trị bất thường xuất hiện ở cuối:```
5
2 4 6 8 7
```Đầu ra đúng là:```
5
```Quá trình quét dừng quá sớm sau khi tìm thấy tính chẵn lẻ phù hợp giữa một số giá trị đầu tiên sẽ bỏ lỡ câu trả lời. 

Trường hợp thứ ba là khi hai số đầu tiên có số chẵn lẻ khác nhau:```
3
1 2 3
```Đầu ra đúng là:```
2
```Ở đây, giá trị chẵn lẻ của đa số là số lẻ và giá trị ở giữa là giá trị chẵn duy nhất. Bất kỳ cách tiếp cận nào giả định hai yếu tố đầu tiên mang tính đại diện đều cần được kiểm tra thêm. 

## Phương pháp tiếp cận 

Cách mạnh mẽ là so sánh mọi số với mọi số khác và đếm xem có bao nhiêu giá trị chia sẻ tính chẵn lẻ của nó. Vì có$n$lựa chọn cho số lượng ứng viên và lên đến$n$so sánh cho từng ứng viên, tổng công việc là$O(n^2)$. Điều này đúng vì phần tử duy nhất sẽ là phần tử duy nhất có số chẵn lẻ khác với phần tử còn lại, nhưng việc so sánh lặp đi lặp lại là không cần thiết. 

Nhận xét làm cho vấn đề trở nên đơn giản là tính chẵn lẻ chỉ có hai trạng thái có thể xảy ra. Chúng ta không cần phải so sánh các yếu tố với nhau. Chúng ta chỉ cần biết tồn tại bao nhiêu số lẻ và bao nhiêu số chẵn. Vì có chính xác một phần tử khác nhau nên một trong các số đếm này sẽ là một và phần tử còn lại sẽ là$n-1$. Sau khi xác định số chẵn lẻ thiểu số, một lần quét sẽ đưa ra vị trí của phần tử có số chẵn lẻ đó. 

Giải pháp brute-force hoạt động vì nó trực tiếp kiểm tra thuộc tính mà chúng ta cần, nhưng nó lặp lại cùng một bước kiểm tra chẵn lẻ nhiều lần. Việc đếm hai nhóm chẵn lẻ có thể sẽ nén tất cả thông tin đó vào hai bộ đếm, giảm vấn đề xuống mức tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm đối với đầu vào lớn | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem dãy số đó có bao nhiêu số chẵn và bao nhiêu số lẻ. Số lượng đa số cho chúng ta biết các số bình thường có tính chẵn lẻ nào, trong khi số lượng nhỏ hơn xác định tính chẵn lẻ bất thường. 
2. Lưu trữ chỉ số của số chẵn cuối cùng và số lẻ cuối cùng trong khi quét. Giữ các vị trí này tránh cần phải vượt qua sau này. 
3. So sánh số chẵn và số lẻ. Nếu có ít số chẵn hơn thì câu trả lời là vị trí chẵn được lưu trữ. Ngược lại, câu trả lời là vị trí lẻ được lưu trữ. 
4. In vị trí đã chọn bằng cách sử dụng chỉ mục dựa trên 1 vì bài toán đánh số vị trí bắt đầu từ một. 

Tại sao nó hoạt động: đầu vào đảm bảo rằng chính xác một số có số chẵn lẻ khác. Do đó, giá trị chẵn lẻ xuất hiện một lần phải là giá trị chẵn lẻ của câu trả lời và giá trị chẵn lẻ còn lại xuất hiện cho mọi phần tử còn lại. Thuật toán xác định tính chẵn lẻ thiểu số này bằng cách đếm, sau đó trả về chỉ số duy nhất thuộc nhóm đó. Vì mọi phần tử được tính chính xác một lần và mọi tính chẵn lẻ có thể được xem xét nên không có chỉ mục nào khác có thể thỏa mãn điều kiện. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    even_count = 0
    odd_count = 0
    even_pos = -1
    odd_pos = -1

    for i, x in enumerate(a, start=1):
        if x % 2 == 0:
            even_count += 1
            even_pos = i
        else:
            odd_count += 1
            odd_pos = i

    if even_count == 1:
        print(even_pos)
    else:
        print(odd_pos)

if __name__ == "__main__":
    solve()
```Quá trình quét chỉ lưu trữ bốn thông tin: số giá trị chẵn, số giá trị lẻ và vị trí gần đây nhất của mỗi loại. Các giá trị chính xác không bao giờ cần thiết sau khi tính chẵn lẻ của chúng được xác định. 

điều kiện`even_count == 1`chọn nhóm bất thường khi giá trị khác biệt duy nhất là số chẵn. Ngược lại, giá trị khác nhau duy nhất phải là số lẻ vì bài toán đảm bảo chính xác một ngoại lệ. Các vị trí được lưu trữ đã dựa trên 1 vì việc liệt kê bắt đầu từ một, điều này tránh được việc điều chỉnh chỉ mục ở cuối. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
5
2 4 7 8 10
```Quá trình quét hoạt động như sau. 

| Chỉ mục | Giá trị | Đếm chẵn | Số Lẻ | Vị Trí Chẵn | Vị Trí Lẻ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 0 | 1 | -1 | 
| 2 | 4 | 2 | 0 | 2 | -1 | 
| 3 | 7 | 2 | 1 | 2 | 3 | 
| 4 | 8 | 3 | 1 | 4 | 3 | 
| 5 | 10 | 4 | 1 | 5 | 3 | 

Chỉ có một số lẻ nên thuật toán đưa ra vị trí 3. Điều này chứng tỏ rằng bản thân các giá trị không quan trọng, chỉ có các nhóm chẵn lẻ của chúng là không quan trọng. 

Đối với ví dụ thứ hai:```
4
1 2 1 1
```Quá trình quét hoạt động như sau. 

| Chỉ mục | Giá trị | Đếm chẵn | Số Lẻ | Vị Trí Chẵn | Vị Trí Lẻ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | -1 | 1 | 
| 2 | 2 | 1 | 1 | 2 | 1 | 
| 3 | 1 | 1 | 2 | 2 | 3 | 
| 4 | 1 | 1 | 3 | 2 | 4 | 

Chỉ có một số chẵn nên thuật toán đưa ra vị trí 2. Điều này xác nhận rằng logic tương tự sẽ xử lý tình huống ngược lại trong đó các số lẻ chiếm đa số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được kiểm tra một lần để tính tính chẵn lẻ của nó. | 
| Không gian | O(1) | Chỉ có bộ đếm và hai vị trí được lưu trữ. | 

Thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi phần tử, do đó, nó chia tỷ lệ tuyến tính với kích thước đầu vào. Việc sử dụng bộ nhớ không phụ thuộc vào độ dài chuỗi sau khi đầu vào được đọc. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    even_count = 0
    odd_count = 0
    even_pos = -1
    odd_pos = -1

    for i, x in enumerate(a, start=1):
        if x % 2 == 0:
            even_count += 1
            even_pos = i
        else:
            odd_count += 1
            odd_pos = i

    ans = even_pos if even_count == 1 else odd_pos

    sys.stdin = old_stdin
    return str(ans) + "\n"

assert solution("5\n2 4 7 8 10\n") == "3\n", "sample 1"
assert solution("4\n1 2 1 1\n") == "2\n", "sample 2"

assert solution("3\n2 4 1\n") == "3\n", "single odd value"
assert solution("3\n7 8 10\n") == "1\n", "single odd value at start"
assert solution("7\n6 2 4 8 12 14 3\n") == "7\n", "single odd value at end"
assert solution("5\n1 3 5 8 7\n") == "4\n", "single even value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 2 4 1`|`3`| Sự chẵn lẻ khác nhau xuất hiện ở cuối. | 
|`3 / 7 8 10`|`1`| Sự chẵn lẻ khác nhau xuất hiện ngay từ đầu. | 
|`7 / 6 2 4 8 12 14 3`|`7`| Một nhóm đa số lớn với một ngoại lệ muộn. | 
|`5 / 1 3 5 8 7`|`4`| Trường hợp giá trị chẵn duy nhất. | 

## Vỏ cạnh 

Đối với trường hợp số bất thường là phần tử đầu tiên:```
3
7 8 10
```Thuật toán đếm hai số chẵn và một số lẻ. Vì số chẵn không phải là một nên nó chọn vị trí lẻ được lưu trữ là 1. Nó xử lý chính xác thực tế là giá trị đầu tiên không thiết lập đa số. 

Đối với trường hợp số bất thường nằm ở cuối:```
5
2 4 6 8 7
```Quá trình quét ghi lại bốn giá trị chẵn và một giá trị lẻ. Vị trí lẻ được lưu trữ trở thành 5 và thuật toán trả về vị trí đó sau khi quét toàn bộ. Nó không dựa vào những giả định ban đầu về trình tự. 

Đối với trường hợp hai giá trị đầu tiên có tính chẵn lẻ khác nhau:```
3
1 2 3
```Số đếm cuối cùng là hai giá trị lẻ và một giá trị chẵn. Thuật toán chọn vị trí chẵn được lưu trữ là 2. Phần lớn được phát hiện từ tổng số thay vì từ một số phần tử đầu tiên, tránh lỗi phổ biến là đoán quá sớm.
