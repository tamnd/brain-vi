---
title: "CF 106449A - Giả mạo dữ liệu"
description: "Bài toán đưa ra một tập hợp các số nguyên đại diện cho một tập dữ liệu. Chúng ta cần kiểm tra chữ số có ý nghĩa đầu tiên của mỗi số, nghĩa là chữ số khác 0 ngoài cùng bên trái sau khi bỏ qua dấu."
date: "2026-06-25T09:21:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106449
codeforces_index: "A"
codeforces_contest_name: "2026 Spring UT CS104c Midterm #2"
rating: 0
weight: 106449
solve_time_s: 33
verified: true
draft: false
---

[CF 106449A - Giả mạo dữ liệu](https://codeforces.com/problemset/problem/106449/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một tập hợp các số nguyên đại diện cho một tập dữ liệu. Chúng ta cần kiểm tra chữ số có ý nghĩa đầu tiên của mỗi số, nghĩa là chữ số khác 0 ngoài cùng bên trái sau khi bỏ qua dấu. Nhiệm vụ là quyết định xem chữ số`1`xuất hiện dưới dạng chữ số dẫn đầu này thường xuyên hơn mọi chữ số khác. Nếu đúng như vậy, tập dữ liệu được coi là đáng ngờ; nếu không thì không. 

Ví dụ, số`-4200`đóng góp chữ số hàng đầu`4`, vì dấu trừ không phải là một phần của số và chữ số đầu tiên mang giá trị là`4`. số`0`là trường hợp đặc biệt vì không có chữ số nào khác 0. Vì định nghĩa về chữ số có nghĩa đứng đầu không áp dụng cho số 0 nên giá trị 0 không đóng góp vào bất kỳ số chữ số nào. 

Kích thước đầu vào là lý do chính khiến giải pháp cần phải đơn giản. Với số lượng lên tới hàng triệu con số, chúng ta chỉ có thể đảm đương công việc tỷ lệ thuận với lượng đầu vào. Bất kỳ phương pháp so sánh các số với nhau hoặc quét liên tục các chữ số theo những cách đắt tiền sẽ nhanh chóng trở nên quá chậm. Đọc mỗi số một lần và trích ra chữ số đầu tiên của nó là đủ. 

Các trường hợp phức tạp chủ yếu xoay quanh việc trích xuất chữ số một cách chính xác. Một con số như`-1000`phải tính là dẫn đầu`1`, không`-`hoặc`0`. Một con số như`0`không được vô tình tăng số lượng của một số chữ số. Một số có chữ số đầu tiên không phải là duy nhất trong dữ liệu đầu vào cũng có thể dẫn đến những so sánh không chính xác. 

Hãy xem xét đầu vào:```
3
0 0 1
```Đầu ra đúng là:```
suspicious
```Chỉ có số`1`đóng góp, vì vậy chữ số`1`xuất hiện một lần và tất cả các chữ số khác xuất hiện 0 lần. Việc triển khai bất cẩn coi số 0 là chữ số đứng đầu`0`vẫn sẽ vượt qua ở đây, nhưng nó sẽ thất bại trong trường hợp tồn tại nhiều số không. 

Ví dụ khác:```
4
0 0 0 1
```Đầu ra đúng là:```
suspicious
```Số 0 không nên cạnh tranh với chữ số`1`. Đếm số 0 là chữ số đầu sẽ tạo thành chữ số không chính xác`0`thắng. 

Một giá trị âm cũng cần được chú ý:```
2
-999 100
```Đầu ra đúng là:```
suspicious
```Các chữ số đứng đầu là`9`Và`1`. Dấu hiệu phải được bỏ qua trước khi trích xuất chữ số. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thu thập chữ số đầu của mọi số và liên tục so sánh tần số để xác định xem chữ số đó có`1`là lớn nhất. Điều này đúng vì thông tin cần thiết duy nhất là tần số của mỗi chữ số đầu. Tuy nhiên, nếu việc so sánh được thực hiện bằng cách kiểm tra từng chữ số với từng chữ số khác cho mọi số thì nó có thể trở nên lớn một cách không cần thiết. Với`n`số lượng, phong cách đó có thể đạt tới khoảng`10n`hoạt động, ở đây vẫn chưa khủng khiếp, nhưng nó bỏ lỡ cấu trúc đơn giản hơn của vấn đề. 

Quan sát hữu ích là chỉ có mười chữ số đứng đầu. Chúng ta không cần lưu trữ các con số hoặc thực hiện các phép so sánh phức tạp. Chúng ta chỉ cần một dải tần có kích thước 10. Trong khi đọc từng số, chúng ta tìm chữ số có nghĩa đứng đầu nó và tăng số đếm tương ứng. Sau khi xử lý toàn bộ tập dữ liệu, chúng tôi so sánh số lượng chữ số`1`với số lượng tối đa trong chín chữ số còn lại. 

Phương pháp brute-force có hiệu quả vì câu trả lời chỉ phụ thuộc vào tần số chữ số, nhưng nó tốn nhiều công sức để kiểm tra liên tục những thông tin có thể tóm tắt ngay lập tức. Mảng tần số làm giảm vấn đề xuống còn một lần truyền qua đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10n) | O(1) | Được chấp nhận về mặt lý thuyết, nhưng không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng gồm mười bộ đếm, trong đó chỉ số`d`lưu trữ bao nhiêu số có chữ số đứng đầu`d`. 
2. Đọc từng số và bỏ dấu nếu nó âm. Giá trị tuyệt đối là đủ vì chữ số đứng đầu chỉ phụ thuộc vào độ lớn. 
3. Nếu số bằng 0 thì bỏ qua vì nó không có chữ số đứng đầu có ý nghĩa. 
4. Đối với mọi số khác 0, hãy liên tục loại bỏ các chữ số cuối cùng cho đến khi chỉ còn lại chữ số đầu tiên. Tăng bộ đếm của chữ số đó. 
5. Sau khi xử lý xong tất cả các số, hãy so sánh số chữ số`1`với mọi chữ số khác. Nếu nó thực sự lớn hơn tất cả chúng, hãy in`suspicious`; nếu không thì in`unfortunately not`. 

Lý do điều này có tác dụng là vì vấn đề chỉ hỏi về sự phân bố của các chữ số đứng đầu. Thuật toán duy trì chính xác các tần số đó và không có gì khác. Sau khi biết được số lượng, câu trả lời sẽ được xác định bằng một so sánh duy nhất. 

Tại sao nó hoạt động: 

Bất biến là sau khi xử lý bất kỳ tiền tố nào của đầu vào, mảng tần số chứa số lần xuất hiện chính xác của từng chữ số có nghĩa đứng đầu trong số các số được xử lý. Mỗi số khác 0 được xử lý đóng góp chính xác một lần vào bộ đếm của chữ số hàng đầu thực sự của nó, trong khi số 0 không đóng góp gì. Sau khi tất cả các số được xử lý, bất biến sẽ đưa ra phân bố tần số hoàn chỉnh, vì vậy hãy kiểm tra xem chữ số có`1`có tần số lớn nhất cho kết quả chính xác như yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)

    cnt = [0] * 10

    nums = input().split()
    for s in nums:
        x = int(s)
        if x == 0:
            continue

        x = abs(x)
        while x >= 10:
            x //= 10

        cnt[x] += 1

    for d in range(10):
        if d != 1 and cnt[1] <= cnt[d]:
            print("unfortunately not")
            return

    print("suspicious")

if __name__ == "__main__":
    solve()
```Mã chỉ lưu trữ mười bộ đếm vì các câu trả lời có thể có là các chữ số từ`0`ĐẾN`9`. Các số đầu vào được xử lý ngay lập tức nên không cần phải lưu toàn bộ tập dữ liệu vào bộ nhớ. 

các`abs`cuộc gọi loại bỏ ảnh hưởng của các dấu hiệu tiêu cực. Vòng lặp chia cho mười sẽ di chuyển từ một số đầy đủ về chữ số đầu tiên của nó. Ví dụ,`58320`trở thành`5832`, sau đó`583`, sau đó`58`, sau đó`5`. 

Việc kiểm tra số 0 xảy ra trước quá trình này vì việc chia số 0 nhiều lần sẽ khiến số 0 mãi mãi bằng 0, xử lý không chính xác nó như một chữ số hàng đầu hợp lệ. Vòng lặp cuối cùng sử dụng một so sánh nghiêm ngặt. Nếu một chữ số khác có cùng tần số với chữ số`1`, chữ số`1`không xuất hiện thường xuyên hơn nên câu trả lời phải là phủ định. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
99999999999 999999999999999 -99999999999 9999999999 420
```Dấu vết: 

| Số | Chữ số hàng đầu | Đếm 1 | Số lượng khác đã thay đổi | 
| --- | --- | --- | --- | 
| 99999999999 | 9 | 0 | 9 trở thành 1 | 
| 999999999999999 | 9 | 0 | 9 trở thành 2 | 
| -99999999999 | 9 | 0 | 9 trở thành 3 | 
| 9999999999 | 9 | 0 | 9 trở thành 4 | 
| 420 | 4 | 0 | 4 trở thành 1 | 

Số đếm cuối cùng có chữ số`9`xuất hiện bốn lần và chữ số`1`xuất hiện 0 lần, vì vậy kết quả đầu ra là:```
unfortunately not
```Dấu vết này cho thấy dấu hiệu bị bỏ qua và chỉ có chữ số đứng đầu mới quan trọng. 

### Mẫu 2 

đầu vào:```
10
38123 2118328 11273 -44884 18381238 99484832 87372 777372 -588382 123
```Dấu vết: 

| Số | Chữ số hàng đầu | Đếm 1 | Đếm 9 | 
| --- | --- | --- | --- | 
| 38123 | 3 | 0 | 0 | 
| 2118328 | 2 | 0 | 0 | 
| 11273 | 1 | 1 | 0 | 
| -44884 | 4 | 1 | 0 | 
| 18381238 | 1 | 2 | 0 | 
| 99484832 | 9 | 2 | 1 | 
| 87372 | 8 | 2 | 1 | 
| 777372 | 7 | 2 | 1 | 
| -588382 | 5 | 2 | 1 | 
| 123 | 1 | 3 | 1 | 

chữ số`1`xuất hiện ba lần, trong khi mọi chữ số khác xuất hiện nhiều nhất một lần. Kết quả là:```
suspicious
```Điều này thể hiện yêu cầu so sánh chặt chẽ. chữ số`1`không cần phải là chữ số duy nhất xuất hiện, nó chỉ cần có số lượng lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi số được giảm bằng cách chia cho mười, trong đó`A`là giá trị tuyệt đối của số Vì số chữ số bị giới hạn nên đây thực sự là O(n). | 
| Không gian | O(1) | Chỉ có mười quầy được lưu trữ. | 

Số chữ số tối đa là nhỏ ngay cả đối với số nguyên rất lớn, vì vậy mọi giá trị đầu vào đều được xử lý với một khối lượng công việc không đổi nhỏ. Thuật toán dễ dàng phù hợp với các giới hạn dự định vì nó thực hiện một lần duyệt qua tập dữ liệu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n_line = input().strip()
    if not n_line:
        return ""

    n = int(n_line)
    cnt = [0] * 10

    for s in input().split():
        x = int(s)
        if x == 0:
            continue
        x = abs(x)
        while x >= 10:
            x //= 10
        cnt[x] += 1

    for d in range(10):
        if d != 1 and cnt[1] <= cnt[d]:
            return "unfortunately not\n"
    return "suspicious\n"

assert solve_io("""5
99999999999 999999999999999 -99999999999 9999999999 420
""") == "unfortunately not\n", "sample 1"

assert solve_io("""10
38123 2118328 11273 -44884 18381238 99484832 87372 777372 -588382 123
""") == "suspicious\n", "sample 2"

assert solve_io("""1
1
""") == "suspicious\n", "single one"

assert solve_io("""5
0 0 0 10 19
""") == "suspicious\n", "zeros ignored"

assert solve_io("""4
-999 -888 123 456
""") == "unfortunately not\n", "negative values and ties"

assert solve_io("""6
111 123 199 1000 0 -100
""") == "suspicious\n", "many leading ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`suspicious`| Trường hợp tối thiểu khác không | 
|`0 0 0 10 19`|`suspicious`| Giá trị 0 không được tính | 
|`-999 -888 123 456`|`unfortunately not`| Số âm và chữ số cạnh tranh | 
|`111 123 199 1000 0 -100`|`suspicious`| Chữ số đầu lặp lại và bỏ qua số 0 | 

## Vỏ cạnh 

Đối với trường hợp:```
3
0 0 1
```Thuật toán bỏ qua cả hai số 0, sau đó trích chữ số đầu`1`từ số cuối cùng. Mảng đếm trở thành`cnt[1] = 1`và tất cả các số khác vẫn bằng 0, nên câu trả lời là`suspicious`. 

Đối với trường hợp:```
2
-999 100
```Thuật toán chuyển đổi`-999`vào trong`999`, trích chữ số`9`, sau đó trích chữ số`1`từ`100`. Số lượng là`cnt[9] = 1`Và`cnt[1] = 1`. Vì chữ số`1`được buộc thay vì lớn hơn, câu trả lời là`unfortunately not`. 

Đối với một bộ sưu tập lớn trong đó mọi giá trị đều bắt đầu bằng cùng một chữ số, chẳng hạn như:```
5
700 71 7999 0 -7123
```thuật toán đếm bốn lần xuất hiện của chữ số đầu`7`và bỏ qua số 0. chữ số`1`không vượt quá số lượng đó, vì vậy nó sẽ từ chối tập dữ liệu một cách chính xác. Điều này nắm bắt các giải pháp chỉ kiểm tra xem chữ số`1`xuất hiện thường xuyên thay vì kiểm tra xem nó có xuất hiện thường xuyên hơn mọi chữ số khác hay không.
